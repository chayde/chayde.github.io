---
layout: post
title: "Pihole - Forwarding Custom Domains"
date: 2022-09-17 21:00:00 -0400
categories: [homelab]
tags: [homelab, ubuntu, bash, pihole]
---

# Background
In my home network I have some separation for regular devices my wife and I use like our laptops/cell phones/smart tvs/etc and the devices I have in my lab environment. 

I've recently added a new network for a kubernettes testing lab I'm building. This gives me three main environments I have devices/services deployed to. Previously I used pihole installed on an Ubuntu VM for DNS/DHCP on my main network serving a DHCP addresses from a 192.168.x.x/24 pool. I used static DNS entries in the pihole server for hosts in my lab environment but this was getting annoying because of how many VMs and services I'd deployed. Additionally I wanted a separate domain for lab hosts (chayde.lab) vs hosts on my main network (chayde.lan) vs hosts in my kubernettes environment (k3.chayde.lab). 

# Solution
To accomplish this I need the pihole servers to forward lookups for specific domains to an alternate destinations. Pihole runs a service called dnsmasq for dns resolution. In order to accomplish what I wanted to so I deployed 2 more pihole servers one for each of the separate networks. 

In order to make sure any changes we made to the dnsmasq config were not over written when we perform upgrades for pihole I added a new file `03-custom-domains.conf` in the `/etc/dnsmasq.d/` folder. 

``` shell
# /etc/dnsmasq.d/03-custom-domains.conf

server=/k3.chayde.lab/172.20.x.x
server=/chayde.lab/172.17.x.x
server=/chayde.lan/192.168.x.x 
```

The IP addresses above are the server IPs for the pihole servers responsible for those domains/networks. I created that same file on each of the three pihole servers. I probably didn't need have the exact same file for each server but it was easier to setup. 