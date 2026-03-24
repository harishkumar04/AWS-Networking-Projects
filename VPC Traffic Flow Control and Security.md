# Step 1 : Create the VPC

Name: Practice-vpc
IPv4: 10.0.0.0/16

# Step 2 : Create the Subnet

Attach it to the VPC 

```text
Name :  practice-subnet-01
IPv4 CIDR : 10.0.1.0/24
```

Choose the AZ whatever you want

In the Actions menu, select Edit subnet settings.
Check the box next to Enable auto-assign public IPv4 address.

<img width="907" height="277" alt="Screenshot 2026-03-24 at 2 45 10 PM" src="https://github.com/user-attachments/assets/6330bf59-019b-4ba3-b737-dc40fa2daa9c" />

# Step 3 : Create a IGW and attach to a VPC

Name: pratice-igw

# Step 4 : Edit the Route table 

Edit the default route then attach this to the routes

0.0.0.0/0 and then the igw that we created

<img width="1177" height="187" alt="Screenshot 2026-03-24 at 2 52 42 PM" src="https://github.com/user-attachments/assets/62804ae1-c834-464a-9f14-7c08598ef1dd" />

Then Choose the Subnet associations tab.
Under the Explicit subnet associations tab, choose Edit subnet associations
Select pratice-subnet-1.
Choose Save associations.

<img width="1173" height="201" alt="Screenshot 2026-03-24 at 2 58 28 PM" src="https://github.com/user-attachments/assets/e118b62e-5d0e-4973-aa64-6ff7b1e0c934" />

# Step 5 : Create a Security Group

Add this rule of HTTP and 'Anywhere - IPv4'

<img width="1409" height="187" alt="Screenshot 2026-03-24 at 3 02 27 PM" src="https://github.com/user-attachments/assets/851d764e-ee9f-48c8-9328-683f0821669c" />

Why not configure the Outbound Rules?

By default, AWS security groups already allow all outbound traffic. So unless you specify otherwise, any resource associated with the security group can access and send data to any IP address - whether it's in your VPC, other VPCs (if you have the right permissions) and on the public internet.


# Access Control Lists (ACL's):

What do the rules under the Inbound and Outbound rules tabs mean? Just like security groups, network ACLs use inbound and outbound rules to decide which data packets are allowed to enter or leave subnets:

## What's the difference between security groups and network ACLs?

- Network ACLs are used to set broad traffic rules that apply to an entire subnet. For example, blocking incoming traffic from a particular range of IP addresses or denying all outbound traffic to certain ports.

- Security groups allow for more granular control, managing access to individual resource. You can specify which ports and protocols are allowed for each connected resource.

Choose the network ACL that's associated with practice-subnet-01 subnet, and check out the tabs for Inbound rules and Outbound rules. 

<img width="568" height="358" alt="Screenshot 2026-03-24 at 3 11 54 PM" src="https://github.com/user-attachments/assets/dee0e377-0ada-4543-b4ce-889014aa1b8d" />
  
Rule 100 Inbound allows all inbound traffic into the Public Subnet.
Rule 100 Outbound allows all traffic out of the Public Subnet.

The second line in each ruleset shows an asterisk (*) that acts as a catch-all rule in case traffic does not match any of the earlier rules. In our case, since Rule 100 already allows all traffic, the asterisk rule won't actually come into play.

This means default network ACLs allow all inbound and outbound traffic, unless customized.

## My subnet already has an ACL, can I associate another?

Only one ACL can be associated with a subnet at a time. Once you associate your Public 1 subnet with a new ACL, the default ACL that AWS created for you gets replaced.

## Create a Custom Network ACL

<img width="1243" height="494" alt="Screenshot 2026-03-24 at 3 14 15 PM" src="https://github.com/user-attachments/assets/d60f1841-1464-4858-b214-f4b8e3afd768" />

**The default network ACLs that AWS creates allow all inbound and outbound traffic. But for custom network ACLs that we create, all inbound and outbound traffic are denied until you add rules about the kind of traffic you'll allow.**

Now in both the Inbound and Outbound roles attach the Rule number 100 that uses HTTP protocol and allows All traffic
