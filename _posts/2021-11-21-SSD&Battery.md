---
layout: post
title: Change SSD and Battery for my old MBP
date: 2021-11-20
tags: Tutorial
author: Stefan Wan
---

## Intro
I bought my MacBook Pro five years ago. Now in the winter of 2021, its battery life is significantly short, about 1 hour for typing words, and it contains a small-space SSD with 256GB space. I have to store my virtual machines and some data on an external SSD. 
Fortunately, I found that for my model, Retina, 15-inch, Mid 2015(A1398), both the battery and the SSD Card could be replaced. 

## My choices
The SSD strategy was learned from [1] and the Battery choice was learned from [2]. It costs me about 1000 RMB altogether. I think they are worth the price because in this way I do not need to buy a new computer. 
### SSD
The SSD interface in mac is mSATA. However, an SSD card with mSATA is very expensive, so I choose an M.2 interface SSD with a [M.2-to-mSATA converter](https://item.m.jd.com/product/100017938802.html), which is much cheaper. 
Since my computer model supports PCIe 3.0 x4 whose theoretical maximal bandwidth is 4GB/s, it better to buy a high-speed SSD for me. My choice is Samsung 970 EVO plus, 512G, which could achieve a sequential read/write speed of 3500MB/s and 3300MB/s. (This is not an advertisement.) Note that Samsung 980 could not be used as the system disk in Mac OS.(I wasted an afternoon on that.)
### Battery
I buy a new set of batteries from [Jingdong](https://item.jd.com/4494203.html) which contains a full set of tools, like screwdrivers and anti-static gloves.

## Procedures
The following are my procedures:
+ backup the system using TimeMachine
+ take out the screws on the behind of the computer, and take down the back lid(There is so much dirt!)
+ unplug the Battery interface to prevent accidents
+ change SSD
	-   unscrew the screws of the SSD, like this
		<img src="/images/posts/SSD&Battery/mSATA_SSD.jpg" style="zoom:20%" />
	-   take down the above old SSD
	-   put our M.2-to-mSATA converter to the mSATA interface
	-   put the new M.2 SSD on the M.2-to-mSATA converter 
	-   Screw the screws

+ change Battery (There is a detailed specification in the battery case)
	- release the wires of the touchpad
	- use wane and ethanol to pry open the old battery(it may take you a while)
	-  put the new battery on and plug the Battery interfaces
	 - put back the wires of the touchpad

+ Re-tighten the screws
+ Start the system by “cmd+option+r+power”, and it will recover from the Internet
+ Erase the Disk APFS mode with GUID. Then recover the system from TimeMachine.

## Ends
My old battery looks like this:
<img src="/images/posts/SSD&Battery/Old_battery.jpg" style="zoom:20%" />
If I had known that previously, I would not take it out every day. It seems to explode at any time.
Now, I have a good better life. Most importantly, I don’t need to care about the disk space too much.
<img src="/images/posts/SSD&Battery/newstorage.png" style="zoom:50%" />


## Reference

+ [1][SSD](https://post.smzdm.com/p/a783vk9g/)
+ [2][Battery](https://post.smzdm.com/p/a78zn859/)