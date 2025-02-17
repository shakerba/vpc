---

copyright:
  years: 2018, 2020
lastupdated: "2020-09-17"

keywords: ssh public keys, OpenSSH, add ssh key, ssh key, manage ssh key, virtual server instance, instance, virtual servers, vsi, virtual machines, server

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# SSH keys
{: #ssh-keys}
[comment]: # (linked help topic)

When you create a virtual server instance, you must select an existing SSH key or add a new SSH key. SSH keys are used by virtual servers to identify a user or device through public-key cryptography. SSH keys are made up of an alpha-numeric combination and are unique to the device to which they are assigned. You can add, edit, or delete SSH keys by using the {{site.data.keyword.cloud}} console.

By adding an SSH key to a virtual server instance, the instance can be accessed with the corresponding SSH key instead of a password. You can add SSH keys to a virtual server instance only when you initially create the instance. After a Linux instance is created, you can edit keys directly in the `~/.ssh/` directory of the instance.
{: shortdesc}

Creating a virtual server instance with a password option for connecting is not supported. You must specify an SSH key when provisioning the instance and use the private key to connect to the instance after it is created. 
{: note}

## Locating or generating your SSH key
{: #locating-ssh-keys}

Before you can add a key in the {{site.data.keyword.cloud_notm}} console, you must have your SSH key available. Your SSH key must be an RSA key with a key size of either 2048 bits or 4096 bits.

To locate your SSH key or generate an SSH key, complete one of the following steps.

 * Locate an SSH key: Look for a file called `id_rsa.pub`. It might be in an `.ssh` directory under your home directory, for example, `/Users/<USERNAME>/.ssh/id_rsa.pub`. The content of the file typically starts with `ssh-rsa` and ends with your user ID.  

* Generate an SSH key: If you don't have a public SSH key or if you forgot the password of an SSH key, generate a new one by running the `ssh-keygen` command and following the prompts. 

    For example, you can generate an SSH key on your Linux or Mac system by running the command `ssh-keygen -t rsa -C "user_ID"`.

    If your Mac system, generates a key size of 3072 bits (by default), run the following command to ensure the generated key is a supported size: `ssh-keygen -t rsa -b 4096 -C "user_ID"`.
    {: tip}

    You can press Enter to accept the default location for the file. The command generates two files. The generated public key is in the `<your key>.pub` file. For Windows systems, you can use [PuTTYgen](https://www.ssh.com/ssh/putty/windows/puttygen){: external} to generate an SSH key.

    If you are using OpenSSH version 7.8 or higher and plan to to access a Windows instance, use the following command to generate the key in PEM format. `$ssh-keygen -m PEM -t rsa -C "user_ID"`
    {: important}

When you copy an SSH key from a terminal to add the key to your VPC, sometimes extra line breaks are introduced which cause a parsing error. To avoid this issue, first paste your SSH key into a text editor and remove any extra line breaks. Then, copy the SSH key from text editor and paste it into the VPC UI, CLI, or API.
{: tip}

## Next steps
{: #next-steps-ssh}

After you choose a profile, it's time to plan for and create an instance. 
* [Planning for instances](/docs/vpc?topic=vpc-vsi_best_practices)
* [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers-cli)
