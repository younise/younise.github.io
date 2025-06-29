---
id: 81
title: 'NetApp Simulator Cluster Mode Getting Started Part 3'
date: '2014-09-05T12:00:38-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=81'
permalink: /netapp-simulator-cluster-mode-getting-started-part-3/
categories:
    - Storage
tags:
    - CMODE
    - NetApp
    - vSphere
---

<span style="text-decoration: underline;">**Preparing the ****DataONTAP**** 2nd Simulator Node** </span>

1. Upload the 2nd node simulator files to an existing or create a new datastore.
2. Register the simulator VM (right click .vmx file and register VM).
3. Right click and select edit setting.
4. Under the Virtual Hardware tab, add a new Serial Port device and click add.[![NetApp-P3-1](https://emadyounis.com/assets/img/2014/09/NetApp-P3-1.jpg?resize=599%2C625)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-1.jpg)
5. Change the Serial Port output to “Use named pipe.”
6. Enter the pipe name using the following syntax: <span style="color: #ff0000;">**\\\\. \\pipe\\”name of simulator folder on datastore”-cons** <span style="color: #000000;">and click OK.  
    [![NetApp-P3-2](https://emadyounis.com/assets/img/2014/09/NetApp-P3-2.jpg?resize=602%2C255)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-2.jpg)</span></span>
7. Add a second Serial Port and change the output to “Use named pipe.”
8. Use the same syntax as above for the pipe name, except change the -cons to<span style="color: #ff0000;"> **-gdb** <span style="color: #000000;">and click OK.  
    [![NetApp-P3-3](https://emadyounis.com/assets/img/2014/09/NetApp-P3-3.jpg?resize=600%2C277)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-3.jpg)</span></span>

<span style="text-decoration: underline;">**Booting the DataONTAP Simulator 2nd Node**</span>

**<span style="color: #ff0000;">Note: The system ID is hardcoded on the DataONTOP simulator image. Each node requires a unique system ID to be permitted into the cluster.</span>**

1. Open the simulator vSphere client console, press the space bar when the “Hit \[Enter\] to boot immediately, or any other key for command prompt.” is displayed.  
    [![NetApp-P3-4](https://emadyounis.com/assets/img/2014/09/NetApp-P3-4.jpg?resize=720%2C164)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-4.jpg)
2. At the VLOADER&gt;prompt change the Serial Number and System ID for this node.[![NetApp-P3-5](https://emadyounis.com/assets/img/2014/09/NetApp-P3-5.jpg?resize=456%2C33)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-5.jpg)
3. Verify the information was saved correctly by using the following commands:  
    **VLOADER&gt; printenv SYS\_SERIAL\_NUM VLOADER&gt; printenv bootarg.nvram.sysid**  
    [![NetApp-P3-6](https://emadyounis.com/assets/img/2014/09/NetApp-P3-6.jpg?resize=421%2C82)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-6.jpg)
4. Enter the boot command which begins the process that changes the serial number and system id.  
    [![NetApp-P3-7](https://emadyounis.com/assets/img/2014/09/NetApp-P3-7.jpg?resize=384%2C19)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-7.jpg)
5. Follow the same steps in the [previous post](http://emadyounis.com/storage/netapp-simulator-cluster-mode-getting-started-part-2/ "NetApp Simulator Cluster Mode Getting Started Part 2") for **Booting the DataONTAP Simulator 1st Node.**

<span style="text-decoration: underline;">**Joining the Cluster**</span>

1. At the “Welcome to the cluster setup wizard” screen you will be prompted to create or join a cluster. Since this is the second node in the cluster type **join** and press enter.  
    [![NetApp-P3-8](https://emadyounis.com/assets/img/2014/09/NetApp-P3-8.jpg?resize=719%2C269)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-8.jpg)
2. When creating the cluster interfaces (e0a &amp; e0b) you have the option of keeping the system defaults or changing the network defaults during the cluster configuration.[![NetApp-P3-9](https://emadyounis.com/assets/img/2014/09/NetApp-P3-9.jpg?resize=720%2C171)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-9.jpg)
3. Enter the name of the cluster you would like to join, in this example it is “cluster1” which was recognized by default.  
    [![NetApp-P3-10](https://emadyounis.com/assets/img/2014/09/NetApp-P3-10.jpg?resize=720%2C127)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-10.jpg)
4. The 2nd node starts the joining cluster process.  
    [![NetApp-P3-11](https://emadyounis.com/assets/img/2014/09/NetApp-P3-11.jpg?resize=477%2C90)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-11.jpg)
5. Enter the required network information for the cluster management and node interfaces.
6. The cluster setup is complete.  
    [![NetApp-P3-12](https://emadyounis.com/assets/img/2014/09/NetApp-P3-12.jpg?resize=465%2C71)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-12.jpg)
7. To validate your two node cluster setup, log in and run “cluster show.” [![NetApp-P3-13](https://emadyounis.com/assets/img/2014/09/NetApp-P3-13.jpg?resize=575%2C172)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-13.jpg)

**<span style="text-decoration: underline;">Assigning Storage</span>**

1. To add storage to a node use the following command: **Storage disk assign -all true -node “name.”**  
    [![NetApp-P3-14](https://emadyounis.com/assets/img/2014/09/NetApp-P3-14.jpg?resize=653%2C95)](https://emadyounis.com/assets/img/2014/09/NetApp-P3-14.jpg)
2. To display disk information use the following command: **Storage disk show**.
