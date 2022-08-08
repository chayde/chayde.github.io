---
layout: post
title: "Install Docker and Docker compose"
date: 2022-07-24 09:50:00 -0400
categories: [homelab]
tags: [homelab, ubuntu, bash, docker, docker-compose, howto]
---

## Install Docker
```bash
sudo apt-get update
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```

## Verify Docker Install
```bash
docker -v 
``` 

## Install Docker Compose
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.8.0/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

> NOTE: in the command above v2.8.0 was the latest version at the time of writting. You will need to update that value as new versions are released.
{: .prompt-info }

## Verify Docker Compose Install
```bash
docker-compose -v
```

## Run Docker Without Sudo
```bash
sudo usermod -aG docker $USER
```
