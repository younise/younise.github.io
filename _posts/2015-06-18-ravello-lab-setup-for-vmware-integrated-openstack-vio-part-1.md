---
id: 509
title: 'Ravello Lab Setup for VMware Integrated OpenStack (VIO) Part 1'
date: '2015-06-18T07:00:59-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=509'
permalink: /ravello-lab-setup-for-vmware-integrated-openstack-vio-part-1/
image: 'https://emadyounis.com/assets/img/2015/06/Ravello-with-VIO.png'
categories:
    - OpenStack
tags:
    - OpenStack
    - Ravello
    - VIO
    - VMware
    - vSphere
---

![](https://emadyounis.com/assets/img/2015/06/Ravello-with-VIO.png?resize=300%2C88)The past few months I have been getting more familiar with <span style="color: #0000ff;">**[OpenStack](https://www.openstack.org/)**</span>. By now it should be obvious that OpenStack will not be going away anytime soon, so it’s time to embrace it. Yes, I’m talking to you VMware admins!

**<span style="color: #0000ff;">[DevStack](http://docs.openstack.org/developer/devstack/)</span>** is a quick way to get started, having all the components required to deploy OpenStack in a single VM. This type of deployment is great for labs that have minimal resources, or for development environments. For a production environment, DevStack is ***not*** the deployment you’re looking for. But don’t worry, from the same folks who bought you vSphere now comes **<span style="color: #0000ff;">[VMware Integrated OpenStack](https://www.vmware.com/products/openstack)</span>** (VIO). VIO provides an easy familiar deployment type for VMware admins using an OVA. The deployment is resource intensive, but you will get a highly available OpenStack environment. This environment will be able to access VMware features, such as DRS, HA, and vMotion. Let’s jump in and see how VIO is deployed and configured.

**<u>Prerequisites</u>**

![](https://emadyounis.com/assets/img/2015/06/Prereq.png?resize=198%2C181)

- vSphere 5.5 update 2 or vSphere 6.0 Enterprise Plus.
- ESXi host(s) with eight or more CPUs.
- Management cluster of at least three hosts with 56 CPUs &amp; 192GB of RAM.
- 665GB of storage for NSX deployment or 585GB for vDS deployment (**separate license required for NSX**).
- Compute cluster with at least one host and 20GB of storage.
- vCenter &amp; ESXi hosts must use the same NTP server.

Deployment of VIO takes place in two stages. First, the management cluster. The 15 virtual machines that make up the VIO components / services live here. Second, is the compute cluster. Deployment and management of tenant workloads take place here. As I go through the VIO deployment process I’ll go into more detail with each step. I’ll also try to explain more of the OpenStack terminology as it relates to the VMware administrator.

<span style="text-decoration: underline;">**Ravello Lab Environment**</span>

The great folks at **<span style="color: #0000ff;">[Ravello](http://www.ravellosystems.com/)</span>** were kind enough to give me access to try out their product. They’re the perfect solution for labs, dev, and test environments. They already have a large library of VMs to choose from, all that’s required are the ISOs. All you need to do is drag and drop the VM type from the library to the canvas. Configuring a VM is done by simply selecting a VM and going through the configuration tabs on the right hand side (General, System, Disk, Network, and Services). Once your changes are saved, update the configuration and you’re ready to start your VM.

[![](https://emadyounis.com/assets/img/2015/06/Ravello-Library.jpg?resize=1024%2C295)](https://emadyounis.com/assets/img/2015/06/Ravello-Library.jpg)

[![](https://emadyounis.com/assets/img/2015/06/Ravello-Update.jpg?resize=1024%2C638)](https://emadyounis.com/assets/img/2015/06/Ravello-Update.jpg)

If the VM you’re looking for is not in the library, you have the ability to create it or add your own. The library has an Empty VM that can be used to create a new VM type, and you’ll have the ability to save it later. You can also upload your existing VMs, ISOs, QCOW, and OVFs using their upload tool. I uploaded an ISO for ESXi and Windows Server for management. My storage VM and VIO OVA will be uploaded to my MGMT-VM for deployment to vCenter.

[![](https://emadyounis.com/assets/img/2015/06/Ravello-Import-Tool.jpg?resize=1024%2C436)](https://emadyounis.com/assets/img/2015/06/Ravello-Import-Tool.jpg)

My Ravello configuration consists of the following:

- vSphere 6.0
- 3 hosts for a management cluster
- 2 hosts for a compute cluster
- 1 MGMT-VM running vCenter

[![](https://emadyounis.com/assets/img/2015/06/Lab.jpg?resize=654%2C510)](https://emadyounis.com/assets/img/2015/06/Lab.jpg)

<span style="text-decoration: underline;">**vCenter Prep**</span>

Before deploying VIO, make sure your vCenter server configuration meets the deployment standards for VIO.

**\[While a dedicated vCenter server is recommended, VIO can be deployed leveraging your existing vCenter server\].**

- Create a datacenter for your VIO deployment.  
    <span style="color: #ff0000;">**Note:**</span> Don’t use any special characters in your datacenter name such as commas.  
    [![](https://emadyounis.com/assets/img/2015/06/VIO-Error.jpg?resize=344%2C129)](https://emadyounis.com/assets/img/2015/06/VIO-Error.jpg)
- Create a dedicated management cluster with at least three hosts.  
    <span style="color: #ff0000;">**Note:**</span> Although not supported, you can get away with two hosts for POC / Lab environments.
- Attach datastore(s) to the management cluster for deployment of VIO VMs, also for use with <span style="color: #0000ff;">**[glance](http://docs.openstack.org/developer/glance/)**</span> (image service).
- Create a dedicated compute cluster with at least one host.
- Attach datastore(s) to the compute cluster for deployment of <span style="color: #0000ff;">**instances**</span> (virtual machines).
- Network options include using VDS or NSX (comparison can be found **<span style="color: #0000ff;">[here](http://emadyounis.com/openstack/vmware-integrated-openstack-vio-primer/)</span>**):

<span style="color: #ff0000;">**Note:**</span> Network type of vDS or NSX must be decided prior to VIO deployment as you will see once one is selected there is no way to change without a redeploy.

**<span id="GUID-4B404DEC-BBE3-49FB-BEC4-022A7DAC5E02__productname_0C90CE0C8421442D8AECA1D345317952">VDS </span>**

- Create a VDS.
- Add hosts in the Management and Compute cluster.
- Create a Management and API Access port group, and tag each with a VLAN ID for their respective network.

[![](https://emadyounis.com/assets/img/2015/06/VDS.jpg?resize=537%2C361)](https://emadyounis.com/assets/img/2015/06/VDS.jpg)

**NSX**

- If you’re using NSX, create a dedicated cluster for Edge nodes (provide DHCP, routing, and floating IP addresses).
- Create a separate VDS for Management, Compute, and Edge.
- Create a management port group on each VDS (Management, Edge, and Compute), and tag them with the VLAN ID assigned to the Management network.
- Create the API Access port group on the Management VDS, and tag it with the VLAN ID assigned to the API Access network.
- Create an External Network port group on the Edge VDS, and tag it it with the VLAN ID assigned to the External network.

[![](https://emadyounis.com/assets/img/2015/06/NSX.jpg?resize=537%2C450)](https://emadyounis.com/assets/img/2015/06/NSX.jpg)

<span style="text-decoration: underline;">**Stage 1: Management Cluster Configuration**</span>

This is where your VIO deployment takes place, more on that in a later post.

[![](https://emadyounis.com/assets/img/2015/06/Mgmt-Cluster.jpg?resize=529%2C246)](https://emadyounis.com/assets/img/2015/06/Mgmt-Cluster.jpg) [![](https://emadyounis.com/assets/img/2015/06/WebClient.jpg?resize=534%2C255)](https://emadyounis.com/assets/img/2015/06/WebClient.jpg)

1. Enable Hosts VT BIOS setting  
    [![](https://emadyounis.com/assets/img/2015/06/Bios.jpg?resize=694%2C244)](https://emadyounis.com/assets/img/2015/06/Bios.jpg)
2. Enable vSphere DRS: <span style="color: #800000;">**Manage –&gt; Settings –&gt; Services –&gt; vSphere DRS.**</span>  
    [![](https://emadyounis.com/assets/img/2015/06/VIO-DRS.jpg?resize=963%2C275)](https://emadyounis.com/assets/img/2015/06/VIO-DRS.jpg)  
    **Steps 3-7 are all done under vSphere HA settings.**
3. Enable Host Monitoring: **<span style="color: #800000;">Manage –&gt; Settings –&gt; Services –&gt; vSphere HA.</span>**  
    <span style="color: #ff0000;">**Note:**</span> Host Monitoring was turned on by default, but it’s always good to verify.  
    [![](https://emadyounis.com/assets/img/2015/06/Host-Monitoring.jpg?resize=962%2C160)](https://emadyounis.com/assets/img/2015/06/Host-Monitoring.jpg)
4. Enable Admission Control: **<span style="color: #800000;">Manage –&gt; Settings –&gt; Services –&gt; vSphere HA –&gt; Admission Control.</span>**  
    [![](https://emadyounis.com/assets/img/2015/06/Admission-Control.jpg?resize=1024%2C603)](https://emadyounis.com/assets/img/2015/06/Admission-Control.jpg)
5. Set VM restart priority to High: <span style="color: #800000;">**Manage –&gt; Settings –&gt; Services –&gt; vSphere HA –&gt; Failure conditions and VM response –&gt; VM restart priority.**</span>  
    [![](https://emadyounis.com/assets/img/2015/06/VM-restart-Priority.jpg?resize=1024%2C564)](https://emadyounis.com/assets/img/2015/06/VM-restart-Priority.jpg)
6. Set VM Monitoring to VM and Application Monitoring: <span style="color: #800000;">**Manage –&gt; Setting –&gt; Services –&gt; vSphere HA –&gt; Virtual Machine Monitoring.**</span>  
    [![](https://emadyounis.com/assets/img/2015/06/VM-Monitoring.jpg?resize=961%2C622)](https://emadyounis.com/assets/img/2015/06/VM-Monitoring.jpg)
7. Set Monitoring sensitivity to High: **<span style="color: #800000;">Manage –&gt; Settings –&gt; Services –&gt; vSphere HA –&gt; Failure conditions and VM response –&gt; VM monitoring and sensitivity.</span>**  
    [![](https://emadyounis.com/assets/img/2015/06/VM-sensitivity.jpg?resize=1024%2C212)](https://emadyounis.com/assets/img/2015/06/VM-sensitivity.jpg)
8. Enable vMotion and Fault Tolerance Logging: <span style="color: #800000;">**Add new VMkernel port for vMotion.**</span>  
    [![](https://emadyounis.com/assets/img/2015/06/vMotion.jpg?resize=957%2C561)](https://emadyounis.com/assets/img/2015/06/vMotion.jpg)
9. Enable vMotion and Fault Tolerance for Management Network VMkernel port: <span style="color: #800000;">**Host –&gt; Manage –&gt; Networking –&gt; VMkernel Adapters.**</span>  
    [![](https://emadyounis.com/assets/img/2015/06/FT-and-VMotion-on-VMK.jpg?resize=963%2C563)](https://emadyounis.com/assets/img/2015/06/FT-and-VMotion-on-VMK.jpg)

<span style="text-decoration: underline;">**Stage 2: Compute Cluster Configuration**</span>

Rinse and repeat for the compute cluster, settings should mirror the Management cluster above. Your tenants will take residency here!

[![](https://emadyounis.com/assets/img/2015/06/Compute-Cluster.jpg?resize=322%2C226)](https://emadyounis.com/assets/img/2015/06/Compute-Cluster.jpg)[![](https://emadyounis.com/assets/img/2015/06/Compute-Cluster-WebClient.jpg?resize=538%2C370)](https://emadyounis.com/assets/img/2015/06/Compute-Cluster-WebClient.jpg)

<span style="text-decoration: underline;">**Summary**</span>

Using Ravello for my VIO lab was easy to set up and saved me time from having to free up resources in my home lab. I also enjoyed having external access to my environment via RDP for my management VM or SSH to my ESXi hosts. Now I have an environment that meets the VIO prerequisites and can proceed with the deployment. I will cover the deployment in the next post as well as some of the errors that I ran across in hopes of saving you time. Stay tuned!
