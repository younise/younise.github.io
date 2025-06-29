---
id: 279
title: 'vCenter Appliance (vCSA) 6.0 &#8211; New &#038; Improved'
date: '2015-02-02T15:14:31-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=279'
permalink: /vcenter-appliance-vcsa-6-0-new-improved/
image: 'https://emadyounis.com/assets/img/2015/01/vCenter-6.0.jpg'
categories:
    - vCenter
tags:
    - Appliance
    - VMware
    - vSphere
---

![vCenter 6.0](https://emadyounis.com/assets/img/2015/01/vCenter-6.0.jpg?resize=223%2C205)

It’s that time again, a new release of vSphere is upon us. Some features we’ve been waiting for a while now, others are new or revamped. It’s evident that VMware has put effort into the vCenter appliance. In previous releases it was lacking several features needed to make it equal to its Windows Counterpart. Well it seems that VMware has heard the many cries, and the appliance is now, in my opinion, ready for prime time. Let’s take a look at what’s new and you decide if you will be a vCenter 6.0 appliance adopter.

**<span style="text-decoration: underline;">Dude Where’s My OVA / OVF?</span>**

The first thing you’ll notice when downloading the new vCenter appliance is the lack of OVA / OVF, they’re gone! Replaced now by a self contained ISO, which was typical for a windows deployment but an appliance? The ISO is 2GB in size and uses an HTML 5 based installer. It contains the following:

- vCenter Binaries &amp; Installer bits
- The necessary plugins for both Windows and Mac 
    - VMware Client integration Plug-in.pkg
    - VMware Client Integration Plugin 6.0.exe
- Binaries &amp; Templates for scripted installs
- Index.html – Appliance Guided Installer

[![](https://emadyounis.com/assets/img/2015/02/VCSA6-NEW.jpg?resize=1024%2C467)](https://emadyounis.com/assets/img/2015/02/VCSA6-NEW.jpg)

<span style="text-decoration: underline;"> **Deployment Types**</span>

For this release of the vCSA there are only 2 supported deployment types. The first one is guided, which can be found when using the HTML 5 installer. The installer has built in validations process. The validation process takes place up front throughout the installation, assuring the targeted vSphere environment is supported and system requirements are met. Once the guided install is complete the vCSA appliance is ready for use.

[![](https://emadyounis.com/assets/img/2015/01/VCSA6-3.jpg?resize=871%2C558)](https://emadyounis.com/assets/img/2015/01/VCSA6-3.jpg)[![](https://emadyounis.com/assets/img/2015/01/VCSA6-7.jpg?resize=872%2C559)](https://emadyounis.com/assets/img/2015/01/VCSA6-7.jpg)

The second deployment type is scripted. The scripted installer allows for the automation of the vCSA deployments. The Install.exe uses a mandatory template file and several optional parameters. VMware has supplied 5 sample template files, which can be found under vCSA-CLI-Installer directory. The templates are good for various deployments including distributed. The scripted installed also has a handy help option (install.exe -h) which provides an explanation of the parameter types.

[![](https://emadyounis.com/assets/img/2015/01/VCSA6-4.jpg?resize=707%2C518)](https://emadyounis.com/assets/img/2015/01/VCSA6-4.jpg)[![](https://emadyounis.com/assets/img/2017/01/VCSA-6.0.jpg?resize=634%2C242)](https://emadyounis.com/assets/img/2017/01/VCSA-6.0.jpg)

<span style="text-decoration: underline;">**Appliance Access**</span>

After it’s deployment there are 3 ways to access the vCSA:

**vSphere Web Client** **–** This release of the Web Client has seen improvement in regards to speed and the clumsy animation of the past. The vCSA setting can be found under System Configuration. Select the desired node and use the manage tab to make configuration changes.

[![VCSA-8](https://emadyounis.com/assets/img/2015/01/VCSA-8.jpg?resize=1024%2C710)](https://emadyounis.com/assets/img/2015/01/VCSA-8.jpg)[![VCSA-9](https://emadyounis.com/assets/img/2015/01/VCSA-9.jpg?resize=1024%2C742)](https://emadyounis.com/assets/img/2015/01/VCSA-9.jpg)

**Appliance Shell –** The Appliance shell contains all the API commands and plugins needed for monitoring, troubleshooting and configuration of the vCSA. Access to the Appliance Shell is granted through SSH or DCUI using Alt+F1.

[![VCSA12](https://emadyounis.com/assets/img/2015/01/VCSA12.jpg?resize=788%2C403)](https://emadyounis.com/assets/img/2015/01/VCSA12.jpg)[![VCSA-11](https://emadyounis.com/assets/img/2015/01/VCSA-11.jpg?resize=640%2C480)](https://emadyounis.com/assets/img/2015/01/VCSA-11.jpg)

**DCUI (Direct Console User Interface) –** This is the interactive text based menu of the vCSA. From the Web Client browse to the vCSA and click the summary tab. Click Launch Console to access the DCUI.

[![VCSA-10](https://emadyounis.com/assets/img/2015/01/VCSA-10.jpg?resize=769%2C278)](https://emadyounis.com/assets/img/2015/01/VCSA-10.jpg)

<span style="text-decoration: underline;">**Size Does Matter**</span>

Out of the box the vCenter appliance has 4 supported sizes. They range from Tiny to Large, still waiting on big gulp. You are not locked in to a particular size if you infrastructure grows. Changing the size is as simple as editing the Appliance VM.

[![VCSA6-2](https://emadyounis.com/assets/img/2015/01/VCSA6-2.jpg?resize=1024%2C408)](https://emadyounis.com/assets/img/2015/01/VCSA6-2.jpg)[![VCSA6-5](https://emadyounis.com/assets/img/2015/01/VCSA6-5.jpg?resize=872%2C557)](https://emadyounis.com/assets/img/2015/01/VCSA6-5.jpg)

<span style="text-decoration: underline;">**Supported Features** </span>

- Support for Hardware version 11
- Embedded tuned vPostgres database / Oracle for external
- Linked Mode (Mix and match between windows &amp; vCSA)
- IPv6 Support
- SRM (Site Recovery Manager)
- PowerCLI – (Connection from a PowerCLI session)
- Native Replication replaces Microsoft ADAM
- Replication of Policies &amp; Tags in Link Mode
- Patching (Product &amp; Security) via https://www.vmware.com/patchmgr/findPatch.portal

<span style="text-decoration: underline;">**Missing Features**</span>

- 100% VMware Update Manager Support
- Microsoft SQL Server Support
- SQL Server to vPostgres Migration

<span style="text-decoration: underline;">**Summary**</span>

Although the appliance now has feature parity, there’s still a dependency on Windows. VMware Update Manager (VUM) still requires a Windows Server to install. I was hoping that we would see VUM transition to an appliance deployment as well. Another item missing is a migration path from a Windows to an Appliance vCenter. There is currently not a supported way to go from a MS SQL database to postgreSQL. While the having these options would be welcomed they are not show stoppers in considering the vCSA as the go to vCenter deployment. What are your thoughts?
