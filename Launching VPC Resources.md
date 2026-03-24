After creating the Public and Private subnets we will create some EC2 instance in these subnets

# Creating and Launching EC2 Instances in public subnet

Make sure to edit the Network rules to use the Public subnet that we created and then security group.

<img width="885" height="588" alt="Screenshot 2026-03-24 at 3 44 22 PM" src="https://github.com/user-attachments/assets/ab19851c-d7d9-4685-8330-05dc635d1543" />

# # Creating and Launching EC2 Instances in Private subnet

Same steps as above but in the security do this, 

- Create Security group
- In the inbound rules choose "Custom" and then the use the Public secuirty group that we created.

<img width="903" height="322" alt="Screenshot 2026-03-24 at 3 52 50 PM" src="https://github.com/user-attachments/assets/cf678d24-e0be-420e-bc7e-7a7f5eb7f939" />

Public Security Group  has rules that allow broad inbound traffic (like HTTP from 0.0.0.0/0) for the resources it protects. 
However, when you select the Public Security Group as the source for your private instance's security group, it means something different.
It doesn't mean your private instance now allows all the same traffic as the public security group. Instead, it means that your private instance will only accept traffic that originates from resources that are themselves associated with the Public Security Group.

Think of it this way:
If the source is Anywhere (0.0.0.0/0), it's like saying "anyone from anywhere can come in."
If the source is NextWork Public Security Group, it's like saying "only people who are already inside the 'Public' building can come into this 'Private' building."

This creates a secure pathway, allowing your public resources (like a web server) to communicate with your private resources (like a database server) without exposing the private resources directly to the entire internet. It's a key principle for securing private subnets.

