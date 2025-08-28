# Creating a Virtual Private Cloud
[001]: img/default_vpc.png "Default VPC"
[002]: img/cmd_create%20vpc.png "AWS CLI code for creating a vpc"
[003]: img/PlanetVPC%20created.png "Image of planet VPC created in my AWS account"
[004]: img/upload%20files-1.png "Title"
[005]: img/upload%20files-%20succeeded.png "Title"
[006]: img/static%20web%20hosting-1.png "Title"
[007]: img/static%20web%20hosting-2.png "Title"


![Create bucket][001]


## Project Overview
In this project, I created a Virtual Private Cloud (VPC) on AWS using AWS Command Line Interface (CLI). I have kept the scope of this project simple, the primary aim was to demonstrate the use of the AWS CLI to build a virtual private cloud on AWS instead of using the AWS Management console.  In this project, I will:
1. Create an Amazon VPC
2. Create a public subnet
3. Create an internet gateway


create VPC
create public subnet
enale auto-assign ipv4 for public subnet
create an internet gateway
create route table
associate route table with subnet
create routes
create ACL
associate acl with subnet
create network group

Repeat for private subnet
provision services

## Architecture Diagrams


## AWS Services Used
In building this project, I used the following AWS services:
1. Amazon VPC

## Deployment Instructions
### Step 1: Create a VPC
An Amazon Virtual Private Network (VPC) is a logically isolated virtual network within the AWS cloud where a user can launch, manage and control AWS resources such as compute instances. In simple terms, a VPC is your own private network within the AWS cloud. Without a VPC, all AWS resources would exist randomly in one giant open network without privacy, personal space or control.

On the AWS management console, the VPC dashboard shows an existing VPC. This is a default VPC that AWS automatically sets up on every new account created. This default VPC allows new users to launch AWS resources easily without the need to first configure a VPC for themselves.
![AWS CLI create vpc code][002]

... and here's our virtual private network in our AWS account.
 ![VPC created][003]

The entered code creates a vpc with the specified ipv4 cidr block.

IPv4 stands for Internet Protocol version 4. It is the most common way to write an IP address. IP is a foundational componet of the internet protocol suite TCP/IP (Transmission Control Protocol / Internet Prototocol), which are essentially standardized rules that enable computers to communicate on a network. Each device on a network is assigned a unique IP address to locate it for communication.

An IP

# Code Documentation
### Code for creating VPC
```
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications ResourceType=vpc,Tags=[{Key=Name,Value=PlanetVPC}]

```

This command creates a VPC with the specified IPv4 cidr block.


# Security Considerations


## Challenges and Solutions
While building this project, challenges that are most likely to occur may include:
### Challenge 1:  Not seeing the VPC I created.
Solution: Make sure you are in the right region for your account. This is usually the region that your AWS cli is configu=red on.
