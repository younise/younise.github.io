---
id: 305
title: 'SimpliVity&#8217;s Approach to Deduplication'
date: '2015-02-12T10:03:56-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=305'
permalink: /simplivitys-approach-to-deduplication/
image: 'https://emadyounis.com/assets/img/2015/02/Simplivity-Bigger.jpg'
categories:
    - 'Tech Field Day'
tags:
    - SimpliVity
    - 'Tech Field Day'
    - TFD
    - VFD
    - VFD4
---

There are two industry standards when it comes to duplication. The first type is Inline which occurs as data is being written. Second is Post-Process where data is analyzed after being written. So what’s the difference to using one over the other?

<span style="color: #ff0000;">**Note**</span>: While I recognize there are 5 billion things to keep in mind when it comes to Inline and Post-Process deduplication these are the ones that stood out to me.

With Inline new blocks are only created if the data does not exist. If the data is there, new meta data will reference the existing block. Few things to keep in mind with Inline are:

![Inline](https://emadyounis.com/assets/img/2015/02/Inline1.jpg?resize=976%2C162)

1. A hash calculation is created in memory and stored on a storage device as data is written. This calculation has the potential of slowing down the process as data is being validated and indexed (latency).
2. Since blocks of data are created as needed, less storage is required.
3. There is CPU tax, unless you offload the process to dedicated hardware.
4. Less IOPS are being sent to disk.

Post-Process removes duplicate data after it has been written to storage. There is a potential of having duplicate data until after the process validates. Few things to keep in mind with Post-Process are:

![Post-Process](https://emadyounis.com/assets/img/2015/02/Post-Process1.jpg?resize=993%2C146)

1. No hash calculation needed up front on storage device, allowing the initial write process to take less time (in theory).
2. Requires more storage since all data is written up front.
3. There is a CPU and Disk tax during the Post-Process operation more latency is introduced.
4. Less IOPS for applications (rereading blocks that have already been written).

During Virtualization Field Day 4 we got to see how Simplivity handles deduplication, here are my notes.

**<span style="color: #ff0000;">Disclaimer</span>**: **I was a delegate at Virtualization Field Day 4. Gestalt IT paid my travel, lodging, and meals. I don’t receive any compensation nor am I required to write anything related to the event.**

[SimpliVity](https://www.simplivity.com/) has a solution to solve the issues that plague using Inline and Post-Process deduplication. The secret sauce comes from using the virtual controller (brains) coupled with software and the OmniStack Accelerator card (muscle). By the way this is all done on their own proprietary operating system (SVT). Few things to keep in mind with SimpliVity:

![Simplivity](https://emadyounis.com/assets/img/2015/02/Simplivity1.jpg?resize=1021%2C233)

1. Less IOPS going to hard drives (typical w/ Inline).
2. Lower CPU &amp; memory utilization.
3. <span class="s1">Lower latency on hard drive(s) doing less which allows more optimize IO.</span>
4. <span class="s1">Less IOPS are being sent to disk more are available for applications.</span>

As you can see from this simplistic (no pun intended) overview, SimpliVity provides deduplication in ways others don’t. The benefits include having the hash calculation take place on the OminStack Accelerator card instead of in memory. Meta data is separated out from data and held in pool of SSD, faster for reads. This eliminates the CPU and Disk tax as well as guaranteed IOPS for applications. This is just what they are doing with deduplication, compression gains the same benefits from the OminStack Accelerator card as well.
