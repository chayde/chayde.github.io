---
layout: post
title: "Self-Hosted Git Repositories"
date: 2022-11-05 09:52:00 -0400
categories: [homelab]
tags: [homelab, ubuntu, bash, NetDevOps]
---

## Intro
I have been dipping my toe into trying to learn DevOps/NetDevOps concepts in the last few months. One of the things I've been curious about was learning a bit more about git and how it works, I wanted to be able to host an internal set of repositories for any projects I work on while I learn about these things. Github as a free service is great if you are ok with all of your respositories being public, while I'm learning I would greatly prefer a private respository which I'd have to pay for on Github. It's not a huge cost but I'm trying to learn here and figuring out how to do it myself is the reason I'm working on this stuff in the first place. 

After a bit of research I find that it is indeed possible to internally host your own repositories and its actually faily simple to setup. I found a great write up on the [Linux Foundation](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-how-to-run-your-own-git-server) website that goes through the details. For this example I'm going with the bare bones Git on my own server. They have another option for installing and configuring Gitlab that offers a web based GUI similar to Github which I may try later on.

## Install Git 
The tutorial uses a remote-server and a local-host to setup the demo and they were using Ubuntu as their Linux server of choice. The first step was to install `git` via 
``` bash
sudo apt install get-core
```
I did not have to do this step as `git` was already installed in the Ubuntu 22.04 version I was running. 

They also suggest setting up password-less ssh login and provide the steps to do so. Again in my environment this is already setup via cloudinit whenever I clone a new VM. 

## Setup git on remote-server
SSH to the remote server and create a project directory for git. For my examples I'm going to store things in the home directory for 'user' who is my lab user account. In the exmaple below you dont need to include `.git` in the directory name 
``` bash
user@server:~ $ mkdir -p /home/user/gittest.git
```
Next we need to change to that directory and create an empty repo
``` bash
cd /home/user/gittest.git
git init --bare
```

## Setup git on local-host
Now we need to setup git on our local host, I'm using a Windows 11 workstation for this but the instructions should work the same for other operating systems. 

First we need to create a directory to work from then change into that directory. For my exmaple, I'm putting that on my E: drive under the repositories folder. 
``` cmd
C:\Users\User> E:
E:\> cd repositories
E:\repositories\> mkdir gittest
E:\repositories\> cd gittest 
E:\repositories\gittest\> 
```
Next we need to create some files for this project. I created a really basic `README.md` file and saved it in the directory. Then we need to initialize git
```
E:\repositories\gittest\> git init 
Initialized empty Git repository in E:/repositories/gittest/.git/
```
Now we add the files from the folder into the repo
```
E:\repositories\gittest\> git add .
```
Any time you want make changes to the repo by adding/removing/modifying you will need to include a commit message describing what changed. The  `-a` switch in the command below will apply that message to all files added with the "`git add .`" command above.
```
E:\repositories\gittest\> git commit -m "initial commit" -a
[master 373b548] initial commit
 1 file changed, 1 insertion(+), 1 deletion(-)

 E:\repositories\gittest\> 
```
You can add a message to individual files if you specify the file name instead of using `-a` like this: 
```
E:\repositories\gittest\> git commit -m "message" README.md
[master e517b10] message
 1 file changed, 1 insertion(+)
```
Finally we need to push these changes to the remote-server. Up to this point we have only been working on our local instance of the repository. This step will sync these changes with the remote server so you can access them from other locations. If this repository is hosted on an internet accessible server you can even collaborate with others by supplying them with the server and path information for your repository. 
```
E:\repositories\gittest\> git remote add origin ssh://user@remote-server/home/user/gittest.git/
```
Now you can push or pull changes between the remote-server and the localhost using the push or pull option in git
```
E:\repositories\gittest\> git push origin master
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 16 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 292 bytes | 292.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To ssh://remote-server/home/user/gittest.git/
   2d1bd8a..373b548  master -> master
```
If you are collaborating with others or need to work on the project from another host you can clone that repo from the server with the following: 
```
git clone user@remote-server:/home/user/gittest.git
```

## A note about vscode
> If you're using `vscode` to as your text editor once you've initialized a directory for get or cloned a repository you can use the built in `git` feature in vscode to handle the add/commit/push/pull functions instead of having to do it manually from the cli.
{: .prompt-tip }