---
id: 1153
title: 'vCenter Server 6.7 Update 1 Convergence Tool'
date: '2018-10-22T07:29:58-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=1153'
permalink: /vcenter-server-6-7-update-1-convergence-tool/
image: 'https://emadyounis.com/assets/img/2018/10/Convergence-Tool-Image.png'
categories:
    - vCenter
tags:
    - VCSA
    - 'VCSA 6.7'
---

The embedded deployment is now VMware’s recommended deployment model for vCenter Server. This means all the required vCenter Server services are running on the same node. The release of [vSphere 6.7 Update 1 introduces the convergence CLI tool](http://emadyounis.com/whats-new-in-vcenter-server-6-7-update-1/). Customers with an existing external deployment can now change their topology to embedded. This deployment model also supports Enhanced Linked Mode (ELM). In the past, an external Platform Services Controller (PSC) was only needed for ELM. This is no longer the case with a simplified deployment model. While we are on the subject, let me clear up a few misconceptions I’ve seen about embedded deployments.

- The first misconception is an embedded deployment is a new deployment model. The embedded deployment model is not a new concept. It has been around since the initial VirtualCenter (2004) deployment. Over time VMware introduced new vCenter Server services including the PSC. With the addition of the PSC services, the embedded deployment model did not support ELM. Greenfield deployments on vSphere 6.5 and 6.7 support embedded deployments with ELM.
- Another misconception is the maximums supported differs between the two deployment models. The embedded deployment supports the same vCenter Server maximums as the external deployment. This includes the same number of hosts, VMs, datastores, etc. there is no difference. Also, the embedded deployment supports a total of 15 vCenter Servers in a vSphere SSO domain.
- A third misconception is with a PSC failure, an embedded deployment is likely to fail. A PSC failure, whether external or embedded, has the same result. Users will no longer be able to authenticate to the registered vCenter Server(s). The probability of PSC service failing when embedded is slim. Using vCenter Server High Availability (VCHA) is the preferred method for protecting against these types of failures. This also removes the overhead of using a load balancer for PSC high availability.

## Convergence Tool Overview

\[<span style="color: #ff0000;">**Update:**</span> The vCenter Server Appliance convergence tool is now also included in [vCenter Server Update 2d](https://docs.vmware.com/en/VMware-vSphere/6.5/rn/vsphere-vcenter-server-65u2d-release-notes.html). Same steps below apply\]

The convergence tool is only available on the vCenter Server Appliance (VCSA) 6.7 Update 1 ISO. If you are familiar with the VCSA lifecycle JSON templates, the converge CLI tool is similar. There are two JSON templates, converge and decommission. In an external deployment, the PSC is running on a separate node. The necessary PSC RPMs are not installed on the registered vCenter Server. The converge tool will handle the installation of the PSC RPMs on the vCenter Server. There is no manual intervention required and it does not make any changes to the external PSC. After the installation of the PSC, it then registers with the vCenter Server.

Next is the replication of the vSphere SSO data from the external PSC to the embedded PSC. The PSC replication partner is part of the converge tool JSON template, more on that in a bit. Only during convergence is replication from an external PSC to embedded PSC supported. Once the replication is complete, it time to decommission the external PSC. Let’s walk through the steps in more detail.

**<span style="color: #ff0000;">Note:</span>** The convergence process can only run on one VCSA at a time.

## Prerequisites

There are a few prerequisites and considerations before starting the convergence process:

- The converge tool only supports the VCSA and PSC 6.7 Update 1. All nodes must be on 6.7 Update 1 before converting.
- If you are currently running a Windows vCenter Server or PSC, you must first migrate to the appliance.
- Before converting take a backup of your VCSA(s) and a least one of the PSCs in your vSphere SSO domain.
- Know all other solutions (i.e. SRM, vROPS, NSX, etc.) using the PSC for authentication in your environment. They will need to be re-registered after the convergence completes and before decommissioning.
- A machine on a routable network which can communicate with the VCSA and PSC will be used to run the convergence and decommission process.
- <span style="color: #000000;">It is recommended to change the DRS Automation Level to partially automated or manual and the Migration Threshold to conservative. There could be issues if the VCSA being converged is moved during the process.</span>
- If VCHA is enabled, it must be disabled prior to running the convergence process.
- The converge process will handle PSC HA load balancers, make sure you point to the VIP in the JSON template.
- All vSphere SSO data is migrated with the exception of local OS users.

**<span style="color: #ff0000;">Note:</span>** If the VCSA File-Based backup is running using the scheduler the convergence process will fail. You must delete the scheduler prior to starting the convergence process. It can be re-enabled after the convergence process is complete.

##  Convergence Steps

1. Backup all VCSAs and at least one PSC (multi-master) within your vSphere environment.
2. Mount the VCSA 6.7 Update 1 ISO on a machine with routable network access to the VCSA and PSC. The VCSA ISO supports macOS, Linux, and Windows.![](https://emadyounis.com/assets/img/2018/10/Converge-VCSA-ISO.png?resize=933%2C403)
3. Expand the vcsa-converge-cli directory and go to templates. Open the converge directory and copy the converge.json file to your local machine.![](https://emadyounis.com/assets/img/2018/10/Converge-VCSA-Directory.png?resize=908%2C616)
4. Open the local copy of the coverge.json file with your favorite editor. Fill out the JSON template with the required information. There are two sections to pay close attention to. The first is the “ad\_domain\_info” section. If your PSC is a member of an active directory domain, this section needs filled out. Otherwise, this entire section is not needed. The second section is the “replication” section of the JSON template. If there is only a single PSC with no replication partners, then this section is not needed. Use the [vdcrepadmin command](https://kb.vmware.com/s/article/2127057) to check the replication agreements if you are unsure. **<span style="color: #ff0000;">Note:</span>** Use ./vcsa-util converge – –template–help for additional help filling out the converge JSON template.
    
    ```
    <pre class="lang:default decode:true">{
        "__version": "2.11.0",
        "__comments": "Template for VCSA with external Platform Services Controller converge",
            "vcenter": {
                "description": {
                   "__comments": [
                        "This section describes the vCenter appliance which you want to",
                        "converge and the ESXi host on which the appliance is running. "
                    ]
                },
                "managing_esxi_or_vc": {
                    "hostname": "esx-02.cpbu.corp",
                    "username": "root",
                    "password": "P@ssw0rd"
                },
                "vc_appliance": {
                    "hostname": "vcsa-01.cpbu.corp",
                    "username": "administrator@vsphere.local",
                    "password": "P@ssw0rd",
                    "root_password": "P@ssw0rd"
                },
                "ad_domain_info": {
                    "__comments": [
                        "Important Note: This section is needed only when PSC (Platform Services Controller) appliance is joined to a domain.",
                        "Remove this section if PSC appliance is not joined to a domain.",
                        "Keeping this section without valid values results in JSON validation errors."
                    ],
                    "domain_name": "cpbu.corp",
                    "username": "emad",
                    "password": "P@ssw0rd",
                    "dns_ip": "192.168.13.50"
            },
        "replication": {
                "description": {
                   "__comments": [
                   "Important Note: Make sure you provide the information in this section very carefully, as this changes the replication topology.",
                   "Refer to the documentation for complete details. Remove this section if this is first converge operation in your setup.",
                   "This section provides details of the PSC node which will be set up as a replicated node for a new PSC on the target VCSA node."
                ]
                    },
                "partner": {
                    "hostname": "psc-02.cpbu.corp"
                }
            }
    }
    ```
    
    **<span style="color: #ff0000;">Note:</span>** You can leave the passwords for any of the sections blank in the template and will be prompted during the convergence process.
5. After completing the converge JSON template, make sure you save it.
6. Open a terminal window to the vcsa-util application path. Depending on the local operating system in use will determine the directory. In this example, I’m using macOS and using the vcsa-util under the mac directory.
7. Run ./vcsa-util converge – –help to get a list of the supported parameters.  
    ![](https://emadyounis.com/assets/img/2018/10/Converge-VCSA-Util.png?resize=1280%2C323)
8. To start the convergence process, use the correct parameters followed by the path of the converge JSON template you saved earlier. ```
    <pre class="wrap:true lang:default decode:true">./vcsa-util converge --no-ssl-certificate-verification --backup-taken --verbose /Users/eyounis/Documents/converge.json
    ```
9. Once the convergence process begins, your VCSA will not be available until the process is complete.![](https://emadyounis.com/assets/img/2018/10/Converge-Process-Started.png?resize=1234%2C618)
10. After the convergence process has completed, verify the VCSA is now running with an embedded PSC. There are two ways to verify. The first is using the VMware Appliance Management on port 5480. Go to the Summary tab and verify the type is “vCenter Server with an embedded Platform Services Controller”. A second option is using the vSphere Client (HTML5). Look under the Administration menu for System Configuration and verify the type as above.
11. If there are any products using the PSC for authentication they will need to be re-registered. Re-register them with the embedded VCSA deployment before decommissioning the external PSC. Also, do not decommission an external PSC until all VCSAs registered have gone through the convergence process.

## **Decommissioning Steps**

**<span style="color: #ff0000;">\*Verify the steps above have been completed prior to starting the decommissioning process\*</span>**

1. Expand the vcsa-converge-cli directory and go to templates. Open the decommission directory and copy the decommission\_psc.json file to your local machine.
2. Open the local copy of the decommission\_psc.json file with your favorite editor. Fill out the JSON template with the required information. If the default port of 443 was changed, make to enter the custom port that was used in the decommission template in the specified sections. ```
    <pre class="wrap:true lang:default decode:true">{
        "__comments": "Template for decommissioning PSC node with converge CLI tool.",
        "__version": "2.11.0",
            "psc": {
                "description": {
                   "__comments": [
                       "This section describes the PSC appliance which you want to",
                        "decommission and the ESXi host on which the appliance is running. "
                    ]
                },
                "managing_esxi_or_vc": {
                    "hostname": "esx-02.cpbu.corp",
                    "username": "root",
                    "password": "P@ssw0rd"
                },
                "psc_appliance": {
                    "hostname": "psc-01.cpbu.corp",
                    "username": "administrator@vsphere.local",
                    "password": "P@ssw0rd",
                    "root_password": "P@ssw0rd"
                }
            },
            "vcenter": {
                "description": {
                   "__comments": [
                        "This section describes the embedded vCenter appliance which is in ",
                        "replication with the provided PSC"
                    ]
                },
                "managing_esxi_or_vc": {
                    "hostname": "esx-02.cpbu.corp",
                    "username": "root",
                    "password": "P@ssw0rd"
                },
                "vc_appliance": {
                    "hostname": "vcsa-01.cpbu.corp",
                    "username": "administrator@vsphere.local",
                    "password": "P@ssw0rd",
                    "root_password": "P@ssw0rd"
                }
            }
    }
    ```
3. After completing the decommission JSON template, save it.
4. Open a terminal window to the vcsa-util application path, same steps as above.
5. Run ./vcsa-util decommission – –help to get a list of the supported parameters.
6. To start the decommission process, use the correct parameters followed by the path of the decommission JSON template you saved earlier. ```
    <pre class="wrap:true lang:default decode:true ">./vcsa-util decommission --no-ssl-certificate-verification --verbose /Users/eyounis/Documents/decommission_psc.json
    ```
7. The decommissioning process will begin removing the external PSC from the vSphere SSO domain. There are validations included to ensure there are no registered VCSAs to the external PSC being decommissioned. If any are found, the decommissioning process will fail.  
    ![](https://emadyounis.com/assets/img/2018/10/Decommision-Process.png?resize=1189%2C649)

## Summary

Great job by the vCenter Server engineering team for all their hard work on this tool. As stated earlier VMware’s recommended deployment model going forward is embedded. I would say the writing is on the wall for the external deployment now that the convergence tool is available. The next blog post will cover a few topology convergence examples. Happy converting 🙂
