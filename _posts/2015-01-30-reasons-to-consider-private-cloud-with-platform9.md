---
id: 336
title: 'Reasons to Consider Private Cloud with Platform9'
date: '2015-01-30T11:30:21-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=336'
permalink: /reasons-to-consider-private-cloud-with-platform9/
image: 'https://emadyounis.com/assets/img/2015/01/Platform9.jpg'
categories:
    - 'Tech Field Day'
tags:
    - 'Tech Field Day'
    - TFD
    - VFD
    - VFD4
    - 'Virtualization Field Day'
---

![Platform9](https://emadyounis.com/assets/img/2015/01/Platform9.jpg?resize=254%2C147)I have to be honest, [Platform9](http://platform9.com/) caught me by surprise at VFD4 with what seemed to be their answer for an AWS like offering. Their approach is private clouds should be simple without the worry of large IT staffs and ROI. Their solution is delivered onto your existing infrastructure using a SaaS managed approached to OpenStack. Of course; as you read this and think no way can a private cloud utilizing OpenStack be simple. Well tell that to the folks at Platform9, because they’re doing it. Here are 10 reasons to consider:

**<span style="color: #ff0000;">Disclaimer:</span> I was a delegate at Virtualization Field Day 4. Gestalt IT paid my travel, lodging, and meals. I don’t receive any compensation nor am I required to write anything related to the event.**

<span style="text-decoration: underline;">**1. Setup**</span> – Platform9 Managed OpenStack takes roughly five to ten minutes from start to finish. No previous experience of OpenStack or large IT staff required. Let that sink in because you saw OpenStack and no experience needed in the same sentence.

<span style="text-decoration: underline;">**2. Infrastructure**</span> – You can leverage your existing or new compute, storage (DAS, NFS, ISCSI), and network to create logical pools of resources. As an added bonus you can use your infrastructure from any geographical location. So far I’m impressed, but wait there’s more.

<span style="text-decoration: underline;">**3. Price**</span> – Not only are you provided with a simple setup and the ability to leverage your own hardware but reasonable pricing and 3 tier model too! This model includes Lite, Business, and Enterprise. Lite is free, yes free and provides a way to get familiar with Platform9’s services while learning OpenStack. Can you say lab environment. Business offers a production ready competitive price point of $49 per CPU per month with an annual commitment. Definitely competitive for this type of service. Enterprise uses the same model as the Business tier, but offers the most services including 24/7 email and phone support with SSO and other features coming soon. More information can be found [here](http://platform9.com/product/pricing.html)

**![Platform9-2](https://emadyounis.com/assets/img/2015/01/Platform9-2.jpg?resize=487%2C292)**

**<span style="text-decoration: underline;">4. Supported Platforms</span>** – Platform9 wants users to care about their applications and not the platform they’re running on. Which is why their vision of private cloud must be flexible and not limited to only a single virtualization platform or hypervisor. Supporting KVM, ESXi (currently in beta), and containers with Docker (coming soon) allows them to leverage greenfield and brownfield deployments. Hyper-V was mentioned several time during the discussion but there is no mention from Platform9, including their website when or if they plan on supporting, sorry [Jeff](https://twitter.com/agnostic_node1).

<span style="text-decoration: underline;">**5. Updates / Patches** </span>– One of the biggest pains is having to keep up with updates or patches, well don’t have to worry about it here. Platform9 does all the heavy lifting. Maintaining at least 1 version behind the latest releases and providing at least a months notice prior to update. They also maintain a regular patch cycle providing a weeks notice prior to patching. They also follow this approach internally within their own environment.

![Platform9 agent](https://emadyounis.com/assets/img/2015/01/Platform9-agent.jpg?resize=176%2C71)<span style="text-decoration: underline;">**6. Agents**</span> – The magic really starts when you download and install the bootstrap agent provided to your specific environment once you sign up for an account. These are light weight agents that will be installed on your Linux servers and in the case of vSphere an OVF appliance. A few things to keep in mind when it comes to the agents:

- You only have to install an agent on the servers you want to be part of your private cloud in Platform9.
- The agent then goes through a discovery process, gathering information from your infrastructure that will later be displayed on your dashboard.
- Any install packages will be discovered as well.
- Any changes made on the hosts outside the dashboard are still reported through the agents. It’s assumed that with root privileges also comes great responsibility.
- Security is built in. All communication is encrypted between the agent and the control plane (hosted by Platform9).

<span style="text-decoration: underline;">**7. Data**</span> – Is using a cloud based management tool to communicate to on-premises infrastructure safe for our data? The answer here is yes. Platform9 has absolutely no insight to your data. As stated above, the agent does a discovery of the infrastructure and reports back to the control plane via encrypted communication. Your data is your data and never leaves, so tell infosec to relax.

<span style="text-decoration: underline;">**8. Single Pane of Glass**</span> – Having multiple platforms to support will require multiple management tools right? Not the case here, Platform9 provides a single pane of glass to manage all platforms running within your infrastructure. I personally really enjoy the simple uncluttered look of the Web UI (based on HTML5). The Web UI is very responsive and lacks all the unnecessary animation. Here is a list of some of the features available through the self service portal:

- Two role based groups
- Dashboard displaying current and used resources of compute, memory, storage, and network
- Infrastructure mappings
- Host Role Assignment / Tagging
- Deployment Customization

![Platform9-4](https://emadyounis.com/assets/img/2015/01/Platform9-4.jpg?resize=1024%2C210)

**<span style="text-decoration: underline;">9. Monitoring / Troubleshooting</span>** – Platform9 continuously monitors your infrastructure and maintains its health but also can detect issues before you do. This causes more work for them but ensures that customers get the best service they can possibly get.

<span style="text-decoration: underline;">**10. Reporting**</span> – So with all these great features we didn’t hear anything about reporting. Well no solution will have everything day one but also keep in mind in order to keep things simple they can’t add all the bells and whistles. In the case of reporting you can currently leverage the APIs to create your own custom reports. Platform9 is also working towards adding custom reporting to all administrators to have more visibility into tenant usage and history.

<span style="text-decoration: underline;">**\*Bonus\***</span>

Since I mentioned 10 reasons above here are a few honorable mentions.

![rich_executive_who_just_got_a_bonus_or_raise_0521-1011-0416-4349_SMU](https://emadyounis.com/assets/img/2015/01/rich_executive_who_just_got_a_bonus_or_raise_0521-1011-0416-4349_SMU.jpg?resize=150%2C144)

<span style="text-decoration: underline;">**Security**</span> – How secure is this solution you ask? well the following measures are in place:

- Control plane communicates over HTTPS to receive meta data.
- Multi-factor authentication.
- Dedicated account per customer.
- Access to restricted networks.

<span style="text-decoration: underline;">**APIs**</span> – Native OpenStack API’s are available as well as some from Platform9.

<span style="text-decoration: underline;">**Backups**</span> – Control plane is backed up every night with a retention policy of 30 days.

When looking at Platform9 some VMware administrators will be quick to compare it to something like vRA (vCAC). Although both may provide some of the same features such as a self service portal and multi-tenancy, to name a few. Platform9 requires no initial knowledge and large IT department, where as vRA (vCAC) requires a lot of knowledge, skill set and time to fully have a private cloud in place. Platform9 also leverages the OpenStack community which allows them to add more features quickly, but they also plan on contributing so its a win-win situation. Another thing to consider is that Platform9 may not be for everyone, true they target customers with 50-5000 physical servers, but there may be some companies who’s culture is not ready or security team comfortable with allowing internet access to their private cloud. I plan to leverage Platform9 to deploy and learn OpenStack in my lab, especially since the Lite tier comes with a great price point (free).
