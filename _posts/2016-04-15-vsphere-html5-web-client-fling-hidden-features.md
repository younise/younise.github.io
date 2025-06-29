---
id: 814
title: 'vSphere HTML5 Web Client Fling Hidden Features'
date: '2016-04-15T15:53:12-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=814'
permalink: /vsphere-html5-web-client-fling-hidden-features/
image: 'https://emadyounis.com/assets/img/2016/04/HTML5-Fling-icon-1.1.png'
categories:
    - vCenter
tags:
    - 'HTML5 Client'
    - 'Web Client'
---

![](https://emadyounis.com/assets/img/2016/04/HTML5-Fling-icon-1.1.png?resize=166%2C166)I recently wrote about the <span style="color: #3366ff;">**[release of the vSphere HTML5 Web Client Fling](http://emadyounis.com/vcenter/new-fling-vsphere-html5-web-client/)**</span> and <span style="color: #3366ff;">**[how to configure](http://blogs.vmware.com/vsphere/2016/03/vsphere-html5-web-client-fling-getting-started.html)** </span>it. Since then there has already been an **<span style="color: #3366ff;">[update to the Fling](https://labs.vmware.com/flings/vsphere-html5-web-client)</span>**. I’ve gone through update process in my lab using the rpm, but I also decided to deploy a new OVA (version 1.1) and found a few hidden features.

During the configuration of the appliance I found a directory called “vspherefeatures”. The directory contains a configuration file with three disabled entries. Of course the first thing that came to mind is what’s the harm in just enabling one to see what happens. What happened was me enabling all three after seeing the first one worked.

The Three hidden features are Datastore File Browser, Add Host Wizard, and Network Selector. I’m assuming that these are features that will be available in future releases of the vSphere HTML5 Web Client Fling. For now looks like we can get a sneak peak 🙂

**Enabling Hidden Features**

<span style="color: #ff0000;">**Note:**</span> This was tested using a new vSphere HTML5 Web Client Fling version 1.1 OVA.

1. SSH into vSphere HTML5 Web Client Fling Appliance.
2. Go to **/etc/vmware/vsphere-client/vsphereFeatures**  
    [![](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-1.png?resize=1000%2C126)](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-1.png)
3. Edit the configuration file **“vsphereFeatures.cfg**” I’m using VI to make changes.  
    [![](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-2.png?resize=1003%2C175)](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-2.png)
4. There are three disabled options 
    - Datastore File Browser
    - Add Host Wizard
    - Network Selector
5. Change the setting from disabled to **enabled** and save the configuration file.  
    [![](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-3.png?resize=1003%2C178)](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-3.png)
6. Restart the web server service /etc/init.d/vsphere-client restart.  
    [![](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-4.png?resize=999%2C146)](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-4.png)
7. Once the web service has been restarted, login the HTML5 Web Client **https://&lt;FQDN or IP of HTML5 Appliance&gt; :9443/ui**

**Features**

The Datastore File Browser feature can be found by going to Storage and selecting a Datastore –&gt; Manage –&gt; Files. Here you can download files, create files, search within a datastore, and delete files or folders.

[![](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-5.png?resize=1271%2C562)](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-5.png)

Add Host Wizard can be found by performing a right click at the Datacenter level or using Datacenter Actions. We now have the ability to add a new ESXi host.

[![](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-6.png?resize=1271%2C403)](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-6.png)

[![](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-7.png?resize=869%2C558)](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-7.png)

Network Selector can be found when editing a VM –&gt; Network Adapter –&gt; Network drop down –&gt; Show more networks. This feature provides the ability to change a virtual machine’s network when editing.

[![](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-8.png?resize=1271%2C704)](https://emadyounis.com/assets/img/2016/04/vSphere-HTML5-Client-Hidden-Features-8.png)

There you have it three hidden features that are not currently available today in the vSphere HTML5 Web Client Fling. I would keep an eye out on this configuration file as new updates are released there may be more hidden features. Now if you’ll excuse me I have some feedback to provide 🙂
