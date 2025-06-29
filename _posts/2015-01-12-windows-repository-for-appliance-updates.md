---
id: 253
title: 'Windows Repository for Offline Appliance Updates'
date: '2015-01-12T11:05:24-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=253'
permalink: /windows-repository-for-appliance-updates/
categories:
    - VMware
tags:
    - Appliance
    - Upgrade
    - VMware
    - vSphere
    - Windows
---

To take advantage of the new features that vRA 6.2 has to offer, it’s time to upgrade. The upgrade will make it easier to transition from the vCAC branding to vRA. This was also a good time to start thinking about how to go about the process.

<span style="text-decoration: underline;">**Upgrade Choices**</span>

- Download Updates directly from VMware – requires internet access.
- Internal repository (offline bundle) – restricted internet access.
- Update with CD-ROM – restricted internet access / requires enabled CD-ROM.

The easiest way to update is from the VMware repository using the internet. I had to consider my past experiences working in silo environments. Then I had to think about my current role as a consultant and my customers. With these experiences in mind it made more sense to explore other options. Update with CD-ROM is the second easiest but I had consider who has access to what. The person applying the updates may not be the same person who has access to edit the VMs. We also have a tendency of downloading the same ISO several times when we can’t find it. The worst is having several copies of the same ISO scattered in different directories on the network. It made more sense to create a centralized repository for updates. The repository would provide more control and management over which updates are available. This would be a guaranteed method for servers with limited or no internet access to receive updates. Finally, if the server has repository access, it eliminates worrying over user VM permissions.

This post will walk through the configuration of a repository on a Windows Server. Why Windows? Well I already have a Windows Server 2012 R2 server available for management. Also I couldn’t find documentation on setting one up, fair enough? 🙂

**<span style="text-decoration: underline;">Windows Server 2012 R2</span>**

1. Add the Web Server (IIS) server role.  
    [![](https://emadyounis.com/assets/img/2015/01/Windows-Server-1.jpg?resize=360%2C144)](https://emadyounis.com/assets/img/2015/01/Windows-Server-1.jpg)
2. Add the prompted features required for the Web Server (IIS).  
    [![](https://emadyounis.com/assets/img/2015/01/Windows-Server-2.jpg?resize=430%2C428)](https://emadyounis.com/assets/img/2015/01/Windows-Server-2.jpg)
3. Take the defaults on Features,Web Server Role (IIS), Role Services and click Next.
4. Confirm all the setting you selected are correct and click Install.  
    [![](https://emadyounis.com/assets/img/2015/01/Windows-Server-3.jpg?resize=682%2C340)](https://emadyounis.com/assets/img/2015/01/Windows-Server-3.jpg)
5. Create a directory for your repository.  
    <span style="color: #ff0000;">**Note:**</span> You can always go back and add other features, if needed.

<span style="text-decoration: underline;">**Setting up IIS**</span>

1. Open IIS Manager.
2. In the Connections Pane expand the server.
3. Expand Sites and right click Default Web Site, select Add Virtual Directory.  
    <span style="color: #ff0000;">**Note:**</span> Here I’m using the Default Web Site, but a new one can be created if you choose.  
    [![](https://emadyounis.com/assets/img/2015/01/IIS-11.jpg?resize=1022%2C512)](https://emadyounis.com/assets/img/2015/01/IIS-11.jpg)
4. Add an Alias and Physical path.  
    [![](https://emadyounis.com/assets/img/2015/01/IIS-2.jpg?resize=1023%2C498)](https://emadyounis.com/assets/img/2015/01/IIS-2.jpg)
5. Click Test Settings, make sure at least you pass authentication, click Close and OK.
6. Double click on Directory Browsing, set the action (right side) to enable.  
    [![](https://emadyounis.com/assets/img/2015/01/IIS-7.jpg?resize=812%2C250)](https://emadyounis.com/assets/img/2015/01/IIS-7.jpg)
7. Open a web browser and verify access to your virtual directory.  
    [![](https://emadyounis.com/assets/img/2015/01/IIS-3.jpg?resize=300%2C77)](https://emadyounis.com/assets/img/2015/01/IIS-3.jpg)
8. Go to Default Web Site and select MIME Types.
9. Add the following MIME type **.sig** with a **file/download** type (file located under the manifest directory in updates).  
    [![](https://emadyounis.com/assets/img/2015/01/IIS-4.jpg?resize=364%2C211)](https://emadyounis.com/assets/img/2015/01/IIS-4.jpg)  
    [![](https://emadyounis.com/assets/img/2015/01/IIS-5.jpg?resize=300%2C72)](https://emadyounis.com/assets/img/2015/01/IIS-5.jpg)
10. The following command is needed since the update will stop due to the + characters in some of the update files (security risk). More info can be found [here](http://blogs.iis.net/thomad/archive/2007/12/17/iis7-rejecting-urls-containing.aspx).  
    [![](https://emadyounis.com/assets/img/2015/01/IIS-8.jpg?resize=639%2C120)](https://emadyounis.com/assets/img/2015/01/IIS-8.jpg)  
    <span style="color: #0000ff;">%windir%\\system32\\inetsrv\\appcmd set config “Default Web Site” -section:system.webServer /security/requestfiltering -allowDoubleEscaping:true</span>  
    [![](https://emadyounis.com/assets/img/2015/01/IIS-6.jpg?resize=1024%2C47)](https://emadyounis.com/assets/img/2015/01/IIS-6.jpg)

<span style="text-decoration: underline;">**Appliance Setup**</span>

<span style="color: #ff0000;">**Note:**</span> I will be using the vRA appliance to test.

1. Using a web browser navigate to the vRA Appliance management console “https://vRA FQDN:5480” and log in.
2. Go to the Update tab –&gt; Settings.
3. Select the use Specified Repository radio button.
4. Repository URL will be http://webserver FQDN/virtual directory name/path to update.  
    [![](https://emadyounis.com/assets/img/2015/01/appliance-1.jpg?resize=828%2C473)](https://emadyounis.com/assets/img/2015/01/appliance-1.jpg)  
    <span style="color: #ff0000;">**Note:**</span> I created a directory for each appliance update
5. Save Settings.
6. Go to Status –&gt; Check Updates  
    [![](https://emadyounis.com/assets/img/2015/01/appliance-2.jpg?resize=830%2C410)](https://emadyounis.com/assets/img/2015/01/appliance-2.jpg)
7. Click Install Updates.  
    **<span style="color: #ff0000;">Note:</span>** Update will take a while, which means time for a break!
8. Once the update is complete, reboot the appliance.
9. Validate new version.  
    [![](https://emadyounis.com/assets/img/2015/01/appliance-3.jpg?resize=830%2C182)](https://emadyounis.com/assets/img/2015/01/appliance-3.jpg)

I didn’t focus much on security since this was in my lab. Things to consider are permissions on your directories and adding Secure HTTP. Check the documentation of the appliance(s) to verify supported update methods.
