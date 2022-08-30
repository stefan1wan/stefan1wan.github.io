---
layout: post
title: LITE Kernel RDMA
date: 2020-12-10
tags: KnowledgeShare
author: Junpeng Wan
---

# Paper Read: LITE Kernel RDMA
Next week I'll make a presentation in *Advanced Network*, a graduate course. Our teacher provided a paper list about Computer Network, from which we can choose a paper and make a presentation, and then introduce it in class. The paper I choose is a 2017 *[SOSP](https://www.sigops.org/s/conferences/sosp/2017/program.html)* paper *[LITE Kernel RDMA Support for Datacenter Applications](https://cseweb.ucsd.edu/~yiying/LITE-sosp17.pdf)*. Here are some important points of their work.


## The Abstraction Mismatch
<img src="/images/posts/LITE_RDMA/Native_RDMA.png" style="zoom:50%" />
<!-- ![](/images/posts/LITE_RDMA/Native_RDMA.png) -->

As in the picture, in Native RDMA, the programmer will write codes on the libraries provided by the RNIC hardware which totally bypassing the kernel. So what the native RDMA mechanism has is Low-level and Difficult-to-use APIs, whilst what the developer wants is high-level and easy-to-use APIs. Therefore there is an Abstraction Mismatch. 

Things worked well in HPC(High-Performance Computing)  because it has special hardware, few applications, and relatively cheaper developers. While in the datacenters, we just have commodity and cheaper hardware and what we handle are a lot of changing apps. The resource sharing and isolation will also be a problem in this scenario. 

Hince, things will be very complicated when trying to use native RDMA in datacenters.

## What This Paper Do In General
This paper adds an Indirection tier in the Linux kernel to support RDMA operations at the OS(operating system) level. The Name *LITE* comes from "Local Indirection TiEr", as it refers, the designer just adds one local kernel layer in the local node. The remote side is the same as the native RDMA. With the support of OS, it can provide high-level APIS to userspace, so the applications will be simpler. *LITE* also on-load the *Permission check* and *Address mapping* operations into the kernel, so *LITE* needs simpler hardware.  

## Design and Abstraction Principles
If we want the features like *High-level abstraction*, *Resource sharing*, *Performance isolation*, and *Protection*, one easy way is to use kernel. And as Butler Lampson says, "All problems in computer science can be solved by another level of indirection". So what the *LITE* do is to add an indirection layer in the kernel. *LITE* works on RDMA verbs so it's easy to support different hardware. By the way, verbs are just low-level descriptions of RDMA, not APIs.

They list three design principles:
* 1. Indirection only at local for one-sided RDMA
* 2. Avoid hardware indirection
* 3. Hide kernel cost

To avoid the present hardware indirection,  the author finds an API which can register physical address in the kernel. In this way, there is no need to cache the PTEs. *LITE* registers the whole memory at once and manages them in the kernel, so we just need to store one pair of global keys in RNIC SRAM.

## Some Performance
LITE scales much better than native RDMA wrt MR size and numbers.
<img src="/images/posts/LITE_RDMA/MR_SC.png" style="zoom:50%" />
<!-- ![](/images/posts/LITE_RDMA/MR_SC.png) -->
LITE only adds a very slight overhead even when native RDMA doesn’t have scalability issues
<img src="/images/posts/LITE_RDMA/Latency.png" style="zoom:50%" />
<!-- ![](/images/posts/LITE_RDMA/Latency.png) -->


## In The End
In the *[code](https://github.com/WukLab/LITE)* of LITE, we can see that it was written and compiled into several kernel modules, but only supports kernel version *3.11.1*, *3.10.108* and *4.9*.  If possible, I will read the source code and write another blog(it's a flag :)). But since the *IO_uring* have already appeared in the kernel, LITE will be harder to be put in use(as my advisor says). 
