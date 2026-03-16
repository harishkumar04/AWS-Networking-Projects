Step 1 : Create the VPC

Step 2 : Create the Subnet

Access Control Lists (ACL's):

What do the rules under the Inbound and Outbound rules tabs mean? Just like security groups, network ACLs use inbound and outbound rules to decide which data packets are allowed to enter or leave subnets:


  
Rule 100 Inbound allows all inbound traffic into the Public Subnet.
Rule 100 Outbound allows all traffic out of the Public Subnet.
The second line in each ruleset shows an asterisk (*) that acts as a catch-all rule in case traffic does not match any of the earlier rules. In our case, since Rule 100 already allows all traffic, the asterisk rule won't actually come into play.

This means default network ACLs allow all inbound and outbound traffic, unless customized.


My subnet already has an ACL, can I associate another?
Only one ACL can be associated with a subnet at a time. Once you associate your Public 1 subnet with a new ACL, the default ACL that AWS created for you gets replaced.
