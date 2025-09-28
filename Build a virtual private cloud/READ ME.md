# Create a Virtual Private Cloud
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





## Project Overview
In this project, I created a Virtual Private Cloud (VPC) on AWS using AWS Command Line Interface (CLI). I have kept the scope of this project simple, the primary aim was to demonstrate the use of the AWS CLI to build a virtual private cloud, and lunch instances, on AWS instead of using the AWS Management console.  In this project, I will:
1. Create an Amazon VPC
2. Create public subnets
3. Enable auto-assign ipv4 for public subnet
4. Create an internet gateway
5. Create route tables and add routes
6. Associate route table with subnets
5. Create ACLs and network group
6. Lunch EC2 instances in each subnet

All CLI commands used in this project can be found in the codes section of this documentation.

## Architecture Diagrams


## AWS Services Used
In building this project, I used the following AWS services:
1. Amazon VPC
2. AWS network subnets, and internet gateway
3. ACLs and Network groups
4. EC2
5. AWS CLI

AWS Command Line Interface (CLI) is a tool that allows AWS users manage AWS services from the command line. Using the command line, one can interact with services and run scripts to automate tasks. 

The commands generally follow a consistent structure:

```

aws <service> <subcommands> [parameters]

aws: The main executable that invokes or calls the AWS CLI.

<service>: Specifies the AWS service you want to interact with (e.g., ec2 for Amazon EC2, iam for Identity and access management etc.).

<subcommands>: The specific action you want to perform within the specified service (e.g., create-vpc for creating a virtual cloud environment to run your instances in).

[parameters]: Optional arguments that provide additional information or configurations for the subcommand.


```

## Amazon CLI Commands Documentation

I have documented all CLI commands used in this project. While using these commands, remember to edit the parameters (such as cidr-block, vpc-id, tags etc) to your prefered values.

### Command for creating VPC

```
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications ResourceType=vpc,Tags=[{Key=Name,Value=PlanetVPC}]

```

This command creates a VPC with the specified IPv4 cidr block.

### Command for creating subnets

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

The first command enables auto-assign ipv4 address, while the second command attaches it to the VPC that is represented by the specified ID.

### Command to Create Route Table and Add Routes

```
    aws ec2 create-route-table --vpc-id vpc-0a5bde0e7363d1ae1 --tag-specifications ResourceType=route-table,Tags=[{Key=Name,Value=PlanetPublicRouteTable}]

    aws ec2 create-route --route-table-id rtb-011d960d85b4d7c17 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-08f32086348b50be6

```
First command creates a route table, second command adds routes to the route tables.

### Associate Route table with PlanetPublicSubnet1 and PlanetPublicSubnet2

```
    aws ec2 associate-route-table --subnet-id subnet-0f1065aa8bf3e6a78 --route-table-id rtb-011d960d85b4d7c17

    aws ec2 associate-route-table --subnet-id subnet-035d189ff5e6826b8 --route-table-id rtb-011d960d85b4d7c17

```

First command associates the route table with the specified with PlanetPublicSubnet1, while second command associates it with planetPublicSubnet2

### Create Network ACL and add traffic rules

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

### Launch EC2 instances
Explain Amazon Ec2
Explain requirements (parameters e.g AMI, key-pair, instance type etc.) for launching an instance.

```
    aws ec2 describe-images --owners amazon

    >aws ec2 run-instances --image-id ami-00e5ae8e73b80a91e --instance-type t3.micro --security-group-ids sg-0e93747e9db244c60 --subnet-id subnet-0f1065aa8bf3e6a78 --associate-public-ip-address

    aws ec2 run-instances --image-id ami-00e5ae8e73b80a91e --instance-type t3.micro --security-group-ids sg-0e93747e9db244c60 --subnet-id subnet-035d189ff5e6826b8 --associate-public-ip-address

```

First line of command gives a list of available instance images with their image-ids.

Second line uses the image-id we choose from the output of command one to launch an Amazon EC2 instance in a public subnet, and assign a public ipv4 aaddress.

Third line repeats step 2, only this time our EC2 is launched in the second Public subnet.



## Deployment Instructions
### Step 1: Create a VPC
An Amazon Virtual Private Network (VPC) is a logically isolated virtual network within the AWS cloud where a user can launch, manage and control AWS resources such as compute instances. In simple terms, a VPC is your own private network within the AWS cloud. Without a VPC, all AWS resources would exist randomly in one giant open network without privacy, personal space or control.

Make sure you're on the AWS region closest to you. On the AWS management console, the VPC dashboard shows an existing VPC. This is a default VPC that AWS automatically sets up on every new account created. 

![AWS default VPC][001]

This default VPC allows new users to launch AWS resources easily without the need to first configure a VPC for themselves. 

The objective of this project is to create our own Virtual private cloud and provision some resources using AWS CLI, therefore we will ignore the default VPC and build our own from scratch.


To create a VPC, in the AWS CLI, enter the code for creating a VPC:

```
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications ResourceType=vpc,Tags=[{Key=Name,Value=PlanetVPC}]

```

![AWS CLI create vpc code][002]

... and here's our virtual private network in our AWS account.
 ![VPC created][003]

The entered code creates a vpc with the specified ipv4 cidr block.

IPv4 stands for Internet Protocol version 4. It is the most common way to write an IP address. There is also IPv6, which is essentially the latest version of Internet Protocol and was designed to replace IPV4 as there is a shortage of available IP addresses.

An IP address is a unique set of numbers assigned to each device that is connected to a computer network. IPv4 addresses range from 0.0.0.0 and go all the way to 255.255.255.255. That is 4,294,967,296 possible combinations of ipv4 addresses. Two devices cannot share thesame IPv4 address in the same network. 

The TCP/IP (Transmission Control Protocol / Internet Protocol), are standardized rules that enable computers to communicate on a network. Each device on a network is assigned a unique IP address to locate it for communication. AWS resources use IP addresses to identify other resources and communicate/exchange data.

Many types of devices and resources also have IP addresses. Computers, printers, smartphones, tablets, and smart home devices like security cameras all have their own IP addresses. Websites also have IP addresses. While we use domain names (like www.awazon.com) for convenience, these domain names map to IP addresses that the internet uses to route traffic.

CIDR Blocks:
CIDR (Classless Inter-Domain Routing) is a way to assign a block of IPv4 addresses. For example, 10.0.0.0/16 means the first 16 bits of your IP address (10.0) are fixed, but the remaining 16 bits (i.e. the second half of the IP address) can be assigned however you like. Addresses within this CIDR block start at 10.0.0.0 and go up to 10.0.255.255. There are 2^16 (65,536) possible IP addresses within this subnet. The number after the slash will tell us how big a CIDR block is; the smaller the number, the larger the CIDR block.


### Step 2: Create Subnets
 Subnets basically mean subnetworks. They're like well organised neighbourhoods (each with its own traffic rules and restrictions) within a city (the VPC). Each subnet is just a smaller section within a network (in this case our VPC) where you can launch and organise resources such as Amazon EC2 instances, as well as manage traffic flow. When you logically divide the range of IP addresses of a VPC into smaller ranges, you get subnets.

 A subnet can either be Public (this means it is configured with direct access to the internet through an Internet Gateway) or Private (which means there is no direct access to the internet, and would require a NAT device to access the internet).

 On the AWS management console, again the clicking on the subnets tab on the side shows there are existing subnets in our default VPC. These are default subnets that AWS pre-creates in each region for every account. This enables users to launcg AWS resources without needing to configure networking from scratch. 

 ![Default AWS Subnets][004]

 I will go ahead and delete these default subnets because I intend to create and configure new subnets from scratch using Amazon CLI.

 ![Blank AWS VPC after deleting default subnets][005]
 
 In this project, I will be creating two public subnets and one private subnet. All the commands I used can be found in the codes section of this documentation.

 - To create the first public subnet, here's the command I used:
 
 ```

    aws ec2 create-subnet --vpc-id vpc-0a5bde0e7363d1ae1 --cidr-block 10.0.1.0/24 --availability-zone us-east-1a --tag-specifications ResourceType=subnet,Tags=[{Key=Name,Value=PlanetPublicSubnet1}]

 ```

 ![Create public subnet][006]

... and here's the management console showing PlanetPublicSubnet1 has been created. Notice the arrow in the image pointing to the part that says 'auto-assign public IPv4 address: No'. This feature is important and corrently turned off. We will need to enable it later on in the project.

 ![Planet1 Public Subnet Created][007]

Repeat the step for two more subnets: PlanetPublicSubnet2 and PlanetPrivateSubnet1
 ![Planet1 Public and private subnets][008]

All three subnets created.
 ![Image of all three subnets created][009]


### Step 3: Enable auto-assign ipv4 for public subnet
By default, resources launched in our subnets will already have private IPv4 addresses. However, these IP addresses only work for internal communication within the VPC. To access the internet, or to be accessible from the internet, our resources (EC2 instances, for example) will need Public IP addresses.

When auto-assign Public ipv4 address is enabled for a subnet, it means any EC2 instances launched in the subnet will automatically get a public IP address, and we would'nt need to do it manually. it is a good time saver.

![Assign IPv4 to public subnets][010]

![results on management console][011]


### Step 4: Create an Internet Gateway and Attach to PlanetVPC

The internet gateway is the communication bridge between the resources in our VPC and the wider internet. It allows resources like EC2 instances launched within our VPC to access the internet and our applications to be accessible from anywhere through the internet.

In this step, I created an internet gateway and attached it to the PlanetVPC.

![Create Internet Gateway][012]

Console image below shows internet gateway created, but not yet assigned to a VPC.

![results of created internet gateway on managemen console][013]

... assigning internet gateway to PlanetVPC.
![Assign internet gateway to PlanetVPC][014]

Internet gateway attached.
![Management console showing attached internet gateway][015]

 
### Step 5: Create Route Table and Add Routes


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





## Security Considerations


## Challenges and Solutions
While building this project, challenges that are most likely to occur may include:
### Challenge 1:  Not seeing the VPC I created.
Solution: Make sure you are in the right region for your account. This is usually the region that your AWS cli is configu=red on.

### Challenge 2: Issues with network acl association