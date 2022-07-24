---
layout: post
title: "Start ESXI VM From CLI"
date: 2022-07-24 10:30:00 -0400
categories: [homelab]
tags: [homelab, esxi]
---

# ESXI CLI

Enable ESXI SSH access then connect the ESXi host via SSH. 

To get a list of all VMs run: 
```bash
vim-cmd vmsvc/getallvms
```

Find the ID for the VM you want to start and Replace <##> in the command below: 
```bash
vim-cmd vmsvc/power.on <##>
```

