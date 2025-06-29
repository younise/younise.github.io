---
id: 542
title: 'Ravello Lab Setup for VMware Integrated OpenStack (VIO) Part 2'
date: '2015-06-26T06:33:58-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=542'
permalink: /ravello-lab-setup-for-vmware-integrated-openstack-vio-part-2/
image: 'https://emadyounis.com/assets/img/2015/06/Ravello-with-VIO-Part-2.png'
categories:
    - OpenStack
tags:
    - OpenStack
    - Ravello
    - VIO
    - VMware
    - vSphere
---

![](https://emadyounis.com/assets/img/2015/06/Ravello-with-VIO-Part-2.png?resize=300%2C113)Previously, we talked about the prerequisites (<span style="color: #0000ff;">**[link](http://emadyounis.com/openstack/ravello-lab-setup-for-vmware-integrated-openstack-vio-part-1/)**</span>) needed for a successful VIO deployment. Now that we’ve met them, let’s take things to the next level and deploy VIO. VIO is available at no cost to those who have purchased the following VMware licensing:

- vCloud Suite (all editions).
- vSphere with Operations Management Enterprise Plus (vSOM).
- vSphere Enterprise Plus.

“At no cost” means you’re entitled to download and use VIO, but support from VMware is not included. While support isn’t required, it’s recommend in case you need to troubleshoot with VMware. Support can be purchased on a per CPU basis, for more information take a look at the VIO <span style="color: #0000ff;">**[FAQ page](https://www.vmware.com/files/pdf/openstack/VMware-Integrated-OpenStack-Faqs.pdf)**</span>.

<span style="text-decoration: underline;">**Downloading VIO**</span>

If you don’t have one of the VMware licenses that give you access to VIO, the other option is to download a 60 day evaluation from this <span style="color: #0000ff;">**[link](https://my.vmware.com/web/vmware/info/slug/datacenter_cloud_infrastructure/vmware_integrated_openstack/1_0)**</span> (a My VMware account is required).

Under the Product Downloads tab, click Download.

[![](https://emadyounis.com/assets/img/2015/06/Download-VIO.jpg?resize=1024%2C343)](https://emadyounis.com/assets/img/2015/06/Download-VIO.jpg)

Go to the License &amp; Download tab, and click Manually Download to download the VIO OVA.

[![](https://emadyounis.com/assets/img/2015/06/VIO-OVA.jpg?resize=1024%2C553)](https://emadyounis.com/assets/img/2015/06/VIO-OVA.jpg)

<span style="text-decoration: underline;">**Ravello VIO Lab**</span>

![](https://emadyounis.com/assets/img/2015/06/Lab.png?resize=86%2C81)The Visio-like canvas in <span style="color: #0000ff;">**[Ravello](http://www.ravellosystems.com/)**</span> is a nice touch in my opinion. One feature that helps from a logical perspective, is called Group. As the feature states, Group allows the grouping of VMs. Once VMs are in a group, options can be set on the entire group. These include start, stop, restart, power off, and delete. You still have the option of controlling individual VMs within the group. To see the options, click on a particular VM. Options will vary depending on whether the VMs is powered on.

[![](https://emadyounis.com/assets/img/2015/06/Ravello-Group.jpg?resize=901%2C685)](https://emadyounis.com/assets/img/2015/06/Ravello-Group.jpg)

[![](https://emadyounis.com/assets/img/2015/06/Ravello-Setting-2.jpg?resize=1024%2C489)](https://emadyounis.com/assets/img/2015/06/Ravello-Setting-2.jpg)

A Group is not static once created, you can add or subtract VMs as needed. Other group settings include providing a name and selecting a pretty color. Also you’re not limited to just creating a single group. In my case I have a Management Cluster group and a Compute Cluster group.

[![](https://emadyounis.com/assets/img/2015/06/Ravello-Group-Setting.jpg?resize=589%2C219)](https://emadyounis.com/assets/img/2015/06/Ravello-Group-Setting.jpg)

Another feature that makes Ravello simple to use is Networking. Provide either a static IP or DHCP address, and see the magic happen from the network tab. This includes a visual mapping of the network for you. There is also the option of using an Elastic IP repository.

[![](https://emadyounis.com/assets/img/2015/06/Ravello-Network.jpg?resize=693%2C412)](https://emadyounis.com/assets/img/2015/06/Ravello-Network.jpg)

[![](https://emadyounis.com/assets/img/2015/06/Ravello-Elastic-1024x388.jpg?resize=1024%2C388)](https://emadyounis.com/assets/img/2015/06/Ravello-Elastic.jpg)

Other settings include: VM start order, Application Scheduling, and Blueprint.

[![](https://emadyounis.com/assets/img/2015/06/Ravello-Setting-21.jpg?resize=1024%2C506)](https://emadyounis.com/assets/img/2015/06/Ravello-Setting-21.jpg)

VM start order allows the stop and start order of VMs within an application. For example if you have a three tier application you can control the boot order and shutdown of the VMs within the application.

Application Scheduling controls the start and stop of an application. For example if you want to schedule an application to start on every week on Wednesday at 2pm and stop at 5pm.

Blueprint allows you to save your configuration as a template. This template can then be redeployed several times in parallel. You can share with individuals or groups. If you publish your application in one cloud (Amazon or Google) and would like to change to another, a blueprint can help do that.

<span style="color: #ff0000;">**Note:**</span> Some cloud providers will charge by the hour. For example if you run your VM(s) for 10 minutes and then shut it down, the bill will be for an entire hour, not the use time of 10 minutes. This is not a Ravello limitation, but based on how most cloud providers charge. So use your time wisely.

[![](https://emadyounis.com/assets/img/2015/06/BluePrint.jpg?resize=600%2C259)](https://emadyounis.com/assets/img/2015/06/BluePrint.jpg)

<span style="text-decoration: underline;">**Deploying VIO OVA ![](https://emadyounis.com/assets/img/2015/06/OVA.png?resize=90%2C90)**</span>

1. Create a folder under VM and templates view, mine is called VIO.  
    [![](https://emadyounis.com/assets/img/2015/06/VIO-Folder.jpg?resize=705%2C263)](https://emadyounis.com/assets/img/2015/06/VIO-Folder.jpg)
2. Right click on the Mgmt Cluster and select Deploy OVF Template.  
    [![](https://emadyounis.com/assets/img/2015/06/VIO-OVA-Deploy-New.jpg?resize=537%2C742)](https://emadyounis.com/assets/img/2015/06/VIO-OVA-Deploy-New.jpg)
3. Browse to the location of the VIO OVA and click Next.  
    [![](https://emadyounis.com/assets/img/2015/06/OVA-Location.jpg?resize=960%2C561)](https://emadyounis.com/assets/img/2015/06/OVA-Location.jpg)
4. Review the details of the VIO OVA and click Next.  
    [![](https://emadyounis.com/assets/img/2015/06/Review-details-OVA.jpg?resize=958%2C565)](https://emadyounis.com/assets/img/2015/06/Review-details-OVA.jpg)
5. Accept the EULA and click Next.  
    [![](https://emadyounis.com/assets/img/2015/06/EULA-VIO.jpg?resize=961%2C572)](https://emadyounis.com/assets/img/2015/06/EULA-VIO.jpg)
6. Enter a name that will be used for the VIO vApp and select the deployment folder. Click Next.  
    [![](https://emadyounis.com/assets/img/2015/06/VIO-name-and-folder.jpg?resize=958%2C568)](https://emadyounis.com/assets/img/2015/06/VIO-name-and-folder.jpg)
7. Select the storage location for the VIO OVA, disk format, and storage policy. Click Next.  
    [![](https://emadyounis.com/assets/img/2015/06/VIO-Storage.jpg?resize=960%2C563)](https://emadyounis.com/assets/img/2015/06/VIO-Storage.jpg)
8. Select the network the VIO manager will be connected to. vCenter should also have access to this network. Click Next.  
    [![](https://emadyounis.com/assets/img/2015/06/VIO-Network.jpg?resize=957%2C563)](https://emadyounis.com/assets/img/2015/06/VIO-Network.jpg)
9. Input all the properties values for the VIO OVA deployment. One value to pay close attention to is the VC SSO Lookup Service. When using vSphere 5.5 the SSO port of 7444 is required, which is not for vSphere 6.0.  
    **vSphere 5.5 – https://vCenter FQDN or IP:7444/lookupservice/sdk**  
    **vSphere 6.0 – https://vCenter FQDN or IP/lookupservice/sdk**  
    <span style="color: #ff0000;">**Note:**</span> If SSO configuration is not correct here, it will cause issues when configuring VIO.  
    [![](https://emadyounis.com/assets/img/2015/06/Customize-VIO-OVA.jpg?resize=1024%2C548)](https://emadyounis.com/assets/img/2015/06/Customize-VIO-OVA.jpg)
10. Validate the vService bindings and click Next.  
    [![](https://emadyounis.com/assets/img/2015/06/vService-Bindings.jpg?resize=1024%2C520)](https://emadyounis.com/assets/img/2015/06/vService-Bindings.jpg)
11. Review settings, check power on after deployment, and click Finish.  
    [![](https://emadyounis.com/assets/img/2015/06/Review-VIO.jpg?resize=1024%2C520)](https://emadyounis.com/assets/img/2015/06/Review-VIO.jpg)
12. Verify the VMware Integrated OpenStack icon appears on the home page of the Web Client. If the Icon is not present then log out and back in the Web Client.  
    [![](https://emadyounis.com/assets/img/2015/06/Web-Client-ICON.jpg?resize=923%2C222)](https://emadyounis.com/assets/img/2015/06/Web-Client-ICON.jpg)

<span style="text-decoration: underline;">**<span style="color: #000000;">Power On </span>**</span>

![](https://emadyounis.com/assets/img/2015/06/power-button.png?resize=41%2C41)If you attempt to power on the VIO management-server outside of the vApp, the following message will appear. Click No and power on the vApp.

[![](https://emadyounis.com/assets/img/2015/06/VIO-Mgmt-SRV-power-on.jpg?resize=317%2C199)](https://emadyounis.com/assets/img/2015/06/VIO-Mgmt-SRV-power-on.jpg)

[![](https://emadyounis.com/assets/img/2015/06/vApp-Power-On.jpg?resize=592%2C226)](https://emadyounis.com/assets/img/2015/06/vApp-Power-On.jpg)

<span style="color: #ff0000;">**Note:**</span> Since I’m deploying VIO on Ravello, running nested ESXi hosts the following error was encountered. To resolve this follow the instructions in <span style="color: #0000ff;">**[VMware KB 2108724](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2108724)**</span>.

[![](https://emadyounis.com/assets/img/2015/06/Hypervisor-Error.jpg?resize=294%2C119)](https://emadyounis.com/assets/img/2015/06/Hypervisor-Error.jpg)

<span style="text-decoration: underline;">**Summary**</span>

At this point you should have the VIO OVA deployed, the vAPP powered on, and see the VMware Integrated OpenStack icon in the home page of the vSphere Web Client. The next few blog posts will cover configuring VIO and errors I encountered.
