+++
slug = 'proxmox-server'
title = "Cheap low-power Proxmox server"
date = 2025-02-12T10:00:00+00:00
draft = false
tags = ["Homelab", "proxmox"]
+++

After I decided to reduce hosting costs and generally bring my web applications from the cloud to my home, I needed an energy-saving server on which I could install Proxmox. After some research, I decided to buy a refurbished Dell Wyse 5070 thin client. I was able to purchase this from a commercial dealer for just under 50€ including power supply, stand and shipping. As I bought it, it had the following specs:

 * Celeron(R) J4105 CPU @ 1.50GHz
 * 8GB SO-DIMM DDR4 RAM
 * 64GB SSD

Fortunately, I still had two suitable RAM modules with 8GB each at home, so I was able to update directly to 16GB RAM. I upgraded the SSD to 256GB for €20, so the server ended up costing €70.

The installation of Proxmox via a USB flash drive went absolutely smoothly. I decided to use Proxmox after hearing about the great [Proxmox VE Helper Scripts](https://community-scripts.github.io/ProxmoxVE/scripts). These allow you to install and keep a lot of popular applications up to date with a one-liner. Using these scripts I have installed the following apps so far:

 * [AdGuard Home](https://community-scripts.github.io/ProxmoxVE/scripts?id=adguard) for blocking ads and tracker in my local network
 * [Wavelog](https://community-scripts.github.io/ProxmoxVE/scripts?id=wavelog) for logging ham radio contacts
 * [The Lounge](https://community-scripts.github.io/ProxmoxVE/scripts?id=the-lounge) because IRC is not dead
 * [FreshRSS](https://community-scripts.github.io/ProxmoxVE/scripts?id=freshrss) because RSS will never die
 * [Node-Red](https://community-scripts.github.io/ProxmoxVE/scripts?id=node-red) because I want to learn more about it
 * [TasmoAdmin](https://community-scripts.github.io/ProxmoxVE/scripts?id=tasmoadmin) to control my Tasmota devices
 * [Gitea](https://community-scripts.github.io/ProxmoxVE/scripts?id=gitea) to host my code
 * [Zoraxy](https://community-scripts.github.io/ProxmoxVE/scripts?id=zoraxy) as a reverse proxy for all internal apps

I've written a separate post on Zoraxy and how to have a wildcard SSL/TLS certificate for your internal network domain which you can find [here](/homelab-wildcard-cert-zoraxy-clouflare).

At first I wasn't sure whether the small thin client really had enough power to host several applications at the same time. But so far I'm more than convinced that I've made the right choice:

![](/img/homelab-server-01.jpg)

