---
id: 872
title: 'vCenter Server Appliance (VCSA) 6.5 Installer macOS Ovftool Error'
date: '2016-11-21T12:42:43-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=872'
permalink: /vcenter-server-appliance-vcsa-6-5-installer-macos-ovftool-error/
image: 'https://emadyounis.com/assets/img/2016/11/macOS-feature-Image.png'
categories:
    - vCenter
tags:
    - Appliance
    - macOS
    - VCSA
    - 'VCSA 6.5'
    - VMware
    - 'vSphere 6.5'
---

Recently, some macOS Sierra users have had an issue with the VCSA 6.5 installer. At the “Set up appliance VM” (step 5), the VCSA deployment and storage sizes are not available. The following error is received **“A problem occurred while reading the OVF file.. Error: ovftool is not available”**. Let me start by saying this is **not** an issue with the VCSA 6.5 ISO or installer, it has to do with a change in Sierra. Also, before I go any further, Sierra **is currently not supported** with the VCSA 6.5 installer. Engineering is working to support Sierra in a future update. The supported system requirements for the VCSA installer are <span style="color: #0000ff;">**[here](http://pubs.vmware.com/vsphere-65/index.jsp#com.vmware.vsphere.install.doc/GUID-BA4FA18C-1049-42AA-A5CD-DE863565251B.html#GUID-BA4FA18C-1049-42AA-A5CD-DE863565251B)**</span>. With that said, I want to shed more light on why this is happening and what options are available with Sierra.

[![](https://emadyounis.com/assets/img/2016/11/VCSA-OVF-Error.png?resize=1004%2C694)](https://emadyounis.com/assets/img/2016/11/VCSA-OVF-Error.png)

Apple prefers that applications are signed and distributed from the App store. Recommendations for delivery outside the App store are signed disk images or installer packages. These images are signed with a unique Developer ID. Apple has made a change starting with Sierra on how applications are launched. An application from an ISO will be isolated by Gatekeeper to an unspecified read-only location. If you look at the VCSA installer log, there is a randomly given path. The change is to help prevent malware. Currently, Apple is not providing a way to sign ISO images. By the way, Apple has removed the option to allow apps downloaded from anywhere in Sierra as seen in the screenshot below. There is a command to enable it, but it still doesn’t work as mentioned above regarding the ISO. There are a few workaround options to use the VCSA 6.5 installer on Sierra.

[![](https://emadyounis.com/assets/img/2016/11/Sierra-Image.png?resize=668%2C543)](https://emadyounis.com/assets/img/2016/11/Sierra-Image.png)

## Option 1:

Extract or copy the contents of the VCSA ISO to a directory on the local Mac filesystem. This option, in my opinion, is the quickest and easiest. It can be helpful having a local copy for scripting. You will need to run the following command to remove the attributes. Removing the attributes clears the restrictions and lets Sierra know the VCSA installer is trusted. Another reason I prefer this method is its persistency only needs to be done once.

sudo xattr -rc Installer.app/

[![](https://emadyounis.com/assets/img/2016/11/VCSA-Command.png?resize=1027%2C236)  ](https://emadyounis.com/assets/img/2016/11/VCSA-Command.png)<span style="color: #ff0000;">**Note:**</span> copying the VCSA ISO contents to a directory and running it without the above command is hit and miss. You may end up with the following error. If so, run the command to remove the attributes.

[![](https://emadyounis.com/assets/img/2016/11/VCSA-Installer-Open-error.png?resize=420%2C153)](https://emadyounis.com/assets/img/2016/11/VCSA-Installer-Open-error.png)

## Option 2:

Look at the installer log, which is available to download from the installer. The log points to the path where the ovftool is missing. Copy the entire VCSA directory from the ISO to the private directory. Keep in mind this solution is non-persistent and will have to be done every time the ISO is mounted. The location is random which will require looking at the installer logs every time.

[![](https://emadyounis.com/assets/img/2016/11/VCSA-ISO.png?resize=770%2C436)](https://emadyounis.com/assets/img/2016/11/VCSA-ISO.png)

2016-11-21T06:44:11.114Z – debug: Deployment size page description: Select the deployment size for this vCenter Server with an Embedded Platform Services Controller.  
2016-11-21T06:44:11.115Z – debug: scope.backupProfileDataArr: undefined  
2016-11-21T06:44:11.116Z – error: could not find ovftoolCmd:<span style="color: red;"> /private/var/folders/j7/f6\_0rwmd2w571\_3jn6y\_hz1w00\_tnx/T/AppTranslocation/vcsa/ovftool/mac/ovftool</span>  
2016-11-21T06:44:11.116Z – info: ovftoolCmd: null  
2016-11-21T06:44:11.117Z – error: OVF probe error: Error: ovftool is not available

There you have it, two possible ways to use the VCSA 6.5 installer on Sierra. As stated earlier, **this not a supported operating system**, but will be in a future update. Special thanks to Nikhil from engineering for helping me while troubleshooting.
