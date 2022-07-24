---
layout: post
title: "Change Hostname On Ubuntu Linux CLI"
date: 2022-07-24 09:30:00 -0400
categories: [homelab]
tags: [homelab, ubuntu, bash]
---

# Change Hostname On Ubuntu Linux CLI

To view the current hostname setup
```bash
hostnamectl
```

Run command to change the hostname
```bash
sudo hostnamectl set-hostname <NEW_HOSTNAME_HERE>
```

Edit the /etc/hosts file and update any localhost entries
```bash
sudo nano /etc/hosts
```

Reboot the host
```bash
sudo reboot
```
