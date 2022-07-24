---
layout: post
title: "Reset Uptimekuma Password From Docker"
date: 2022-07-24 09:00:00 -0400
categories: [homelab]
tags: [homelab, ubuntu, bash, docker, uptimekuma]
---

## Uptimekuma Password Reset

Login into uptimekuma.chayde.lab via SSH, then run the following command:
```bash
docker exec -it uptime-kuma npm run reset-password
```