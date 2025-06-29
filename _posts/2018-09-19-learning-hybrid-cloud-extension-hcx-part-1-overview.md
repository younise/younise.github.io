---
id: 1101
title: 'Learning Hybrid Cloud Extension (HCX) Part 1: Overview'
date: '2018-09-19T15:02:04-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=1101'
permalink: /learning-hybrid-cloud-extension-hcx-part-1-overview/
image: 'https://emadyounis.com/assets/img/2018/09/HCX-Blog-Feature-IMG.png'
categories:
    - 'Hybrid Cloud Extension'
tags:
    - HCX
    - 'Hybrid Cloud Extension'
    - NSX
    - VMC
    - 'VMware Cloud on AWS'
---

[Hybrid Cloud Extension (HCX)](https://hcx.vmware.com/#/home) is an all in one solution for workload mobility. Customers can freely move workloads between multiple on-premises environments as well as VMware Cloud on AWS (VMC). During the [VMworld US 2018 day 1 keynote](https://www.youtube.com/watch?v=mjYP2IuZK6k) there were several demos shown, one included a data center evacuation using HCX (51:57). The data center evacuation was using a new HCX migration type called Cloud Motion with vSphere Replication. I’ll cover the HCX migration types in a later post. The workloads were being migrated from an on-premises data center to VMware Cloud on AWS (VMC). The big deal here is the impact during the migration to the users was NONE, ZIP, ZERO downtime. And there was also no replatforming of the workloads or applications.

![HCX Dashboard](https://emadyounis.com/assets/img/2018/09/HCX-Dashboard.png?resize=1280%2C768 "HCX Dashboard")

Think about what’s usually involved when migrating workloads from one location to another. The network is at the top of the list, ensuring adequate bandwidth and routing is in place. Not only from data center to data center or data center to cloud, but also across regions and remote sites. Validating vSphere version compatibility between the source and destination is also important. Other workload migration considerations could include:

- Moving across different vSphere SSO domains independent of enhanced linked mode
- Mapping workload dependencies
- Older hardware preventing from upgrading to vSphere 6.x
- Bi-directional migrations independent of vSphere SSO domains, vSphere versions, hardware, or networks
- Validation of resources (compute and storage) on the destination side
- SLA and downtime involved
- Mobility options correlating to SLAs (downtime, low downtime, no downtime)
- No replatforming of the workloads or applications including no changes to IP or MAC Addresses, VM UUID, certs

These are only a few examples of workload migration considerations, but you get the point. I’ve been working with the HCX team for the past few months and I have to say it’s definitely great technology. They have built-in the necessary tooling within the product and removed several boundaries.

## HCX Overview

HCX is the swiss army knife of workload mobility. It abstracts and removes the boundaries of underlying infrastructure focusing on the workloads. A HCX vMotion, for example, requires no direct connectivity to ESXi hosts in either direction compared to a vSphere vMotion. All HCX vMotion traffic gets managed through the HCX vMotion Proxy at each location. The HCX vMotion Proxy resembles an ESXi host within the vCenter Server inventory. It’s deployed at the data center level by default, no intervention is necessary. One thing to mention is the HCX vMotion proxy gets added to the vCenter Server host count by default. The HCX team is aware and will be changing this in the future, but this has no impact on your vSphere licensing.

![HCX vMotion Proxy](https://emadyounis.com/assets/img/2018/09/HCX-Proxy.png?resize=1280%2C768 "HCX vMotion Proxy")

Another boundary HCX removes is it supports several versions of vSphere going back to vSphere 5.0 to the most current release of vSphere 6.7 Update 1. This provides flexibility in moving workloads across vSphere versions, on-premises locations, and vSphere SSO domains. For on-premises to on-premises migrations, a [NSX Hybrid Connect license](https://www.vmware.com/products/nsx-hybrid-connect.html) is required per HCX site pairing. We will cover site pairing in the configuration blog post. Migrating workloads from on-premises to VMC does not require a separate HCX license. By default when deploying a VMC SDDC HCX is included as an add-on and is enabled by default. From the VMC add-ons tab, all that is required is clicking open Hybrid Cloud Extension and then deploy HCX. The deployment of HCX is completely automated within the VMC SDDC.

![HCX Components](https://emadyounis.com/assets/img/2018/09/HCX-Interconnect-Components.png?resize=1280%2C769 "HCX Components")

In order to start migrating workloads, network connectivity between the source and destination needs to be in place. The good news is it’s all built-in to the product. HCX has WAN optimization, deduplication, and compression to increase efficiency while decreasing the time it takes to perform migrations. The minimum network bandwidth required to migrate workloads with HCX is 100 Mbps. HCX can leverage your internet connection as well as direct connect. The established network tunnel is secured using suite B encryption. The on-premises workloads being migrated with no downtime will need to reside on a vSphere Distributed Switch (VDS). It also supports a 3rd party switch in the Nexus 1000v. Cold and bulk HCX migration types are currently the only two options which support the use of a vSphere standard switch but implies downtime for the workload (The HCX team is working on adding support for the vSphere standard switch for other migration types). To minimize migration downtime, HCX has a single click option to extend on-premises networks (L2 stretch) to other on-premises sites or VMware Cloud on AWS. Once the workloads have been migrated there is also an option to migrate the extended network, if you choose. Other built-in functionality includes:

- Native scheduler for migrations
- Per-VM EVC
- Upgrade VM Tools / Compatibility (hardware)
- Retain mac address
- Remove snapshots
- Force Unmount ISO images
- Bi-directional migration support

![HCX Migration](https://emadyounis.com/assets/img/2018/09/HCX-Migrations.png?resize=1280%2C800 "HCX Migration")

## Summary

HCX provides enhanced functionality on top of the built-in vSphere VM mobility options. Customers can now use HCX to migrate workloads seamlessly from on-premises to other paired on-premises sites (multisite) and VMware Cloud on AWS. Workload mobility can also help with hardware refreshes as well as upgrading from unsupported vSphere 5.x version. The next post will cover the different migration options available within HCX, followed by how to setup and configure the product.
