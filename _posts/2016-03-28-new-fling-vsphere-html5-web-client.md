---
id: 787
title: 'New Fling: vSphere HTML5 Web Client'
date: '2016-03-28T10:41:02-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=787'
permalink: /new-fling-vsphere-html5-web-client/
image: 'https://emadyounis.com/assets/img/2016/03/HTML5-Fling-icon.png'
categories:
    - vCenter
tags:
    - 'HTML5 Client'
    - 'Web Client'
---

![](https://emadyounis.com/assets/img/2016/03/HTML5-Fling-icon.png?resize=166%2C166)VMware first introduced the vSphere Web Client with the release of vSphere 5.0. The vSphere Web Client would be the future management tool replacing the vSphere client aka “Legacy, Thick or C# Client”. This would be become more evident over time with each release of vSphere. New features could only be managed through the vSphere Web Client. While the concept of a browser based client is a step in the right direction, the dependency of flash was not. Keep in mind the decision to use flash was made when it was the standard for web applications, times and standards have changed. With that change it has taken some time to work on replatforming the vSphere Web Client. This always leads to the question, when will we see a HTML5 version of the Web Client? I’m happy to announce the wait is over!

The first step towards making a HTML5 Web Client a reality is the <span style="color: #0000ff;">**[vSphere HTML5 Web Client Fling](https://labs.vmware.com/flings/vsphere-html5-web-client)**</span>. This release of the Fling will focus primary on VM management, with more updates coming. Here is a list of the features and operations available in this first release:

- VM power operations
- VM Edit Settings (simple CPU, Memory, Disk changes)
- VM Console
- VM and Host Summary pages
- VM Migration (only to a Host)
- Clone to Template/ VM
- Create VM on a Host (limited)
- Additional monitoring views: Performance charts, Tasks, Event
- Global Views: Recent tasks, Alarms (view only)
- Integrated Feedback Tool

**HTMl5 Client**

The vSphere HTML5 Web Client Fling is a standalone appliance that can be deployed in your existing or new vSphere 6.0 environments. It supports both the vCenter Server Appliance or vCenter for Windows. The Fling does not make any changes to your existing vCenter or Platform Service Controller components. Nor does it affect any operations, such as the use of the current vSphere Web Client, as it is meant to run side by side.

[![](https://emadyounis.com/assets/img/2016/03/HTML5-Client.png?resize=1365%2C837)](https://emadyounis.com/assets/img/2016/03/HTML5-Client.png)

<span style="color: #ff0000;">**Update 3/30/16**</span> – how to install and configure the vSphere HTML5 Web Client Fling for the vCenter Server Appliance and vCenter for Windows can be found in a blog post I wrote on the [VMware vSphere blog](http://blogs.vmware.com/vsphere/2016/03/vsphere-html5-web-client-fling-getting-started.html).

One of the most import features in my opinion is the feedback tool. Located in the upper right hand corner, Smiley as I like to call him is your chance to provide valuable input back to the engineering team and help shape the future of the product. If you would like to be kept up to date with the latest vSphere HTML5 Web Client Fling news fill out the the following [form](https://docs.google.com/forms/d/1d9OGrJk_u-w-QXdJPRMoX14hxNBTfW4RXmNDzsso1cA/viewform?c=0&w=1&usp=send_form) and stay tuned for more to come.

Take the vSphere HTML5 Web Client Fling for a test drive and most importantly provide feedback. It took six months or so for the Embedded Host Client (EHC) Fling to be a feature rich product now included in vSphere 6.0 U2. The same could happen with the vSphere HTML5 Web Client Fling, this is your client, help shape its future 🙂
