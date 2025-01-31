+++
slug = 'how-i-do-email'
title = 'How I do Email'
date = 2022-10-13T00:00:00+00:00
draft = false
tags = ["Email"]
+++
I've used one single mail address for over a decade until it was involved in some breaches and thereupon has been sold to spammers. This address has been rendered unusable for me because I was drowning in spam since then. I've then decided to start over with a custom domain, unique mail addresses per website/account and to update all my online accounts to the new addresses. In fact, I am now using 200+ mail addresses.

## The new system
  * For every person/website/etc. a unique mail address is used.
  * The part of the address before the _@_ consists of a descriptive name (e.g. _ebay_), an underscore (_\__) and finally some sort of public password. This can be any string and is used in all the mail addresses. Here is an example: _ebay\_xyz@domain.tld_. Only the part before the _\_xyz@domain.tld_ is account specific and therefore unique.
  * Email addresses will not be explicitly created. With the help of a catch-all rule any mails sent to _domain.tld_ will be received and processed by filter rules.
  * A filter rule deletes all incoming mails that don't end with _\_xyz@domain.tld_. This prevents getting spam sent to guessed and/or generic mail addresses.
  * An additional filter rule deletes all incoming mails sent to a list of "burnt" mail addresses (see below).
  * The new mail provider supports sending mails from any mail address of validated custom mail domains. When replying to mails sent to a unique mail address, the _FROM_ field is automatically populated with the address of the _TO_ field from the original mail.

## Benefits

If an address is sold to spammers or a website gets compromised and my mail address gets stolen, I now immediately will know where the leak was. Furthermore, all mails sent to this address can then be automatically deleted with a filter rule.

Leaked mail addresses are constantly used e.g. in credential stuffing attacks. If I would use the same address anywhere, the attackers would then have one of (most of the time) two factors, the user name. It can furthermore be problematic if someone looks up a mail address in haveibeenpwned.com. People would then know where I have user accounts/memberships and might get the wrong impression on how I chose and handle passwords.

If someone asks me on the phone or in person for my mail address, I don't have to think about whom I give which address. I just generate one in my head and give it away.

## Downsides

I have not identified any downsides of this approach yet. One could fear that it's impossible to keep track of where which address is in use. But since I am using a password manager, the mail addresses are documented there so it is not an issue to me.

## Improvements

I am thinking about changing the "public password" on a yearly basis in a way, that it reflects the year. One idea is to use the hex value of the year when the mail address has been created. This makes generating new addresses for the same account easier (if it doesn't happen twice a year) and helps with the filtering of burnt addresses.