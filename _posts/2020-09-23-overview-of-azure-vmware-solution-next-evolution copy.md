---
id: 2564
title: 'Overview of Azure VMware Solution Next Evolution'
date: '2020-09-23T11:20:22-07:00'
last_modified_at: '2021-03-18T12:00:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=2564'
permalink: /overview-of-azure-vmware-solution-next-evolution/
image: 'assets/img/Feature Images/Azure_VMware_Solution_Post_Feature.png'
categories:
    - 'Azure VMware Solution'
tags:
    - Azure
    - Cloud
    - Microsoft
    - 'VMware Cloud'
---

<span data-preserver-spaces="true">The general availability (GA) for the next evolution of Azure VMware Solution (AVS) was announced <del>yesterday</del> during the [Microsoft Ignite 2020](https://news.microsoft.com/ignite-2020-book-of-news/#158-next-generation-azure-vmware-solution-now-generally-available) virtual conference. This is a joint partnership between Microsoft and VMware, where Azure VMware Solution is a Microsoft managed service built on Azure bare metal infrastructure and cloud verified by VMware. The initial launch of the Azure VMware Solution in May of 2019 was by CloudSimple; this latest release is built and architected by Microsoft, providing an integrated experience with Azure services. Azure VMware Solution is currently available in the following 10 regions: East US, North Central US, West US, UK South, Japan East, West Europe, North Europe, Canada Central, Australia East, and Southeast Asia. More regions will be available in the future; additional details can be found by searching the </span>[<span data-preserver-spaces="true">Microsoft Products available by region page</span>](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=azure-vmware&regions=all)<span data-preserver-spaces="true">. Customers running the CloudSimple Azure VMware Solution version also have a </span>[<span data-preserver-spaces="true">migration path to this latest release</span>](https://docs.microsoft.com/en-us/azure/azure-vmware/faq)<span data-preserver-spaces="true">, leveraging VMware HCX.</span>

![](https://emadyounis.com/assets/img/2020/09/Azure-VMware-Solution-Releases.jpg?resize=723%2C362)

Azure VMware Solution is powered by VMware Cloud Foundation: vCenter Server, vSphere, vSAN, and NSX-T. Also included is VMware HCX, the Swiss army knife of workload mobility. Customers can securely extend their networks and migrate workloads from on-premises (vSphere 6.x -7.x) to AVS or between AVS private clouds in different regions using a combination of migration options. Microsoft will handle the billing, lifecycle operations (upgrades), and troubleshooting of the service, allowing customers to focus on their workloads.

> Note: Updated 3/18/21 to include changes to the service.
{: .prompt-info }
## Getting Started

<span data-preserver-spaces="true">There are a few things you need to have in place before you can deploy an Azure VMware Solution private cloud. First, an </span>[<span data-preserver-spaces="true">Azure account</span>](https://azure.microsoft.com/en-us/free/)<span data-preserver-spaces="true">, which you can get for FREE. Next, your account must be associated with a </span>[<span data-preserver-spaces="true">subscription</span>](https://docs.microsoft.com/en-us/azure/azure-vmware/enable-azure-vmware-solution#eligibility-criteria)<span data-preserver-spaces="true"> that is either part of a Microsoft enterprise agreement or a Cloud Solution Provider (CSP). Finally, is requesting host quota for your subscription. Once quota is applied to a subscription, search for “Azure VMware Solution” in the Azure portal’s search box. Alright, you don’t need to type out the entire thing; it will appear within the first couple of characters in VMware. An option to deploy your first private cloud in your subscription will be displayed on the next screen. As part of the private cloud creation, there is some basic information needed:</span>

- **<span data-preserver-spaces="true">Subscription </span>**<span data-preserver-spaces="true">– Billing framework that provides entitlement to deploy and consume Azure resources</span>
- **<span data-preserver-spaces="true">Resource Group </span>**<span data-preserver-spaces="true">– Logical way to group Azure services</span>
- **<span data-preserver-spaces="true">Location – </span>**<span data-preserver-spaces="true">Where to deploy the private cloud</span>
- **<span data-preserver-spaces="true">Resource Name </span>**<span data-preserver-spaces="true">– Name of your private cloud</span>
- **<span data-preserver-spaces="true">SKU </span>**<span data-preserver-spaces="true">– Node type used during deployment</span>
- <span data-preserver-spaces="true">**ESXi Hosts** – Number of hosts to deploy, min of 3 by default with the option to increase to a max of 16 per cluster</span>
- <del>**<span data-preserver-spaces="true">vCenter Admin Password </span>**<span data-preserver-spaces="true">– password used to log in vCenter with cloudadmin@vsphere.local</span></del>
- <del>**<span data-preserver-spaces="true">NSX-T Manager Password </span>**<span data-preserver-spaces="true">– password used to log in NSX-T manager with admin </span></del>
- **<span data-preserver-spaces="true">Address Block </span>**– <span data-preserver-spaces="true">CIDR block used when deploying management components, requires a /22</span>
- **<span data-preserver-spaces="true">Virtual Network </span>**<span data-preserver-spaces="true">– A representation of cloud networking and provides abstraction and logical isolation. An Azure environment can contain multiple VNets.</span>


{: .prompt-info }
> Note: Providing vCenter Server and NSX-T credentials during a private cloud deployment is no longer required, they are now auto generated. If they need to be rotated you will currently need to open a support request.

**\[Previous private cloud creation screen\]**

![](https://emadyounis.com/assets/img/2020/09/Create-Private-Cloud-Final-Image.jpg?resize=1718%2C1810)

**\[Updated private cloud creation screen\]**

## ![](https://emadyounis.com/assets/img/2020/09/Updated-AVS-Deployment.jpg?resize=1840%2C1738)Private Cloud

- **<span data-preserver-spaces="true">ExpressRoute </span>**<span data-preserver-spaces="true">– Is a private and secure connection from a customer’s physical datacenter providing dedicated bandwidth into Microsoft Azure. </span>

- **<span data-preserver-spaces="true">Global Reach </span>**<span data-preserver-spaces="true">– Connects ExpressRoute bi-directionally from a customer’s environment to Azure VMware Solution. </span>
  
A subscription can have 1-4 private clouds, each with a maximum of 4 clusters per cloud. An initial private cloud deployment starts with a 3-node minimum with the opportunity to add additional nodes during or scale-up later to a maximum of 16 nodes per cluster in the Azure portal. The hardware specification dropdown lists AVS36 as the current selectable node type. Below is a visual representation of the hardware specs for the AVS36, but it does not represent the actual server 🙂 An Azure Virtual Network (VNet) can be created during the initial private cloud deployment or afterward. If an existing VNet exists, it can also be used. A VNet is created to support an ExpressRoute from Azure VMware Solution to connect to other Azure services and allow connectivity back to an on-premises environment via Azure Global Reach. Once ready, click review and create. After you verify everything entered is correct, click the magical create button and wait for roughly 2+hrs for the process to complete. The process is mostly self-service from the Azure portal allowing you to get from zero to a private cloud which includes:

- <span data-preserver-spaces="true">Provisioning of hardware and backbone networking</span>
- <span data-preserver-spaces="true">Installation and configuration of ESXi, vCenter Server, vSAN, NSX-T, and HCX</span>
- <span data-preserver-spaces="true">Creation of initial cluster including vSAN datastore encryption</span>

{: .prompt-info }
>Note: if you don’t have an Azure ExpressRoute, you can use a site-to-site VPN to connect to Azure VMware Solution private cloud, but you will not be able to use HCX for workload migration as this is not supported.   

![](https://emadyounis.com/assets/img/2020/09/AVS-Server.jpg?resize=703%2C560)

## Post Deployment

When your private cloud is ready, you’ll be redirected to the overview page in the resource menu. This page is handy with valuable information; you can always come back here by searching your private cloud name or simply bookmarking. The first section we’ll want to select is identity. Here is where you’ll find your login information for NSX Manager and vCenter Server. Next is clustering, where you can edit (aka increase/decrease) the number of nodes in a private cloud, with 3 being the magic number of minimum nodes. Keep in mind increasing the number of nodes is tied to your allocation associated with the subscription used.

![](https://emadyounis.com/assets/img/2020/09/Azure-VMware-Solution-Identity-Final-Image.jpg?resize=966%2C555)

The NSX T-1 router is where all workload network segments need to be created before VMs are deployed or being migrated to a new segment. These segments can be created in NSX manager or directly in the Azure Portal under segments, including the ability to create DHCP servers to handle DHCP requests and DHCP relay services to relay DHCP traffic to external DHCP servers. Additional workload networking options such as port mirroring and DNS are also available.

![](https://emadyounis.com/assets/img/2020/09/AVS-Workload-Networking-scaled.jpg?resize=2560%2C1385)

I’ve mentioned VMware HCX; the good news, it’s automatically deployed as part of the private cloud provisioning. The HCX Cloud manager is where you’ll get the necessary HCX Connector bits to deploy in your on-premises environment, which can be found under the connectivity section in the Azure portal of your private cloud. The HCX Cloud manager address is provided, and you will use cloudadmin@vsphere.local credentials to login and download the HCX connector bits. Licensing your HCX connector is also part of the self-service offering, allowing you to request up to 3 advanced licenses. Additionally, there is an option to upgrade your HCX advanced license to an HCX enterprise license by opening a support request with Microsoft. HCX enterprise will provide features like mobility groups, replication assisted vMotion, mobility optimized networking, and more. These features make use cases like datacenter extension and evacuation easier to any VMware powered cloud.

![](https://emadyounis.com/assets/img/2020/09/Azure-VMware-Solution-HCX-Final-Image.jpg?resize=747%2C411)

Once the HCX connector is deployed and configured on-premises, customers can access and manage directly from the vSphere client, the dedicated HCX Client, or via automation using PowerCLI or REST APIs. A site pairing can then be established with on-premises environments and an Azure VMware Solution private cloud, followed by L2 connectivity for workloads that will retain their IP addresses. Let the migration planning begin!

![](https://emadyounis.com/assets/img/2020/09/Azure-VMware-Solution-vCenter-Final-Image.jpg?resize=1226%2C776)

<span data-preserver-spaces="true">There are additional support and licensing benefits that come with Azure VMware Solution. </span>[<span data-preserver-spaces="true">Microsoft</span>](https://support.microsoft.com/en-us/help/4456242/end-of-support-for-sql-server-2008-and-sql-server-2008-r2)<span data-preserver-spaces="true"> is providing extended support for Windows Server 2008 and SQL SQL 2008 workloads when they are migrated to Azure, including the Azure VMware Solution. There is also the </span>[<span data-preserver-spaces="true">Azure Hybrid Benefit</span>](https://azure.microsoft.com/en-us/pricing/hybrid-benefit/)<span data-preserver-spaces="true"> which is a licensing benefit allowing customers to use their on-premises Windows Server and SQL Server licenses on Azure.</span>

## Resources

Here are a few resources help get you started in learning more about Azure VMware Solution and more will be added as they are available. Also please reach out if you know of any others!

- [VMware Hands On Lab Azure VMware Solution](http://hol.pub/avs)
- [Platform updates for Azure VMware Solution](https://docs.microsoft.com/en-us/azure/azure-vmware/azure-vmware-solution-platform-updates?WT.mc_id=enterprise-0000-shkuehn)
- [FAQ: Common questions about Azure VMware Solution](https://docs.microsoft.com/en-us/azure/azure-vmware/faq)
- [VMworld: Azure VMware Solution Networking &amp; Security in a Hybrid Cloud Environment](https://www.vmworld.com/en/video-library/video-landing.html?sessionid=1586528544020001TS4T)
- [Azure VMware Solution Product Page](https://azure.microsoft.com/en-us/services/azure-vmware/)
- [Azure VMware Solution Documentation](https://docs.microsoft.com/en-us/azure/azure-vmware/)
- [Video Playlist from Microsoft Global Blackbelt Trevor Davis](https://www.youtube.com/watch?v=qASXi5xrFzM&list=PLS9k3ksxRe_l-UpfAjmi0BoDSpo6AtLyh)
- [Virtual Workloads Blog – Trevor Davis](https://www.virtualworkloads.com/)
- [Microsoft Ignite 2020 Session: Accelerate your Cloud Journey with Azure VMware Solution](https://myignite.microsoft.com/sessions/adcaabd7-9038-45d0-8e41-cf5fa3be5f1e)
- [Microsoft Ignite 2020 Session: An introduction to Azure VMware Solution](https://myignite.microsoft.com/sessions/1515e183-53a5-49e0-b39a-34c81d913ed2)
- [Set up vRealize Operations for Azure VMware Solution](https://docs.microsoft.com/en-us/azure/azure-vmware/vrealize-operations-for-avs)
- [Deploying VMware Horizon on Azure VMware Solution](https://docs.vmware.com/en/VMware-Horizon/2006/horizon-installation/GUID-76CF25F7-B26A-4C6D-A5C5-F18D3A56590A.html)
