---
id: 49
title: 'NetApp Simulator Cluster Mode Getting Started Part 2'
date: '2014-09-02T09:04:15-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=49'
permalink: /netapp-simulator-cluster-mode-getting-started-part-2/
categories:
    - Storage
tags:
    - CMODE
    - NetApp
    - vSphere
---

The<span style="color: #0000ff;"> [<span style="color: #0000ff;">first post</span>](http://emadyounis.com/storage/netapp-simulator-cluster-mode-getting-started-part-1/ "NetApp Simulator Cluster Mode Getting Started Part 1")</span> in this series discussed how to deploy the NetApp Simulator in your vSphere environment. Now we will move forward with the configuration of the first node &amp; cluster.

<span style="text-decoration: underline;">**Booting the DataONTAP Simulator 1st Node**</span>

1. Open the simulator vSphere client console, press Ctrl-C for the Boot Menu when displayed on the screen.  
    [![NetApp-P2-1](https://emadyounis.com/assets/img/2014/08/NetApp-P2-1.jpg?resize=531%2C182)](https://emadyounis.com/assets/img/2014/08/NetApp-P2-1.jpg)
2. Once the boot menu is loaded, select Opt 4 ” Clean configuration and initialize all disks.”  
    [![NetApp-P2-2](https://emadyounis.com/assets/img/2014/08/NetApp-P2-2.jpg?resize=530%2C188)](https://emadyounis.com/assets/img/2014/08/NetApp-P2-2.jpg)
3. At the zero disk, reset config and install a new file system prompt, type “yes” or “y” and press Enter. Confirm the selection and press enter.[![NetApp-P2-3](https://emadyounis.com/assets/img/2014/08/NetApp-P2-3.jpg?resize=720%2C166)](https://emadyounis.com/assets/img/2014/08/NetApp-P2-3.jpg)
4. The simulator VM restarts and will continue the initialization process.  
    [![NetApp-P2-4](https://emadyounis.com/assets/img/2014/08/NetApp-P2-4.jpg?resize=720%2C99)](https://emadyounis.com/assets/img/2014/08/NetApp-P2-4.jpg)




<span style="color: #ff0000;">**Do not interrupt this process as it might result in corruption of the simulator disks.**</span>

<span style="text-decoration: underline;">**Creating the Cluster**</span>

1. At the “Welcome to the cluster setup wizard” screen you will be prompted to create or join a cluster. Since this is the first node in the cluster type create and press enter.  
    [![NetApp-P2-5](https://emadyounis.com/assets/img/2014/08/NetApp-P2-5.jpg?resize=721%2C245)](https://emadyounis.com/assets/img/2014/08/NetApp-P2-5.jpg)
2. When prompted “Do you intend for this node to be used as a single node cluster”, continue with the default no and press enter.  
    [![NetApp-P2-7](https://emadyounis.com/assets/img/2014/08/NetApp-P2-7.jpg?resize=719%2C58)](https://emadyounis.com/assets/img/2014/08/NetApp-P2-7.jpg)
3. Allow the cluster network to use network switches and press enter.[![NetApp-P2-8](https://emadyounis.com/assets/img/2014/08/NetApp-P2-8.jpg?resize=710%2C53)](https://emadyounis.com/assets/img/2014/08/NetApp-P2-8.jpg)
4. When creating the cluster interfaces (e0a &amp; e0b) you have the option of keeping the system defaults or changing the network defaults during the cluster configuration.  
    [![NetApp-P2-10](https://emadyounis.com/assets/img/2014/08/NetApp-P2-10.jpg?resize=720%2C116)](https://emadyounis.com/assets/img/2014/08/NetApp-P2-10.jpg)
5. Once the cluster interfaces are created, enter the name of your cluster &amp; license key(s).[![NetApp-P2-11](https://emadyounis.com/assets/img/2014/09/NetApp-P2-11.jpg?resize=604%2C33)](https://emadyounis.com/assets/img/2014/09/NetApp-P2-11.jpg)
6. Create a cluster admin password.  
    [![NetApp-P2-12](https://emadyounis.com/assets/img/2014/09/NetApp-P2-12.jpg?resize=613%2C60)](https://emadyounis.com/assets/img/2014/09/NetApp-P2-12.jpg)
7. Enter the required network information for the cluster management interface.[![NetApp-P2-13](https://emadyounis.com/assets/img/2014/09/NetApp-P2-13.jpg?resize=720%2C131)](https://emadyounis.com/assets/img/2014/09/NetApp-P2-13.jpg)
8. Enter the required network information for the node management interface.[![NetApp-P2-14](https://emadyounis.com/assets/img/2014/09/NetApp-P2-14.jpg?resize=721%2C111)](https://emadyounis.com/assets/img/2014/09/NetApp-P2-14.jpg)
9. Cluster creation is now complete.  
    [![NetApp-P2-15](https://emadyounis.com/assets/img/2014/09/NetApp-P2-15.jpg?resize=720%2C140)](https://emadyounis.com/assets/img/2014/09/NetApp-P2-15.jpg)
10. Management for the NetApp cluster or node(s) is now accessible via CLI or GUI (browser based) by downloading the OnCommand System Manager.  
    [![NetApp-P2-16](https://emadyounis.com/assets/img/2014/09/NetApp-P2-16.jpg?resize=1044%2C199)](https://emadyounis.com/assets/img/2014/09/NetApp-P2-16.jpg)
11. To validate your cluster setup via CLI, log in and run “cluster show.” [![NetApp-P2-17](https://emadyounis.com/assets/img/2014/09/NetApp-P2-17.jpg?resize=639%2C148)](https://emadyounis.com/assets/img/2014/09/NetApp-P2-17.jpg)



Next steps include the configuration and addition of a secondary node to the cluster as well as provisioning of storage.
