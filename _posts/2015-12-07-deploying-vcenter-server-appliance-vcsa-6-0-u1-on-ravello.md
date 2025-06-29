---
id: 695
title: 'Deploying vCenter Server Appliance (VCSA) 6.0 U1 on Ravello'
date: '2015-12-07T07:00:01-08:00'
author: 'eyounis'
layout: post
guid: 'http://emadyounis.com/?p=695'
permalink: /deploying-vcenter-server-appliance-vcsa-6-0-u1-on-ravello/
image: 'https://emadyounis.com/assets/img/2015/12/VCSA-Ravello-feature-e1449433742639.jpg'
categories:
    - vCenter
tags:
    - Appliance
    - Ravello
    - VCSA
    - 'VCSA 6.0'
    - VMware
---

![](https://emadyounis.com/assets/img/2015/12/VCSA-Ravello.png?resize=183%2C203)

This blog post allows me to bring two of my favorite things together. I have been a fan of the vCenter Server Appliance (VCSA) since its release. With continuous improvements, it’s evident that VCSA is the future. My other favorite thing is <span style="color: #0000ff;">**[Ravello](http://www.ravellosystems.com)**</span>. They have given us the ability to have a home lab in the cloud, extend our current labs, and share blueprints. Two great products, that up to this point were not able to be used together. This is like having peanut butter on one slice and jelly on the other, and eating them individually.

![](https://emadyounis.com/assets/img/2015/12/2015-12-03_19-49-36-bigger.jpg?resize=720%2C354)

We all know the awesomeness that happens when you combine the two slices. This is the same effect when bringing together the VCSA and Ravello. This wasn’t an easy task, consisting of several attempts and asking lots of questions, but it’s now doable!

**The Problem**

With the VCSA 6.0 VMware changed the deployment format from OVA/OVF to ISO. This introduces some problems when trying to directly import on Ravello. Some of the problems are:

- ISOs are a supported format on Ravello, but there’s no way to access the VCSA files natively.
- Currently there’s no support for uploading an OVA. VCSA OVA format (vmware-vcsa) located on ISO.
- Deploying a VCSA to a ESXi host, Workstation, Fusion, or vSphere environment and export to OVF. This results in the following error:

[![](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-Error.jpg?resize=598%2C255)](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-Error.jpg)

The problem is the number of disks. The VCSA requires 11 disks, while Ravello currently supports a maximum of 7 disks per HBA on their end.

Fear not, there’s a way to get the VCSA on Ravello!

**Considerations**

There are a few things you should consider before starting this process.

- Naming – When deploying your VCSA once you provide it a FQDN or IP address it can’t be changed, at least in a supported manner. The PNID is tied to the FQDN or IP address and is referenced in many places (**<span style="color: #0000ff;">[KB2130599](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2130599) &amp; [KB2124422](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2124422)</span>**<span style="color: #000000;">). The only option is to redeploy.</span>
- DNS, DNS, DNS – Make sure forward and reverse DNS is configured.
- NTP – Configuring time is important for troubleshooting, configuration management, and security.

<span style="color: #ff0000;">**Note:**</span> **I deployed my VCSA based on my home lab domain, using a FQDN. I knew that I would be building the same domain in Ravello. If you aren’t sure or not planning on doing the same, your best bet is to deploy using an IP address.**

**VCSA Prep Work**

1. Deploy the VCSA to an ESXi host, vCenter Server (6.0 U1), Workstation, Fusion, or vSphere environment. My example is based on deploying to my vSphere lab.
2. Once your VCSA has been deployed, power it off.
3. Add a second SCSI controller to the VCSA.  
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-SCSI.jpg?h=625)](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-SCSI.jpg)
4. Move Hard disks **8-11** to the new SCSI controller (SCSI controller 1) .\[table id=6 /\]
5. Edit the VCSA settings in the vSphere web client. 
    - Click Manage other disks, and expand the hard disk you’re making the change to.
    - Change the Virtual Device Node to SCSI controller 1 and the next available SCSI Bus / Device number starting with 1:0.
    
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-SCSI-Disks.jpg?h=403)](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-SCSI-Disks.jpg)  
    Here’s a logical view of the VCSA hard disks under their respected SCSI controller from the vSphere C# client.  
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-Hard-Disk.jpg?h=623)](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-Hard-Disk.jpg)  
    <span style="color: #ff0000;">**Note:**</span> **Ravello will automatically add a second HBA on their end to support the newly added SCSI controller.**
6. Power on the VCSA, make sure that it’s still functional and you have access to the web client. Once that’s been verified power off the VCSA.
7. Export the VCSA as a OVF.  
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-OVF.jpg?h=396)](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-OVF.jpg)
8. Select the export directory and verify format is OVF. Click OK.  
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-OVF-Config.jpg?h=290)](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-OVF-Config.jpg)
9. Convert stream-optimized VMDK disks to non-stream-optimized disks before uploading them to Ravello. Per the following Ravello [**KB article**](https://support.ravellosystems.com/hc/en-us/articles/207785138-Converting-stream-optimized-VMDK-disks)<span style="color: #000000;">. Download and install QEMU for windows or Linux and run the following command: </span>qemu-img convert -p -f vmdk -O vmdk “original vmdk file” “new vmdk file”
    
    <span style="color: #ff0000;">**Note:**</span> **This must be done for all 11 VCSA disks.**  
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-Stream-Command.jpg?h=417)](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-Stream-Command.jpg)
10. Now there should be 22 VMDK disks, 11 original &amp; 11 new. I recommend backing up the 11 original in case you need them at a later date. I created a backup directory and moved them there. Now rename the new VMDK disks to the original names.  
    <span style="color: #ff0000;">**Note:**</span> **Another option is to create the new VMDK disks in a separate directory, this will save time with having to rename. You will have to copy the .mf and .ovf files over.**  
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-Disk-Conversion.jpg?h=591)](https://emadyounis.com/assets/img/2015/11/Ravello-VCSA-Disk-Conversion.jpg)
11. The manifest file (.mf) stores the SHA1 hases for each disk and needs to be updated. Microsoft has a free tool called <span style="color: #0000ff;">**[File Checksum Integrity Verifier (FCIV)](https://www.microsoft.com/en-us/download/details.aspx?id=11533)**</span><span style="color: #000000;">.</span>fciv.exe -sha1 “path of the vmdk”
    
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-SHA1.jpg?h=306)](https://emadyounis.com/assets/img/2015/11/Ravello-SHA1.jpg)
12. Open the manifest file (.mf) with your favorite editor and replace each VMDK hash with the newly generated one.  
    [![](https://emadyounis.com/assets/img/2015/12/Ravello-Edit-Manifest.jpg?h=298)](https://emadyounis.com/assets/img/2015/12/Ravello-Edit-Manifest.jpg)

Finally, done with the prep work and ready to start getting this bad boy on Ravello.

**Importing VCSA on Ravello**

1. Login the Ravello VM Import Tool and select Upload a VM from OVF file or Ravello Export file. Click Next.  
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-Upload-VMs.jpg?h=650)](https://emadyounis.com/assets/img/2015/11/Ravello-Upload-VMs.jpg)
2. Navigate to the VCSA directory and select the OVF. Click Upload.  
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-Select-VM-Upload.jpg?h=649)](https://emadyounis.com/assets/img/2015/11/Ravello-Select-VM-Upload.jpg)
3. Away we go! The upload process has started, sit back and relaxed you’ve earned it. Plus there’s nothing more to do until the upload is finished.  
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-Upload-Progress.jpg?h=247)](https://emadyounis.com/assets/img/2015/11/Ravello-Upload-Progress.jpg)

**Ravello VCSA Configuration**

1. Login your Ravello account and create a new application. I created one called VCSA 🙂
2. Prior to adding the VCSA to the newly created application we need to verify its configuration. Go to Library –&gt; VM and select the VCSA that was just uploaded. One the right hand side click Edit &amp; Verify VM.  
    [![](https://emadyounis.com/assets/img/2015/11/Ravello-Verify-Configuration.jpg?h=387)](https://emadyounis.com/assets/img/2015/11/Ravello-Verify-Configuration.jpg)
3. Select the System tab, here you can increase the number of CPUs or RAM. By default a tiny deployment starts with 2 CPUs and 8GB of RAM.
4. Go to the Disk tab and verify that the VCSA hard disks are in order. If not place them in the correct number order using the arrows.
5. Select the Network tab and add a static IP address, Network Mask, Gateway, and DNS.
6. Go to the Services tab, add the following services  
    \[table id=7 /\]
7. After verifying all setting, save changes and click Publish.  
    [![](https://emadyounis.com/assets/img/2015/12/Ravello-Publish-VCSA.jpg?h=255)](https://emadyounis.com/assets/img/2015/12/Ravello-Publish-VCSA.jpg)
8. Start the VCSA and launch the console when available.
9. You will be greeted with the following warning message, for now press any key to continue. The VCSA will continue the start up process.  
    [![](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Console-Message.jpg?h=315)](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Console-Message.jpg)
10. Login the VCSA console (Alt-F1).  
    [![](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Console.jpg?h=337)](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Console.jpg)
11. Type “**shell.set –enabled True**“, hit enter and type “**shell**”  
    [![](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Config-1.jpg?h=537)](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Config-1.jpg)
12. Edit the boot.compliance script **vi /etc/init.d/boot.compliance**  
    [![](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Config-2.jpg?h=540)](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Config-2.jpg)
13. Changes will be made to lines 47 and 48.  
    [![](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Config-3.jpg?h=527)](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Config-3.jpg)
14. Line 47 should read **MSG = ‘/usr/bin/isCompliant -q’**
15. Line 48 should read **CODE=0**  
    [![](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Config-5.jpg?h=525)](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Config-5.jpg)
16. Save changes and exit.  
    [![](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Config-6.jpg?h=532)](https://emadyounis.com/assets/img/2015/12/Ravello-VCSA-Config-6.jpg)
17. Restart the VCSA to verify you no longer receive the boot warning.
18. Now that the VCSA configurations are all done, time to save your hard work to the VM library for future use.  
    [![](https://emadyounis.com/assets/img/2015/12/VCSA-Ravello-to-Library.jpg?h=303)](https://emadyounis.com/assets/img/2015/12/VCSA-Ravello-to-Library.jpg)

**Web Client Access**

1. To access the web client, from a web browser type “https://public IP address of the VCSA/vsphere-client”.
2. Accept the self signed certificates.
3. If you’re having issues accessing the web client, then there’s an issue with your DNS. Easy test is to edit the host file on your local machine adding the public IP address of the VCSA. Refresh your browser and see if you have access to the web client.  
    [![](https://emadyounis.com/assets/img/2015/12/Ravello-Web-Client.jpg?h=539)](https://emadyounis.com/assets/img/2015/12/Ravello-Web-Client.jpg)  
    <span style="color: #ff0000;">**Note:**</span> Don’t forget that VCSA public IP address will change, so if it’s added to the host file on your machine you will need to change it.

There you have it, the VCSA can now be deployed directly on Ravello providing even more flexibility. Have fun building your lab in the cloud, I know I will.
