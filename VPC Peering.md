# Create two VPC

We will create two VPC's each with one public subnet and zero private subnet.
1st VPC IPv4 CIDR : 10.1.0.0/16
2nd VPC IPv4 CIDR : 10.2.0.0/16

<img width="1193" height="299" alt="Screenshot 2026-03-25 at 9 58 17 AM" src="https://github.com/user-attachments/assets/755b67ed-e3c6-4732-a902-92ecd653b4a2" />

# Creating a Peering connection

## What is a peering connection?

A VPC peering connection is a direct connection between two VPCs.
A peering connection lets VPCs and their resources route traffic between them using their private IP addresses. This means data can now be transferred between VPCs without going through the public internet.
Without a peering connection, data transfers between VPCs would use resources' public address - meaning VPCs have to communicate over the public internet. 

<img width="999" height="430" alt="Screenshot 2026-03-25 at 10 03 28 AM" src="https://github.com/user-attachments/assets/fc56e883-84de-4169-983c-1ee9f7d04f6e" />
## What does Requester mean?
In VPC peering, the Requester is the VPC that initiates a peering connection. As the requester, they will be sending the other VPC an invitation to connect!

## What does Accepter mean?
In VPC peering, the Accepter is the VPC that receives a peering connection request! The Accepter can either accept or decline the invitation. This means the peering connection isn't actually made until the other VPC also agrees to it!

<img width="1083" height="379" alt="Screenshot 2026-03-25 at 10 03 59 AM" src="https://github.com/user-attachments/assets/03d3e4e9-bf71-4ed8-b393-eef1f06a1470" />

After that it will show "Pending acceptance"
<img width="1202" height="428" alt="Screenshot 2026-03-25 at 10 05 53 AM" src="https://github.com/user-attachments/assets/a4d3c5f8-9857-4807-a38e-a3cc69362284" />

Click on "Actions" and then "Accept Request"
<img width="806" height="312" alt="Screenshot 2026-03-25 at 10 09 34 AM" src="https://github.com/user-attachments/assets/ce90f1a8-867f-405c-b738-06dd2a9a23b0" />

# Update the Route table
Even if your peering connection has been accepted, traffic in VPC 1 won't know how to get to resources in VPC 2 without a route in your route table! You need to set up a route that directs traffic bound for VPC 2 to the peering connection you've set up.
<img width="1439" height="466" alt="Screenshot 2026-03-25 at 10 13 35 AM" src="https://github.com/user-attachments/assets/618c1a3b-bb02-49e6-ba7a-566e138dafc5" />

Similarly, update the Route table of the 2nd VPC too.

# Launching EC2 instances in both the VPC's
<img width="1436" height="650" alt="Screenshot 2026-03-25 at 10 23 03 AM" src="https://github.com/user-attachments/assets/35b657bf-308d-4d18-b813-87da25195648" />

# Elastic IP address
Since we disabled the option to give public IP to the EC2 instances we can't connect to those instances with the help of the EC2 Instance Connect since they need a Public IPv4 Address to connect.
Therefore we will create a Elastic IP and then associate it with the EC2 instance.
<img width="947" height="651" alt="Screenshot 2026-03-25 at 10 17 32 AM" src="https://github.com/user-attachments/assets/3d9de5ef-5515-488d-99cd-4bf9b2e77c9b" />

Edit the Inbound rules in the Security group to allow us to do SSH

# Checking the connection
Copy the Private IP address of the 2nd EC2 instance and then try to ping that IP from the 1st EC2 instance.

<img width="469" height="59" alt="Screenshot 2026-03-25 at 10 32 07 AM" src="https://github.com/user-attachments/assets/6112e0d3-85e1-4f56-a2fe-db093869ea79" />

This output means that the 1st EC2 instance has sent the Packet to the 2nd EC2 instance but it didn't receive any reply back. This might be because the EC2 instance by default blocks all the ICMP packets to prevent attacks.

So go to the 2nd EC2 instance Security group and then add this Inbound Rule
<img width="1166" height="182" alt="Screenshot 2026-03-25 at 10 39 10 AM" src="https://github.com/user-attachments/assets/84981595-018c-4891-9c8f-cce40b35a80a" />

Make sure to keep the Source to `10.1.0.0/16` which is the IPv4 CIDR of the 1st VPC.

Now if I ping the Private IPv4 address of the 2nd EC2 instance we will see the output

## Why use the Private IP to ping and not the Public IP?

If the peering connection is succesful, our instances can communicate using each other's private IPv4 addresses.
If they can't communicate using their private IPv4 addresses, then... we have an error!

That's why it's a good idea to test with the private IPv4 addresses - using public IPv4 addresses wouldn't give you much information about whether the peering connection was successful!

<img width="666" height="426" alt="Screenshot 2026-03-25 at 10 41 17 AM" src="https://github.com/user-attachments/assets/2522a370-b0e7-4fc9-ac9f-9bb4356b9735" />

