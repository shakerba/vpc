---

copyright:
  years: 2021, 2022
lastupdated: "2022-02-16"

keywords: custom image, creating a custom image, migrating a custom image

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:external: target="_blank" .external}
{:download: .download}

# Planning for custom images
{: #planning-custom-images}

When you're planning to provision custom images in {{site.data.keyword.vpc_full}}, you might find this configuration checklist helpful in preparing your custom images and understanding the process for importing to VPC. The image requirements are the same whether you are creating a custom image from scratch or using an image template from IBM Cloud classic infrastrucure to 
migrate an instance to VPC. 
{: shortdesc}

ISO images of licensed operating systems, such as Windows and Red Hat Enterprise Linux, and open source operating systems, such as CentOS and Ubuntu, are not provided by IBM. If you need these ISO images, you can download them from the respective vendor website.

| Task              | Details           |
|-------------------|-------------------|
| ☐ Review custom image [requirements](/docs/vpc?topic=vpc-about-images#custom-image-reqs). | The image must contain a single file or volume, and be cloud-init enabled. The image can be 10 GB to 250 GB. Images below 10 GB will be rounded up to 10 GB. |
| ☐ Choose a supported operating system.| Make sure that your image is supported as a [stock image](/docs/vpc?topic=vpc-about-images#stock-images) in VPC. If you choose to create a custom image and use you own license, select the _BYOL_ option. |
| ☐ Provision an instance of {{site.data.keyword.cos_full_notm}} if you don't have one. | You must also create an IAM authorization between the Image Service for VPC and Cloud Object Storage. For more information, see [Granting access to IBM Cloud Object Storage to import images](/docs/vpc?topic=vpc-object-storage-prereq). |
| ☐ Create a Linux custom image. | For more information, see [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image). |
| ☐ Or create a Windows custom image. | For more information, see [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image). | 
| ☐ Or use an image template from IBM Cloud classic intrastructure. | For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc). |
| ☐ Optionally, encrypt your custom image with LUKS encryption and your own passphrase. | Importing encrypted VHD images is not supported. For more information, see [Creating an encrypted custom image](/docs/vpc?topic=vpc-create-encrypted-custom-image). |
| ☐ Optionally, generate a SHA256 checksum locally for validating your image. You can compare the output with the checksum that's generated for the image when it's imported.| Example Linux command: `sha256sum ubuntu_image.qcow2` <br> Example Mac command: `shasum -a 256 ubuntu_image.qcow2` <br> You receive output similar to: `6809606da67eb83670e6249e54e94043eb43c0471669fb96ea4050c4c07e2df7`.  |
| ☐ Upload your custom image to Cloud Object Storage. | On the **Objects** page of your IBM Cloud Object Storage bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB. |
| ☐ Import your custom image to VPC. | When you have a supported custom image available in Cloud Object storage, you are ready to import. See [Importing a custom image](/docs/vpc?topic=vpc-managing-images#import-custom-image). |
| ☐ Create a virtual server by using your custom image. | For more information, see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers). |

## Next steps
{: #next-plan-custom-images}
When you're ready to get started, see one of the following topics:
 * [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image)
 * [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image)
 * [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc)
