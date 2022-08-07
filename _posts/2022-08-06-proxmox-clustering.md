---
layout: post
title: "Proxmox Clustering Notes"
date: 2022-08-06 20:00:00 -0400
categories: [homelab]
tags: [homelab, proxmox, bash]
---

# Proxmox Clustering Notes

## Creating a new Cluster

### Create a cluster via the GUI

Creating a cluster is fairly straight forward you click on `Datacenter > Cluster > Create Cluster` then give the cluster a unique name, you cannot change this name later without destroying the cluster and starting over. 

### Create a cluster via the CLI
Log into your first Proxmox node via SSH and run: 
```shell
pvecm create <CLUSTERNAME>
```
Once the cluster has finished being created you can check the status with the following command: 
```state
pvecm status
```
You can list the nodes that are members of the cluster with the following command: 
```shell
pvecm nodes
```


## Adding Nodes to an Existing Cluster

From the [Proxmox Clustering Documentation](https://pve.proxmox.com/pve-docs/chapter-pvecm.html#pvecm_separate_node_without_reinstall): 

> A node that is about to be added to the cluster cannot hold any guests. All existing configuration in /etc/pve is overwritten when joining a cluster, since guest IDs could otherwise conflict. As a workaround, you can create a backup of the guest (vzdump) and restore it under a different ID, after the node has been added to the cluster.
{: .prompt-info }

### Join a node to the cluster via the GUI
Joining nodes to an existing cluster is almost as straight forward as creating the cluster was. 
* Log into both the first node in the cluster as well as the node you want to add then navigate to click on `Datacenter > Cluster` 
* On the first node click on `Join Information` then click `Copy Information`
* Next from the node you want to add to the cluster click on `Join Cluster` paste the join information you copied from the first node, enter the first node's root password in the password box and click `Join`

### Join a node to the cluster via the CLI

Log into the node you want to join to the cluster via SSH and run: 
```shell
pvecm add <IP_FIRST_CLUSTER_NODE>
```

## Removing Cluster Configurations
If you have any issues with clustering or just want to completely remove the cluster configs from your nodes you will need to do the following. 

> This is not the recommended method per the official Proxmox documentation however these were the only instructions that worked for me. Running through these steps prevented me from having to reinstall Proxmox on all my nodes. Additionally you need to ensure that all shared resources are cleanly separated or you may run into conflicts later.
{: .prompt-danger }

Stop the corosync and pve-cluster services:
```shell
systemctl stop pve-cluster
systemctl stop corosync
```

Restart the cluster file system in local mode:
```shell
pmxcfs -l
```

Remove the corosync configuration files
```shell
rm /etc/pve/corosync.conf
rm -r /etc/corosync/*
```

Restart the file system again as a normal service
```shell
killall pmxcfs
systemctl start pve-cluster
```

The node is now separated from the cluster. You can log into the other nodes in the cluster and delete it.
```shell
pvecm delnode <NODE_NAME>
```

If the command fails with an error that says there was a loss of quorum you can work around that with the following command
```shell
pvecm expected 1
```

Next switch back to the node you are removing and delete the rest of the clustering config files
```shell
rm /var/lib/corosync/*
```

In addition to the steps above I had to also remove the entries in the `/etc/pve/nodes` folder. The clustering config creates a directory for each node added to the cluster. 
```shell
rm -rf /etc/pve/nodes/*
```

> Clustering and configuration sync happen over SSH, the nodes exchange ssh keys stored in the `/etc/pve/priv/authorized_keys` file, you will need to remove these entries as well. 
{: .prompt-info }