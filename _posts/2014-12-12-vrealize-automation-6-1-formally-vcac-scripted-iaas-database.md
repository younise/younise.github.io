---
id: 226
title: 'vRealize Automation 6.1 (formally vCAC) – Scripted IaaS Database'
date: '2014-12-12T11:00:59-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=226'
permalink: /vrealize-automation-6-1-formally-vcac-scripted-iaas-database/
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

Recently we discussed [how to setup the IaaS database](http://emadyounis.com/vrealize-automation/vrealize-automation-6-1-formally-vcac-iaas-database/ "vRealize Automation 6.1 (formally vCAC) – IaaS Database"). Did you know there are scripts available to do the IaaS database install? There is a catch to using the scripts to pre-create the IaaS database, anyone? anyone? A pre-created IaaS database will only work with the custom install (distributed) for the IaaS Server. This supports the two scripted methods, but more on that later.

[![](https://emadyounis.com/assets/img/2014/12/Custom-Install.jpg?resize=780%2C382)](https://emadyounis.com/assets/img/2014/12/Custom-Install.jpg)

The complete (simple) install will not allow the IaaS installer to continue if the database already exists. The only options are deleting the existing database or creating a new one. This setup may work in a lab environment and maybe if you work in a small shop. Let’s face it a good database administrator (DBA) on staff is not going to hand over sysadmin rights. The scripts help by allowing the DBA to assess the database deployment. Also allowing the DBA to pre-create the database, removes the need for sysadmin rights in most cases.

[![](https://emadyounis.com/assets/img/2014/12/complete-install.jpg?resize=489%2C170)](https://emadyounis.com/assets/img/2014/12/complete-install.jpg)

The two scripted methods for the IaaS database are manual and empty. Manual script uses a batch file to create the database, tables and schema. Did I mention the manual scripted install requires sysadmin rights? Empty script creates an empty database, but relies on the IaaS installer to populate. Here are the steps when using either scripted method.

<span style="text-decoration: underline;">**Scripts Location**</span>

Typically the SQL scripts can be found in the install guide, but that not the case with vRealize Automation. There are two ways to download the database installation scripts, both requiring the [vRA appliance](http://emadyounis.com/vrealize-automation/vrealize-automation-6-1-formally-vcac-vra-appliance-deployment-configuration/ "vRealize Automation 6.1 (formally vCAC) – vRA Appliance Deployment & Configuration"):

1. The **IaaS** **Install tab** from the vRA appliance directly.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-Script-1.jpg?resize=829%2C484)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-Script-1.jpg)
2. Navigating to **https://vRA Appliance FQDN:5480/i**  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-Script-2.jpg?resize=1022%2C467)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-Script-2.jpg)

<span style="text-decoration: underline;">**Manual IaaS Database Scripted Method**</span>

<span style="color: #ff0000;">**Before starting: Java 1.7 or higher needs to be installed &amp; environment variable for JAVA\_HOME must be created.**</span>

1. Download the database installation scripts (DBInstall.zip).
2. Navigate to the directory where you downloaded the installation scripts zip file.
3. Extract the DBInstall.zip.
4. Log in to the SQL server with sufficient rights to create and drop databases, sysadmin privileges are needed within the SQL server instance.
5. Open a Windows command prompt with administrator rights.
6. Navigate to DBInstall directory.
7. Run the following command making the necessary variable changes (variable definitions below). Database Name only requires instance if changed from the default MSSQLSERVER  
    <span style="color: #ff0000;">**Note: Brackets around the database log directory “\[log dir\]” in the original script are not needed.** </span>

BuildDB.bat /p:DBServer=db\_server; DBName=db\_name;DBDir=db\_dir; LogDir=log\_dir;ServiceUser=service\_user; ReportLogin=web\_user

[![](https://emadyounis.com/assets/img/2014/12/Database-Values.jpg?resize=762%2C360)](https://emadyounis.com/assets/img/2014/12/Database-Values.jpg)

**Initial Setup**  
[![](https://emadyounis.com/assets/img/2014/12/manual-1.jpg?resize=1024%2C463)](https://emadyounis.com/assets/img/2014/12/manual-1.jpg)

**Results**  
[![](https://emadyounis.com/assets/img/2014/12/manual-2.jpg?resize=730%2C185)](https://emadyounis.com/assets/img/2014/12/manual-2.jpg)

<span style="text-decoration: underline;">**Empty IaaS Database Scripted Method**</span>

1. Download the database installation scripts (DBInstall.zip).
2. Navigate to the directory where you downloaded the installation scripts zip file.
3. Extract the DBInstall.zip.
4. Log in to the SQL server with sufficient rights to create and drop databases, sysadmin privileges are needed within the SQL server instance.
5. Locate the **CreateDatabase.sql** file within the extracted DBInstall directory.
6. Edit the file using a text editor or SQL Server Management Studio and make the following edits:
7. Comment out the On Error exit, this is not a SQL command  
    [![](https://emadyounis.com/assets/img/2014/12/Empty-DB-1.jpg?resize=563%2C193)](https://emadyounis.com/assets/img/2014/12/Empty-DB-1.jpg)
8. Replace all the variables with your values ( DB Name, SQL Data Directory, SQL Log Directory)  
    [![](https://emadyounis.com/assets/img/2014/12/Empty-DB-4.jpg?resize=330%2C152)](https://emadyounis.com/assets/img/2014/12/Empty-DB-4.jpg)  
    [![](https://emadyounis.com/assets/img/2014/12/Empty-DB-2.jpg?resize=655%2C244)](https://emadyounis.com/assets/img/2014/12/Empty-DB-2.jpg)
9. Save the file (if using another editor) and open in SQL Server Management Studio.
10. Click Execute.

 [![](https://emadyounis.com/assets/img/2014/12/Empty-DB-5.jpg?resize=1019%2C485)](https://emadyounis.com/assets/img/2014/12/Empty-DB-5.jpg)
