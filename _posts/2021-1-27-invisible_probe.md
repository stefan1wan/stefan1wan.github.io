---
layout: post
title: Invisible Probe
date: 2021-3-2
tags: Research
author: Stefan Wan
---

# Invisible Probe: Timing Attacks with PCIe Congestion Side-channel

## Introduction
In this blog, I will introduce a research project done by our group, [*Invisible Probe: Timing Attacks with PCIe Congestion Side-channel*](a.b.c). [My supervisor](http://homepage.fudan.edu.cn/zz113/) leaded this program and my contribution is mainly in the experiment part which contains a little exploration. The details are on the paper.

## Attack Surface - PCIe (Peripheral Component Interconnect express) Link

This is the first work focus on a side-channel attack through the congestion in PCIe. If the attacker commits congestion upon PCIe link, she may percept what is transferring on this PCIe link. And we find 2 threat scenarios and test them by design 4 specific experiments.

## Two Threat Scenarios
![](/images/posts/inv_probe/topology.png)

### PCH: NVMe SSD & NIC
PCH (Platform Controller Hub) was designed to connect multiple relatively slow devices, like hard disks, sound cards, and NICs. We assume that an NVMe SSD card and a NIC are both connected by PCH, so the attacker can repeatedly access the SSD by [SPDK](https://spdk.io/) to congest PCH and log the intervals between every 2 accession. If a victim is browsing a website at the same time, the traffic transferred back by NIC will increase the intervals logged by our attacker. If we train the different logs with deep learning, we can distinguish the different websites which the victims are browsing. 

### PCIe Switch: RDMA NIC & GPU
PCIe Switch allows several devices connected to one interface offered by CPU. We assume that an RDMA NIC and a GPU are connected by a PCIe switch. The attacker will repeatedly access memory through another machine's RDMA NIC. Therefore, traffic transferred on PCIe switch will be discovered. Victim's activities which related to GPU, like browsing website, training models and input password, will be perceived.

## Four Specific Experiment

|  NO  | Congested at  | Attacker operates on | Victim operates on  |  Information stealed  |
|  ----  | ----  | ----  | ----  | ---- |
| A  | PCH | NVMe SSD | NIC | Websites |
| B  | PCIe switch |  RDMA NIC   | GPU |  Websites|
| C  | PCIe switch |  RDMA NIC   | GPU |  Trained models|
| D  | PCIe switch |  RDMA NIC   | GPU |  Password keystrokes|

In the 4 experiments above, we first use our attack scripts to congest with logging the access time intervals and then recover information in 2 ways. For A, B and C, we collect enough data and train deep learning models.  For D, we just extract the keystrokes from logs. When the victim inputs the password in Chrome, the intervals are like Fig. 3, and red stars are the keystrokes related to the password. 
![](/images/posts/inv_probe/strokes.png)

<!-- +  Congest PCH with NVMe to distinguish website 
+  Congest PCIe switch with RDMA NIC to distinguish websites
+  Congest PCIe switch with RDMA NIC to distinguish trained models
+ Congest PCIe switch with RDMA NIC to distinguish password keystrokes -->



