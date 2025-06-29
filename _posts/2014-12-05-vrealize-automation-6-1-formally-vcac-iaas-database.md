---
id: 206
title: 'vRealize Automation 6.1 (formally vCAC) – IaaS Database'
date: '2014-12-05T10:00:35-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=206'
permalink: /vrealize-automation-6-1-formally-vcac-iaas-database/
image: 'https://emadyounis.com/assets/img/2014/11/vcac-logo.jpg'
categories:
    - 'vRealize Automation'
tags:
    - IaaS
    - 'MS SQL'
    - vCAC
    - 'vCAC 6.1'
    - VMware
    - vRealize
---

Up to this point we had the convenience of deploying appliances for both [SSO](http://emadyounis.com/vrealize-automation/vrealize-automation-6-1-formally-vcloud-automation-center-identity-appliance-deployment-configuration/ "vRealize Automation 6.1 (formally vCloud Automation Center) – Identity Appliance – Deployment / Configuration") &amp; [vRA](http://emadyounis.com/vrealize-automation/vrealize-automation-6-1-formally-vcac-vra-appliance-deployment-configuration/ "vRealize Automation 6.1 (formally vCAC) – vRA Appliance Deployment & Configuration"). The deployment of the Infrastructure as a Service (IaaS) requires Microsoft Windows &amp; SQL. IaaS uses Microsoft SQL to maintain information about managed virtual machines and policies. Microsoft SQL can either be a remote server deployment or local with IaaS. So before we can setup IaaS we need to have SQL installed, let’s get started.

<span style="text-decoration: underline;">**Requirements**</span>

Windows Server 2008 R2 / Server 2012:

- DNS (forward / reverse) entry.
- TCP / IP protocol enabled.
- Microsoft Distributed Transaction Coordinator Service (MSDTC) enabled.
- No windows Firewall between Database server (if deployed separately) &amp; IaaS Server
- Microsoft SQL 2012 or 2014 / SQL Express (SQL Server Browser service must be running).
- SQL Service Account.

<span style="text-decoration: underline;">**Required Ports**</span>

The vRA [Install &amp; Configure](http://pubs.vmware.com/vCAC-61/topic/com.vmware.ICbase/PDF/vcloud-automation-center-61-installation-and-configuration.pdf) document provides the required ports for SQL / IaaS.

<span style="text-decoration: underline;">**Database Options**</span>

The following are a few ways to consider for your SQL deployment:

1. Deploy a remote SQL server with a blank database, allowing IaaS installer to do the configuration.
2. Deploy a remote SQL server and using the database installation scripts for configuration.
3. Install SQL local on IaaS server and use either of the above configuration methods.
4. Install SQL local on IaaS server allowing IaaS installer to configure.

<span style="color: #ff0000;">**Note: Things to keep in mind for the Database options:**</span>

1. Deployment method: minimal or distributed.
2. Database high availability.
3. Backup of Database.

<span style="text-decoration: underline;">**Database Setup**</span>

1. Launch the SQL Installer from within your windows machine.
2. Select installation method of Stand-alone or Cluster. Since this is a lab deployment, I am using a stand-alone installation.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-1.jpg?resize=796%2C290)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-1.jpg)
3. SQL System Configuration Checker will determine if there are or will be issues with the install. If there are any issues, correct before proceeding otherwise click OK.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-2.jpg?resize=817%2C467)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-2.jpg)
4. Enter SQL product key or select a free edition such Express, click Next.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-3.jpg?resize=816%2C381)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-3.jpg)
5. Accept the EULA.
6. Product Updates can either be installed now or later, for this deployment I have decided to skip the updates for a later time.
7. SQL Setup Support Rules will verify all prerequisites are met.  
    **<span style="color: #ff0000;">Note: Make Windows Firewall is either disabled or allows the required ports. Also if working in an isolated environment ignore messages about internet access.</span>**
8. Select SQL Server Feature Installation.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-4.jpg?resize=816%2C355)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-4.jpg)
9. Select the standard features to install, I selected the following to get started as well as kept the default install directories for my lab environment. 
    - Database Engine Services
    - Client Tools Connectivity
    - Integration Services
    - Management Tools
    
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-5.jpg?resize=818%2C725)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-5.jpg)  
    <span style="color: #ff0000;">**Note: There may be other features that make sense in your database environment.**</span>
10. Verify all and click Next on Installation Rules.
11. Keep or change the SQL instance name and root directory, click Next.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-6.jpg?resize=818%2C439)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-6.jpg)
12. Disk Space Requirements validates enough space is available for the SQL install. Click Next.
13. Set the services on the Server Configuration to automatic. I have a SQL service account that will be used for the services.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-7.jpg?resize=819%2C423)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-7.jpg)
14. Use either Windows or Mix Mode Authentication (can be changed after the setup). Things to keep in mind here: 
    - If the IaaS installer is used to create the database with Windows authentication, the installer credentials need the sysadmin role.
    - If the IaaS installer is used to create the database not using Windows authentication, then SQL credentials with sysadmin role are required.
    - If the IaaS installer is used to populate a pre-created database require either windows or SQL credentials with db\_owner privileges.
    
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-8.jpg?resize=817%2C724)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-8.jpg)
15. Click Next on error reporting, unless you really want to send information to Microsoft.
16. Start the SQL install and wait for the magic to happen!
17. Rejoice to the successful completion of the SQL install.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-9.jpg?resize=816%2C116)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-9.jpg)

<span style="text-decoration: underline;">**Database Settings -MSDTC**</span>

1. Open Component Services, Start –&gt; run –&gt; dcomcnfg or Start –&gt; Administrative Tools –&gt; Component Services.
2. Expand Component Services –&gt; Computers –&gt; My Computer –&gt; Distributed Transaction Coordinator (MSDTC).  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-MSDTC-1.jpg?resize=862%2C278)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-MSDTC-1.jpg)
3. Right click the Local DTC and select Properties.
4. Under the security tab check Network DTC Access, Allow Remote Clients and Allow Remote Administration.  
    **<span style="color: #ff0000;">Updated</span> :** also check Allow Inbound and Allow Outbound under Transaction Manager Communication.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-MSDTC-2-Updated.jpg?resize=462%2C503)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-MSDTC-2-Updated.jpg)
5. Click OK and allow the service to be restarted.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-MSDTC-3.jpg?resize=476%2C198)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-MSDTC-3.jpg)

<span style="text-decoration: underline;">**Database Settings – TCP/IP**</span>

1. Open SQL Server Configuration Manager.
2. Expand SQL Server network Configuration and click Protocols for MSSQLSERVER.
3. Verify TCP/IP is enabled.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-TCP-IP.jpg?resize=792%2C226)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-TCP-IP.jpg)

<span style="text-decoration: underline;">**Database** </span>**<span style="text-decoration: underline;">Credentials</span>**

Make sure to assign the account that will be creating the IaaS database the correct privileges. Sysadmin role unless the database is already pre-created then db\_owner privileges will be needed.

[![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-Creds.jpg?resize=703%2C630)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-Creds.jpg)

[![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-Creds-2.jpg?resize=703%2C629)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-Creds-2.jpg)

Now that the SQL database is prepared, we can move forward with the IaaS install.
