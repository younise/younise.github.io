---
id: 182
title: 'vRealize Automation 6.1 (formally vCAC) – vRA Appliance Deployment &#038; Configuration'
date: '2014-11-20T17:43:28-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=182'
permalink: /vrealize-automation-6-1-formally-vcac-vra-appliance-deployment-configuration/
image: 'https://emadyounis.com/assets/img/2014/11/vcac-logo.jpg'
categories:
    - 'vRealize Automation'
tags:
    - vCAC
    - 'vCAC 6.1'
    - VMware
    - 'vRA Appliance'
    - vRealize
---

At this point we have deployed &amp; configured SSO, of course with HA and backups in mind. The next piece is the vRealize Automation appliance (vRA). The vRA appliance provides cloud management &amp; a self-service portal for vRA. We are now going walk through the deployment and configuration of the appliance.

<span style="text-decoration: underline;">**Prerequisites**</span>

- DNS (forward / reverse) entry.
- Network Time Protocol (NTP).
- 2 vCPU / 8 GB of RAM / 30 GB of storage.
- Administrator@vsphere.local account (SSO host).

<span style="text-decoration: underline;">**Database**</span>

The vRA appliance uses Postgres, but you have options in its placement:

**Option 1:** Use the initial vPostgres that is created on the first vRA appliance.  
**Option 2:** Install Postgres on a separate server.  
**Option 3:** Install Postgres on several servers to establish HA.  
**Option 4:** Configure vRA Appliance as a standalone Postgres database.

To find out more about the supported databases for the vRA appliance, check out the [vCloud Automation Center Support Matrix](https://www.vmware.com/pdf/vcloud-automation-center-61-support-matrix.pdf).

<span style="text-decoration: underline;">**Ports Required for vRA Appliance**</span>

The vRA [Install &amp; Configure](http://pubs.vmware.com/vCAC-61/topic/com.vmware.ICbase/PDF/vcloud-automation-center-61-installation-and-configuration.pdf) document provides the required ports for the vRA appliance.

**<span style="text-decoration: underline;">Deploying the vRA Appliance</span>**

<span style="color: #ff0000;">**Note : deployment of OVA done from the vSphere Web Client.**</span>

1. Log in the vSphere Web Client with an account that has administrator rights.
2. Select Hosts and Clusters.
3. Select an inventory object such datacenter, cluster, host or resource pool.
4. Right Click –&gt; Deploy OVF Template or use Actions –&gt; Deploy OVF Template.
5. Select Source location by browsing to the Identity Appliance file with either an extension of “.OVA “or “.OVF “and click Open. Click Next.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-1.jpg?resize=958%2C330)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-1.jpg)
6. Click Next on the Review details screen.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-2.jpg?resize=960%2C409)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-2.jpg)
7. Accept the EULA and click Next.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-3.jpg?resize=960%2C562)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-3.jpg)
8. Provide the identity appliance VM a name (FQDN) and location for deployment such as a datacenter or folder. Click Next.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-4.jpg?resize=959%2C562)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-4.jpg)
9. Select the disk format, VM storage policy (if configured) and datastore for deployment. Click Next.  
    <span style="color: #ff0000;">**Note: Before you click next verify the disk format is the one you selected, I have seen it change (refresh) after selecting the datastore in the vSphere Web Client.**</span>  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-5.jpg?resize=960%2C562)](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-5.jpg)
10. Setup Network by providing the destination (portgroup) and IP protocol, click Next.  
    [![](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-6.jpg?resize=961%2C562)](https://emadyounis.com/assets/img/2014/10/vCAC-Deploy-6.jpg)
11. Customize template by providing the properties values under the Application &amp; Network Properties sections. Click Next. **<span style="color: #ff0000;">  
    Note: Network information can be change from the command line, this comes in handy for troubleshooting</span>**.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-7.jpg?resize=1024%2C671)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-7.jpg)
12. Review the VM configuration settings, auto power on after deployment is an option here or it can be done manually. If all checks out click Finish.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-8.jpg?resize=1024%2C672)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-8.jpg)
13. Power On the Identity Appliance if it was not done in the previous step.
14. Verify that FQDN / IP can be resolved successfully on the network.

**<span style="text-decoration: underline;">\*Command Line Network Settings\*</span>**

To verify, change or troubleshoot any network settings prior to vRA appliance configuration do the following

1. Open the vRA appliance console or use SSH (if enabled).
2. Log in in using the credentials supplied during the appliance deployment.
3. Type “<span style="color: #ff0000;">**/opt/vmware/share/vami/vami\_config\_net**</span>“
4. Verify the current configuration by selecting “option 0”.
5. Correct or add any needed configuration, exit when done using option 1.

**<span style="color: #ff0000;">Note: I recommend restarting the appliance, if any changes were made and verify the FQDN / IP can be resolved successfully.</span>**

<span style="text-decoration: underline;">**Configuring the vRA Appliance**</span>

1. Using a web browser navigate to the vRA Appliance management console “https://FQDN:5480”.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-9.jpg?resize=523%2C27)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-9.jpg)
2. Log in as root and the password specified during the deployment (step 11 above).  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-10.jpg?resize=830%2C228)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-10.jpg)
3. Select **Admin** tab –&gt; **Time Settings**.
4. Select an option (Host time or time server) from the Time Sync Mode menu and save settings.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-11.jpg?resize=830%2C369)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-11.jpg)
5. Refresh and verify the time settings are correct.
6. Select **System** tab –&gt; **Time Zone**, change the system time zone. Click save settings. <span style="color: #ff0000;"> **Note: Verify time settings are correct between the time source and the vRA appliance**</span>.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-12.jpg?resize=830%2C262)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-12.jpg)
7. Select **vCAC Settings** tab –&gt; **Host Setting**, click Resolve Host Name which will display the current name of the appliance. If correct, click Save Settings. <span style="color: #ff0000;"> **Note: DNS is key here, if vRA appliance can not resolve, check both forward and reverse DNS settings. Also if using a load balancer enter the FQDN of the load balancer for vCAC Host Name.**</span>  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-13.jpg?resize=829%2C287)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-13.jpg)
8. Click **SSL** and select the certificate type from the drop down menu and fill in the required fields: 
    - Common Name = the FQDN of the VMware vCAC Server for the Common Name.
    - Organization = Company
    - Organizational Unit = Department / Location
    - Two Letter Country Code
9. Click Replace Certificate.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-14.jpg?resize=831%2C368)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-14.jpg)
10. Click **SSO** and fill in the required fields: 
    - SSO host name and port = FQDN of SSO host:7444
    - SSO Default Tenant = vsphere.local (already added)
    - SSO Admin = administrator@vsphere.local
    - SSO Admin password = Password set in SSO host for Admin
11. Click Save Settings and wait for SSO the update SSO status.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-16.jpg?resize=828%2C338)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-16.jpg)
12. Verify the certificate.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-15.jpg?resize=358%2C251)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-15.jpg)
13. Click **Licensing**, enter a valid license key and click Submit.
14. Click **Database,** if planning on using external Postgres database.
15. Confirm all services have started from the **Services** tab.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-17.jpg?resize=830%2C755)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-17.jpg)  
    <span style="color: #ff0000;"> **Note: To monitor service startup run the following command: tail -f /var/log/vcac/catalina.out**</span>
16. Validate access to the vRA console. 
    - Open a browser and enter the https://FQDN of vRA appliance/vcac
    - Accept the vRA certificate &amp; SSO certificate
    - Log in using administrator@vsphere.local account
    
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-18.jpg?resize=872%2C501)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-18.jpg)
17. Confirm the default vsphere.local tenant is present.  
    [![](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-19.jpg?resize=1024%2C354)](https://emadyounis.com/assets/img/2014/11/vRA-Appliance-19.jpg)

There you have it we now have SSO &amp; vRA appliances setup, next up IaaS. One honorable mention for the vRA appliance is that is does have built HA, which will be covered in a future post.
