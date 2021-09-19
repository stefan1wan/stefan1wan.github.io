---
layout: post
title: The Network-On-Chip Structure of Skylake and Congestion Monitoring
date: 2021-3-3
tags: Research
author: Stefan Wan
---

# The Network-On-Chip Structure of Skylake and Congestion Monitoring
If we want to understand the functions and behaviors of the mesh network in Skylake, one way is via [PMON](http://kib.kiev.ua/x86docs/Intel/PerfMon/336274-001.pdf). We can monitor the number of counters for several specific events through PMON to speculate the inner states of CPU. For example, if we monitor event HORZ_RING_BL_IN_USE and read the corresponding counters, it will tell us how many Uncore cycles the horizontal block ring is in use. One of our aims is to illustrate the congestion degree by counting PMON events among where are a lot of events related to congestion, but it's a pity that Intel didn't explain these events clearly. However, if we know some design ideas about the Skylake Network On Chip(NOC) structure, especially the design of the routers and the functions of flow control, we are able to learn more from these events.


![](/images/posts/Skylake_NOC/mesh.png)
## The Router
From the macro-perspective, the NOC of Skylake is a mesh network. The routing algorithm is a Y-X route that could avoid deadlock and is easy to implement. The data goes along the vertical ring firstly and then the horizontal ring. The Common Ring Stop(CMS) is actually the router of the mesh and connects rings of four directions. The picture below is CMS on the PMON document(Ref. 1). It has two agents with partly different functions, which can transfer data from AD, AK, BL, and IV rings.  AD, AK, and BL rings have two directions respectively, but the IV ring has only one direction.
![](/images/posts/Skylake_NOC/cms.png)
We can see that from the descriptions of Unit Masks for TxR_VERT_CYCLES_FULL.
![](/images/posts/Skylake_NOC/cycles_full.png)
By the way, in the PMON events, Egress has vertical and horizontal descriptions while Ingress just has the horizontal description. And the EGR area is about twice of IGR. So we guess that the Egress buffers have a specific buffer for each direction and the Ingress buffer stores all the incoming packets.

To understand the microarchitecture, one of the possible sources is a paper written by Intel(Ref. 3), which is published about when the Skylake was designed. The router's design is as follows.
![](/images/posts/Skylake_NOC/modex.png)

## Flow Control
According to the descriptions in PMON events, the flow control method of Skylake is Flowless routing, and specifically, Credit-based routing.

## Congestion Moniter
We can infer the congestion state from many CMS events, and the following are some:
* RING_IN_USE: the Uncore cycles during which the rings are in use 
* NACK: not get responded when CMS send messages
* BYPASS:  packets that bypass CMS Ingress or Egress buffer
* ADS: Anti Deadlock Slot was used
* SINK_STARVED: discarded packets due to starvation
* STALL: stalled  due to lacking credits

The experiment evaluation can be found on our [paper](https://arxiv.org/pdf/2103.04533.pdf).

## Reference
1. [Intel® Xeon® Processor Scalable Memory Family Uncore Performance Monitoring](http://kib.kiev.ua/x86docs/Intel/PerfMon/336274-001.pdf) 
2. [Topology and Cache Coherence in Knights Landing and Skylake Xeon Processors](https://slideplayer.com/slide/14268395/)
3.  MoDe-X: Microarchitecture of a Layout-Aware Modular Decoupled Crossbar for On-Chip Interconnects, IEEE Transactions on Computers, Vol. 63, No. 3, March 2014 P622.
4. [Intel's patent about On-chip mesh interconnect](https://patents.google.com/patent/US20150006776)
5. [Intel's patent about shared mesh](https://patents.google.com/patent/US20170019350A1/en)
6. [Cms answer on stackoverflow](https://stackoverflow.com/questions/50077189/skylake-and-newer-ring-bus)
7. [mesh priciples](https://en.wikichip.org/wiki/intel/mesh_interconnect_architecture)