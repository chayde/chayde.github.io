---
layout: post
title: "Proxmox Ubuntu Cloud Init Template Notes"
date: 2022-08-07 19:00:00 -0400
categories: [homelab]
tags: [homelab, ubuntu, bash, cloud init, proxmox]
---

# Ubuntu Cloud Cloud Init Teplates
After following all the steps to setup an Ubuntu Cloud Init template in my proxmox lab using the following: [Perfect Proxmox Template with Cloud Image and Cloud Init](https://www.youtube.com/watch?v=shiIi38cJe4)

I ran into an issue where the cloud init image creates a virtual machine with a really small virtual disk (2.2G) which was too small to be useful.

According to the official [Proxmox Documentation](https://pve.proxmox.com/wiki/Resize_disks) you could resize your disks while they were either online or offline with the following command from the Proxmox VE node the VM lives on:
```shell
qm resize <vmid> <disk> <size>
```
Example: if you wanted to add 15G to a scsi0 disk on VM 100:
```shell
qm resize 100 scsi0 +15G
```

Alternatively you can do this via the GUI by navigating to `VM -> Hardware -> Hard Disk` then click on `Disk Action -> Resize`. Then just enter in the dialog box how many additional gigabytes you wish to increase the disk by. 

If you resize the disks before you boot the VM for the first time you dont have any issues. The problem I had was I'd already started a bunch of VMs with the original size, so I went searching online for a way to expand the VMs disks and found several posts about expanding the disks but you need to run several steps. 

I followed the steps above to resize the virtual disks. Once the disks were "physically" resized you need to make some updates in the OS so it expands the existing partitions. 

To start with I needed to list all the partitions: 
```shell
user@hostname:~$ sudo fdisk -l
...

Device      Start      End  Sectors  Size Type
/dev/sda1  227328 40263646 40036319 2.2G Linux filesystem
/dev/sda14   2048    10239     8192    4M BIOS boot
/dev/sda15  10240   227327   217088  106M EFI System

Partition table entries are not in disk order.
```

Next I used `sudo parted /dev/sda` and `sudo resize2fs /dev/sda1` to resize the file system then I rebooted the system to be safe. 
```shell
user@hostname:~$ sudo parted /dev/sda
GNU Parted 3.2
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) print                                                          
Warning: Not all of the space available to /dev/sda appears to be used, you can fix the GPT to use all of the space (an extra 1048576000
blocks) or continue with the current setting?
Fix/Ignore? fix                                                         
Model: QEMU QEMU HARDDISK (scsi)
Disk /dev/sda: 2GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name  Flags
1      1049kB  2097kB  1049kB                     bios_grub
2      2097kB  2.2GB   2.2GB   ext4

(parted) resizepart 2 100%                                              
Warning: Partition /dev/sda2 is being used. Are you sure you want to continue?
parted: invalid token: 100%                                              
Yes/No? yes
(parted) quit

user@hostname:~$ sudo resize2fs /dev/sda2
user@hostname:~$ sudo reboot
```

Once the system came back online I can see the root partition is now 20G
```shell
user@hostname:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           198M  964K  197M   1% /run
/dev/sda1        19G  1.9G   17G  11% /
tmpfs           989M     0  989M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
/dev/sda15      105M  5.3M  100M   5% /boot/efi
tmpfs           198M  4.0K  198M   1% /run/user/1000
```