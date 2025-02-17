---

copyright:
  years: 2021, 2022
lastupdated: "2022-03-30"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Bare metal server images
{: #bare-metal-image}

When you provision a bare metal server on your VPC, you need to select an image to determine the operating system for the server.
{: shortdesc}

## Supported images
{: #bare-metal-images-supported}

The following operating systems are available as images when you create a bare metal server.

| Image | Architecture |
|---|---|
| [RHEL 8.4](#bare-metal-images-rhel-considerations) | x86-64 |
| [VMware ESXi](#bare-metal-images-vmware-esxi-considerations) | x86-64 |
{: caption="Table 1. Bare metal server images" caption-side="bottom"}

Support for Windows, Linux, and custom images is planned. 
{: note}

### Special considerations for RHEL 8.4
{: #bare-metal-images-rhel-considerations}

* By default, the release lock feature for RHEL 8.4 is disabled. To prevent the RHEL from going beyond version 8.4 when you run an update, run the following commands from the command line:

   ```text
   # subscription-manager release --set=8.4
   ```
 {: codeblock}

   ```text
   # yum clean all
   ```
   {: codeblock}

* When you deploy a server with RHEL, it's possible that the deployment might not exit the pending state. If the server is in the pending state for longer than 15 minutes, delete the server and redeploy the server.

### Special considerations for VMware ESXi images 
{: #bare-metal-images-vmware-esxi-considerations}

You can specify how a bare metal server is licensed with VMware&reg; ESXi by either bringing your own license (_ESXi 7.x BYOL_), or you can rent a license through {{site.data.keyword.cloud}} (_ESXi 7.x_)

* The _ESXi 7.x BYOL_ option provides ESXi in an evaluation mode. The evaluation period is 60 days and begins at the time of provisioning. Anytime during the 60-day evaluation period, you can convert from evaluation mode to licensed mode with your appropriate license that you provide.

* The _ESXi 7.x_ option provides ESXi in licensed mode and is activated during the provisioning process. Billing applies for {{site.data.keyword.cloud}} rented licenses. 

ESXi on a Bare Metal Servers for VPC is charged monthly and is calculated per CPU based on the selected profile. If you choose to rent VMware ESXi with your server, you are subject to a prorated monthly cost for the license instead of an hourly rate. Proration amount is variable based on your billing anniversary date. 
{: note}

For more information about how to license ESXi, see [Licensing ESXi hosts](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.esxi.install.doc/GUID-28D25806-748B-49C0-97A1-E7DE5CB335A9.html){: external}.

You can view and manage your VMWare licenses [here](https://cloud.ibm.com/classic/devices/vmwarelicenses){: external}.

## Next steps
{: #bare-metal-images-next-steps}

* [Planning for bare metal servers](/docs/vpc?topic=vpc-planning-for-bare-metal-servers)
* [Creating a bare metal server](/docs/vpc?topic=vpc-creating-bare-metal-servers)
