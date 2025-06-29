---
id: 122
title: 'vRealize Automation 6.1 (formally vCloud Automation Center) &#8211; Identity Appliance &#8211; Deployment / Configuration'
date: '2014-11-05T11:16:48-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=122'
permalink: /vrealize-automation-6-1-formally-vcloud-automation-center-identity-appliance-deployment-configuration/
categories:
    - 'vRealize Automation'
tags:
    - 'Identity Appliance'
    - SSO
    - vCAC
    - 'vCAC 6.1'
    - VMware
    - vRealize
---

The introduction of Single Sign On (SSO) in vSphere 5.1 has provided a new major dependency of the infrastructure. SSO provides a secure way of authenticating to your vSphere environment. It not only serves as an authentication broker but also a token exchange, so what does this all mean you? In a nutshell this is a handshake.

When logging in a vSphere environment (C# client or Web Client) it sends a token to SSO containing username &amp; password. In turn SSO authenticates against the selected identity source. An identity source can be Active Directory, Open LDAP or Local OS. SSO can also support more than one identity source.

Once the original token authenticates, SSO issues a security token. This becomes your key.

![](https://emadyounis.com/assets/img/2014/11/vRA-SSO-KeyMaster.gif?resize=500%2C198)

Access to VMware products such as vCenter is now permitted. At this point you must be asking what does SSO have to do with vRealize Automation? Well its simple, vRealize Automation (formerly vCloud Automation Center) or vRA uses SSO as well. In fact SSO is one of the core deployment components, without it vRA would not function.

Before we get started, we have to be sure we have everything we need:

<span style="text-decoration: underline;">**Prerequisites**</span>

- DNS (forward / reverse) entry.
- Network Time Protocol (NTP).
- 1 vCPU / 2 GB of RAM / 2 GB of storage.
- Active Directory domain account.

<span style="text-decoration: underline;">**Ports Required for Identity Appliance**</span>

\[table id=1 /\]

**<span style="text-decoration: underline;">Deploying the Identity Appliance</span>**

<span style="color: #ff0000;">**Note : deployment of OVA done from the vSphere Web Client.**</span>

1. Log in the vSphere Web Client with an account that has administrator rights.
2. Select Hosts and Clusters.
3. Select an inventory object such datacenter, cluster, host or resource pool.
4. Right Click –&gt; Deploy OVF Template or use Actions –&gt; Deploy OVF Template.
5. Select Source location by browsing to the Identity Appliance file with either an extension of “.OVA “or “.OVF “and click Open. Click Next.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-1.jpg?resize=959%2C350)](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-1.jpg)
6. Click Next on the Review details screen.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-2.jpg?resize=958%2C409)](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-2.jpg)
7. Accept the EULA and click Next.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-3.jpg?resize=960%2C561)](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-3.jpg)
8. Provide the identity appliance VM a name (FQDN) and location for deployment such as a datacenter or folder. Click Next.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-4.jpg?resize=961%2C561)](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-4.jpg)
9. Select the disk format, VM storage policy (if configured) and datastore for deployment. Click Next. <span style="color: #ff0000;">**Note: Before you click next verify the disk format is the one you selected, I have seen it change after selecting the datastore in the vSphere Web Client.**</span>  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-5.jpg?resize=960%2C562)](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-5.jpg)
10. Setup Network by providing the destination (portgroup) and IP protocol, click Next.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-6.jpg?resize=961%2C562)](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-6.jpg)
11. Customize template by providing the properties values under the Application &amp; Network Properties sections. Click Next. <span style="color: #ff0000;">**Note: Network information can be change from the command line, this comes in handy for troubleshooting.**</span>  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-7.jpg?resize=1024%2C772)](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-7.jpg)
12. Review the VM configuration settings, auto power on after deployment is an option here or it can be done manually. If all checks out click Finish.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-8.jpg?resize=1024%2C771)](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-8.jpg)
13. Power On the Identity Appliance if it was not done in the previous step.
14. Verify that FQDN / IP can be resolved successfully on the network.

**<span style="text-decoration: underline;">\*Command Line Network Settings\*</span>**

1. Open the identity appliance console or use SSH (if enabled).
2. Log in in using the credentials supplied during the appliance deployment.
3. Type “<span style="color: #ff0000;">**/opt/vmware/share/vami/vami\_config\_net**</span>”  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-CMD-Line-1.jpg?resize=526%2C51)](https://emadyounis.com/assets/img/2014/10/vCAC-CMD-Line-1.jpg)
4. Verify the current configuration by selecting “option 0”.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-CMD-Line-2.jpg?resize=672%2C193)](https://emadyounis.com/assets/img/2014/10/vCAC-CMD-Line-2.jpg)
5. Correct or add any needed configuration, exit when done using option 1. **<span style="color: #ff0000;">Note: I would recommend restarting the appliance, if any changes were made and verify the FQDN / IP can be resolved successfully.</span>**

<span style="text-decoration: underline;">**Configuring the Identity Appliance**</span>

1. Using a web browser navigate to the Identity Appliance management console “https://FQDN:5480”.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Config-11.jpg?resize=406%2C24)](https://emadyounis.com/assets/img/2014/10/vCAC-Config-11.jpg)
2. Log in as root and the password specified during the deployment (step 11 above).  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Config-2.jpg?resize=826%2C302)](https://emadyounis.com/assets/img/2014/10/vCAC-Config-2.jpg)
3. Select Admin tab –&gt; Time Settings.
4. Select an option (Host time or time server) from the Time Sync Mode menu and save settings.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Config-3.jpg?resize=829%2C390)](https://emadyounis.com/assets/img/2014/10/vCAC-Config-3.jpg)
5. Refresh and verify the time settings are correct.
6. Select System tab –&gt; Time Zone, change the system time zone. Click save settings. <span style="color: #ff0000;">**Note: Verify time settings are correct between the time source and the identity appliance.**</span>  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Config-4.jpg?resize=829%2C290)](https://emadyounis.com/assets/img/2014/10/vCAC-Config-4.jpg)
7. Select SSO tab –&gt; SSO, type the password that will be assigned to the administrator@vsphere.local account. Click Apply and wait patiently.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Config-5.jpg?resize=828%2C310)](https://emadyounis.com/assets/img/2014/10/vCAC-Config-5.jpg)
8. Click the Host Settings tab, verify that the SSO Hostname does not include the SSO port:7444 (This was needed in vCAC 6.0 but is now set by default). Click Apply.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Config-6.jpg?resize=828%2C251)](https://emadyounis.com/assets/img/2014/10/vCAC-Config-6.jpg)
9. Select the SSL tab, you have the option of importing or generating a certificate. Common Name, which is the FQDN of the identity appliance should already be filled in. Enter the name of the organization &amp; organizational unit.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Config-7.jpg?resize=830%2C376)](https://emadyounis.com/assets/img/2014/10/vCAC-Config-7.jpg)
10. Click apply settings.
11. Select the Active Directory tab, enter the name of your Active Directory domain in Domain name as well as the domain administrator account. Click Join domain.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Config-8.jpg?resize=830%2C313)](https://emadyounis.com/assets/img/2014/10/vCAC-Config-8.jpg)
12. Verify that the SSH settings are correct from the Admin tab –&gt; Admin. When SSH service enabled is selected, SSH is enabled for all but the root user. Select or uncheck Administrator SSH login enabled to enable or disable SSH login for the root user.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Config-10.jpg?resize=830%2C342)](https://emadyounis.com/assets/img/2014/10/vCAC-Config-10.jpg)
13. The identity appliance logs can be downloaded from within the appliance by going to the Admin tab –&gt; Logs then click Create support bundle. The log bundle creation process starts and the browser refreshes every 10 seconds until ready to download.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Config-9.jpg?resize=830%2C195)](https://emadyounis.com/assets/img/2014/10/vCAC-Config-9.jpg)

And with that, our identity appliance is ready to go. With the identity appliance ready, we can prepare for the next steps. Stay tuned and we will bring you more information on how to [protect and backup your identity appliance](http://emadyounis.com/vrealize-automation/vrealize-automation-6-1-formally-vcac-identity-appliance-high-availability-backup/ "vRealize Automation 6.1 (formally vCAC) – Identity Appliance High Availability & Backup").
