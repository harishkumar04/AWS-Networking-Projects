In the last project where we accessed the S3 bucket from the VPC, there is a problem with that setup which is that the EC2 instance is accessing the S3 bucket through the public internet. When traffic is going through the internet, there is a higher risk of external threats and attacks looking at your data!

For example, attackers can expose your EC2 instance's keys to your AWS account if they manage to break into the traffic between your instance and S3 bucket. 

# What are VPC Endpoints?

VPC endpoints gives your VPC private, direct access to other AWS services like S3, so traffic doesn't need to go through the internet.

Just like how internet gateways are like your VPC's door to the internet, you can think of VPC endpoints as private doors to specific AWS services.

Normally, to access some AWS services like S3 from your VPC, your traffic would go out to the public internet.

But, VPC endpoints let you connect your VPC privately to AWS services without using the public internet. This means your data stays within the AWS network, which can improve security and reduce data transfer costs.

# Creating a VPC Endpoint

<img width="865" height="251" alt="Screenshot 2026-03-25 at 12 57 31 PM" src="https://github.com/user-attachments/assets/6c01b9f4-3604-4eb0-a60a-73c7bb985c06" />

## What are Gateway and Interface Endpoints?
What's does Gateway mean?
A Gateway is a type of endpoint used specifically for Amazon S3 and DynamoDB (DynamoDB is an AWS database service).

Gateways work by simply adding a route to your VPC route table that directs traffic bound for S3 or DynamoDB to head straight for the Gateway instead of the internet.

As AWS evolved and more companies started using it for diverse and complex tasks, there was a need for more detailed control over how data moves within AWS networks. Gateway endpoints they could manage traffic but didn’t offer detailed settings - this led to AWS creating Interface endpoints, which offer more security settings.


block off your S3 bucket from ALL traffic... except traffic coming from the endpoint.

It's the ultimate test - if your endpoint was truly set up properly, then your EC2 instance should still be able to access S3. Otherwise, your instance's access is blocked


What's a bucket policy?
A bucket policy is a type of IAM policy designed for setting access permissions to an S3 bucket. Using bucket policies, you get to decide who can access the bucket and what actions they can perform with it.

<img width="908" height="112" alt="Screenshot 2026-03-25 at 1 01 31 PM" src="https://github.com/user-attachments/assets/4cb2d3f8-602c-4e28-9ef7-18dfbe5d1381" />

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyOutsideVPCEExceptAccount",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::<bucket-name>",
        "arn:aws:s3:::<bucket-name>/*"
      ],
      "Condition": {
        "StringNotEquals": {
          "aws:sourceVpce": "<vpc-endpoint-id>"
        },
        "ArnNotEquals": {
          "aws:PrincipalArn": [
            "arn:aws:iam::<account-id>:root"
          ]
        }
      }
    }
  ]
}
```
<img width="1123" height="582" alt="Screenshot 2026-03-25 at 1 04 43 PM" src="https://github.com/user-attachments/assets/331e03b6-239e-4136-866c-5bca943de421" />

Why am I denied access?

Your policy denies all actions unless they come from your VPC endpoint. This means any attempt to access your bucket from other sources, including the AWS Management Console, is blocked!


