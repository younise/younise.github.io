---
id: 829
title: 'vCenter Server Appliance (VCSA) 6.5 What&#8217;s New Rundown'
date: '2016-10-31T07:05:53-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=829'
permalink: /vcenter-server-appliance-vcsa-6-5-whats-new-rundown/
image: 'https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Whats-New-Rundown-e1477888912313.png'
categories:
    - vCenter
tags:
    - Appliance
    - VCSA
    - 'VCSA 6.5'
    - VMware
    - vSphere
    - 'vSphere 6.5'
---

During VMworld Barcelona 2016, VMware announced vSphere 6.5. With lots of new features and updates, this is one of the biggest vSphere releases we have had in a while. Leading the charge is none other than the vCenter Server Appliance (VCSA). VMware’s direction for vCenter Server is the VCSA and it’s quite evident from this release. The VCSA and its Windows counterpart remain on par when it comes to scalability. From an operational and feature perspective, the VCSA has advantages. This post is a quick rundown of the VCSA 6.5, I’ll go into more details in future blog posts. Time to take a look at what’s new and why migrate2vcsa you should 🙂

## VCSA 6.5 ISO

Starting with the VCSA 6.5 ISO, improvements are easily visible when comparing to 6.O:

[![](https://emadyounis.com/assets/img/2016/10/VCSA-ISO.png?resize=1024%2C550)](https://emadyounis.com/assets/img/2016/10/VCSA-ISO.png)

- The Migration Tool is now built into the VCSA installer.
- One of the biggest asks from customers is now part of this release. VMware Update Manager (VUM) **no longer requires a Windows Server and runs natively on the VCSA**. Included on the ISO is Update Manager Download Service (UMDS). UMDS is available for those that have restrictions on internet connectivity to VUM.
- The VCSA ova is available and can be used as one alternative for deployments.
- My favorite part, the JSON templates have been updated and include improvements. This release includes around 30 templates covering install, upgrade, and migrate
- The VCSA installer now sports a new modern look, independent of a browser. There is also support for macOS, Linux, and Windows. Deploying VCSA from my macOS, yes, please!

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Installer.png?resize=1024%2C619)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Installer.png)

## VCSA Only Features

**Migration**

As mentioned earlier the Migration Tool in the VCSA 6.5 is now built-in. This version of the Migration Tool improvements include:

- Migration of **Windows vCenter Server 5.5 or 6.0** to VCSA 6.5
- Improved Performance in the migration process
- More options for historical and performance data selection
- VMware Update Manager migration part of the VCSA

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Migration.png?resize=709%2C113)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Migration.png)

**vCenter High Availability**

This solution consists of an Active, Passive, and Witness nodes. The Passive and Witness nodes are clones from the original (Active) vCenter Server. There are two supported workflows: Basic &amp; Advanced.

- The Basic workflow will clone the nodes, and create rules for placement. DRS and Storage DRS are not requirements but provide automated placement when available. The Basic workflow is flexible, allowing the choice of node placement within a cluster or across clusters within a vCenter Server.
- Placing nodes in different clusters, datacenters, or vCenter Servers requires the Advanced workflow. This is a manual process, including cloning of the nodes and placement.

Remember, this is a <span style="font-weight: bold;">High Availability solution, not a Disaster Recovery solution. </span>Make sure to repeat that as you’re deploying VCHA.

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-VCHA.png?resize=1024%2C344)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-VCHA.png)

<span style="font-weight: bold;">Backup / Restore</span>

A native file-based solution allows the backup of an embedded or external deployment. Also supports the backup of individual appliances: VCSA or PSC.

- Start the Backup workflow from the VMware vSphere Appliance Management Interface or API.
- This release supports the following protocols: HTTP(s), FTP(s), and SCP.
- Streamed are a set of files that make up the VCSA configuration to the supported protocols.
- Hot backup of the VCSA, no quiescence required.
- Launch the Restore workflow from the VCSA ISO and point to the protocol used to backup.
- Deployed is a new VCSA, keeping its personality (FQDN, IP, UUID, etc.) and configuration including inventory.

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Backup.png?resize=1024%2C159)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Backup.png)

**Management Interface**

The VMware vSphere Appliance Management Interface (VAMI) has a new sexy facelift in 6.5.

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-VAMI.png?resize=1195%2C591)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-VAMI.png)

Not only that but new features that prove the much-needed visibility to the VCSA. This interface now shows CPU, Memory, and Network statistics. Also visibility to the VCSA Database statistics, disk space, and health providing improved monitoring. Syslog Configuration is now part of the VAMI as well.

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-VMAI-Monitoring.png?resize=1024%2C448)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-VMAI-Monitoring.png)

## More Improvements

The ISO is the not the only place you’ll see improvements. The vCenter Server appliance and application also got some of the new hotness, take a look.

- **Photon OS** – the VCSA is the first VMware appliance to run on Photon OS. 
    - This provides a significant reduction in boot time and vCenter application startup times.
    - It also means that VMware owns the stack, which is beneficial for patching and support.
- **2 Stage deployment** provides better validation checks. 
    - Stage 1 deploys OVF
    - Stage 2 for configuration
- **VMON** -Enhanced watchdog functionality 
    - Checks health of each service
    - vCenter Server: Windows and Appliance
    - First level of defense – application restart

- **Client Integration Plugin (CIP)** – is no longer required, replaced by native browser functions.
- **Enhanced Authentication Plugin** – only If you plan on using Windows Authentication (SSPI) or Smart Card Authentication.

[![](https://emadyounis.com/assets/img/2016/10/Windows-Auth.png?resize=986%2C522)](https://emadyounis.com/assets/img/2016/10/Windows-Auth.png)

[![](https://emadyounis.com/assets/img/2016/10/Smart-Card.png?resize=580%2C153)](https://emadyounis.com/assets/img/2016/10/Smart-Card.png)

**Clients**

Before I talk about the Web Client, let me state the C# (thick) client is gone and not available. The 6.0 version of the C# client will not work with a vSphere 6.5 environment. That was for you [<span style="font-weight: bold; color: #0000ff;">Adam</span>](https://twitter.com/eck79), hope you’re happy.

- The new HTML5 “vSphere Client” based on the Fling is now available on the VCSA.
- **vSphere Client is fully supported** in VCSA 6.5.
- Limited functionality of the vSphere Client is available in this release.
- Future updates of the vSphere Client will be available separately from vCenter Server.

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Clients.png?resize=580%2C181)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Clients.png)

- vSphere Web Client – Adobe Flex (<span style="color: #0000ff;">**https://FQDN or IP Address of VCSA/vsphere<span style="color: #0000ff;">–</span>client**</span>)
- vSphere Client -HTML5 (**<span style="color: #0000ff;">https://FDQN or IP Address of VCSA/UI</span>**)
- Host Client – HTML5 (<span style="color: #0000ff;">**https://FQDN or IP Address of ESXi host/UI**</span> **-&gt; available since vSphere 6.0 Update 2**
- vSphere Appliance Management – HTML5 (<span style="color: #0000ff;">**https://FQDN or IP Address of VCSA:5480**</span>)
- PSC Management UI – HTML5 **-&gt;** **available since vSphere 6.0 Update 1**
    - External deployments (<span style="color: #0000ff;">**https://FQDN or IP Address of PSC/PSC**<span style="color: #000000;">)</span></span>
    - Embedded deployments (**<span style="color: #0000ff;">[https://FQDN or IP Address of VCSA/PSC](https://FQDN%20or%20IP%20Address%20of%20VCSA/PSC)</span>**<span style="color: #000000;">)</span>

**Web Client** has received love this release with performance improvements and live refresh. The days of hitting the refresh button are over, it now happens in real time! The Related Object tab is gone, replaced with a flat menu comparable to the C# client. Also, renamed is the Manage tab to Configure, more intuitive of the available tasks.

**CLI / Automation**

- Simplified Bash Shell now just type “shell”

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Bash-Shell.png?resize=580%2C278)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Bash-Shell.png)

- REST APIs / API Explorer.

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Rest-APIs.png?resize=580%2C205)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-Rest-APIs.png)

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-API-Explorer-1024x500.png?resize=580%2C283)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-API-Explorer.png)

- **Datacenter CLI (DCLI)** – a native client for the REST APIs. 
    - Accessible via vCLI, VCSA Shell, Windows CMD prompt.
    - DCLI also has you covered with auto-tab completion, no need to remember commands.

**Note:** example below using SSH from my Mac to VCSA 6.5.

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-DCLI-1.png?resize=580%2C278)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-DCLI-1.png)

[![](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-DCLI-2.png?resize=580%2C278)](https://emadyounis.com/assets/img/2016/10/VCSA-6.5-DCLI-2.png)

The VCSA has seen a lot of improvements since its initial release in vSphere 5.0. Now with the vSphere 6.5 release, the VCSA will show why it’s the first choice deployment. In this release the VCSA has an abundant amount of exclusive features and improvements. Not only that, now there are Migration Tools that will do all the heavy lifting to get you there. Future blog posts will cover these feature and improvements more in depth, so stay tuned. In the mean time start planning those VCSA deployments and migrations 🙂
