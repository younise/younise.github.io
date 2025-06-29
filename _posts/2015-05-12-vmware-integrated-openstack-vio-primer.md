---
id: 503
title: 'VMware Integrated OpenStack (VIO) Primer'
date: '2015-05-12T10:10:28-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=503'
permalink: /vmware-integrated-openstack-vio-primer/
image: 'https://emadyounis.com/assets/img/2015/05/VIO-1.jpeg'
categories:
    - OpenStack
tags:
    - VIO
    - VMware
    - vSphere
---

![VIO-1](https://emadyounis.com/assets/img/2015/05/VIO-1.jpeg?resize=300%2C76)Your boss has requested that you look at OpenStack and what it would take to install it into your environment. One of the requirements is to leverage your current VMware infrastructure. Of course your plate is already full, and now you need to learn a new platform. Here are a few things that should ease your mind when considering OpenStack. First, OpenStack is a framework that can use your current hardware and software infrastructure. Second, OpenStack services allow integration at the software layer with vendors like VMware. Finally, OpenStack uses drivers to translate requests to calls on the software infrastructure. Why is this important? Let’s take a look at VMware Integrated OpenStack (VIO) to answer these questions.

<span style="text-decoration: underline;">**VMware Integrated OpenStack (VIO)**</span>

At this point you probably have some questions similar to the following:

- What is VIO?
- How does it integrate with my current vSphere environment?
- Why should I use VIO instead of running OpenStack on RHEL, KVM, etc?
- Who should use VIO?

First lets focus on VMware Integrated OpenStack (VIO) and what it is. Simply put VIO allows the use of OpenStack framework on your current VMware vSphere infrastructure. OpenStack services have VMware specific drivers and plugins built to translate requests to calls.

![VIO OpenStack Services](https://emadyounis.com/assets/img/2015/05/VIO-OpenStack-Services.jpg?resize=644%2C387)

Deploying VIO consists of a virtual appliance (vApp) to your vCenter. The vApp deploys two VMs: management-server (OMS) and openstack-template. The management VM deploys, configures, and manages VIO. The OpenStack template VM has the base images for the VIO VMs. A plugin is available in vCenter that will allow the deployment of a highly available OpenStack production environment. This environment will consist of 15 VMs:

- **VIO Compute Driver** – this is the Nova compute node(s) which handle deploying and deleting instances. Each vSphere cluster added to OpenStack will add a Nova compute node, otherwise it starts with one.
- **VIO Controller** – The OpenStack APIs and schedulers live here. Services consist of Nova, Neutron, Glance, Cinder, Heat and Keystone. Two controller VMs are deployed in an Active / Active configuration.
- **VIO DB** – This is where the metadata lives. There are 3 MariaDB VMs deployed in an Active/ Passive / Passive state with replication between the databases.
- **VIO DHCP** – Manages tenant IP addresses. Two VMs in an active / active state.
- **VIO LoadBalancer** – Internal and external communication is handled by two load balancers in an active / passive state.
- **VIO Memcache** – Are memory stores for database call results. Two initials active / active VMs are deployed but it’s scalable.
- **VIO Objectstorage –** Allows storage and retrieval of data, also known as Swift**.** Initially 1 VM is deployed.
- **VIO RabbitMQ** – communication between services are handled by messaging. Two RabbitMQ VMs are deployed both are active.

Now that you have an basic understanding of what VIO is and its components lets move on to how it integrates with vSphere. VIO sits on top of your current vSphere infrastructure. Once VIO has been deployed and configured on vSphere it leverages the following:

- Compute clusters managed via Nova. VIO manages the resources at the cluster level not the individual host level.
- Neutron (Networking) can leverage your current vDS or NSX via NSX plugin.
- OpenStack Manager (OMS) allows addition of components such as compute, storage, etc post deployment.
- vCenter driver and Cinder driver is used to connect the OpenStack services with vSphere.
- vSphere Web Client plug-in is available for management. All instance deployment and management should be done from the Horizon web portal or OpenStack API.
- Glance images can be either a VMDK, OVA, or ISO.

<span style="text-decoration: underline;">**Do you spreken the OpenStack lingo?**</span>

Usually when learning something new, one must be prepared to take on a new vocabulary as it relates to that subject. OpenStack is no different, using code names as it relates to these terms. Here is just a quick cheat sheet to get you up to speed and help you understand when you hear someone mention any of the OpenStack terms.

\[table id=5 /\]

**<span style="text-decoration: underline;">Using VIO with vDS –vs- NSX</span>**

VIO supports vSphere Distributed Switch (vDS) or NSX via the Neutron plugin. Although supported, the vDS has limitations in a VIO environment compared to NSX. NSX can provide support for options such as security groups and fenced environments via Firewall. Here is a quick comparison:

<span style="text-decoration: underline;">**vDS**</span>

- VLANs for Provider Networks
- Layer 2 Support
- Management Plane High Availability

<span style="text-decoration: underline;">**NSX**</span>

- VLANs for Provider Networks
- Layer 2 and 3 Support
- NAT &amp; Floating IP Support
- Security Groups
- DHCP Service
- Stateful distributed Firewall

<span style="text-decoration: underline;">**The Benefits of VIO**</span>

VIO should not be mistaken for a VMware proprietary flavor of OpenStack, it in fact is based on a VMware OpenStack distribution recognized by the OpenStack Foundation. There are benefits for deploying VIO compared to using KVM or RHEL:

- VMware administrators will not be as intimidated to learn OpenStack since they are all ready familiar with the underlying infrastructure.
- Ease of deployment with a vAPP. More importantly it deploys a highly available production ready environment.
- Free to customers who own the vCloud suite &amp; vSphere with Operations Management (vSOM).
- One stop shop for support. VMware will provide support for OpenStack and VMware infrastructure. This will come at a cost of $200 per CPU. Keep in mind that VMware is eating it’s own dog food here and running a large OpenStack deployment internally using VIO on vSphere.
- Ease of management such as adding or removing capacity.
- Leveraging benefits of the vSphere environment such as DRS, HA, and vMotion.
- Simplified upgrades and patching with the potential of minimum disruption to the OpenStack environment. This would depend on the underlying infrastructure.
- Integration with other VMware products such as log insight and vRealize Operations manager.
- Support for any storage supported by vSphere infrastructure including VSAN.
- Developers have an area to spin up virtual machines for development and testing.

OpenStack is usually thought of as hard to implement; VIO simplifies that. VIO allows VMware administrators to get familiar with OpenStack quicker while leveraging their current vSphere environment, also giving developers an area to code and operations a test area without looking elsewhere causing shadow IT. As organizations get more familiar with OpenStack over time they can also begin to contribute. Stay tuned as we’ll go more in to the deployment and configuration of VIO on vSphere 6.
