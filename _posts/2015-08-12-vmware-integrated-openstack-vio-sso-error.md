---
id: 603
title: 'VMware Integrated OpenStack (VIO) SSO Error'
date: '2015-08-12T08:56:50-07:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=603'
permalink: /vmware-integrated-openstack-vio-sso-error/
image: 'https://emadyounis.com/assets/img/2015/08/SSO-with-VIO-Graphic.png'
categories:
    - OpenStack
tags:
    - OpenStack
    - Ravello
    - VIO
    - VMware
    - vSphere
---

![](https://emadyounis.com/assets/img/2015/08/SSO-with-VIO-Graphic.png?resize=309%2C217)Like other VMware products, VIO leverages Single Sign On (SSO). Luckily, the SSO configuration with VIO is fairly simple. When used with vSphere 5.5 it requires IP or FQDN of your vSphere SSO along with port 7444. With vSphere 6.0 you don’t have to use a port, it’s that easy. I ran into this error while installing VIO. Here is the process I went through.

[![](https://emadyounis.com/assets/img/2015/07/VIO-SSO-Error.jpg?resize=322%2C140)](https://emadyounis.com/assets/img/2015/07/VIO-SSO-Error.jpg)

<span style="text-decoration: underline;">**Troubleshooting**</span>

Sometimes the first intuition when dealing with a lab or new deployment is uninstall. That may seem like a quick solution, but not always the best. What if the reinstall provides the same results, then you’re back to square one. I wanted to troubleshoot and confirm my setup was correct. This validation would help in case I ran into this issue again.

- Checked DNS entries are correct.
- Necessary firewall ports are open. Check the VMware’s documentation for the required firewall ports.
- Verified network communication from vCenter SSO and VIO management-server. This was done by validating by IP and FQDN (forward and reverse lookup).
- Verify that you have used the correct syntax when adding your SSO during the VIO install.
- Of course the obvious, review the logs (installer.log).

After going through these troubleshooting steps and not finding anything, I decided to uninstall VIO and reinstall. Not only will you have to uninstall VIO but there is also some clean up required. This ensures that the VIO plugin is removed from the vSphere web client prior to re-installing.

<span style="text-decoration: underline;">**Uninstalling VIO Correctly**</span>

1. Shutdown the VIO vAPP and delete from disk.
2. The VIO plugin remains in vCenter until its unregistered, go to **https://IP or FQDN of vCenter&gt;/mob/?moid=ExtensionManger**  
    <span style="color: #ff0000;">**Note:**</span> The Manage Object Browser (MOB), provides visibility to both ESXi and vCenter’s object runtime information via web browser.
3. Accept the untrusted cert, if not using self signed cert.
4. If prompted to login, use the administrator account same as the vSphere web client, for example administrator@vsphere.local.  
    [![](https://emadyounis.com/assets/img/2015/08/Mob-Login-VIO.jpg?resize=363%2C287)](https://emadyounis.com/assets/img/2015/08/Mob-Login-VIO.jpg)
5. Under the methods section click on Unregister Extension. A pop-up window will open for Unregister Extension.  
    [![](https://emadyounis.com/assets/img/2015/08/VIO-Unrgister-Extenion.jpg?resize=1264%2C584)](https://emadyounis.com/assets/img/2015/08/VIO-Unrgister-Extenion.jpg)
6. Under the properties section expand out the values by clicking on more.  
    **Before Expanding**  
    [![](https://emadyounis.com/assets/img/2015/08/VIO-Properties-More.jpg?resize=1264%2C276)](https://emadyounis.com/assets/img/2015/08/VIO-Properties-More.jpg)  
    **After Expanding**  
    [![](https://emadyounis.com/assets/img/2015/08/VIO-Properties-More-Expanded.jpg?resize=1241%2C558)](https://emadyounis.com/assets/img/2015/08/VIO-Properties-More-Expanded.jpg)
7. Unregister OpenStack extensions which include the following:  
    **com.vmware.openstack.vcext.instance-#**  
    **org.openstack.compute**  
    **com.vmware.openstack.ui**  
    **org.os.vmw.plugin**  
    <span style="color: #ff0000;">**Note:**</span> There could be several **vcext.instances** depending on how many times you tried to install VIO without removing the plugin.  
    **Before Unregister Extension**  
    [![](https://emadyounis.com/assets/img/2015/08/VIO-Unregister-Extension.jpg?resize=818%2C303)](https://emadyounis.com/assets/img/2015/08/VIO-Unregister-Extension.jpg)  
    **After Unregister Extension**  
    [![](https://emadyounis.com/assets/img/2015/08/After-VIO-Unrgister-Extension.jpg?resize=796%2C340)](https://emadyounis.com/assets/img/2015/08/After-VIO-Unrgister-Extension.jpg)
8. Once the extension is removed, refresh the MOB and the extension is removed. Rinse and repeat until all the OpenStack extensions have been removed from the MOB.
9. Logout and back in the vSphere web client and the VIO plugin should no longer be available.

As you can see removing the vApp is only part of the process. Redeploying without properly removing the VIO plugin from the vSphere web client will result in the same error or potentially others. This method can be used to uninstall other plugins from the vSphere web client as well. Make sure you know which extensions to remove or else you could cause more harm than good. After going through this process I was able to re-install VIO without any issues.
