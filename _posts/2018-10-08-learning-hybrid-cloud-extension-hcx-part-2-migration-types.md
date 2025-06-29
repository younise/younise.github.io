---
id: 1119
title: 'Learning Hybrid Cloud Extension (HCX) Part 2: Migration Types'
date: '2018-10-08T12:50:03-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=1119'
permalink: /learning-hybrid-cloud-extension-hcx-part-2-migration-types/
image: 'https://emadyounis.com/assets/img/2018/10/HCX-Migration-Types-Blog-Logo.png'
categories:
    - 'Hybrid Cloud Extension'
---

I recently attended a cloud migration session. The first thing the speaker said was “we want you to migrate to the Cloud, but we don’t care how you do it”. There was no discussion of service level agreements (SLA), dependencies, or migration options, only get to the cloud! Before migrating any workload, it’s important to have a good understanding of its SLAs, dependencies, and data. Once you have a good grasp of the workload, the next step is selecting the right migration tool or type. The following is in no particular order, but here are a few things to consider before starting a migration:

- How will the workloads get migrated?
- Does the migration type meet the SLA of the workloads?
- What is the impact of the migration to the workloads (downtime)?
- Are there any required changes pre and post-migration?
- Is the proper networking (secure) in place and is there enough bandwidth?
- Was a discovery of the environment done, including mapping of dependencies?

As you start to ask these questions more will come up. The point is you can’t migrate to the Cloud without understanding the impact. The [first blog post](http://emadyounis.com/learning-hybrid-cloud-extension-hcx-part-1-overview/) of this series was an overview of Hybrid Cloud Extension (HCX). This post will focus on the available migration types HCX offers.

## Migration Types

HCX supports four different migration types:

- Cold Migration – offline
- vMotion (live) – no downtime
- Bulk Migration – low downtime
- Cloud Motion with vSphere Replication – no downtime

Cold migration and vMotion should be familiar to those using the built-in vSphere mobility options. Bulk Migration and Cloud Motion with vSphere Replication are new options and only available with HCX. Let’s take a look at how each option works and what is the impact to the workloads.

![](https://emadyounis.com/assets/img/2018/10/Migration-Types-of-HCX.png?resize=1149%2C643)

## Cloud Motion with vSphere Replication

The new kid on the block announced during the VMworld 2018 US keynote. This new option provides zero downtime for workload mobility from source to destination. First, the workload disk(s) (VMDK) get replicated to the destination site. The replication is handled using the HCX built-in vSphere replication. This process is dependent on the amount of data and available network bandwidth. Once the data sync is complete the HCX switchover initiates a vMotion. The vMotion migrates the workload to the destination site and synchronizes only the remaining data (delta) and workload memory state. There is an option to schedule a maintenance window for the vMotion data sync switchover otherwise, it happens immediately.

When using the scheduling option, there are a few things to be aware of. First, the switchover can be re-scheduled any time prior to the actual scheduled time. Secondly, if the initial vSphere Replication data sync has not completed prior to the scheduled window, the workload(s) being migrated will fail. In this case, these workload migrations must be restarted in their entirety. Finally, if the vSphere Replication data sync has completed prior to the scheduled switchover window, HCX will continue to sync delta information to keep the migrated data as current as possible prior to the vMotion switchover. The vMotion switchover happens in a serialized fashion, only one at a time. Today Cloud Motion with vSphere Replication is uni-directional from on-premises to VMware Cloud on AWS (VMC). This is due to HCX leveraging changes in the newer vSphere platform available in VMC. As a result, only VMware Cloud on AWS supports Cloud Motion with vSphere Replication today. Stay tuned, more will be coming for on-premises to on-premises migrations using Cloud Motion with vSphere Replication.

#### <span style="color: #0000ff;">**Cloud Motion with vSphere Replication Notes**</span>

- Uni-directional from on-premises to VMware Cloud on AWS (see details above)
- Supports source vSphere (vCenter Server / ESXi) versions 5.5 and above
- Cross-version migration from vSphere 5.5 to the current release of 6.7 Update 1
- Currently no support for on-premises to on-premises migrations
- Concurrent migrations = 100 (initial data sync using vSphere Replication)

![](https://emadyounis.com/assets/img/2018/10/Cloud-Motion.png?resize=1920%2C1080)

![](https://emadyounis.com/assets/img/2018/10/VMC-Cloud-Motion.png?resize=1920%2C1080)

## Bulk Migration

Bulk Migration creates a new virtual machine on the destination site. This can either be on-premises or VMC and retains the workload UUID. Then it uses vSphere Replication to copy a snapshot of the workload from source to destination site while the workload is still powered on. In this case, a snapshot is a point of the time of the workload disk state, but not a traditional vSphere snapshot. The Bulk Migration is managed by the HCX interconnect cloud gateway proxy. We’ll go into more details about the HCX interconnect cloud gateway in the deployment post. During the data sync, there is no interruption to the workloads. The data sync is dependent on the amount of data and available bandwidth. There is an option to schedule a maintenance window for the switchover (similar to the schedule options described in the Cloud Motion section above) otherwise, the switchover happens immediately. Once the initial data sync completes, a switchover takes place (unless scheduled). The source site workloads are quiesced and shut down leveraging VMware Tools. If VMware Tools is not available, HCX will prompt you to force power off the workload(s) to initiate the switchover. During the switchover process, a delta sync occurs based on changed block tracking (CBT) to sync the changes since the original snapshot. The workloads on the destination site will begin to power on once the data sync is complete (including delta data changes). There are checks in place to ensure resources are available to power on the workloads. If a destination workload cannot power on due to resources, the source workload will get powered back on.

Unlike cold migration, the downtime incurred after the bulk migration cut over is low. The HCX manager renames the original source workloads by appending a binary timestamp. A migrated VMs folder gets created in the VM and templates view. Finally, the original source workloads get placed in the migrated VMs folder. This also provides an option for easy rollback and quick data seeding since bulk migration supports reverse migrations. One thing worth mentioning is HCX has built-in network resiliency and will pick up where it left off during a data sync if there is any network interruption. On the flip side, if there is an issue powering on workloads on the destination site or missed scheduling a cut over, the entire data sync process will need to be started over. **This technology allows a workload migration between different chipset versions (e.g.** Sandy Bridge to Skylake) and across different CPU families (e.g. AMD to Intel).

#### <span style="color: #0000ff;">**Bulk Migration Notes**</span>

- Automatic update of VM compatibility (hardware) version and VMware Tools available during the switchover
- Supports vCenter Server 5.1 and above / ESXi 5.0 and above
- Cross-version migration from vSphere 5.0 / 5.1 to the current release of 6.7 Update 1
- Concurrent migrations = 100
- Bi-directional migration (reverse HCX option)

## ![](https://emadyounis.com/assets/img/2018/10/Bulk-Migration.png?resize=1280%2C720)

## vMotion “Live Migration”

HCX supports the vMotion we know and love today. The workloads are migrated live with no downtime similar to Cloud Motion with vSphere Replication. vMotion should not be used to migrate hundreds of workloads or workloads with large amounts of data. Instead, use Cloud Motion with vSphere Replication or Bulk Migration. Usually, a vMotion network needs to be configured and routed to the target vSphere host, in this case, the vMotion traffic is handled by the HCX Interconnect cloud gateway for cross-cloud vMotion. vMotion through HCX encapsulates and encrypts all traffic from source to destination removing network complexity of routing to cloud. The vMotion captures workload:

- Active Memory
- Execution State
- IP Address
- MAC address

HCX has a built-in option to retain the workloads MAC address. If this option is not checked, the workloads will have a different MAC on the destination site. Workloads must be at compatibility (hardware) version 9 or greater and 100 Mbps or above of bandwidth must be available. With vMotion and bi-directional migration, it’s important to consider Enhanced vMotion Compatibility (EVC). The good news here is HCX also handles EVC. The workloads can be migrated seamlessly and once rebooted will inherit the CPU features from the target cluster. This allows a cross-cloud vMotion between different chipset versions (e.g. Sandy Bridge to Skylake) but within the same CPU family (e.g. Intel). Also, an important thing to note is **vMotion is done in a serialized manner**. Only one vMotion occurs at a time and queues the remaining workloads until the current vMotion is complete.

#### <span style="color: #0000ff;">vMotion Notes</span>

- Does not support disk sizes over 2TB and shared VMDK files
- Attached virtual media or ISOs can be removed prior using the HCX global option
- Does not support compatibility (hardware) version 8 or below
- Supports vSphere ESXi Host versions 5.5 and above
- Cross-version migration from vSphere 5.5 to the current release of 6.7 Update 1
- Bi-directional migration (reverse HCX option)

![](https://emadyounis.com/assets/img/2018/10/HCX-vMotion.png?resize=1137%2C630)

## Cold Migration

As the name states, a cold migration occurs when a workload is offline. CPU compatibility checks do not apply during a cold migration. During a cold migration, VMware’s Network File Copy (NFC) protocol is used to transfer the workloads from source to destination which includes:

- Configuration files including BIOS settings (NVRAM)
- Disks associated with the workload
- Log files

Usually, cold migration traffic takes place over the host management network. This traffic type is known as provisioning traffic. Provisioning traffic is not encrypted but uses run-length encoding. Run-length encoding is a simple form of lossless data compression. In the case of HCX, cold migration traffic will get encrypted using **Suite B encryption**. It leverages the same host management network to migrate the data through the HCX vMotion proxy. HCX only displays the cold migration option if the selected workload is not powered on. Although HCX provides the option to multi-select it is still limited. vCenter Server only handles 8 concurrent vMotions (cold and live) at a time. It places the remaining selected workloads in a queue until ready to migrate.

#### <span style="color: #0000ff;">Cold Migration Notes</span>

- If transferring large amounts of data consider using Bulk Migration or Cloud Motion
- vCenter Server templates are good candidates
- Supports vSphere ESXi Host versions 5.5 and above
- Cross-version migration from vSphere 5.5 to the current release of 6.7 Update 1
- Bi-directional migration (reverse HCX option)

![](https://emadyounis.com/assets/img/2018/10/HCX-Clold-Migration.png?resize=875%2C325)

## Summary

HCX offers customers several different workload mobility options covering different SLAs and requirements. No reconfiguration of the source or destination sites is necessary to get started. The platform offers built-in encryption, WAN optimization, and [cross-version migration](https://docs.vmware.com/en/VMware-NSX-Hybrid-Connect/3.5.1/user-guide/GUID-54E5293B-8707-4D29-BFE8-EE63539CC49B.html) through the HCX interconnects. The HCX service is evolving at a rapid rate, the team is making improvements and adding new functionality on almost a monthly basis. Next, we’ll be diving into how to deploy and configure HCX in VMware Cloud on AWS and on-premises.
