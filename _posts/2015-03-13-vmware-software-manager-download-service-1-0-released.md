---
id: 426
title: 'VMware Software Manager &#8211; Download Service 1.0 Released'
date: '2015-03-13T11:49:12-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=426'
permalink: /vmware-software-manager-download-service-1-0-released/
image: 'https://emadyounis.com/assets/img/2015/03/Interface-7.jpg'
categories:
    - VMware
tags:
    - 'Software Manager - Download Service'
---

VMware Software Manager – Download Service 1.0 release coincides with the launch of vSphere 6.0. This tool is to assist with the download management of VMware products. It’s an easy button to download all the bits for a particular suite or individual products. The download catalog is currently limited to the following Datacenter and Cloud infrastructure products:

- VMware vCloud Suite 6.0, 5.8, and 5.5
- VMware vSphere w/ Operations Management 6.0 and 5.5
- VMware vSphere 6.0, 5.5, and 5.1

[![Intro](https://emadyounis.com/assets/img/2015/03/Intro.jpg?resize=634%2C575)](https://emadyounis.com/assets/img/2015/03/Intro.jpg)

There is a detection mechanism that is aware when a new suite, product, and version are available. Another benefit is an integrity check that validates all downloads. The Software Manager – Download Service has the following requirements:

<span style="text-decoration: underline;">**Operating Systems**</span>

- Windows 8.1 64-bit
- Windows 7 64-bit SP2
- Windows Server 2012 64-bit R2
- Windows Server 2008 64-bit R2 SP1

<span style="text-decoration: underline;">**Supported Browsers**</span>

- Internet Explorer 10 and 11
- Firefox 34 and 35
- Google Chrome 39 and 40

The installation and setup is simple and requires minimal time. The MSI is close to 17mb in in size and can be found [here](https://my.vmware.com/web/vmware/details?downloadGroup=VSMDS10_GA&productId=486&rPId=7507%200B). The installation also requires a location for the software depot. The minimal requirement for the software depot is 80GB. Along with each product is the download size which will assist in determining the necessary size for your software depot. I would recommend a separate disk that can be allocated to your software depot to be used as a central repository.

<span style="text-decoration: underline;">**Installation**</span>

1. Click Next on the VMware Software Manager – Download Service Setup.  
    [![Install-1](https://emadyounis.com/assets/img/2015/03/Install-1.jpg?resize=501%2C394)](https://emadyounis.com/assets/img/2015/03/Install-1.jpg)
2. Accept the EULA and click Next.  
    [![Install-2](https://emadyounis.com/assets/img/2015/03/Install-2.jpg?resize=508%2C403)](https://emadyounis.com/assets/img/2015/03/Install-2.jpg)
3. Select the install and depot locations. Click Next.  
    <span style="color: #ff0000;">**Note**</span>: Install depot on separate disk to provide flexibility for disk expansion and potential central repository.  
    [![Install-3](https://emadyounis.com/assets/img/2015/03/Install-3.jpg?resize=506%2C407)](https://emadyounis.com/assets/img/2015/03/Install-3.jpg)
4. Click Install.  
    [![Install-4](https://emadyounis.com/assets/img/2015/03/Install-4.jpg?resize=505%2C404)](https://emadyounis.com/assets/img/2015/03/Install-4.jpg)
5. It’s off to the races, install process is fairly quick.  
    [![Install-5](https://emadyounis.com/assets/img/2015/03/Install-5.jpg?resize=505%2C403)](https://emadyounis.com/assets/img/2015/03/Install-5.jpg)  
    **<span style="color: #ff0000;">Note</span>**: Depending on your Windows User Account Control Setting (UAC), you may receive a prompt to allow the MSI to install. Click Yes to proceed.  
    [![Install-5A](https://emadyounis.com/assets/img/2015/03/Install-5A.jpg?resize=463%2C257)](https://emadyounis.com/assets/img/2015/03/Install-5A.jpg)
6. Click Finish.  
    <span style="color: #ff0000;">**Note**</span>: Check box for open download service web application is set by default.  
    [![Install-6](https://emadyounis.com/assets/img/2015/03/Install-6.jpg?resize=508%2C405)](https://emadyounis.com/assets/img/2015/03/Install-6.jpg)

<span style="text-decoration: underline;">**Interface**</span>

1. Log in the Software Manager – Download Service using your My VMware account. Click Connect or press Enter.  
    [![interface-1](https://emadyounis.com/assets/img/2015/03/interface-1.jpg?resize=785%2C426)](https://emadyounis.com/assets/img/2015/03/interface-1.jpg)
2. The first login requires initialization, which takes a minute.  
    [![Interface-2](https://emadyounis.com/assets/img/2015/03/Interface-2.jpg?resize=1023%2C711)](https://emadyounis.com/assets/img/2015/03/Interface-2.jpg)
3. Select category, suite, version, and edition under software. Download will display what is being and has been downloaded, consider it your current status and history.  
    <span style="color: #ff0000;">**Note**</span>: To log out click the Disconnect button in the upper right hand corner.  
    [![Interface-3](https://emadyounis.com/assets/img/2015/03/Interface-3.jpg?resize=1025%2C709)](https://emadyounis.com/assets/img/2015/03/Interface-3.jpg)
4. Each suite or product displays the required size on disk.  
    [![Interface-4](https://emadyounis.com/assets/img/2015/03/Interface-4.jpg?resize=724%2C649)](https://emadyounis.com/assets/img/2015/03/Interface-4.jpg)
5. Each suite can be expanded to display the related products and a download button to for the entire suite. If you would prefer to download an individual product expand the suite and download that product.  
    [![Interface-6](https://emadyounis.com/assets/img/2015/03/Interface-6.jpg?resize=1021%2C468)](https://emadyounis.com/assets/img/2015/03/Interface-6.jpg)
6. The Software Manager – Download Service icon in the windows taskbar has several settings 
    - Show the download location
    - Set the download location
    - Enable debugging information

[![interface-5](https://emadyounis.com/assets/img/2015/03/interface-5.jpg?resize=655%2C137)](https://emadyounis.com/assets/img/2015/03/interface-5.jpg)

<span style="text-decoration: underline;">**Summary**</span>

VMware Software Manager – Download Service is tool that allows downloading VMware products in a simple manner. There are few features I would like to see added, such as support for Mac OS X and Linux operating systems, and notifications for when new versions or downloads are available and have completed. Some other helpful features would be proxy settings for those environments that require it, and download of plugins for those products that have them available. This is a great start for a new 1.0 tool, and I am looking forward to seeing how the product will evolve.
