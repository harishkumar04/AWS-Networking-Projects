Follow the steps from the VPC Traffic Flow control and secuirty groups to setup the other things.

## Difference between Public and Private subnet?

A public subnet is a subnet that has a route to an Internet Gateway, allowing resources like EC2 instances to directly access the internet (and be accessed from it if permitted). A private subnet does not have a direct route to the Internet Gateway, so its resources cannot be accessed directly from the internet. Instead, private subnets typically use a NAT Gateway or similar mechanism to access the internet securely for outbound traffic only.

# Step 1 : Creating a Private subnet

```text
Name : practice-private-subnet-02
IPv4: 10.0.2.0/24
```
Choose any AZ

<img width="1178" height="211" alt="Screenshot 2026-03-24 at 3 31 41 PM" src="https://github.com/user-attachments/assets/e3696514-2042-4bbf-9603-4ad18e474637" />

# Step 2 : Create a Route Table

```text
Name : private-rt
```
Select Edit subnet associations under the Explicit subnet associations tab.
Select the checkbox next to Private Subnet.
Select Save associations.

## Should your private subnet use the same route table as your public subnet?
Probably not! NextWork route table has a route to an internet gateway, and having that route would make your subnet public! It's time to set up a new route table for your private subnet.

# Step 3 : Create another route table

Name : private-nacl

And add the Subnet in the Subnet Associations tab.

<img width="1179" height="264" alt="Screenshot 2026-03-24 at 3 38 38 PM" src="https://github.com/user-attachments/assets/c59d7a17-c82f-4e10-9007-e475f3c293c9" />

## We've already removed the route to an internet gateway. Isn't that enough to prevent traffic from the internet?
Removing a route to the internet gateway does prevent direct internet access to and from the subnet. However, relying only on route table configurations might not be great for security.

If any part of your VPC (e.g. your public subnet) gets compromised, the attacker could take advantage of your permissive ACL setup to access or attack the resources in your private subnet!
