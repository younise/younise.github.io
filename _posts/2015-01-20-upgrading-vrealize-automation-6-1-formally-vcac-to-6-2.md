---
id: 301
title: 'Upgrading vRealize Automation 6.1 (formally vCAC) to 6.2'
date: '2015-01-20T10:03:12-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=301'
permalink: /upgrading-vrealize-automation-6-1-formally-vcac-to-6-2/
image: 'https://emadyounis.com/assets/img/2015/01/vRealize1.jpg'
categories:
    - 'vRealize Automation'
tags:
    - IaaS
    - 'Identity Appliance'
    - Upgrade
    - vCAC
    - 'vCAC 6.1'
    - vRA
    - 'vRA 6.2'
    - 'vRA Appliance'
---

In a previous post, we created a [Windows Server repository ](http://emadyounis.com/vmware/windows-repository-for-appliance-updates/ "Windows Repository for Offline Appliance Updates")for updates. This repository will serve to upgrade both the Identity Appliance and vRA Appliance(s). Before upgrading we need to ensure backups are in place. I know I’m preaching to the choir, but still need to stress the importance. We are not short of ways to backup the environment using VM snapshots (temporarily) and vSphere data protection to name a few. For SQL using a maintenance plan is also an option. The success of the upgrade relies on following the correct order. The steps below follow a simple deployment model but I will try to add areas where a distributed model differs.

<span style="text-decoration: underline;">**Upgrade Steps**</span>

1. Backup Identity Appliance, vRA Appliance(s), IaaS database, and IaaS server(s).
2. Stop vRA Appliance(s) Services.
3. Stop IaaS Server(s) services.
4. Identity Appliance Upgrade.
5. vRA Appliance Upgrade.
6. IaaS Database Upgrade.
7. IaaS Upgrade.

<span style="text-decoration: underline;">**Prerequisites**</span>

1. Log in the vRA Appliance(s) via SSH or VM console and disable the vco-service running the following commands: **service vco-server stop** &amp; **chkconfig vco-server off**.
2. The following services also need to be stopped on all vRA Appliances: 
    - service vcac-server stop
    - service apache2 stop
    - service rabbitmq-server stop
3. Check the services have been stopped use the status command, ex: service vco-server status.
4. Shutdown the following services on the IaaS server ( make sure services are stopped on all servers in a distributed model): 
    - All VMware vCloud Automation Center agents
    - All VMware DEM workers
    - VMware DEM orchestrator
    - VMware vCloud Automation Cneter Manager Service

<span style="color: #ff0000;">**Note:**</span> Do not reboot any of the servers or start the services until the upgrade process is complete.

<span style="text-decoration: underline;">**Appliance(s) Upgrade**</span>

Update the Identity appliance followed by the vRA Appliance. The upgrade procedure is similar for both appliances.

1. Log in the appliance(s). 
    - Identity Appliance: **https://Identity Appliance FQDN:5480**.
    - vRA Appliance: **https://vRA Appliance FQDN:5480**.
2. Select the Update tab, then click settings.
3. Ensure to select a repository type, in this case I’m using a specified repository.  
    [![](https://emadyounis.com/assets/img/2015/01/appliance-1.jpg?resize=828%2C473)](https://emadyounis.com/assets/img/2015/01/appliance-1.jpg)
4. Save settings and click status.
5. Click Check Updates and then Install Updates. Wait patiently until updates are applied.  
    [![](https://emadyounis.com/assets/img/2015/01/appliance-2.jpg?resize=830%2C410)](https://emadyounis.com/assets/img/2015/01/appliance-2.jpg)  
    [![](https://emadyounis.com/assets/img/2015/01/appliance-3.jpg?resize=830%2C182)](https://emadyounis.com/assets/img/2015/01/appliance-3.jpg)

**<span style="text-decoration: underline;">IaaS Upgrade</span>**

1. Navigate to **https://vRA Appliance FQDN:5480/i or installer**.
2. Download the database upgrade scripts.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-1.jpg?resize=1003%2C502)](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-1.jpg)
3. Extract the DBUpgrade.zip.
4. Open a command prompt with Administrator privileges and navigate to the DBUgrade directory.
5. Run the DBUpgrade.exe command using the switches below: 
    - **-S** SQL instance, port if needed
    - **-d** DBname
    - **-E**<span style="line-height: 1.5;"> Windows Authentication or </span>**-U**<span style="line-height: 1.5;"> SQL login</span>
    - **-l**<span style="line-height: 1.5;"> upgrade log – default is DBUpgrade directory</span>
    
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-2.jpg?resize=800%2C78)](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-2.jpg)  
    <span style="color: #ff0000;">**Note:**</span> VMware’s documentation states to run this command only once, make sure a DB backup is taken prior to upgrade.
6. Download the IaaS installer from the vRA Appliance (step 1) and don’t change the file name.
7. Run the installer with Administrator privileges.
8. Click Next on the Welcome to the vCloud Automation Center Configuration screen.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-3.jpg?resize=800%2C600)](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-3.jpg)
9. Accept the EULA, click Next.
10. Enter the vRA Appliance root account and password, check Accept Certificate and click Next.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-4.jpg?resize=797%2C597)](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-4.jpg)
11. Select Upgrade and click Next.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-5.jpg?resize=800%2C599)](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-5.jpg)
12. Select the components to upgrade (my setup has all components on the same server). Verify service account information and add database information. Click Next.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-6.jpg?resize=799%2C599)](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-6.jpg)  
    <span style="color: #ff0000;">**Note:**</span> In a distributed model the following upgrade order must be used: 
    - Websites
    - Manager Services
    - DEM orchestrator and workers
    - Agents
13. Confirm all components are listed and click Upgrade.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-7.jpg?resize=797%2C599)](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-7.jpg)
14. Once the upgrade is complete click Next.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-8.jpg?resize=800%2C600)](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-8.jpg)
15. vRA upgrade is now complete, click Finish.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-9.jpg?resize=800%2C599)](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-9.jpg)
16. Verify all required services are started on vRA Appliance(s) and IaaS Server(s).  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-10.jpg?resize=829%2C668)](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-10.jpg)  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-11.jpg?resize=819%2C185)](https://emadyounis.com/assets/img/2015/01/IaaS-Upgrade-11.jpg)

<span style="text-decoration: underline;">**vRA Branding**</span>

While this step is optional, it allows the product to display the new branding that marketing has bestowed upon us.

1. Log in the vRA Appliance **https://vRA Appliance FQDN:5480**.
2. Select the Setting tab and click the SSO tab.
3. Enter SSO setting and check Apply Branding then save settings.  
    [![](https://emadyounis.com/assets/img/2015/01/Rebranding-1.jpg?resize=829%2C367)](https://emadyounis.com/assets/img/2015/01/Rebranding-1.jpg)  
    [![](https://emadyounis.com/assets/img/2015/01/Rebranding-2.jpg?resize=1022%2C564)](https://emadyounis.com/assets/img/2015/01/Rebranding-2.jpg)

There you have it from start to finish, the upgrade process is really not that difficult once all the steps are followed in order. You can now delete those temporary snapshots 🙂
