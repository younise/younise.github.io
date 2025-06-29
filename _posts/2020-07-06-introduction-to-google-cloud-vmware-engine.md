---
id: 2520
title: 'Introduction to Google Cloud VMware Engine'
date: '2020-07-06T11:22:26-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=2520'
permalink: /introduction-to-google-cloud-vmware-engine/
image: 'https://emadyounis.com/assets/img/2020/07/GCVE-Feature-Image.jpg'
categories:
    - 'Google Cloud VMware Engine'
tags:
    - GCVE
    - 'Google Cloud'
    - VMware
    - 'VMware HCX'
---

Last month [Google announced](https://cloud.google.com/blog/topics/hybrid-cloud/announcing-google-cloud-vmware-engine) a first-party and fully managed solution called Google Cloud VMware Engine (GCVE), which was in limited early access since January 2020. Google Cloud VMware Engine is [now generally available](https://cloud.google.com/blog/topics/hybrid-cloud/google-cloud-vmware-engine-is-generally-available) to all starting in US-east and US-west regions with plans to add eight more regions by the end of the year. When deployed, customers will receive a GCVE private cloud, which consists of ESXi Hosts, vCenter Server, vSAN, and NSX-T running on Google Cloud bare metal. Also included is VMware HCX, which allows customers to extend and migrate workloads from on-premises to GCVE and between GCVE private clouds.

As part of the GCVE service availability, customers have several pricing options: on-demand (per-hour), 1yr commitment, and 3yr commitment, which includes the cost of infrastructure, licensing, and support. Billing is handled through Google Cloud; for more details, please see the [following](https://cloud.google.com/vmware-engine#section-13). The GCVE service allows customers to focus on workloads while Google manages all other aspects of the service, including the lifecycle of the vSphere environment.

![](https://emadyounis.com/assets/img/2020/07/GCVE-Welcome.jpg?resize=1024%2C457)

## <span style="color: #000000;">GCVE Service</span>

<span style="color: #000000;"><span style="color: #000000;">**\* Project – must be created as part of the Google Cloud service and is a way to group and manage resources.**  
 **\* Private Cloud – is the equivalent of a VMware Software-Defined Data Center (vCenter Server, ESXi, vSAN, and NSX).** </span></span>

To get started with GCVE first requires enabling the VMware Engine API in the Google Cloud console. This will need to be done on a per-project basis. Next, you’ll need to request a quota which is located under IAM &amp; Admin. A quota is a way to verify, protect against unexpected usage, and prevent misuse. Each project in Google Cloud could have a different quota. Once your quota request has been accepted, we can start [getting ready](https://cloud.google.com/vmware-engine/docs/quickstart-prerequisites) to deploy a GCVE private cloud.

![](https://emadyounis.com/assets/img/2020/07/GCVE-Enable-API.jpg?resize=1024%2C200)

The GCVE service configuration will consist of a minimum of 3 nodes and can scale up to 16 nodes per cluster (roughly 15 mins per node add) or 64 nodes in a single private cloud. Each GCVE node has the following specifications:

- - **CPU** – 2x Intel Xeon Gold 6240 (Cascade Lake) 2.6 GHz, 36 Cores, 72 Hyper-Threads
    - **Memory** – 768 GB
    - **Storage:**
        - 2x 1.6 TB (3.2 total TB) NVMe for cache
        - 6 x 3.2 TB (19.2 TB Total) NVMe for data
    - **Network** – 4 x Dual Port 25GbE

GA included [software specifications](https://cloud.google.com/vmware-engine/docs/concepts-vmware-components) (keep in mind these versions will change over time):

\[table id=14 /\]

![](https://emadyounis.com/assets/img/2020/07/GCVE-Quota-1.jpg?resize=936%2C484)

![](https://emadyounis.com/assets/img/2020/07/GCVE-Quota-2.jpg?resize=936%2C256)  
Part of the [<span data-preserver-spaces="true">service level agreement</span>](https://cloud.google.com/vmware-engine/sla)<span data-preserver-spaces="true"> (SLA) ensures customers have the needed reliability. GCVE is providing 99.99% of network uptime (less than an hour per year) for its service and has network redundancy across rack configuration. Connectivity from on-premises to GCVE (or between private clouds within GCVE) can be established using a </span>[<span data-preserver-spaces="true">Google Cloud VPN</span>](https://cloud.google.com/network-connectivity/docs/vpn)<span data-preserver-spaces="true">, the quickest method (self-service) to getting started. There is also the option to use Google’s</span>[<span data-preserver-spaces="true"> Cloud Interconnect</span>](https://cloud.google.com/network-connectivity/docs/how-to/how-to-choose#cloud-interconnect)<span data-preserver-spaces="true"> (available from Google or </span>[<span data-preserver-spaces="true">Service Providers</span>](https://cloud.google.com/network-connectivity/docs/interconnect/concepts/service-providers)<span data-preserver-spaces="true">), a dedicated, highly available, low latency direct connection. As part of the configuration process, customers will need to create Virtual Private Cloud (VPC) within the Google Cloud that allows them to control how workloads connect within a region or globally, create policies within projects, and expand IP spaces without downtime. After the VPC is created, workloads can communicate internally within a project, keeping traffic local and providing extensibility to Google Cloud Services.</span>

A CIDR block is required to deploy all the vSphere management components within GCVE, but workload networks will be provisioned from NSX-T (NSX segments). There will be DNS servers deployed within a private cloud and are only used for management. Within the network section of the GCVE console, customers can allocate public IP addresses to workloads, which are protected by Google Cloud Edge. The VPN Gateway section provides a point to site VPN that gives access to the management components like vCenter Server within GCVE.

<span data-preserver-spaces="true">There are a couple of things I want to highlight within the GCVE service. First, when it comes to deploying a private cloud, GCVE has fast provisioning, where it takes approximately 30 minutes to deploy a private cloud with all components running (the clock starts when you click on create). On the back end, the GCVE service has built-in intelligence and optimization magic sauce allowing it to pre-provision capacity on unused nodes before they are requested. These nodes are then re-purposed based on the customer’s specifications and capacity. This ensures deployments are the same as if they were installed from scratch.</span>

![](https://emadyounis.com/assets/img/2020/07/GCVE-Fast-Mode.jpg?resize=936%2C662)

<span data-preserver-spaces="true">Second, is how Role-Based Access Control (RBAC) works in GCVE. It starts with the Google Cloud console, to gain access to GCVE you either have to be an owner or editor of a project in the Google Cloud console or granted the following roles within a project’s IAM roles and permissions:</span>

- - - <span data-preserver-spaces="true">VMware Engine Service Viewer (read-only access)</span>
        - <span data-preserver-spaces="true">VMware Engine Service admin (full admin access)</span>

Once access has been granted, the GCVE portal has three RBAC options: CloudOwner, Elevated Privilege, and Service Provider. The CloudOwner account is the default account; it allows customers the access needed to handle their day-to-day operations on their workloads and provides full access to NSX-T networking (including NSX-T manager). Elevated privilege is a temporary and time-based request that allows access to hosts, storage, and management VMs to create and add identity sources and auth management; this level of access is monitored. The GCVE service monitors actions of the elevate privilege and provides notification if an action is disruptive to the service and the customer, potentially preventing those actions from happening. It also allows the creation and addition of accounts necessary for 3rd party integration, such as backup solutions. Customers can request Elevated Privilege within the GVCE portal on-demand. Service Provider access is only available for Google support only and provides full access to the customer’s private cloud within GCVE for troubleshooting.

When first logging into vSphere Client, you will be prompted to [change](https://cloud.google.com/vmware-engine/docs/vmware-platform/howto-access-vsphere-client) the default CloudOwner@gve.local account password, which must be changed every 365 days afterward. There are also some pre-created roles that can be used, or you can create your own, but no role can be granted privileges higher than CloudOwner. You can find a list of the GCVE vCenter Server roles and privileges [here](https://cloud.google.com/vmware-engine/docs/concepts-permission-model). GCVE also supports adding your identity solution, which allows you to use your on-premises groups in vCenter Server.

![](https://emadyounis.com/assets/img/2020/07/GCVE-Elevated-Privileges-Updated.jpg?resize=930%2C122)

All service and access configurations are done from the GCVE console; this includes launching the vSphere Client, network configuration/management, and Private Cloud management (deployment, configuration, and capacity). The GCVE console allows the management of different private clouds from a centralized location. It also provides an activity view that displays alarms, events, tasks, and audits, which tracks all the changes made across private clouds. Customers also have the option to download report information from various sections of the GCVE console in CSV format.

![](https://emadyounis.com/assets/img/2020/07/GCVE-Console.jpg?resize=927%2C560)

## Workload Mobility

Included with the GCVE service is [VMware HCX](https://cloud.google.com/vmware-engine/docs/workloads/howto-migrate-vms-using-hcx) (advanced licensing), which allows customers to migrate workloads as far back as vSphere 5.0 from on-premises to GCVE without re-platforming. It’s the swiss army knife of workload mobility. VMware HCX Cloud Manager is deployed, configured, and licensed out of the box as part of the GCVE private cloud deployment, requiring a CIDR block for the VMware HCX components. Once the HCX Cloud Manager is deployed, customers can download the HCX Connector OVA to deploy in their on-premises data center, allowing them to pair with their GCVE private cloud. With a site pairing established, HCX provides the option to extend workload networks (L2 Stretch) and migrate using different migration types based on your SLAs: Bulk Migration – min downtime, vMotion – no downtime, and Cold Migration – downtime.

Additional functionality and migration types are also included with the HCX enterprise add-on licensing:

- - - Replication Assisted vMotion – the ability to migrate with zero downtime
        - Mobility Groups -create migration groups based on criteria and migrate in waves
        - [more](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-32AF32BD-DE0B-4441-95B3-DF6A27733EED.html)

These are a few examples of what’s included with VMware HCX enterprise licensing, contact your VMware/Google representative for more details. This also simplifies uses cases such as data center evacuation, disaster recovery, and hybrid connectivity.

![](https://emadyounis.com/assets/img/2020/07/GCVE-HCX-Deployment.jpg?resize=937%2C335)

## Resources

With the launch of the new Google Cloud VMware Engine service I wanted to share some resources to help get you started and bookmark:

- - [Google Cloud VMware Engine Documentation](https://cloud.google.com/vmware-engine/docs)
    - [Release Notes](https://cloud.google.com/vmware-engine/docs/release-notes)
    - [Known Issues](https://cloud.google.com/vmware-engine/docs/known-issues)
    - [Service Level Agreement](https://cloud.google.com/vmware-engine/sla)
    - [Prerequisites](https://cloud.google.com/vmware-engine/docs/quickstart-prerequisites)
    - [Pricing](https://cloud.google.com/vmware-engine#section-13)
    - [Ecosystem Partners](https://cloud.google.com/vmware-engine#section-14)
    - [Google Cloud Next ’20](https://cloud.withgoogle.com/next/sf/sessions#infrastructure) – Filtered with VMware as a Service
