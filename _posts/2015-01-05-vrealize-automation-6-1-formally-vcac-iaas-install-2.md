---
id: 249
title: 'vRealize Automation 6.1 (formally vCAC) – IaaS Install'
date: '2015-01-05T11:03:37-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=249'
permalink: /vrealize-automation-6-1-formally-vcac-iaas-install-2/
image: 'https://emadyounis.com/assets/img/2015/01/vRealize.jpg'
categories:
    - 'vRealize Automation'
tags:
    - IaaS
    - vCAC
    - 'vCAC 6.1'
    - VMware
    - vRA
    - vRealize
---

IaaS is the component that allows the creation of blueprints and provisioning to endpoints. It also handles machine life cycle, governance, and decommissioning to name a few. IaaS also has some muscle behind it in regards to extensibility. Before getting too far in this discussion, let’s go through the IaaS installation process.

![](https://emadyounis.com/assets/img/2015/01/vCAC-Overview-intro.jpg?resize=750%2C361)

<span style="text-decoration: underline;">**IaaS Server Requirements**</span>

- Windows Server 2008 R2 SP1 / Windows Server 2012 R2
- Microsoft .NET Framework 4.5.1
- PowerShell version 2.0 or 3.0
- Microsoft IIS 7.5
- Java (64-bit) 1.7 or later (32-bit version of Java is not supported)
- vRA service account

<span style="text-decoration: underline;">**IaaS Server Prep**</span>

1. Add the vRA service account to the IaaS server’s local administrators group.
2. Select Local Security Policy under Tools from the Server Manager.
3. Expand Local Policies and select User Rights Assignment.
4. Add the vRA service account to the following policies: 
    - Log on as a batch job
    - Log on as a service
    
    [![](https://emadyounis.com/assets/img/2015/01/Local-Sec.jpg?resize=859%2C514)](https://emadyounis.com/assets/img/2015/01/Local-Sec.jpg)
5. Open Component Services, Start –&gt; run –&gt; dcomcnfg or Start –&gt; Administrative Tools –&gt; Component Services.
6. Expand Component Services –&gt; Computers –&gt; My Computer –&gt; Distributed Transaction Coordinator (MSDTC).
7. Right click the Local DTC and select Properties. Under the security tab check Network DTC Access, Allow Remote Clients, Allow Remote Administration, Allow Inbound and Allow Outbound under Transaction Manager Communication.  
    [![](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-MSDTC-2-Updated.jpg?resize=462%2C503)](https://emadyounis.com/assets/img/2014/12/SQL-IaaS-MSDTC-2-Updated.jpg)
8. Click OK and allow the service to be restarted.
9. Open Services and change the Secondary Logon service to automatic.[![](https://emadyounis.com/assets/img/2015/01/Secondary-Service.jpg?resize=419%2C475)](https://emadyounis.com/assets/img/2015/01/Secondary-Service.jpg)
10. Start the Secondary Logon service.
11. Add the IIS Web Server Role.
12. Add the following Features: 
    - NET Framework 3.5 Feature with Non-HTTP Activation.
    - NET Framework 4.5 Feature &amp; WCF Services.
    - Windows Process Activation Service – All.
    
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Features.jpg?resize=798%2C565)](https://emadyounis.com/assets/img/2015/01/IaaS-Features.jpg)  
    [![](https://emadyounis.com/assets/img/2015/01/Features-2.jpg?resize=798%2C567)](https://emadyounis.com/assets/img/2015/01/Features-2.jpg)
13. Under Role services add the following: 
    - expand Security and select Windows Authentication.
    - expand Application Development and select ASP, ASP.NET 3.5, ASP.NET4.5.
14. Install the selected roles and features.

<span style="text-decoration: underline;">**IIS Configuration**</span>

1. Open IIS Manager.
2. Navigate to the Default Web Site.  
    [![](https://emadyounis.com/assets/img/2015/01/IIS-1.jpg?resize=1008%2C728)](https://emadyounis.com/assets/img/2015/01/IIS-1.jpg)
3. Under IIS select Authentication making the following changes: 
    - Disable Anonymous Authentication.
    - Enable Windows Authentication.
    - Click Advanced Settings, disable Enable Kernel-mode authentication and then click OK.
    - Click Providers, remove Negotiate and NTLM and click OK.
    - Click Advanced Settings, check Enable Kernel-mode authentication and then click OK.
    - Click Providers, add Negotiate and NTLM and click OK.
4. Close IIS Manager.

<span style="text-decoration: underline;">**Java Configuration**</span>

1. Download a 64-bit version of Java 1.7 or later ( using version 7 Update 71 in my lab).
2. Install Java.
3. Go to Control Panel –&gt; System –&gt; Advance system settings –&gt; Advanced Tab –&gt; Environment Variables.
4. Create a new Environment Variable called **JAVA\_HOME**.
5. Variable value is the installation path of the Java bin directory “C:\\Program Files\\Java\\jre7\\”.[![](https://emadyounis.com/assets/img/2015/01/Iaas-Java-2.jpg?resize=394%2C497)](https://emadyounis.com/assets/img/2015/01/Iaas-Java-2.jpg)  
    <span style="color: #ff0000;">**<span style="color: #ff0000;">Note</span>:**</span> variable name must all be in Caps.

**<span style="text-decoration: underline;">Prerequisites</span> Script**

After going through all that you must be thinking, there has to be an easier way to do this. Well there is, thanks to [Brian Graf](https://twitter.com/vbriangraf). His prereq script goes through this process saving time and less room for error. It’s always good to go through the manual process at least once. It helps understand the process and good for troubleshooting.

[![](https://emadyounis.com/assets/img/2015/01/IaaS-Script-1.jpg?resize=838%2C143)](https://emadyounis.com/assets/img/2015/01/IaaS-Script-1.jpg)

[![](https://emadyounis.com/assets/img/2015/01/IaaS-Script-2.jpg?resize=838%2C331)](https://emadyounis.com/assets/img/2015/01/IaaS-Script-2.jpg)

<http://blogs.vmware.com/PowerCLI/2014/09/vcac-6-1-pre-req-automation-script-released.html>

<span style="text-decoration: underline;">**IaaS Install**</span>

1. Navigate to **https://vRA Appliance FQDN:5480/i**
2. Download the IaaS installer  
    <span style="color: #ff0000;">**Note:**</span> Don’t rename the installer file, it is tied to the vRA appliance.
3. Right-click the **IaaS Installer** and run it as Administrator  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Installer.jpg?resize=1005%2C362)](https://emadyounis.com/assets/img/2015/01/IaaS-Installer.jpg)
4. Click Next on the Welcome to the vCloud Automation Center Configuration screen.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Welcome.jpg?resize=799%2C598)](https://emadyounis.com/assets/img/2015/01/IaaS-Welcome.jpg)
5. Accept the EULA and click Next.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-EULA.jpg?resize=799%2C599)](https://emadyounis.com/assets/img/2015/01/IaaS-EULA.jpg)
6. Use the vRA Appliance root account and password and click Next.
7. Select installation type complete since this is a lab setup.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Install-Type.jpg?resize=795%2C600)](https://emadyounis.com/assets/img/2015/01/IaaS-Install-Type.jpg)
8. Verify Prerequisites, click Next.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-PreReq.jpg?resize=799%2C599)](https://emadyounis.com/assets/img/2015/01/IaaS-PreReq.jpg)  
    <span style="color: #ff0000;">**Note:**</span> If any component(s) doesn’t pass PreReq checker, click that item and view instructions to configure correctly. Then click Check Again to verify component(s) have passed.
9. Enter the service account password and passphrase. <span style="color: #000000;">S</span>ince we are already logged in with the service account it has pre-populated the username for us.
10. Enter Microsoft SQL Server Database Install Information, click Next.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Server-and-Account-Settings.jpg?resize=798%2C598)](https://emadyounis.com/assets/img/2015/01/IaaS-Server-and-Account-Settings.jpg)  
    <span style="color: #ff0000;">**Note:**</span> Don’t lose your passphrase, it’s required to provide the encryption key for other IaaS components.
11. Distributed Executions Managers: provide names for the DEM Worker &amp; Orchestrator.
12. Proxy vSphere Agent: name can be changed but make note of it as it is required when adding your endpoint. Click Next.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-DEM-and-Agent.jpg?resize=798%2C599)](https://emadyounis.com/assets/img/2015/01/IaaS-DEM-and-Agent.jpg)
13. Provide the following in the Component Registry: 
    - Click on Load to load the SSO Default tenant.
    - Click on Download to pull in the vRA Automation certificate.
    - Select Accept Certificate.
    - Enter the administrator@vsphere.local account credentials and click Test.
    - Verify the IaaS Server FQDN and click Next.
    
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Component-Registry.jpg?resize=800%2C595)](https://emadyounis.com/assets/img/2015/01/IaaS-Component-Registry.jpg)
14. Verify the components information and click Install.  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Install.jpg?resize=797%2C597)](https://emadyounis.com/assets/img/2015/01/IaaS-Install.jpg)
15. The install process will take some time to complete, good time to take a break!  
    [![](https://emadyounis.com/assets/img/2015/01/IaaS-Complete.jpg?resize=799%2C600)](https://emadyounis.com/assets/img/2015/01/IaaS-Complete.jpg)

<span style="text-decoration: underline;">**Verify IaaS Services &amp; Tenant**</span>

1. Once the IaaS install is complete, verify the following services are running: 
    - VMware DEM-Orchestrator
    - VMware DEM-Worker
    - VMware vCloud Automation Center Agent
    - VMware vCloud Automation Center Service
    
    [![](https://emadyounis.com/assets/img/2015/01/Services.jpg?resize=1024%2C258)](https://emadyounis.com/assets/img/2015/01/Services.jpg)
2. Log in the default tenant https://vRA FQDN/shell-ui-app using administrator@vsphere.local.[![](https://emadyounis.com/assets/img/2015/01/Default-Tenant.jpg?resize=875%2C446)](https://emadyounis.com/assets/img/2015/01/Default-Tenant.jpg)

The IaaS deployment can seeing daunting at times, especially going through it the first time. One piece of advice is to use snapshots during this process. They come in handy if you must revert back to a previous state of the deployment, just don’t forget to clean up when done. Also if you run into any errors during the IaaS install there is a link to open the installer log folder at the bottom of the installer. Once you vRealize the error of your ways, uninstall and re-install or just revert from snapshot.
