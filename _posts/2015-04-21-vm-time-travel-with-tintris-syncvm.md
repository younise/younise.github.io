---
id: 482
title: 'VM Time Travel with Tintri&#8217;s SyncVM'
date: '2015-04-21T09:05:25-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=482'
permalink: /vm-time-travel-with-tintris-syncvm/
image: 'https://emadyounis.com/assets/img/2015/04/sync_vm.jpg'
categories:
    - Storage
tags:
    - SyncVM
    - Tintri
---

![](https://emadyounis.com/assets/img/2015/04/sync_vm.jpg?resize=250%2C80)

Time travel has always been a fascinating concept in our society. Think of the possibilities with the ability to move forward or backwards in time. There even is the idea of an alternate universe. The concept of time travel is so huge that we have several books, TV shows, and movies based on it. If only we had a time machine, like in Back to the Future! The latest release of Tintri’s OS 3.2 includes several new features. SyncVM is one them. The new SyncVM feature lets you experience time travel via your VMs. SyncVM leverages snapshot capabilities to move back and forth in time. This can be at the VM level. SyncVM also lets you propagate changes at a vDisk level from one VM to a set of VMs.

<span style="color: #ff0000;">**Disclaimer:**</span> I am a Tintri employee, this post is based on my experience and not a requirement by my employer.

[![](https://emadyounis.com/assets/img/2015/04/master_child_new.jpg?resize=750%2C401)](https://emadyounis.com/assets/img/2015/04/master_child_new.jpg)

The beauty of SyncVM is that physical data is not moved. Instead it leverages snapshots, SyncVM allows movement between different points in time (aka time travel). This is not possible with traditional snapshots. SyncVM includes a preventive measure<span style="color: #000000;"> that creates a SyncVM safety snapshot. This is </span>a safety precaution: a snapshot taken prior to the data synchronization and is selectable from the snapshot dropdown list.

[![](https://emadyounis.com/assets/img/2015/04/timeline.jpg?resize=750%2C300)](https://emadyounis.com/assets/img/2015/04/timeline.jpg)

The SyncVM feature doesn’t cause any loss of snapshots or performance data within vCenter. Also no storage or VM reconfiguration takes place. Don’t take my word for it, let’s explore a few use cases where SyncVM will be beneficial.

<span style="text-decoration: underline;">**Development and Test environments**</span>

Data management in a dev or test environment can be a challenge to maintain across a number of VMs. These VMs can span separate environments such as development, QA, and production. The diagram below illustrates two types of environments. The one on the left utilizes scripts to maintain the data between each environment, while the environment on the right is leveraging SyncVM capabilities.

[![](https://emadyounis.com/assets/img/2015/04/devtest.jpg?resize=750%2C401)](https://emadyounis.com/assets/img/2015/04/devtest.jpg)

Scripts require maintenance and time. They may also not be scalable to move between other environments, not to mention the clutter that comes from having to keep several scripts available or up to date. SyncVM eliminates the need for all these dependencies and guess work. It provides a dynamic and scalable way of refreshing your data between environments. This can also be useful for other environments such as labs, kiosks, demos, and file servers.

**<span style="text-decoration: underline;">Databases</span>**

Development and troubleshooting of databases really benefit from SyncVM. In the case of applications that require a database, use SyncVM to have either an offline copy or an isolated copy on a different network (VLAN). Another benefit of using SyncVM with databases is updates or patches. Keep in mind since this is all at the VM level, we can use SyncVM on the vDisk level as well. The vDisk recovery feature provides the ability to sync database files and logs to other database servers.

<span style="text-decoration: underline;">**vDisk Recovery**</span>

vDisk recovery can sync data at the disk level. You can have up to 20 destination VMs (at a time) syncing from 1 source VM. The vDisk containing the operating system can have dependencies, so be aware of them prior to attempting a refresh. What if you only want to recover a single or a handful of files, is it possible? The answer is Yes! This requires mounting a separate vDisk to your destination VM, recovering all files to that particular vDisk, and copying the ones you actually need to another vDisk. Think of this as a quick alternative file recovery method.

<span style="text-decoration: underline;">**Getting started with SyncVM – Restore VM**</span>

SyncVM has two capabilities: Restore VM and Refresh virtual disks. The Restore VM allows the refresh of data from any point in time on any individual VM. This point in time can be from an hour ago, days, or weeks. The flexibility SyncVM provides is the ability to move between any point in time, forwards or backwards.

1. Select a VM, right click and choose Restore VM. You can leverage an existing VM snapshot or create a new one.  
    [![](https://emadyounis.com/assets/img/2015/04/SyncVM.png?resize=1024%2C412)](https://emadyounis.com/assets/img/2015/04/SyncVM.png)  
    <span style="color: #ff0000;">**Note:**</span> At least one snapshot (manual or scheduled) must be available on the selected VM.  
    [![](https://emadyounis.com/assets/img/2015/04/Sync-VM-snapshot.png?resize=691%2C174)](https://emadyounis.com/assets/img/2015/04/Sync-VM-snapshot.png)
2. Select a snapshot to restore from.  
    [![](https://emadyounis.com/assets/img/2015/04/SyncVM-Snaps.jpg?resize=692%2C336)](https://emadyounis.com/assets/img/2015/04/SyncVM-Snaps.jpg)
3. Click Restore.  
    [![](https://emadyounis.com/assets/img/2015/04/SyncVM-Restore.jpg?resize=693%2C177)](https://emadyounis.com/assets/img/2015/04/SyncVM-Restore.jpg)

In a few clicks you have restored data to a VM with the ability to select any point in time.

<span style="text-decoration: underline;">**Getting started with SyncVM – Refresh Virtual Disks**</span>

The second functionality that SyncVM provides is Refresh Virtual Disk. We can now select up to 20 destination VMs at a time and refresh a particular vDisk from any source VM in your environment.

1. Select 1-20 VMs, right click and choose Refresh virtual disks.  
    [![](https://emadyounis.com/assets/img/2015/04/Refresh-vDisk-1.jpg?resize=1024%2C395)](https://emadyounis.com/assets/img/2015/04/Refresh-vDisk-1.jpg)
2. Select the VM, Snapshot, and vDisk to refresh from dropdown(s). Click Refresh.  
    [![](https://emadyounis.com/assets/img/2015/04/Refresh-vDisk-2.jpg?resize=692%2C329)](https://emadyounis.com/assets/img/2015/04/Refresh-vDisk-2.jpg)
3. Acknowledge completed synchronization.  
    [![](https://emadyounis.com/assets/img/2015/04/Refresh-vDisk-3.jpg?resize=1024%2C320)](https://emadyounis.com/assets/img/2015/04/Refresh-vDisk-3.jpg)

These are just a few use cases with SyncVM. Disaster Recovery has potential use case as well, we’ll leave that for another post. If you’re a fan of scripting, Tintri’s REST API or PowerShell toolkit is also available. Stay tuned for more use cases and automation goodness around SyncVM.
