---
id: 639
title: 'vCenter Server Appliance (VCSA) 6.0 Update 1 Rundown'
date: '2015-09-14T08:57:33-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=639'
permalink: /vcenter-server-appliance-vcsa-6-0-update-1-rundown/
image: 'https://emadyounis.com/assets/img/2015/09/VCSA-Update-1-Main-Image.png'
categories:
    - vCenter
tags:
    - Appliance
    - Upgrade
    - 'VCSA 6.0'
    - VMware
    - vSphere
---

<span style="color: #000000;">![VCSA Update 1 - Main Image](https://emadyounis.com/assets/img/2015/09/VCSA-Update-1-Main-Image.png?resize=219%2C119)VMworld 2015 has come and gone, at least the US version. There were several sessions giving love to vCenter. This included the vCenter Server Appliance (VCSA). Here are a few of the sessions that are worth a look.</span>

<span style="color: #0000ff;">**[INF5975 – vCenter Server Appliance as First Choice VC](https://www.youtube.com/watch?v=jbbFbdrQMXY)**</span>  
<span style="color: #0000ff;"> **[INF4528- vCenter Server Appliance VCSA Best Practices &amp; Tips Tricks](https://www.youtube.com/watch?v=S8pgFDxTnrY)** </span>  
<span style="color: #008000;">**INF4944- Managing vSphere 6.0 Deployments and Upgrades, Part 1**</span>  
<span style="color: #008000;">**INF4945 – vCenter Server 6 High Availability**</span>

<span style="text-decoration: underline;">**GUI Deployment**</span>

For a new deployment, you now have the option of two deployment targets. ESXi has been the only option until now, introducing vCenter Server as a deploy target.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Deployment-1.jpg?resize=870%2C557)](https://emadyounis.com/assets/img/2015/09/VCSA-Deployment-1.jpg)

If you select vCenter Server as a target, two new options are added to the deployment menu on the left. The first is “Select Datacenter” and second, “Select Resource”. The rest of the deployment process is the same as before.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Deployment-2.jpg?resize=871%2C369)](https://emadyounis.com/assets/img/2015/09/VCSA-Deployment-2.jpg)

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Deployment-3.jpg?resize=870%2C375)](https://emadyounis.com/assets/img/2015/09/VCSA-Deployment-3.jpg)

<span style="text-decoration: underline;">**Scripted Deployment**</span>

Aside from the GUI deployment, there is also a <span style="color: #0000ff;">**[scripted deployment](http://emadyounis.com/vcenter/vcenter-appliance-vcsa-6-0-scripted-install/)**</span>. This was also introduced in 6.0, but now with more additions. The **VCSA-Cli-Installer directory\\Templates** directory contains the JSON scripts.[![](https://emadyounis.com/assets/img/2015/09/VCSA-Scripted-Deploy-1.jpg?resize=887%2C200)](https://emadyounis.com/assets/img/2015/09/VCSA-Scripted-Deploy-1.jpg)

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Scripted-01.jpg?resize=772%2C461)](https://emadyounis.com/assets/img/2015/09/VCSA-Scripted-01.jpg)

For new deployments utilize the scripts from the install directory. This directory now has 8 JSON scripts that spans both ESXi and vCenter.

Introduced in U1, an upgrade script method has been added. The upgrade directory has JSON scripts that allow upgrades from 5.1 U3 and 5.5. More on how to upgrade later.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Script-02.jpg?resize=769%2C756)](https://emadyounis.com/assets/img/2015/09/VCSA-Script-02.jpg)

More details about scripted VCSA deployments can be found [here](http://www.vmware.com/files/pdf/products/vsphere/VMware-vsphere-601-vcenter-server-appliance-cmdline-deploy-and-upgrade.pdf) .

<span style="text-decoration: underline;">**Upgrade**</span>

<span style="color: #ff0000;">**Note:**</span> Supported upgrade paths to VCSA 6.0 U1 include from 5.1 U3 &amp; 5.5.

While on the topic of upgrades, going from VCSA 6.o to 6.0 U1 is quite simple. First, you’ll need a patch ISO, which can be found on <span style="color: #0000ff;">**[here](https://my.vmware.com/group/vmware/patch#search)**</span>. You ‘ll notice there are two patch ISOs, FP and TP.

**FP** is a full product patch for VC &amp; PSC appliances only.

**TP** is third party patch spanning multiple products, such as VCSA, vCenter Windows, VUM, PSC appliance and windows.

<span style="color: #ff0000;">**Note**</span>: Make sure you read the corresponding patch KB prior to upgrading.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Upgrade-1.jpg?resize=972%2C183)](https://emadyounis.com/assets/img/2015/09/VCSA-Upgrade-1.jpg)

Attach the patch ISO to the VCSA cd-rom and run the following command “**software-packages –install –iso –acceptEulas**“.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Scripted-Deploy-2.jpg?resize=881%2C424)](https://emadyounis.com/assets/img/2015/09/VCSA-Scripted-Deploy-2.jpg)

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Scripted-Deployment-3.jpg?resize=1074%2C235)](https://emadyounis.com/assets/img/2015/09/VCSA-Scripted-Deployment-3.jpg)

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Scripted-Deployment-4.jpg?resize=1063%2C519)](https://emadyounis.com/assets/img/2015/09/VCSA-Scripted-Deployment-4.jpg)

After the upgrade is complete a reboot is required for the VCSA.

VCSA 5.1 U3 or 5.5 upgrades can be done from the 6.0 U1 ISO. Select Upgrade from the vcsa-setup and go through the wizard. You will need a temporary IP address, as the process deploys a new VCSA and ports over all the info. Once completed the old version of the VCSA will be shutdown.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Upgrade-11.jpg?resize=549%2C407)](https://emadyounis.com/assets/img/2015/09/VCSA-Upgrade-11.jpg)

As mentioned above scripted upgrades are also supported.

**<u>Virtual Appliance Management Interface (VAMI)</u>**

VCSA 6.0 was minus an old friend, the VAMI. VCSA 6.0 U1 the VAMI has returned! (applause). Not only is it back, but a with a new look in HTML5. To access the VAMI go to <span style="color: #0000ff;">https://FQDN or IP of VCSA:5480</span><span style="color: #000000;">.</span>

[![](https://emadyounis.com/assets/img/2015/09/VCSA-VAMI-1.jpg?resize=1250%2C660)](https://emadyounis.com/assets/img/2015/09/VCSA-VAMI-1.jpg)

The new interface is definitely noticeable. Also the menus are now located on the left hand side of the UI instead of along of the top as in previous releases.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-VAMI-2.jpg?resize=1421%2C531)](https://emadyounis.com/assets/img/2015/09/VCSA-VAMI-2.jpg)

Two things that I would like to point out. First, the update menu has the ability to check for updates. This can be done atomically or by using a repository. Future updates will have the ability to be applied from here.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-VAMI-3.jpg?resize=1419%2C402)](https://emadyounis.com/assets/img/2015/09/VCSA-VAMI-3.jpg)

Secondly, the option to change your root password and expiration settings are under the administration menu. Make sure your vCenter SMTP is configured to received the expiration warnings. In case you were wondering the max number of days allowed is 99,999 (security!).

[![](https://emadyounis.com/assets/img/2015/09/VCSA-VAMI-4.jpg?resize=1414%2C505)](https://emadyounis.com/assets/img/2015/09/VCSA-VAMI-4.jpg)

<span style="text-decoration: underline;">**Web Client**</span>

From the web client perceptive, two new icons have been added by default. The Hybrid Cloud Manager, which extends connectivity to vCloud Air. The other is the Customer Experience Improvement. There are no annoying messages asking to join and is disabled by default.

[![](https://emadyounis.com/assets/img/2015/09/Webclient.jpg?resize=921%2C492)](https://emadyounis.com/assets/img/2015/09/Webclient.jpg)

There is now an option to change to a remote syslog server. Go to System Configuration and select Services. Look for VMware Syslog Service and click the Manage tab.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-WebClient-1.jpg?resize=1306%2C812)](https://emadyounis.com/assets/img/2015/09/VCSA-WebClient-1.jpg)

<span style="text-decoration: underline;">**VUM Integration**</span>

We finally have VMware Update Manager (VUM) integration in the web client, well sort of. A windows installation is still required for VUM. The upside is now all the VUM functions are now available in the web client, so we’re getting closer. Once VUM is pointed to vCenter, logout of the web client and back in, ta-da!

[![](https://emadyounis.com/assets/img/2015/09/VSCA-VUM-1.jpg?resize=1287%2C436)](https://emadyounis.com/assets/img/2015/09/VSCA-VUM-1.jpg)

[![](https://emadyounis.com/assets/img/2015/09/VCSA-VUM-2.jpg?resize=1273%2C458)](https://emadyounis.com/assets/img/2015/09/VCSA-VUM-2.jpg)

<span style="color: #ff0000;">**Note:**</span> This integration is also available with Windows version of vCenter

<span style="text-decoration: underline;">**Backup / Restores**</span>

Support for VCSA backups via VMware vSphere Storage APIs – Data Protection (formley VADP) has been extended. In addition to VCSA with embedded PSC support, now external PSC has been added. This can be done leveraging VMware’s VDP or any third party solution with VMware vSphere Storage APIs – Data Protection, like Veeam. In this example I’m using VDP 6.1 and have taken backups of my external PSC and VCSA.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Backups-1.png?resize=926%2C298)](https://emadyounis.com/assets/img/2015/09/VCSA-Backups-1.png)

If corruption or a crash occurs to the VCSA and a restore is required are you prepared? Your solution although runs on VMware and is integrated with the VCSA should have a way to bring it back. In this example using VDP 6.1 Emergency Restore feature. Verify you have a solution or plan in place to bring the VCSA to life during such catastrophes.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-Backups-2.jpg?resize=960%2C235)](https://emadyounis.com/assets/img/2015/09/VCSA-Backups-2.jpg)

<span style="text-decoration: underline;">**Platform Services Controller (PSC)**</span>

There are a couple new items for the Platform Services Controller with this release. PSC has a new UI just like the VAMI, in HTML5. To access go to **https:// FQDN or IP of VCSA/PSC**. This new UI is useful for troubleshooting and Certificate management. The appliance settings menu has a link to redirect to the VAMI, integration at its finest.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-PSC-1.jpg?resize=1253%2C307)](https://emadyounis.com/assets/img/2015/09/VCSA-PSC-1.jpg)

The command CMSSO-UTIL, now provides more flexibility. Allows you to repoint a VCSA with embedded PSC to external. This is great IMO so you no longer have to redeploy a new setup if you need to switch to an external PSC. Just make sure your PSC are in SSO domain.

[![](https://emadyounis.com/assets/img/2015/09/VCSA-PSC-2-New.jpg?resize=963%2C423)](https://emadyounis.com/assets/img/2015/09/VCSA-PSC-2-New.jpg)

[![](https://emadyounis.com/assets/img/2015/09/VCSA-PSC-3.jpg?resize=1236%2C310)](https://emadyounis.com/assets/img/2015/09/VCSA-PSC-3.jpg)

As you can see, VCSA 6.0 update 1 is packed with a lot of goodness. VMware is definitely listening when it comes to vCenter, especially on the VCSA. This was more evident in this year’s sessions, with more detailed tech previews. I’m definitely looking forward to what the future holds for the VCSA.
