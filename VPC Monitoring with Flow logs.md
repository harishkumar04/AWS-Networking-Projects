Complete all the steps from the VPC Peering Project and then from there I will add the Flow Logs.

# Set up Cloud Watch

Amazon CloudWatch is a powerful monitoring service that watches over everything happening in your AWS environment in real time.
AWS services would report to CloudWatch with their metrics, which are stats or measurements of the service's performance. 
You can then use CloudWatch to take these metrics and transform them into clear, easy-to-read graphs and dashboards to help you 
understand your AWS environment's performance and health health.

For example, if you were hosting an application on AWS, your dashboard could track how quickly your app is loading for your users.

Another example, you could set CloudWatch to automatically shut down a VPC if it detects that it's letting in illegitimate traffic.

## Why is it important to select the same Region?
CloudWatch data is region-specific! That means the data collected on your VPC in one region is not automatically available in other regions.
If you're using a region outside of the one you used to launch your VPCs, you won't get access to CloudWatch's metrics on your VPC!

## Create a Log Group
Logs from the same source or application will go into the same log group, BUT logs are also region-specific. 
This means log data gets created and saved in the region it was created, although you can use CloudWatch dashboards to bring together logs from different regions.
<img width="859" height="594" alt="Screenshot 2026-03-25 at 10 58 10 AM" src="https://github.com/user-attachments/assets/8073c707-07aa-48e1-950c-a8e12b58d493" />

Log class is Standard by default, which means the logs that get created will get accessed or analyzed regularly.
If we chose Infrequent Access instead, your logs will be stored for long-term archiving - you are charged less for storage, but higher for each time you need to access the, for analysis. 

# Creating Flow Logs in the VPC

Go to VPC and then select the VPC 1 and then click on "Actions" and then "Create Flow Logs"

<img width="1420" height="654" alt="Screenshot 2026-03-25 at 11 05 19 AM" src="https://github.com/user-attachments/assets/fcc0b604-beaa-4aeb-8086-d595e6408fa5" />

## What are network interfaces?

Network interfaces are small but powerful components in a network!
We know that subnets are subdivisions, or neighbourhoods, inside your VPC.
Think of network interfaces as another layer down - they are even smaller subdivisions of your subnet! 
Since your subnets are like neighbourhoods, think of a network interface as a specific building or block inside that neighbourhood.

Network interfaces exist to connect your resources to your VPC! For example, whenever you launch an EC2 instance, AWS automatically creates a primary network interface with a private IP address from your subnet's IPv4 CIDR block. 
That's how your EC2 instance gets a private IP address. 
They're not too important in simple networks like ours, but network interfaces become very powerful when you have complex network situations, like wanting an EC2 instance to belong to two subnets at once! 

## Why do flow logs track traffic to network interfaces?
Flow logs are associated with specific network interfaces because each interface represents a distinct point where traffic enters or exits an EC2 instance or other AWS resources. No two resources can share the same network interface.

## Setup IAM Policy and Roles

VPC Flow Logs doesn't have the permission to write logs and send them to CloudWatch.

### Create an IAM Policy

Choose JSON

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams"
      ],
      "Resource": "*"
    }
  ]
}
```
<img width="1438" height="533" alt="Screenshot 2026-03-25 at 11 11 14 AM" src="https://github.com/user-attachments/assets/646a7464-27e7-4bfe-a24f-6ebe21db6e7a" />

### Create an IAM Role

We will choose "Custom Trust Policy"

#### Why did we pick Custom trust policy here?
Don't forget why we're creating an IAM role - to give VPC Flow Logs the permission to write and send logs to CloudWatch. We only want Flow Logs to have this access, not just any service.
By choosing a custom trust policy, we're making sure that only VPC Flow Logs can use this role.

#### But we could assign permissions using the "AWS service" option too, why didn't we pick that?

Select AWS service as the Trusted entity type, and specify we only want to give permissions to VPC Flow Logs? 
you'll find that you can't actually find an option for VPC Flow Logs!

VPC Flow Logs is technically just a feature within the wider Amazon VPC service (instead of being its own standalone AWS service), 
so we'd have to use a custom trust policy to assign permissions specifically to VPC Flow Logs.

Add this is line in the Policy

```json
"Principal": {
   "Service": "vpc-flow-logs.amazonaws.com"
},
```

The statement "Service": " vpc-flow-logs.amazonaws.com " in a trust policy specifically points to VPC Flow Logs as the only service that can use this role!

Combined, the custom trust policy should look like this

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Effect": "Allow",
			"Principal": {"Service": "vpc-flow-logs.amazonaws.com"
      },
			"Action": "sts:AssumeRole"
		}
	]
}
```

And then select the IAM Policy that we created in the previous step

<img width="1075" height="52" alt="Screenshot 2026-03-25 at 11 28 38 AM" src="https://github.com/user-attachments/assets/803cd807-6c58-495a-997c-89f1b97c1bb6" />

# Continue the setup of the CloudWatch

Now, go back to the CloudWatch and then add the Service Role that we just created.

<img width="1002" height="202" alt="Screenshot 2026-03-25 at 11 30 25 AM" src="https://github.com/user-attachments/assets/d9176be1-0ff4-473f-9ae7-c5d39534a483" />

Now, if we can see the Flow Logs in the VPC.
<img width="1185" height="626" alt="Screenshot 2026-03-25 at 11 31 04 AM" src="https://github.com/user-attachments/assets/b3dd0101-a279-4c67-8f14-8f5a64fd08b4" />

# Test the connection 

From the Instance 1 send some traffic to the instance 2.
