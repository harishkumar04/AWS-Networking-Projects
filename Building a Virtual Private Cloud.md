# Creating a VPC in AWS

AWS automatically provides a **Default VPC** in every region. While it’s convenient for quick testing, real-world architectures usually require **custom VPCs** so you can control IP ranges, networking, and security.

In this guide, we’ll create our **own VPC**, add a subnet, connect it to the internet, and finally clean everything up using **AWS CloudShell**.

---

## What We Are Building

* **VPC CIDR:** `10.0.0.0/16`
* **Subnet CIDR:** `10.0.0.0/24`
* **Internet Gateway:** To enable internet access
* **Subnet Type:** Public subnet

---

## Creating a VPC

A **VPC (Virtual Private Cloud)** is your own isolated virtual network in AWS.
Think of it like a **city** where all your cloud resources live.

We’ll create a VPC with:

* **IPv4 CIDR only**
* **No IPv6 CIDR**

**VPC CIDR:** `10.0.0.0/16`

<img width="1440" height="698" alt="Screenshot 2026-02-03 at 9 34 18 PM" src="https://github.com/user-attachments/assets/8b65ae52-1d00-4719-a21c-c198a8f79a2d" />

---

## Creating a VPC Using AWS CloudShell

```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/24 --query Vpc.VpcId --output text
```

### What this command does

* Creates a VPC with the specified IPv4 CIDR block
* Returns **only the VPC ID**, making it easy to reuse in later commands

### Flags Explained

* `--cidr-block` → Defines the IPv4 address range for the VPC
* `--query Vpc.VpcId` → Extracts only the VPC ID from the response
* `--output text` → Outputs the result in plain text (script-friendly)

---

## Adding a Name to the VPC

By default, AWS resources are unnamed. Adding a name makes them easier to identify in the console.

```bash
aws ec2 create-tags --resources=VPC-ID --tags Key=Name,Value="VPC 2"
```

📌 Replace `VPC-ID` with the output from the previous command.

---

# Creating a Subnet Inside a VPC

If a VPC is a **city**, then **subnets** are different **neighborhoods** inside that city.

You use subnets to:

* Group resources
* Control access rules
* Separate public and private infrastructure

The **default VPC** already comes with subnets in every Availability Zone.
Since we’re using a **custom VPC**, we must create our own subnet.

**Subnet CIDR:** `10.0.0.0/24`

<img width="1436" height="702" alt="Screenshot 2026-02-03 at 9 31 54 PM" src="https://github.com/user-attachments/assets/d8a666eb-98ff-49fa-9117-a9877ab78bca" />

---

## Public vs Private Subnets

### Public Subnet

* Has a route to the internet
* Resources can communicate with external networks
* Commonly used for web servers, bastion hosts, load balancers

### Private Subnet

* No direct internet access
* Used for databases, internal services, backend systems

 **Important:**
At this stage, the subnet is **not public yet**.
To make it public, we still need:

1. An **Internet Gateway**
2. A **route table update**
3. Public IP assignment

---

## Enabling Auto-Assign Public IPv4 Address

After creating the subnet:

1. Click **Edit Subnet settings**
2. Enable **Auto-assign public IPv4 address**

<img width="1001" height="575" alt="Screenshot 2026-02-03 at 9 27 05 PM" src="https://github.com/user-attachments/assets/f1e927f0-174c-4ce3-bc84-75187719b6cc" />

### What does this mean?

* Every EC2 instance **already has a private IP**
* Private IPs only allow **internal VPC communication**
* To access the internet, an instance needs a **public IP**

Enabling auto-assign ensures that **any EC2 launched in this subnet automatically receives a public IPv4 address**, saving you from manual configuration.

---

## Creating the Subnet Using AWS CloudShell

```bash
aws ec2 create-subnet --vpc-id VPC-ID --cidr-block ADD-CIDR-BLOCK
```

*Replace:*

* `VPC-ID` with your VPC ID
* `ADD-CIDR-BLOCK` with `10.0.0.0/24`

---

# Creating an Internet Gateway

An **Internet Gateway (IGW)** connects your **VPC (city)** to the **internet (outside world)**.

### Why does AWS already have one?

The **default VPC** comes with an internet gateway attached automatically.
That’s why you could launch internet-connected instances from day one.

For a **custom VPC**, you must create and attach one manually.

---

## Attaching the Internet Gateway to the VPC

<img width="1437" height="381" alt="Screenshot 2026-02-03 at 9 39 23 PM" src="https://github.com/user-attachments/assets/99898895-79f0-4ecb-adb2-ab5ae37ac8be" />

---

## Creating the Internet Gateway Using AWS CloudShell

```bash
aws ec2 create-internet-gateway --query InternetGateway.InternetGatewayId --output text
```

This command:

* Creates an Internet Gateway
* Returns only the **Internet Gateway ID**

---

## Attaching the Internet Gateway to the VPC

```bash
aws ec2 attach-internet-gateway --vpc-id VPC-ID --internet-gateway-id IG-ID
```

*Replace:*

* `VPC-ID` with your VPC ID
* `IG-ID` with the Internet Gateway ID

Once attached, your VPC can now route traffic to and from the internet.

---

# Deleting the Resources

When you’re done practicing, always clean up resources to avoid unnecessary charges.

You can delete everything using **AWS CloudShell**.

```bash
aws ec2 delete-vpc --vpc-id VPC-ID

aws ec2 delete-subnet --subnet-id SUBNET-ID

aws ec2 delete-internet-gateway --internet-gateway-id [IG-ID]
```

Make sure:

* The Internet Gateway is **detached before deletion**
* Replace all placeholders with actual resource IDs
