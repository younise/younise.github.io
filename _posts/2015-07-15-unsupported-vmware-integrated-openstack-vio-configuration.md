---
id: 547
title: 'Unsupported VMware Integrated OpenStack (VIO) Configuration'
date: '2015-07-15T09:00:54-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=547'
permalink: /unsupported-vmware-integrated-openstack-vio-configuration/
image: 'https://emadyounis.com/assets/img/2015/07/VIO-Unsupported-Config1.jpg'
categories:
    - OpenStack
tags:
    - OpenStack
    - Ravello
    - VIO
    - VMware
    - vSphere
---

![](https://emadyounis.com/assets/img/2015/07/VIO-Unsupported-Config1.jpg?resize=264%2C300)

In a perfect datacenter environment, we would all have an additional area for Dev/Test. This is not always the case and we don’t always have the resources readily available to us. Most of us get creative using what old equipment we have. Some carve out a section in their infrastructure, while others leverage the cloud. Currently I’ve been using <span style="color: #0000ff;">**[Ravello](http://www.ravellosystems.com/)**</span> as my Dev/Test area of my lab for VIO. A VIO production-ready deployment requires a significant amount of resources. These same resources may not be instantly available in some environments. The current deployment architecture of VIO requires 15 VMs with a total of 56CPUs and 192GB RAM; details can be found **<span style="color: #0000ff;">[here](http://pubs.vmware.com/integrated-openstack-1/topic/com.vmware.ICbase/PDF/integrated-openstack-101-getting-started-guide.pdf)</span>**. This doesn’t take into account if you’re using NSX. However, there are a few ways you can be conservative with VIO resources. Let me state up front: the following setup is not supported by VMware for production.

![](https://emadyounis.com/assets/img/2015/07/CAUTION.jpeg?resize=264%2C191)

<span style="text-decoration: underline;">**Changing VIO Configuration Settings**</span>

Open the VIO management-server console or use SSH. In the example below I’ll be using SSH to make configuration changes.  
[![](https://emadyounis.com/assets/img/2015/07/VIO-MGMT.jpg?resize=1513%2C663)](https://emadyounis.com/assets/img/2015/07/VIO-MGMT.jpg)

Login using the viouser account.  
[![](https://emadyounis.com/assets/img/2015/07/VIO-User.jpg?resize=669%2C157)](https://emadyounis.com/assets/img/2015/07/VIO-User.jpg)

The file that needs to be modified is **omjs.properties,** located in **/opt/vmware/vio/etc**.  
[![](https://emadyounis.com/assets/img/2015/07/omjs.properties-location.jpg?resize=826%2C179)](https://emadyounis.com/assets/img/2015/07/omjs.properties-location.jpg)

<span style="color: #ff0000;">**Important:**</span> Create a backup of the **omjs.properties** file prior to making any changes.  
**“cp omjs.properties bak-omjs.properties”**  
[![](https://emadyounis.com/assets/img/2015/07/Backup-of-OMJS-File.jpg?resize=672%2C423)](https://emadyounis.com/assets/img/2015/07/Backup-of-OMJS-File.jpg)

Edit **omjs.properties,** in this example I’m using the VI editor.  
[![](https://emadyounis.com/assets/img/2015/07/VI-omjs.properties.jpg?resize=825%2C86)](https://emadyounis.com/assets/img/2015/07/VI-omjs.properties.jpg)

Here is the initial **omjs.properties**.  
[![](https://emadyounis.com/assets/img/2015/07/OMJS-VI.jpg?resize=708%2C1024)](https://emadyounis.com/assets/img/2015/07/OMJS-VI.jpg)

The changes made were to the CPU, Memory, and vSphere settings section.  
[![](https://emadyounis.com/assets/img/2015/07/Changes-OMJS-File.jpg?resize=825%2C544)](https://emadyounis.com/assets/img/2015/07/Changes-OMJS-File.jpg)  
<span style="color: #ff0000;">**Note:**</span> When doing the math only the first 8 entries count for CPU and Memory. The **oms.vmsize.cpu.smoke** and **oms.vmsize.memory.smoke** are currently not used. They could be place holders for future use.

Save the changes and restart the management-server.

##### <span style="color: #ff0000;">**Important:**</span> <span style="color: #000000;">**The omjs.properties settings will be reverted to its original state when applying a VIO update.**</span>

Using <span style="color: #000000;">Ravello</span> for my VIO lab (**<span style="color: #0000ff;">[part 1](http://emadyounis.com/openstack/ravello-lab-setup-for-vmware-integrated-openstack-vio-part-1/)</span>** and <span style="color: #0000ff;">**[part 2](http://emadyounis.com/openstack/ravello-lab-setup-for-vmware-integrated-openstack-vio-part-2/)**</span>), I’ve been able to manage with this setup. <span style="color: #ff0000;">**Keep in mind the above configuration is not supported by VMware**</span>. This should only be for Dev/Test deployments with limited resources. If resources are available, I would recommend staying with the default setup. Another alternative is shutting down some of the highly available VIO VMs, this solution is also not supported in production. To save on resources use this alternative method in conjunction with editing the omjs.properties. There you have it: two ways to save on resources.
