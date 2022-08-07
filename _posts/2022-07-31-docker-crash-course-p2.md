---
layout: post
title: "Crash Course on Docker, Kubernetes, Terraform, and AWS - Part 2: Kubernetes"
date: 2022-07-31 09:00:00 -0400
categories: [homelab]
tags: [homelab, ubuntu, docker, kubernetes, aws, terraform, IaS, infrastructure as code]
---

# Kubernetes
## What is Kubernetes
Kubernetes is an orchestration tool, which means it’s a tool for running and managing applications across a fleet of servers. More specifically, it’s a container orchestration tool, which means it is designed to deploy and manage applications packaged as containers (if you’re new to technologies such as containers and Docker, make sure to check out part 1 of this series, A crash course on Docker). You give Kubernetes a fleet of servers to manage and in return, it gives you all the following functionality, out-of-the-box:

* Scheduling: pick the optimal servers to run your containers (bin packing).
* Deployment: roll out changes to your containers (without downtime).
* Auto healing: automatically redeploy containers that failed.
* Auto scaling: scale the number of containers up and down with load.
* Networking: routing, load balancing, & service discovery for containers.
* Configuration: configure data and secrets for containers.
* Data storage: manage and mount data volumes in containers.

Under the hood, Kubernetes architecture consists of two main pieces: a control plane and worker nodes.

* Control plane: The control plane is responsible for managing the Kubernetes cluster. It is the core of the setup. The control plane is responsible for storing the state of the cluster, monitoring containers, and coordinating actions across the cluster. It runs the API server which provides an interface for various command line tools (ex: kubectl), web UIs (ex: Kubernetes Dashboard), as well as other infrastructure as code tools (ex: Terraform) to control what’s happening throughout the cluster.
* Worker nodes: The worker nodes are the servers the actual compute/memory/storage used to actually run your containers. The worker nodes are entirely managed by the control plane, will handle each worker node and what containers those nodes should run.

