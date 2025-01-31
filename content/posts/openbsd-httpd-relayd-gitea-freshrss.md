+++
slug = 'openbsd-httpd-relayd-gitea-freshrss'
title = 'Hosting Gitea and FreshRSS on OpenBSD with httpd and relayd'
date = 2024-03-04T08:19:00+00:00
draft = false
tags = []
+++

This article tries to document the essential steps to achieve the following on a fresh OpenBSD 7.4 install:

* Serve static files via HTTP(S)
* Host a Gitea instance via HTTPS
* Host a PHP based FreshRSS instance via HTTP(S)
* Install and maintain Let's Encrypt certificates
* Serve all above via IPv4 and IPv6
* Protect SSH via sshguard
* Implement a reasonable secure packet filtering ruleset

The aim is to use built-in services such as httpd, relayd and acme instead of third party packages. You will not find any installation/configuration of database systems here, as I use SQLite for both Gitea and FreshRSS. The focus is on the server components, not the hosted applications, so I will not go into detail on how to configure them.

## Packet Filter: pf and sshguard

Install sshguard:

``` bash
$ pkg_add sshguard
```

Edit __/etc/pf.conf__, paste the following and adapt to your needs:

``` bash
table <martians> {
  0.0.0.0/8 10.0.0.0/8 100.64.0.0/10            \
  127.0.0.0/8 169.254.0.0/16 172.16.0.0/12      \
  192.0.0.0/24 192.0.2.0/24 192.88.99.0/24      \
  192.168.0.0/16 198.18.0.0/15 198.51.100.0/24  \
  203.0.113.0/24 224.0.0.0/3 255.255.255.255/32 \
  ::/128 ::/96 ::1/128 ::ffff:0:0/96 100::/64   \
  2001:10::/28 2001:2::/48 2001:db8::/32        \
  3ffe::/16 fec0::/10 fc00::/7 }
 
## Set http(80)/https (443) port here ##
webports = "{http, https}"
 
## enable these services ##
int_tcp_services = "{domain, ntp, smtp, www, https, ftp, ssh, 31415}"
int_udp_services = "{domain, ntp}"

set block-policy drop
set loginterface egress
 
## Skip loop back interface - Skip all PF processing on interface ##
set skip on lo

match in all scrub (no-df random-id max-mss 1440)
match out on egress inet from !(egress:network) to any nat-to (egress:0)
 
## Blocking spoofed packets
antispoof quick for egress

# Drop all Non-Routable Addresses 
block in quick on egress from <martians> to any
block return out quick on egress from any to <martians>
 
## Set default policy ##
block all

table <sshguard> persist
block in quick proto tcp from <sshguard>
 
pass out quick
 
# Allow SSH
pass in inet proto tcp to egress port ssh
pass in inet6 proto tcp to egress port ssh

# Allow ICMP
pass in on egress inet proto icmp all icmp-type echoreq
pass in on egress inet6 proto icmp6 all icmp6-type echoreq

# Allow access to httpd and relayd
pass in inet proto { tcp udp } to egress port $webports
pass in inet6 proto { tcp udp } to egress port $webports
 
# Allow essential outgoing traffic 
pass out quick proto tcp to any port $int_tcp_services
pass out quick proto udp to any port $int_udp_services
pass out quick inet6 proto tcp to any port $int_tcp_services
pass out quick inet6 proto udp to any port $int_udp_services
```

Open a __screen__ session and execute the following inside a dedicated screen:

``` bash
$ sleep 120; pfctl -d
```

This allows us to reload the pf configuration without locking us out permanently. If make a mistake, pf will be disabled latest afer 2 minutes allowing us to ssh back in and fix the error.

``` bash
$ pfctl -n -f /etc/pf.conf
$ pfctl -f /etc/pf.conf
```

Try to ssh in after reloading the config to make sure that at least this still works. If all is fine, don't forget to kill the "sleep 120; pfctl -d" process.

## Web Server: httpd and PHP

In this step we will configure httpd to serve static HTML as well as PHP files. It furthermore is needed for retrieving SSL certificates from Let's Encrypt via ACME client.

Edit __/etc/httpd__, paste the following and adapt to your needs:

``` bash
server "static.domain.com" {
   listen on * port 80
   root "/htdocs/static.domain.com"
   log style combined

   location "/.well-known/acme-challenge/*" {
      root "/acme"
      request strip 2
    }
}

server "blogs.com" {
   listen on * port 80
   location "/*.php*" { fastcgi socket "/run/php-fpm.sock" }
   root "/freshrss/p/"
   directory index index.php
   log style combined

   location "/.well-known/acme-challenge/*" {
      root "/acme"
      request strip 2
   }
}

server "git.domain.com" {
   listen on * port 80
   log style combined

   location "/.well-known/acme-challenge/*" {
      root "/acme"
      request strip 2
   }

   location * {
       block return 301 "https://$HTTP_HOST$REQUEST_URI"
   }
}
```

Enable and start PHP:

``` bash
$ rcctl enable php81_fpm
$ rcctl start php81_fpm
```

Enable and start httpd:

``` bash
$ rcctl enable httpd
$ rcctl start httpd
```

Now it's time to configure ACME to retrieve our SSL certificates:

Edit __/etc/acme-client.conf__, paste the following and adapt to your needs:

``` bash
authority letsencrypt {
        api url "https://acme-v02.api.letsencrypt.org/directory"
        account key "/etc/acme/letsencrypt-privkey.pem"
}

authority letsencrypt-staging {
        api url "https://acme-staging.api.letsencrypt.org/directory"
        account key "/etc/acme/letsencrypt-staging-privkey.pem"
}

domain blogs.com {
       domain key "/etc/ssl/private/blogs.com.key"
       domain full chain certificate "/etc/ssl/blogs.com.crt"
       sign with letsencrypt
}

domain git.domain.com {
       domain key "/etc/ssl/private/git.domain.com.key"
       domain full chain certificate "/etc/ssl/git.domain.com.crt"
       sign with letsencrypt
}

domain static.domain.com {
       domain key "/etc/ssl/private/static.domain.com.key"
       domain full chain certificate "/etc/ssl/static.domain.com.crt"
       sign with letsencrypt
}
```

We can now get our new certificates:

``` bash
$ /usr/sbin/acme-client -v blogs.com
$ /usr/sbin/acme-client -v git.domain.com
$ /usr/sbin/acme-client -v static.domain.com
```

In order to make sure that they will be updated regularly, we add the following lines to our crontab:

``` bash
$ crontab -e
```

``` bash
0 2 * * * /usr/sbin/acme-client -v beta.blogs.com >> /tmp/acme.log 2>&1
5 2 * * * /usr/sbin/acme-client -v git.domain.com >> /tmp/acme.log 2>&1
10 2 * * * /usr/sbin/acme-client -v static.domain.com >> /tmp/acme.log 2>&1
```

After this has been done, we can now configure and start relayd.

## Relay Daemon: relayd

We will make use of relayd to terminate SSL connections and forward HTTP requests internally to either httpd or the gitea daemon.

To do so, edit __/etc/relayd__, paste the following and adapt to your needs:

``` bash
# Macros -----------------------------------
ext_ipv4="46.23.93.217"
ext_ipv6="2a03:6000:93f4:614::217"
webhost="127.0.0.1"
webhost6="::1"

table <webserver>  { $webhost }
table <gitea> { $webhost }
table <webserver6> { $webhost6 }

http protocol https {
  tls keypair git.domain.com
  tls keypair static.domain.com
  tls keypair blogs.com

  block
  pass request header "Host" value "git.domain.com" \
    forward to <gitea>
  pass request header "Host" value "static.domain.com" \
    forward to <webserver>
  pass request header "Host" value "blogs.com" \
    forward to <webserver>
}

relay https {
  listen on $ext_ipv4 port 443 tls
  protocol https
  forward to <webserver> port 80 mode roundrobin \
    check http "/" code 200
  forward to <gitea> port 3000
}

relay https6 {
  listen on $ext_ipv6 port 443 tls
  protocol https
  forward to <webserver6> port 80 mode roundrobin \
    check http "/" code 200
  forward to <gitea> port 3000
}
```

Enable and start relayd:

``` bash
$ rcctl enable relayd
$ rcctl start relayd
```

## FreshRSS

Install FreshRSS and some dependencies:

``` bash
$ pkg_add freshrss php-zip php-curl
```

FreshRSS should now be accessible and configurable via your browser.

Add the following line to your crontab to automatically update all RSS feeds every 5th minute:

``` bash
*/5 * * * * doas -u www /usr/local/bin/php -f /var/www/freshrss/app/actualize_script.php > /tmp/FreshRSS.log 2>&1
```

## Gitea

``` bash
$ pkg_add gitea
```

Enable and start gitea and gitdaemon:

``` bash
$ rcctl enable gitea gitdaemon 
$ rcctl enable gitea gitdaemon 
$ rcctl start gitdaemon
$ rcctl start gitdaemon
```

Gitea should now be accessible and configurable via your browser.

