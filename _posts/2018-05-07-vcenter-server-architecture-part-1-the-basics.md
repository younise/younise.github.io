---
id: 1042
title: 'vCenter Server Architecture Part 1 &#8211; The Basics'
date: '2018-05-07T14:07:18-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=1042'
permalink: /vcenter-server-architecture-part-1-the-basics/
image: 'https://emadyounis.com/assets/img/2018/05/Ring.png'
categories:
    - vCenter
tags:
    - Architecture
    - 'vCenter Server 6.5'
    - 'vCenter Server 6.7'
---

With the release of [vSphere 6.7](http://emadyounis.com/vcenter/vcenter-server-6-7-whats-new-rundown/) and 6.5 U2 comes architectural changes for vCenter Server. vCenter Server is going back to its roots of a simplified deployment model. vCenter Server used to be a single node deployment. The only debate was about its availability. Over time the architecture of vCenter Server has changed introducing a distributed deployment model. Core services like inventory, web client, SSO, and vCenter could run on separate nodes. vCenter Server had the potential of becoming six nodes in this model when adding the VCDB and VUM. The distributed deployment model became too complex for management, maintenance, and upgrades.

To simplify the distributed model, VMware introduced the Platform Services Controller (PSC). The role of the PSC is to manage authentication, licensing, tags &amp; categories, global permissions, and custom roles. It is also the certificate authority for the vSphere SSO domain. The PSC is a vCenter Server service which manages the components mentioned above. While the PSC is an improvement over the original SSO introduced in 5.1 and 5.5 it also added complexity. So began another debate of external versus embedded deployments. The reality comes down to enhanced linked mode. At the time (6.0 – 6.5 U1) only external deployments supported enhanced linked mode and external as a standalone. Now (6.5 U2 – 6.7) supports enhanced linked mode for embedded deployments. Now we are back to a more simplified deployment model and questions about how to transition.

![](https://emadyounis.com/assets/img/2018/05/vCenter-Server-Deployment-Evolution.png?resize=1240%2C539)  
Figure above represents the evolution of vCenter Server deployments. VMware has recognized the complexities involved in vCenter Server deployments and is putting efforts into getting back to a simpler deployment model allowing administrators can focus more on day-2 operations.

## PSC Basics

Before we actually get into architecture it’s important to understand the basics. This is especially important when it comes to discussing availability. Let’s start with what happens when the PSC service is not available. We cannot authenticate (login) to vCenter Server, doesn’t matter if the PSC is embedded or external. Not only vCenter Server but other VMware products using the PSC for authentication will be affected. Examples are NSX, SRM, and vROPS to name a few. The next time you enter your credentials in the vSphere Client, hit enter, and watch the browser URL. You’ll see the URL change from vCenter Server’s FQDN to the PSC FQDN and back. For external deployments with several PSCs, it means losing a replication partner. This only affects authentication to any vCenter Server registered with the unavailable PSC. Workloads are still running, but the ability to login and centrally manage is not available. Now that you know the impact, think about who does this impact inside your organization. We’ll come back this during the availability discussion in this series.

It’s also important to understand the relationship between PSC and vCenter Server. An embedded PSC is only registered to the vCenter Server on the same node. An external PSC has a one-to-many relationship with vCenter Server.

- vSphere 6.0 – a single external PSC can have up to 10 vCenter Servers registered
- vSphere 6.5 and 6.7 – a single external PSC can have up to 15 vCenter Servers registered
- vSphere 6.0 – supports up to 8 external PSCs within a vSphere SSO domain
- vSphere 6.5 and 6.7 – supports up to 10 external PSCs within a vSphere SSO domain

One of the vSphere [maximums](https://configmax.vmware.com/home) you don’t see is one for sites. A site is nothing more than a logic boundary and a way to group PSCs. You can have as many sites as you want as long as it does not exceed the number of supported PSCs in a vSphere SSO domain. Starting with vSphere 6.7 sites are not required for new deployments. During upgrades and migrations, sites will remain. Adding a new PSC to an upgraded or migrated environment will prompt for site information. External deployments can repoint vCenter Servers to any PSC in a vSphere SSO domain. Also now in vSphere 6.7 across different vSphere SSO domains. Currently, embedded deployments cannot repoint across different vSphere SSO domains. Sites are one less boundary to worry about going forward. The recommendation is to keep the default for new deployments from the GUI or CLI.

![vCenter Server embedded linked mode deployment](https://emadyounis.com/assets/img/2018/05/Embedded-SSO.png?resize=1756%2C435)  
**Example 1**: vSphere 6.7 embedded deployment with enhanced linked mode in a vSphere SSO domain. The PSC is running as a vCenter Server service in this example and everything within the vSphere SSO domain is considered a single default site.

![vCenter Server external linked mode deployment](https://emadyounis.com/assets/img/2018/05/External-SSO.png?resize=1452%2C764)  
**Example 2:** vSphere 6.7 external deployment with enhanced linked mode in a vSphere SSO domain. Everything within the vSphere SSO domain is considered a single default site.

The key thing to remember is the PSC is multi-master. Meaning there is no difference between the first one or the last one in the vSphere SSO domain. The PSCs replicate to each other every 30 seconds; this means if a PSC is unavailable the same data is available on its replication partners. To mitigate data loss, create a replication agreement between the bookends. This is referring to the first and last PSC in a vSphere SSO domain, creating a ring. The ring is creating a second data path in case you lose a data path. I’ve have seen several blog posts refer to the creating the ring as a requirement which is not true. The ring is a recommendation but not a requirement. Outside of the ring, creating more PSC replication agreements can cause unnecessary overhead. Keep it simple!

![](https://emadyounis.com/assets/img/2018/05/Ring.png?resize=1447%2C895)

**Example 3:** An additional replication agreement created using vdcrepadmin from the last PSC to the first PSC (bookends) in the vSphere SSO domain, providing a secondary data path.

## vCenter Server Basics

Most of the architecture discussions revolve around the vSphere SSO domain. There are some core concepts and considerations for vCenter Server as well. I pose the same question, but now what happens when the PSC is available but vCenter Server is not? The impact is you’ve lost management visibility and can not make any changes. Existing workloads are not affected by the outage. There are other implications to a vCenter Server outage. Some solutions communicating to vCenter Server as an endpoint will also lose access. For example, vROPS will not be able to collect metrics. Your backup window may also be affected. There are other products are resilient and not dependent on vCenter Server being available. Take vSAN for example, it is not dependent on vCenter and will continue to function. The VMware ecosystem has grown over the years and it’s important to know the impact of each solution within your environment when vCenter Server is not available.

We discussed the relationship between PSC to vCenter Server as one-to-many, but vCenter Server to PSC is 1:1. A vCenter Server can only be registered to a single PSC at a time.

- vSphere 6.0 – supports 10 external vCenter Server deployments in enhanced linked mode
- vSphere 6.5 &amp; 6.7 – supports 15 external vCenter Server deployments in enhanced linked mode
- vSphere 6.7 – supports 15 embedded deployments in enhanced linked mode
- vSphere 6.5 U2 – supports 15 embedded deployments in enhanced linked mode

In regards to physical location placement, VMware currently does not have any documented latency numbers. The recommendation is up to 100ms in a multisite configuration. This is the same for the new embedded with enhanced linked mode deployment. For external deployments at least one local PSC per site. Remember communication is continuous between vCenter Server and PSC. Stay tuned as we are trying to get official latency numbers published.

vCenter Server [licensing](https://www.vmware.com/products/vsphere.html) comes up quite a bit. A standard vCenter Server license includes:

- All PSCs when in an external deployment.
- Native vCenter Server high availability (VCHA)
- File-Based Backup / Restore
- Other advanced vCenter Server features

vCenter Server also has a foundation version. It has limitations on available features and functionality compared to the standard version.

There are many questions about the new vCenter Server architecture 6.7, but I felt it was important to go basics. Over the next few posts, I’ll go into more details on both the embedded and external deployment models.
