---
id: 1021
title: 'vCenter Server 6.7 What&#8217;s New Rundown'
date: '2018-04-17T05:02:47-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=1021'
permalink: /vcenter-server-6-7-whats-new-rundown/
image: 'https://emadyounis.com/assets/img/2018/04/VCSA-Blog-Feature-Image.png'
categories:
    - vCenter
tags:
    - VCSA
    - 'VCSA 6.7'
    - VMware
    - vSphere
    - 'vSphere 6.7'
---

Today VMware launched [vSphere 6.7](http://vmw.re/vsphere67launch) and there are lots of new enhancements in this release. But first the news. This is the final release for both vCenter Server for Windows and the vSphere Web Client (flash). The vCenter Server path is set and has been for a while with the vCenter Server Appliance (VCSA). The vSphere Client (HTLM5) is getting close to feature complete in this release.

There are some considerations around upgrades for this release. First, I can’t stress it enough read the release notes, they are your friend. vSphere 6.7 will only support upgrades and migrations from vSphere 6.0 or 6.5. What does this mean for vSphere 5.5? In short a vSphere 5.5 to 6.7 upgrade or migration is multi-step. Also, a vCenter Server 6.0 or 6.5 managing ESXi 5.5 hosts will also be a multi-step upgrade. In this case, the ESXi 5.5 hosts will need to be upgraded to at least vSphere 6.0 before upgrading the vCenter Server. OK on to the What’s new with vCenter Server Appliance 6.7.

## vCenter Server Appliance 6.7 CLI Installer

vSphere 6.7 has improvements in several areas of the vCenter Server appliance. Let’s start with the CLI installer. For the most part, the vCenter Server Appliance 6.7 ISO is the same [compared to 6.5](http://emadyounis.com/vcenter/vcenter-server-appliance-vcsa-6-5-whats-new-rundown/) with one exception. There is now a templates schemas directory under the CLI installer directory. The schemas are meant as a data type guide for the JSON templates. This is actually a good segway to the VCSA CLI installer.

![vCenter Server Appliance 6.7 Template Schemas](https://emadyounis.com/assets/img/2018/04/VCSA-6.7-Template-Schemas-Directory.png?resize=1368%2C676 "vCenter Server Appliance 6.7 Template Schemas")  
<span data-offset-key="68tlr-0-0">If you haven’t use the CLI installer, you’re missing out. This release introduces a new enhancement called batch operations. Prior we had to run each JSON individual and in the correct sequence </span><span class="adverb"><span data-offset-key="68tlr-1-0">manually</span></span><span data-offset-key="68tlr-2-0">. With batch operations, we can now drop several JSON files in a single directory and the CLI installer will handle the deployment and sequence. We are still responsible for making sure the JSON templates </span><span class="adverb"><span data-offset-key="68tlr-3-0">correctly</span></span><span data-offset-key="68tlr-4-0"> filled out. Also ensuring the correct replication agreements are in place. Then sit back and watch the magic happen. The Batch operations also handles the VCSA lifecycle of install, upgrade, and migrate 🙂</span>

![vCenter Server Appliance 6.7 Batch Operations](https://emadyounis.com/assets/img/2018/04/VCSA-6.7-Batch-Operations.png?resize=1951%2C652 "vCenter Server Appliance 6.7 Batch Operations")

## CMSSO-Util Repointing Tools

There are a couple of updates to the cmsso-util for repointing. Keep in mind both of these updates are only for external deployments. The first is not actually new but is making a return. If you said repoint across sites, you are correct. Repointing is supported within a site and across sites in vSphere 6.7.

The second repointing update is new in vSphere 6.7. Now you can repoint your vCenter Server across a vSphere SSO domain. Think vSphere SSO domain consolidation! Domain repoint comes with a pre-check option which generates any discrepancies before repointing. These discrepancies are logged in a JSON file to be verified and corrected prior to repointing. If the pre-check option is not used the opportunity to resolve any discrepancies is not available. Therefore, it highly recommended to use it. The domain repoint feature migrates licenses, tags, categories, and permissions from one PSC to another.

![vCenter Server Appliance 6.7 Domain Repoint](https://emadyounis.com/assets/img/2018/04/VCSA-6.7-Domain-Repoint.png?resize=1010%2C600 "vCenter Server Appliance 6.7 Domain Repoint")

## vSphere Appliance Management Interface (VAMI)

The vSphere Appliance Management Interface (VAMI) has received a significant amount of improvement. VAMI is becoming the VCSA tool for monitoring and out of band troubleshooting. Now sporting the new Clarity theme for a more unified look across the VMware portfolio. There are several new tabs and enhancements in the VCSA 6.7 VAMI, these include:

- Monitoring – visibility to CPU, memory, network, and database utilization.
- - A new addition called disks is now available under the monitoring tab. Here we can see all the disk partitions for the VCSA, space available and utilization.
    - Improved alerting in the vSphere Client based on triggered events.
- Services – previously in the vSphere Web Client and now in VAMI for out of band troubleshooting. All the services that make up the VCSA, their startup type, health, and state are visible here. We are also given the option to start, stop, and restart services if needed.
- Syslog – increased the supported number of syslog forwards to three. Prior, vSphere 6.5 only supported forwarding to only one.
- Update – <span data-offset-key="8rk2u-0-0">there is now more flexibility in selecting patches and updates. </span>Information about each patch or update is available such as type, severity, and if a reboot is necessary.
- - <span class="hardreadability"><span data-offset-key="8rk2u-3-0">Expanding a patch or update in the main view will display more information about what </span></span><span class="passivevoice"><span data-offset-key="8rk2u-4-0">is included</span></span><span class="hardreadability"><span data-offset-key="8rk2u-5-0"> a patch or update</span></span><span data-offset-key="8rk2u-6-0">. </span>
    - <span data-offset-key="8rk2u-6-0">Option to either stage or stage and install a patch or update from the VAMI. This capability was </span><span class="complexword"><span data-offset-key="8rk2u-7-0">previously</span></span><span data-offset-key="8rk2u-8-0"> only available from the CLI.</span>

![vCenter Server Appliance 6.7 Disks](https://emadyounis.com/assets/img/2018/04/VCSA-6.7-VAMI-Disk.png?resize=1280%2C661 "vCenter Server Appliance 6.7 Disks")

- File-Based Backup – was first introduced in vSphere 6.5 under the summary tab, now it has its own backup tab.
- - <div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="4smjj-0-0"><span data-offset-key="4smjj-0-0">Native scheduler included in the UI.</span></div>
    - <div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="39kkv-0-0"><span data-offset-key="39kkv-0-0">Retention option available, select the number of backups to keep.</span></div>
    - <div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="11bj5-0-0"><span data-offset-key="11bj5-0-0">Activities of completed and failed backup jobs logged with detailed information.</span></div>

![](https://emadyounis.com/assets/img/2018/04/vCenter-6.7-Backup.png?resize=1680%2C1050)

- File-Based Restore – workflow now includes a backup achieve browser.
- - <div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="30bio-0-0"><span data-offset-key="30bio-0-0">The browser displays all your backups without having to know the entire backup path.</span></div>
    - <div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="f3u1s-0-0"><span data-offset-key="f3u1s-0-0">External PSC restores are not supported when other replication partners are available. </span></div>
    - <div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="3gijb-0-0"><span data-offset-key="3gijb-0-0">External and Embedded Enhanced Linked Mode supported with the reconciliation of vSphere SSO domain credentials.</span></div>

![](https://emadyounis.com/assets/img/2018/04/vcsa-6.7-restore.png?resize=1680%2C1050)

<div class="" data-block="true" data-editor="4tuj3" data-offset-key="9k5rm-0-0">## Simplified Architecture

</div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="9k5rm-0-0"><div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="9k5rm-0-0"><span data-offset-key="9k5rm-0-0">If you are like me, then you remember a time when vCenter Server architecture was simple. All vCenter Server services ran on a single instance. vSphere 5.1 changed this with Single Sign-On and distributed services. vSphere 6.0 simplified the architecture a bit eliminating distributed services. Giving us only two components, vCenter Server and the Platform Services Controller (PSC). One significant change for the VCSA 6.7 is around simplifying the architecture. Going back to running all vCenter Server services on a single instance. Introducing vCenter with Embedded PSC with Enhanced Linked Mode. Here are the benefits of this deployment model:</span></div></div>- <div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="6t3kn-0-0"><span data-offset-key="6t3kn-0-0">No load balancer required for high availability supports native vCenter Server High Availability.</span></div>
- <div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="4dpos-0-0"><span data-offset-key="4dpos-0-0">Site boundary removal provides flexibility of placement</span></div>
- <div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="f6ilm-0-0"><span data-offset-key="f6ilm-0-0">Supports vSphere scale maximums</span></div>
- <div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="775hq-0-0"><span data-offset-key="775hq-0-0">Allows for 15 deployments in a vSphere Single Sign-On Domain</span></div>
- <div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="crv58-0-0"><span data-offset-key="crv58-0-0">Reduces the number of nodes to manage and maintain</span></div>

## Migrate to the VCSA

<div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0"><div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="5gkg1-0-0"><span data-offset-key="5gkg1-0-0"><span data-offset-key="5gkg1-0-0">Now that vSphere 6.7 is the last release to include vCenter Server for Windows, its now time to migrate to the VCSA. The built-in Migration Tool does all the heavy lifting. It brings over the vCenter Server identity, configuration, and inventory by default. Optional is historical and performance data. In vSphere 6.7 we can now select how to import the historical and performance data during a migration:</span></span></div></div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0">- <span data-offset-key="5gkg1-0-0"><span data-offset-key="5gkg1-0-0">Import historical data in the background</span></span>
- Import all data now

</div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0">![](https://emadyounis.com/assets/img/2018/04/VCSA-6.7-Migration-Data-Import.png?resize=1920%2C1080)

Beside each of the data import options is an estimated time of how long each option will take when migrating. Estimated time will vary based on historical and performance data size in your environment. Selecting the data import in the background option gives the ability to pause and resume. Once the VCSA 6.7 <span class="passivevoice"><span data-offset-key="c5qv7-2-0">is deployed</span></span><span class="hardreadability"><span data-offset-key="c5qv7-3-0"> and configured, log into the VAMI and a banner </span></span><span class="passivevoice"><span data-offset-key="c5qv7-4-0">is displayed</span></span><span class="hardreadability"><span data-offset-key="c5qv7-5-0"> at the top to manage the data import along with the current percent</span></span><span data-offset-key="c5qv7-6-0">.</span>

</div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0"><div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="c5qv7-0-0"><span data-offset-key="c5qv7-6-0">![](https://emadyounis.com/assets/img/2018/04/VAMI-Pause-and-Resume.png?resize=1742%2C687)</span></div></div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0">## Workload Mobility

</div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0">Now let’s talk about a different type of migration, the workload kind. vSphere 6.7 has enhancements removing the boundaries for workload mobility. vMotion 6.7 now supports workload mobility across vCenter Server versions going back to 6.0 U3. The only blocker was from 6.5 to 6.0 U3 which has removed in 6.5 U1.

EVC is usually a cluster level setting, now in vSphere 6.7, it’s also a VM level setting with Per-VM EVC. Per-VM EVC takes us across clusters, datacenters, vCenters, and VMware Cloud on AWS. EVC, only allows CPUs from a single vendor, meaning you can not cross the streams between Intel and AMD. Per-VM EVC does require a VM to be on the latest VM compatibility version 14 included in vSphere 6.7.

![](https://emadyounis.com/assets/img/2018/04/Per-VM-EVC-6.7.png?resize=1672%2C797)

</div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0">## vSphere Client (HTML5)

</div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0"><div class="" data-block="true" data-editor="4tuj3" data-offset-key="8985v-0-0"><div class="public-DraftStyleDefault-block public-DraftStyleDefault-ltr" data-offset-key="8985v-0-0"><span data-offset-key="8985v-0-0">With vSphere 6.5 VMware introduced a supported version of the vSphere Client (HTML5). Included in the vCenter Server Appliance it only had partial functionality. The vSphere team has been working hard optimizing and improving workflows. This release takes us a step further with the following new workflows:</span></div></div></div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0">- <span style="font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen-Sans, Ubuntu, Cantarell, 'Helvetica Neue', sans-serif">vSphere Update Manager</span>
- <span data-offset-key="69h1t-0-0">Content Library</span>
- <span style="font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen-Sans, Ubuntu, Cantarell, 'Helvetica Neue', sans-serif">vSAN</span>
- Storage Policies
- Host Profiles
- vDS Topology Diagram
- Licensing
- More…

</div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0"><span data-offset-key="foovs-0-0">Some of the workflows mentioned above are not all feature complete. </span><span class="hardreadability"><span data-offset-key="foovs-1-0">VMware will continue updating the vSphere Client outside of the normal vCenter Server release cycle through updates and patches.</span></span>

![](https://emadyounis.com/assets/img/2018/04/vSphere-Client-6.7.png?resize=1280%2C720)

</div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0">## Platform Services Controller UI

</div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0"><span data-offset-key="3e3ss-0-0">Say goodbye to the PSC UI (/PSC) and hello to one less client. The PSC functionality is now part of the vSphere Client and all functionality is now located under the Administration menu. </span><span class="hardreadability"><span data-offset-key="3e3ss-1-0">Certificate management has its own tab and all other PSC management is under the configuration tab</span></span><span data-offset-key="3e3ss-2-0">.</span>

![](https://emadyounis.com/assets/img/2018/04/PSC-UI.png?resize=1280%2C720)

</div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0">## Conclusion

</div><div class="" data-block="true" data-editor="4tuj3" data-offset-key="5gkg1-0-0"><span data-offset-key="d35hu-0-0">Now that you’ve seen what’s new in the vCenter Server Appliance 6.7, go and give it a spin. I’ll have more information shortly on enhancement mentioned above until then check out the following vSphere 6.7 resources: </span><span data-offset-key="d35hu-0-0">[release notes](https://docs.vmware.com/en/VMware-vSphere/6.7/rn/vsphere-esxi-vcenter-server-67-release-notes.html)</span><span data-offset-key="d35hu-0-0">, </span><span data-offset-key="d35hu-0-0">[documentation](https://docs.vmware.com/en/VMware-vSphere/index.html)</span><span data-offset-key="d35hu-0-0">, </span><span data-offset-key="d35hu-0-0">[videos](https://www.youtube.com/playlist?list=PLymLY4xJSThro4OL4w3PGSwsKi4zKT3Rs)</span><span data-offset-key="d35hu-0-0">, and </span><span data-offset-key="d35hu-0-0">[configuration maximums](https://configmax.vmware.com/home)</span><span data-offset-key="d35hu-0-0"> \[ links coming soon\]</span>

</div>
