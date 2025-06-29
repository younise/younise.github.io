---
id: 40
title: 'NetApp Simulator Cluster Mode Getting Started Part 1'
date: '2014-08-28T15:34:32-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=40'
permalink: /netapp-simulator-cluster-mode-getting-started-part-1/
categories:
    - Storage
tags:
    - CMODE
    - NetApp
    - vSphere
---

Simulators can be very useful tools to learn most of a products features. They can also serve as test platforms for updates, scripts, migrations, etc without the fear of affecting your production environment and causing a resume generating event. NetApp simulates its Data ONTAP software allowing for support for a majority of its functionality within a virtual environment. Supported VMware environments consists of Workstation, Player, Fusion and vSphere. This install process covers deploying the Data ONTAP simulator in cluster mode on a vSphere 5.5 lab environment.

<span style="text-decoration: underline;">**Prerequisites**</span>

1. NetApp Now account with appropriate access, consult with your NetApp rep for further information.
2. Download the Simulator that meets the needs of your environment as there are separate downloads if using vSphere or Workstation, etc.
3. Cluster mode requires 3 IPs per node for setup.
4. Storage for simulator.

<span style="text-decoration: underline;">**Create a NetApp Cluster Network vSphere Web Client**</span>

\*Cluster Mode requires a dedicated network for the cluster traffic\*

<span style="text-decoration: underline;">**vSwitch**</span> **(Per host configuration)**

1. Select an ESXi host.
2. Go to the **Manage** tab and select **Networking**.
3. In the Virtual Switches view, click **Add host networking** (globe with plus sign).
4. In the Add host networking wizard select **Virtual Machine Port Group** for a Standard Switch as the connection type, click **Next**.
5. Determine whether to use a separate vSwitch or to use an existing one as the target device. Keep in mind that the port group requires no connectivity to any uplinks. In this example I am using a new standard switch, click **Next**. <span style="color: #ff0000;"> **Note :**</span> if existing standard switch is selected the wizard goes directly to Connection Settings (step #8).
6. This network is for inter VM communication no uplinks are required, click **Next**.
7. A warning that there no active physical network adapters for the vSwitch are connected, click **OK**.
8. Provide a **Network Label** “NetApp Cluster Network” and VLAN ID if required, click **Next**.
9. Verify the vSwitch information is correct and click **Finish**.
10. A new vSwitch with no uplinks is created.

<span style="text-decoration: underline;">**dvSwitch**</span> **(<span style="line-height: 1.5em;">One time configuration)</span>**

1. From the Home tab select **Networking** under the inventories.
2. Verify if there is a Distributed Switch. If one exists then create a new port and go to step #8
3. Right click and select New Distributed Switch.
4. New Distributed Switch wizard is launched, Provide a **Name** and Click **Next**.
5. Select the **dvSwitch version** based on your environment.
6. Provide the correct settings for your environment (**\# of uplinks**, **Default port group**, etc), click **Next**.
7. Verify the Distributed Switch information is correct and click **Finish**.
8. Right click the port group for your NetApp Cluster Network and **edit settings**.
9. Select Teaming and failover.
10. Uplink failover order should have all uplinks set to **Unused**.
11. A warning that there no active physical network adapters for the port group on the dvSwitch are connected, click **OK**.

<span style="text-decoration: underline;">**Registering Data ONTAP Simulator 1st Node on vSphere**</span>

1. From the Home tab select **Storage** under the inventories.
2. Select an existing or create a new datastore that will host the files for the simulator.
3. Upload the simulator files to the selected datastore.
4. Register the simulator VM by locating the .vmx file called “DataONTAP.vmx”.
5. Right click .vmx file and Register VM.
6. Provide a name and folder location, click Next.
7. Select a cluster or host.
8. Verify registered VM information, click Finish.
9. Right click on the VM and assign the four NICs to the correct networks (2 cluster / 2 data). By default a inaccessible “Cluster Network” is created and will go away once the Cluster NICs have a network assigned.
10. When trying to power on your simulator an error “File/vmfs/volumes/”Datastore”/NetappSimulator/DataONTAP-sim.vmdk was not found.” The reason for this is the “sim.vmdk” is a sparse disk (split in 2GB files). Here are some options to help fix :

- On the ESXi console run the following command **vmkload\_mod multiextent** (this will need to be ran on any host that will have a simulator). <span style="color: #ff0000;">**Warning / C**</span><span style="color: #ff0000;">**aution** </span><span style="color: #000000;">if host is rebooted command will need to be re-run.</span>
- Use VMware Converter.
- ESXi command line use vmkfstools to convert the VM disk: **vmkfstools -i “Current VM disk name” “New VM disk name” -d zeroedthick or thin.**
- Delete the current disk and add a new eager zero thick disk or thin disk.



11\. Power on VM.

At this point the first node is hopefully powered on and booting. [Next steps](http://emadyounis.com/storage/netapp-simulator-cluster-mode-getting-started-part-2/ "NetApp Simulator Cluster Mode Getting Started Part 2") will be getting the node configured and adding another node to the cluster.