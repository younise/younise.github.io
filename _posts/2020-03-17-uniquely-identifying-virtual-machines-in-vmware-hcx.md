---
id: 2457
title: 'Uniquely Identifying Virtual Machines In VMware HCX'
date: '2020-03-17T08:12:40-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=2457'
permalink: /uniquely-identifying-virtual-machines-in-vmware-hcx/
image: 'https://emadyounis.com/assets/img/2020/03/HCX-VM-IDs-Feature-Image.jpg'
categories:
    - 'VMware HCX'
tags:
    - HCX
    - 'MoREF ID'
    - 'VM InstanceUUID'
    - 'VMware HCX'
---

I’ve received several questions lately on what happens to a VM’s Managed Object Reference (MoRef) ID and InstanceUUID when it’s migrated across different vCenter Servers using VMware HCX. This information is essential for anything being used to identify VMs, such as provisioning systems, CMDBs, ticketing systems, and backup solutions are examples of tooling using this information.

When a VM gets created within a vCenter Server, it generates a MoRef ID that helps uniquely identify it. A MoRef ID gets created not just for VMs but all objects (hosts, folders, datastores, etc.) within a vCenter Server, including the vCenter Server itself. If you’ve heard me talk about using the vCenter Server Migration Tool (Windows to VCSA), I reference the importance of a vCenter Server keeping its MoRef ID, which is how other solutions will identify a vCenter Server. In this case, the Windows vCenter Server MoRef ID gets retained during the migration process, which means any registered solutions will continue to identify the original vCenter Server unknowingly of the migration process 🙂

Another way to uniquely identify a VM within a vCenter Server is by using its instanceUUID, introduced in vSphere 4.0. If you’ve explored inside the VM .vmx file, there is an entry called vc.uuid, which is a 128bit integer and looks something like “50 3e 84 9c 71 56 12 29-a6 a7 00 d3 3c a5 d3 28”. Both MoRef and InstanceUUID information is obtainable using the vSphere API. For more details, I recommend looking at the following blog posts by William Lam. This post builds on the information he provided, focusing on HCX migration types with VM MoRef ID and InstanceUUID.

- [Uniquely Identifying Virtual Machines in vSphere and vCloud Part 1: Overview](https://blogs.vmware.com/vsphere/2012/02/uniquely-identifying-virtual-machines-in-vsphere-and-vcloud-part-1-overview.html)
- [Uniquely Identifying Virtual Machines in vSphere and vCloud Part 2: Technical](https://blogs.vmware.com/vsphere/2012/02/uniquely-identifying-virtual-machines-in-vsphere-and-vcloud-part-2-technical.html)
- [Uniquely Identifying VMs in vSphere Part 3: Enhanced Linked Mode &amp; Cross VC-vMotion](https://www.virtuallyghetto.com/2017/07/uniquely-identifying-vms-in-vsphere-part-3-enhanced-linked-mode-cross-vc-vmotion.html)

## HCX VM Migrations

With HCX, VMs can easily migrate using various [migration types](http://emadyounis.com/learning-hybrid-cloud-extension-hcx-part-2-migration-types/) (Cold, vMotion, Bulk, RAV) between vCenter Servers, whether on-prem vSphere (5.0-7.0) or cloud (VMware Cloud on AWS, Azure VMware Solutions, etc.). In this example, I have a source on-prem vCenter Server using HCX to migrate VMs to the destination vCenter Server in VMware Cloud on AWS (VMC). Each VM name contains the migration type that will be used when its migrated to VMC with HCX. Before starting the migration, I grabbed the MoRef ID, InstanceUUID, and Tag for each VM as well as the source vCenter Server Name and its InstanceUUID.

![](https://emadyounis.com/assets/img/2020/03/Source.jpg?resize=1024%2C331)  
After the migrations completed successfully to the destination vCenter Server, we can see only one difference. All the VMs received a new MoRef ID, which is as expected since its managed per vCenter Server independently. The VM instanceUUID did not change, and <span style="color: #000000;">will only change if a VM with the same UUID already exists </span>on the destination vCenter Server.

![](https://emadyounis.com/assets/img/2020/03/Destination.jpg?resize=1024%2C310)  
I was also able to pull the destination vCenter Server UUID, VM Name, and VM MoRef ID (HCX has this labeled as Id) using the Get-HCXVM cmdlet.

![](https://emadyounis.com/assets/img/2020/03/GET-HCXVM.jpg?resize=1024%2C412)  
HCX removes barriers! These are two distinct vSphere Single Sign-On (SSO) domains between on-prem and VMC, by combining VM instanceUUID with the vCenter Server unique identifier, you can identify VMs across multiple vCenter Servers. Another way to uniquely identify VMs in your environment is the use of vSphere tags. HCX will retain VM tag and category information from the source to the destination vCenter Server, see examples above. Now you can migrate successfully using HCX while understanding the impact on your VMs IDs and solutions that rely on them.
