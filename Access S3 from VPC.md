# Create a VPC
Create a VPC by using the "VPC and more" option.

# Launch an EC2 instance
Launch an EC2 instance inside the VPC and make sure the "Auto Assign Public IPv4" is enabled.

# Use EC2 Instance Connect

After connecting to the EC2 instance, do this

```shell
aws s3 ls

# Output:
Unable to locate credentials. You can configure credentials by running "aws login".
```

## What are credentials?
Credentials in AWS are like keys that let you access and manage AWS services securely. Without credentials, you won't have the permission to do things like viewing your S3 bucket list.

Your account by default doesn't have credentials set up for running commands from an EC2 instance. You'll need to manually set these up to securely access and manage AWS services from your instance.

Now, run

```shell
aws configure

# Output:
AWS Access Key ID [None]: 
```

It is asking for the Access Key 

## What is an access key ID? Do I have one?
An access key ID is a part of a credential!
Your credentials are made up of a username and password; think of the access key ID as the username.
You don't automatically have one, but you can create access keys IDs through AWS IAM.

## What's the difference between access keys and key pairs?
Key pairs are used specifically for logging into your EC2 instances through SSH.
Access keys, which are what we're learning about now, are credentials for your applications and other servers to log into AWS and talk to your AWS services/resources.

# Setup Access Keys

Go to IAM and then search for "Access key"

<img width="962" height="202" alt="Screenshot 2026-03-25 at 12 25 04 PM" src="https://github.com/user-attachments/assets/f9d8e084-db63-4aad-bae5-1db04e12992d" />


## What is the recommend way of doing this?

In this project we're creating access keys and manually applying them in our EC2 instance, but typically the recommended way is to create an IAM role with the necessary permissions and then attaching that role to your EC2 instance.

Your EC2 instance would inherit the permissions from the role, and this is best practice as you can easily attach and detach EC2 instances from roles to give and take away their credentials.

We aren't using this method so that we can learn about access keys, but roles are usually a better alternative for security.
<img width="900" height="216" alt="Screenshot 2026-03-25 at 12 27 45 PM" src="https://github.com/user-attachments/assets/a7dde71d-6db0-40ff-a429-a6694a1792be" />

## What is the secret access key?
The secret access key is like the password that pairs with your access key ID (your username). You need both to access AWS services.
Secret is a key word here - anyone who has it can access your AWS account, so we need to keep this away from anyone else!

# Create a Amazon S3 Bucket

Bucket name : hairsh-s3-practice-bucket  
and then keep the rest all as the default settings and then upload any 2 files.

# Now back to your EC2 Instance Connect tab

Open the AccessKeys.csv file you've downloaded.
Copy the Access key ID.
Paste this into your EC2 instance's terminal, and press Enter on your keyboard.
Now, run the command,
<img width="455" height="36" alt="Screenshot 2026-03-25 at 12 37 53 PM" src="https://github.com/user-attachments/assets/229169dc-8cf2-48ab-9092-2f8e11c5a77a" />

```shell
aws s3 ls s3://hairsh-s3-practice-bucket

# Output:
2026-03-25 07:03:58      77846  -5.jpg
2026-03-25 07:03:58      61386 avatar_1755609518.png
```

We will create a file in the EC2 instance and then upload it to the S3 bucket

```shell
sudo yum update -y
sudo yum install -y vim

vim hello.txt

aws s3 cp /home/ec2-user/hello.txt s3://hairsh-s3-practice-bucket

# Output:
upload: ./hello.txt to s3://hairsh-s3-practice-bucket/hello.txt

aws s3 ls s3://hairsh-s3-practice-bucket

# Output:
2026-03-25 07:03:58      77846  -5.jpg
2026-03-25 07:03:58      61386 avatar_1755609518.png
2026-03-25 07:15:02         88 hello.txt
```

This is how we access an S3 bucket from our EC2 instance inside the VPC.
