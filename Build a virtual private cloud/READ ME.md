# Creating a Virtual Private Cloud
[001]: img/default_vpc.png "Default VPC"
[002]: img/cmd_create%20vpc.png "AWS CLI code for creating a vpc"
[003]: img/PlanetVPC%20created.png "Image of planet VPC created in my AWS account"
[004]: img/default_subnets.png "Some AWS default subnets"
[005]: img/deleted_all_subnets.png "Successfully deleted all default subnets"
[006]: img/cmd_create-public-subnet.png "Create a public subnet"
[007]: img/planet1PublicSubnet.png "Title"
[008]: img/cmd-create_more_subnets.png "Title"
[009]: img/additional%20subnets%20created.png "Title"
[010]: img/cmd_assign_public_ipv4%20to%20subnets.png "Title"
[011]: img/sucessfully_auto-assigned%20IPv4.png "Successfully auto-assigned IPv4 for Public subnet 1"
[012]: img/cmd_create-internet-gateway.png "Create Internet gateway"
[013]: img/created_internet_gateway.png "Title - Successfully create internet gateway"
[014]: img/cmd-attach_internet_gateway%20to%20VPC.png "Title - Attach internet gateway to VPC"
[015]: img/successfully%20attached%20internet%20gateway%20&%20deleted%20default.png "Title - ataached internet gateway"
[016]: img/cmd_create_route_table%20&%20add_routes.png "Title - Create route table and add routes"
[017]: img/sucessfully_created-route_tables_and%20_routes.png "Title - Created route tables and routes"
[018]: img/cmd_associate_route_table_with_public_subnets.png "Title"
[019]: img/successful%20subnet%20associations.png "Title"
[020]: img/cmd_create%20acl.png "Title"
[021]: img/ACL_no_associations.png "Title"
[022]: img/cmd_create_rules_for_acl.png "Title"
[023]: img/cmd_associate_acl_with_subnets.png "Title"
[024]: img/successfully%20associated%20acl%20with%20subnets.png "Title"
[025]: img/cmd_create_security%20grops%20and%20inbound%20rules.png "Title"
[026]: img/successfully%20created%20security%20group%20and%20ingress%20rules.png "Title"
[027]: img/static%20web%20hosting-2.png "Title"
[028]: img/static%20web%20hosting-2.png "Title"
[029]: img/static%20web%20hosting-2.png "Title"


![Create bucket][001]


## Project Overview
In this project, I created a Virtual Private Cloud (VPC) on AWS using AWS Command Line Interface (CLI). I have kept the scope of this project simple, the primary aim was to demonstrate the use of the AWS CLI to build a virtual private cloud on AWS instead of using the AWS Management console.  In this project, I will:
1. Create an Amazon VPC
2. Create a public subnet
3. Create an internet gateway


create VPC
create public subnet
enable auto-assign ipv4 for public subnet
create an internet gateway
create route table and add routes
associate route table with subnet
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


### Step 2: Create Subnets
 - Talk about default subnets

 ![Default AWS Subnets][004]

 - Deleted default subnets
 ![Blank AWS VPC after deleting default subnets][005]
 
 - Create public subnet
 ![Create public subnet][006]

 ![Planet1 Public Subnet Created][007]

 ![Planet1 Public and private subnets][008]
 ![Image of all three subnets created][009]


### Step 3: Enable auto-assign ipv4 for public subnet
what does this mean?

![Assign IPv4 to public subnets][010]
![results on management console][011]


### Step 4: Create an Internet Gateway and Attach to PlanetVPC
What is an internet gateway in AWS?

![Create Internet Gateway][012]

![results of created internet gateway on managemen console][013]

![Assign internet gateway to PlanetVPC][014]

![Management console showing attached internet gateway][015]


### Step 5: Create Route Table and Add Routes
What are routes and route tables

![Create route table and add routes][016]

![Results on management console][017]

### Step 6: Associate subnets with route table
Associating routes with subnets

![Associate route table with subnets][018]

![Showing subnet associations][019]


### Step 6: Create Network ACL and create ingress and egress rules
What are Network ACLs and why use them?

![Create network ACL][020]

![Network ACL sucessfully created][021]

Explain inbound (ingress) and outbound (egress) rules.

![ACl create traffic rules][022]

### Associate ACL with subnets
![Associate ACL with subnets][023]

![Successfully associate acl with subnets][024]


### Create security group and ingress rule
What are security groups?

![Create security group and ingress rules][025]

![Successfully created security group and ingress rules][026]



## Code Documentation
### Code for creating VPC
```
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications ResourceType=vpc,Tags=[{Key=Name,Value=PlanetVPC}]

```

This command creates a VPC with the specified IPv4 cidr block.

### Code for creating subnets

```
    aws ec2 create-subnet --vpc-id vpc-0a5bde0e7363d1ae1 --cidr-block 10.0.1.0/24 --availability-zone us-east-1a --tag-specifications ResourceType=subnet,Tags=[{Key=Name,Value=PlanetPublicSubnet1}]

    aws ec2 create-subnet --vpc-id vpc-0a5bde0e7363d1ae1 --cidr-block 10.0.2.0/24 --availability-zone us-east-1b --tag-specifications ResourceType=subnet,Tags=[{Key=Name,Value=PlanetPublicSubnet2}]

    aws ec2 create-subnet --vpc-id vpc-0a5bde0e7363d1ae1 --cidr-block 10.0.3.0/24 --availability-zone us-east-1a --tag-specifications ResourceType=subnet,Tags=[{Key=Name,Value=PlanetPrivateSubnet1}]

```

These lines of commands ask AWS to create two public and one private subnets with specified IPv4 cidr blocks in specified availability zones.

### Command for auto-assigning public IPv4 for public subnets 1 & 2

```
    aws ec2 modify-subnet-attribute --subnet-id subnet-0f1065aa8bf3e6a78 --map-public-ip-on-launch
    aws ec2 modify-subnet-attribute --subnet-id subnet-035d189ff5e6826b8 --map-public-ip-on-launch

```


### Create Internet Gateway and Attach to planetVPC

```
    aws ec2 create-internet-gateway --tag-specifications ResourceType=internet-gateway,Tags=[{Key=Name,Value=PlanetInternetGateway}]

    aws ec2 attach-internet-gateway --vpc-id vpc-0a5bde0e7363d1ae1 --internet-gateway-id igw-08f32086348b50be6

```

### Command to Create Route Table and Add Routes

```
    aws ec2 create-route-table --vpc-id vpc-0a5bde0e7363d1ae1 --tag-specifications ResourceType=route-table,Tags=[{Key=Name,Value=PlanetPublicRouteTable}]

    aws ec2 create-route --route-table-id rtb-011d960d85b4d7c17 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-08f32086348b50be6

```
First command creates a route table, second command adds routes.

### Associate Route table with PlanetPublicSubnet1 and PlanetPublicSubnet2

```
    aws ec2 associate-route-table --subnet-id subnet-0f1065aa8bf3e6a78 --route-table-id rtb-011d960d85b4d7c17

    aws ec2 associate-route-table --subnet-id subnet-035d189ff5e6826b8 --route-table-id rtb-011d960d85b4d7c17

```

### Create Network ACL and add traffic rules
what are network ACLs and why use them?

```
    aws ec2 create-network-acl --vpc-id vpc-0a5bde0e7363d1ae1 --tag-specifications ResourceType=network-acl,Tags=[{Key=Name,Value=PlanetPublicSubnetNACL}]

    aws ec2 create-network-acl-entry --network-acl-id acl-0e29473725ccde6b8 --rule-number 100 --protocol -1 --rule-action allow --ingress --cidr-block 0.0.0.0/0 --region us-east-1

    aws ec2 create-network-acl-entry --network-acl-id acl-0e29473725ccde6b8 --rule-number 100 --protocol -1 --rule-action allow --egress --cidr-block 0.0.0.0/0

```

First command creates a new ACl, second command adds inbbound rules, and third line creates outbound rules.

### Associate ACL with Subnets

```
    aws ec2 describe-network-acls --filters Name=association.subnet-id,Values=subnet-0f1065aa8bf3e6a78 --query NetworkAcls[0].Associations[0].NetworkAclAssociationId --output text

    aws ec2 describe-network-acls --filters Name=association.subnet-id,Values=subnet-035d189ff5e6826b8 --query NetworkAcls[0].Associations[0].NetworkAclAssociationId --output text

```

These commands will output the network association IDs for our two public subnets.

### Create security group and inbound rules for the security group

```
    aws ec2 create-security-group --group-name SG_PlanetWebServers --description "Security group fo PlanetWebServers" --vpc-id vpc-0a5bde0e7363d1ae1

    aws ec2 authorize-security-group-ingress --group-id sg-0e93747e9db244c60 --protocol tcp --port 80 --cidr 0.0.0.0/0

```

## Security Considerations


## Challenges and Solutions
While building this project, challenges that are most likely to occur may include:
### Challenge 1:  Not seeing the VPC I created.
Solution: Make sure you are in the right region for your account. This is usually the region that your AWS cli is configu=red on.

### Challenge 2: Issues with network acl association