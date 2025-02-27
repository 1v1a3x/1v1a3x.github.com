---
layout: post
title: "Netezza Emulator : In-Memory Run"
date: 2016-09-01 00:00:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: ramdisk.png # Add image post (optional)
tags: [DevOps, Netezza, DataWarehouse] # add tag
---

I want two share some experience that I’ve got some time ago on the Netezza related testing.
This article is not just around Netezza emulator, but the approach described below allowed me to save a lot of time on testing within I/O-greedy configurations (the laptop was just equipped by slow IDE HDD but with 16 GB of RAM).

The problem I’ve faced was very slow performance of test SQL statements execution and I assumed that using of RAM Disk approach hypothetically could help me.

I want two share some experience that I’ve got some time ago on the Netezza related testing.
This article is not just around Netezza emulator, but the approach described below allowed me to save a lot of time on testing within I/O-greedy configurations (the laptop was just equipped by slow IDE HDD but with 16 GB of RAM).

The problem I’ve faced was very slow performance of test SQL statements execution and I assumed that using of RAM Disk approach hypothetically could help me.

##  Starting from that point I planned to accomplish the following steps:
<!-- ![RamDisk]({{site.baseurl}}/assets/img/ramdisk.png) -->

- Find and Setup the RAM Disk for Windows software (I’m sure there are kind of alternatives are available for other platforms)
- Setup it
- Place the Netezza VM into the virtual disk and spin it in RAM
- Get the performance profit :)

## The progress and the pitfalls:

- I’ve chosen the [SoftPerfect RAMDisk](https://www.softperfect.com/products/ramdisk/) as a software and it was really perfect. It’s useful and intuitive from installation and using perspectives.
- I’ve placed the VM image into the newly created RAM folder (8GB).
  - !Pitfall is there! When I didn't get expected performance boost, I've just installed the VMPlayer into the RAM folder as well as the VM image I've copied into it.
- Started VMPlayer newly installed into RAMDisk
- Started Netezza VM…

## Viola! My Netezza VM start working 10 times faster
_(on my own hardware of course :)_