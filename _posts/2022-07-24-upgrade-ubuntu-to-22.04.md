---
layout: post
title: "Upgrade to Ubuntu 22.04"
date: 2202-07-24 09:40:00 -0400
categories: [homelab]
tags: [ubuntu, homelab, bash]
---

# How to upgrade to Ubuntu 22.04

The first thing you need to do is run a standard update and upgrade to make sure all of the software installed on the machine is at the latest version. Log into your Ubuntu Server instance and issue the following two commands:
```bash
sudo apt-get update && sudo apt-get upgrade -y
```

When that completes, run the dist-upgrade command like so:
```bash
sudo apt-get dist-upgrade -y
```
Next, remove any packages that are no longer required with:
```bash
sudo apt-get autoremove -y
```

This is a good opportunity to remind you to back up your data and make sure you’re running this on a non-production machine.

We now need to install the update-manager-core package that allows us to upgrade from one release to another. To install this package, issue the command:
```bash
sudo apt install update-manager-core
```

Now, we can run the actual upgrade. Since Ubuntu 22.04 is still in beta, there is no official release available, so we have to force the upgrade. After April 21, 2022 (the planned date for the official release), you won’t have to use the -d flag. While 22.04 is still in beta, the upgrade command is:
```bash
sudo do-release-upgrade -d
```

Once the machine reboots, log in and issue this command to verify:
```bash
cat /etc/lsb-release
```