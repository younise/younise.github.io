---
id: 167
title: 'vRealize Automation 6.1 (formally vCAC) &#8211; Identity Appliance High Availability &#038; Backup'
date: '2014-11-13T10:15:00-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=167'
permalink: /vrealize-automation-6-1-formally-vcac-identity-appliance-high-availability-backup/
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

In a [previous post](http://emadyounis.com/vrealize-automation/vrealize-automation-6-1-formally-vcloud-automation-center-identity-appliance-deployment-configuration/ "vRealize Automation 6.1 (formally vCloud Automation Center) – Identity Appliance – Deployment / Configuration"), we discussed the importance of SSO as it relates to vRA (vRealize Automation). We also demonstrated how to deploy &amp; configure the vRA identity appliance. Due to the nature of the identity appliance we need to make sure that SSO is in a high available state. There currently is no native HA feature built in, so what are our options for protecting the identity appliance you ask? Here are some options:

1. The first option is leveraging vSphere HA, which is the recommend method.
2. The use of FT (Fault Torrance) could be another method; the identity appliance is within the one vCPU boundary.
3. Deploy the vSphere windows SSO in an HA pair. Some things to keep in mind: 
    - Certificates – like it, love it, gotta have it!
    - vCenter SSO 5.5 U2, U2a, or U2b are supported with vRA 6.1. Although SSO 5.5 U2b is the recommendation.
    - External load balancer (F5 documented).

How to configure SSO for high availability with vRA 6.1 can be found [here](http://www.vmware.com/files/pdf/products/vCloud/VMW-vRealize-Automation-61-Deployment-Guide-HA-Configurations.pdf).

<span style="text-decoration: underline;">**Backups Save Lives!**</span>

Does having high availability guarantee 99% up time? How often do we design with HA in mind? Yet gremlins, not ones in the movies but the datacenter kind, can cause the unexpected. Better yet are we protected from ourselves, meaning human error? These questions and concerns lead to making sure there is some sort of “plan B”. Backups provide a plan that minimizes your downtime and saves your bacon, yes I said it! SSO is a critical component of any vRA deployment, regular scheduled backups of the should be in place. Especially when changes to configuration, tenants and identity store(s) occur. Here are a few methods to backing up the identity appliance (SSO):

1. vSphere Data Protection – creates a backups of the entire appliance.
2. vSphere Replication – replicate the virtual appliance to another site.
3. Site Recovery Manager – shadow VM copy of the appliance in another datacenter.
4. Cloning / Snapshot – a lot of up keep here, use as temporary solution only.
5. 3rd party backup solutions.

<span style="text-decoration: underline;">**Backup of Identity Appliance SSO database using vdcbackup**</span>

1. Log in identity appliance console.
2. Create a backup directory “**mkdir /backup**“.
3. Run the following [command](https://emadyounis.com/assets/img/2014/11/Identity-Appliance-DB-backup.jpg) “**/usr/lib/vmware-vmdir/bin/vdcbackup <span style="color: #ff0000;">(space)</span> /storage/db/vmware-vmdir /backup/**“.
4. Verify the following files (data.mdb &amp; lock.mdb)are in the backup directory “/backup”.

[![](https://emadyounis.com/assets/img/2014/11/Identity-Appliance-DB-backup-2.jpg?resize=312%2C32)](https://emadyounis.com/assets/img/2014/11/Identity-Appliance-DB-backup-2.jpg)

**<span style="color: #ff0000;">Note:</span> Copy the cert located in “/etc/vmware-sso/keys” , copy files offline using a tool such as WinSCP.**

**![](https://emadyounis.com/assets/img/2014/11/Backup1.jpeg?resize=259%2C194)**

<span style="text-decoration: underline;">**\*Bonus: Some Troubleshooting Tips\***</span>

1. VMware Knowledge base is your best friend, most of the errors related to SSO can be found in a knowledge base search.
2. Know where your logs are located: 
    - Identity appliance -Admin tab –&gt; Logs allows creation of support bundle or see [KB 2074803](http://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2074803).
    - vSphere SSO – %Temp% in windows explorer , see [KB 2033430](http://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2033430).
3. Verify NTP is configured and synced correctly.
4. If possible use certificates from a certificate authority.
5. Use vdcadmintool to unlock and reset SSO administrator password.
6. SSO administrator password in identity appliance has an expiration date (Admin tab –&gt; Administrator settings). Only current option is change password.
7. SSO password in vSphere expires in 90 days by default, password policies can be adjusted.

Thanks to [Kyle Ruddy](https://twitter.com/RuddyVCP) for his input towards SSO troubleshooting. With SSO highly available and backed up, we can now set our sites on deploying the vCAC appliance.
