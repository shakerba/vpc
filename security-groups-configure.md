---

copyright:
  years: 2018, 2022

lastupdated: "2022-05-03"

keywords:  

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Setting up the security groups for your virtual server instance
{: #configuring-the-security-group} 

You can configure security groups to define the inbound and outbound traffic that they allow for your instance. For example, after you configure ACL rules for the subnet based on your company's security policies, you can further restrict traffic for specific instances depending on their workloads.
{: shortdesc}

## Setting up the security groups for your virtual server instance using the UI
{: #sgg-using-ui}
{: ui}

To configure your security group using the UI: 

1. In the navigation pane, click **Compute > Virtual server instances**.
1. Click your instance to view its details.
1. In the **Network interfaces** section, click the name of the security group.
1. On the **Rules** tab of the security group details page, click **Create** to configure inbound and outbound rules that define what type of traffic is allowed to and from the instance. For each rule, specify the following information:  
   * Select the protocols and ports to which the rule applies.    
   * Specify a CIDR block or IP address for the permitted traffic. Alternatively, you can specify a security group in the same VPC to allow traffic to or from all instances that are attached to the selected security group. 

   **Tips:**  
   * All rules are evaluated, regardless of the order in which they're added.
   * Rules are stateful, which means that return traffic in response to allowed traffic is automatically permitted. For example, you created a rule that allows inbound TCP traffic on port 80. That rule also allows replying outbound TCP traffic on port 80 back to the originating host, without the need for another rule.
   * For Windows images, make sure that the security group that is associated with the instance allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).
1. _Optional:_ To view interfaces that are attached to the security group, click **Attached resources** tab and review the Attached interfaces section.
1. When you finish creating rules, click the **Security groups** breadcrumb at the beginning of the page.

## Setting up the security groups for your virtual server instance using the CLI
{: #sg-using-cli}
{: cli}

In this example, you create a virtual server instance with a security group that is enabled by using the command-line interface (CLI). Figure 1 shows what this scenario looks like.

![Figure showing a virtual server instance with a security group enabled](/images/security-groups-schematic.svg){: caption="Figure 1. Instance with security group enabled" caption-side="bottom"}

Notice in Figure 1 that the instance named **SG4** has the floating IP `169.60.208.144` assigned to it, in addition to its internal VPC address `10.10.10.5`; therefore, **SG4** can talk to the public internet. The security group assigned to instance **SG4** is named `demosg`.

The instance **SG8** is internal-only to the VPC, with a private IP address. The security group assigned to instance **SG8** is named `my_vpc_sg`. Both of these instances exist within the VPC named `sgvpc` and also on the same subnet `10.10.10.0/24` so they can communicate with each other.

### Creating an instance with a security group attached
{: #steps-for-creating-a-vsi-with-a-security-group-attached}

The security group rules for `my_vpc_sg` include the basic functions of SSH, PING, and outbound TCP.

Notice that you must create the security group first, with the `ibmcloud is sgc` command, and then create the instance that uses this security group.

You must enter `ibmcloud plugin install vpc-infrastructure` to get access to `ibmcloud is`. For detailed information about creating a VPC and subnet, see [Creating a VPC by using the CLI](/docs/vpc?topic=vpc-creating-a-vpc-using-cli).
{: tip}

You can copy and paste commands from this example CLI code to begin creating an instance with an attached security group. System responses are not shown completely in this sample code. You must update your commands with the correct resource IDs for your VPC, subnet, image, key, and the correct security group ID number.

1. Create a security group called `my_vpc_sg`:

   ```sh
   ibmcloud is security-group-create my_vpc_sg $vpc
   ```
   {: pre}

   Save the ID in a variable so you can use it later; for example, in a variable named `sg`:

   ```sh
   sg=0738-2d364f0a-a870-42c3-a554-000000632953
   ```
   {: pre}

2. Add rules to allow SSH, PING, and outbound TCP:

   ```sh
   ibmcloud is security-group-rule-add $sg inbound tcp --port-min 22 --port-max 22
   ibmcloud is security-group-rule-add $sg inbound icmp --icmp-type 8 --icmp-code 0
   ibmcloud is security-group-rule-add $sg outbound tcp
   ```
   {: codeblock}

3. Finally, create an instance with the security group:

   ```sh
   ibmcloud is instance-create test-instance $vpc us-south-2 b-4x16 $subnet 1000 \
   --image $image --keys $key --security-groups $sg
   ```
   {: pre}

### Command list cheat sheet
{: #command-list-cheat-sheet}

For a complete list of the available VPC CLI commands for security groups, enter:

```sh
ibmcloud is help | grep sg
```
{: pre}

To see your security group and its metadata, including rules, you can enter (for the previous example):

```sh
ibmcloud is sg $sg
```
{: pre}

To add a security group rule, here's an example command for adding a PING inbound rule to a security group:

```sh
ibmcloud is security-group-rule-add $sg inbound icmp --icmp-type 8 --icmp-code 0

```
{: pre}

## Setting up the security groups for your virtual server instance using the API
{: #sg-using-api}
{: api}

The following example demonstrates how to create and manage security groups by using the {{site.data.keyword.vpc_short}} APIs.

To use security groups, first you must have a running {{site.data.keyword.vpc_short}}.

For instructions about creating a VPC and subnet, see [Creating a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis).

### Step 1: Create a security group
{: #step-1-create-a-security-group}

Create a security group named `my-security-group` in your {{site.data.keyword.vpc_short}}.

```sh
curl -X POST "$vpc_api_endpoint/v1/security_groups?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-security-group",
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Save the ID in a variable so you can use it later; for example, the variable `sg`:

```sh
sg=0738-2d364f0a-a870-42c3-a554-000000632953
```
{: pre}

### Step 2: Add a rule to allow SSH connections
{: #step-2-add-a-rule-to-allow-ssh-connections}

Create a rule on the security group to allow inbound connections on port 22.

```sh
curl -X POST "$vpc_api_endpoint/v1/security_groups/$sg/rules?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "direction": "inbound",
        "protocol": "tcp",
        "port_min": 22,
        "port_max": 22
      }'
```
{: pre}

### Step 3: Delete the security group (optional)
{: #step-3-delete-the-securiy-group-optional}

To clean up the security group, it cannot be associated with any network interfaces, and it cannot be referenced by a rule in a different security group.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/security_groups/$sg?version=$api_version&generation=2" \
  -H "Authorization: $iam_token"
```
{: pre}
