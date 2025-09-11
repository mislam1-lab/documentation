

**What Is a VPC?**
its called Virtual Private Cloud, and what it does on Amazon VPC is your isolated network environment within AWS. It’s like having your own private data center in the cloud, where you control IP ranges, subnets, routing, and access.

 Why Use One?
• 	Security: Full control over inbound/outbound traffic
• 	Isolation: Separate environments for dev, test, prod
• 	Customization: Define your own IP ranges, route tables, gateways
• 	Scalability: Easily add subnets, instances, and services

What Are Subnets?
Subnets divide your VPC into smaller IP ranges.

You can place EC2 instances in either, depending on whether they need public exposure.
<img width="1150" height="840" alt="image" src="https://github.com/user-attachments/assets/459900f7-73bc-4246-a549-78f711875011" />



**Part 1 — Create Custom VPC**
Login to AWS

Go to VPC on the left-hand side

Create VPC and select VPC only

Name and Tag: tech508-mohammed-2tier-first-vpc

IPv4 CIDR: 10.0.0.0/16

Click create VPC

![alt text](/Image%20folder/image-96.png)

**Create Subnets**
Click Subnets on the left-hand side

Create subnet:

VPC ID: enter your VPC name

Subnet name: tech508-mohammed-public-subnet

Availability Zone: Europe (Ireland) / euw1-az3 (eu-west-1a)
IPv4 VPC CIDR block: 10.0.0.0/16
IPv4 subnet CIDR block: 10.0.2.0/24

![alt text](/Image%20folder/image-97.png)
![alt text](/Image%20folder/image-98.png)

**Add new subnet:**
Subnet name: tech508-mohammed-private-subnet

Availability Zone: Europe (Ireland) / euw1-az3 (eu-west-1a)

IPv4 VPC CIDR block: 10.0.0.0/16
IPv4 subnet CIDR block: 10.0.3.0/24

Click Create subnet
![alt text](/Image%20folder/image-99.png)

**Create Internet Gateway**
Click Internet Gateway on the left-hand side

Create Internet Gateway:

Name tag: tech508-mohammed-2tier-first-vpc-ig

Click Create Internet Gateway
![alt text](/Image%20folder/image-100.png)

Create and attach to your VPC
![alt text](/Image%20folder/image-101.png)

**Create Route Table**
Click Route Tables on the left-hand side

Create route table:

Name tag: tech508-mohammed-2tier-first-vpc-public-rt
Select your VPC
Create route table

![alt text](/Image%20folder/image-102.png)

Actions → Edit subnet associations → select public subnet → Save
Edit routes:
Add route:
Destination: 0.0.0.0/0
Target: Internet Gateway (select your IGW)
Save
![alt text](/Image%20folder/image-103.png)

**Part 2 — Launch Database Instance (Private Subnet)**
Find your AMI (Database)
Launch instance:

Name: tech508-mohammed-2tier-sparta-app-database-sg

Enter your aws key: tech508-mohammed-aws

VPC: Private
![alt text](/Image%20folder/image-104.png)
Auto assign public ip: disabled. You should be able to disable it because the subnet set up means they can communicate via the private IP of the db instance.

Create new security group:

Name: tech508-mohammed-2tier-sparta-app-database-sg-allow-27017
Remove default rules
Add rule:
Type: Custom TCP
Port range: 27017
Source: Anywhere

![alt text](/Image%20folder/image-105.png)

![alt text](/Image%20folder/image-106.png)

![alt text](/Image%20folder/image-107.png)

**Part 3 — Launch App Instance (Public Subnet)**
Find your AMI (App)
Launch instance:

Name: tech508-mohammed-2tier-sparta-app-ready-to-run-app
VPC: Public
Auto-assign public IP: enable
Security group:
Name: tech508-mohammed-2tier-vpc-sparta-app-allow-HTTP-SSH

User Data:
`#!/bin/bash`
`cd repo/app`
`export DB_HOST=mongodb://<MYPRIVATEIP_FROM_DATABASE>:27017/posts`
`pm2 start app.js`
Launch

**Part 4 — Auto Scaling & Load Balancer Setup**
Part 4 — Auto Scaling & Load Balancer Setup
Log in to AWS (if needed).

In EC2 → Launch instance and create the base instance to save as a Launch Template:

Name: tech508-mohammed-for-asg-app-lt
AMI: Owned by me → tech508-rubaet-test-sparta-app-ready-to-run
Instance type: t3.micro
Key pair: (select existing)
Security group: tech508-rubaet-sparta-app-allow-SSH-HTTP-3000
Inbound rules:

SSH (22): 0.0.0.0/0
HTTP (80): 0.0.0.0/0
Custom TCP (3000): 0.0.0.0/0
Advanced settings (User Data):

#!/bin/bash
cd repo/app
pm2 start app.js
Save as Launch Template
Blueprint for all auto-scaled instances.

Actions → Launch instance with template
Create a single test instance to confirm it works.

Launch, then copy the public IP and test in a browser.



go search vpc on aws and create vpc

![alt text](/Image%20folder/image-38.png)

then we add our credentails such as devops-upskilling-mohammed-vpc-2 and the doing cidr block 10.0.0.0/16

![alt text](/Image%20folder/image-39.png)

next you the name tag should be the same 

![alt text](/Image%20folder/image-40.png)


The next thing to do is go subnet which on the left side of the next to your vpc and click on subnet and create subnets

![alt text](/Image%20folder/image-42.png)

Next on the VPC ID you go to the dropdown option and find your vpc that you created

![alt text](/Image%20folder/image-44.png)

next we go subnet setting and put down our credentails devops-upskilling-mohammed-public-subnet-2 and then so the avilability zone put down ireland 1a and then for IPv4 subnet CIDR block you put down public cidr block which is 10.0.2.0/24


![alt text](/Image%20folder/image-47.png)

Next we do same thing again but for private subnet. put down our credentails devops-upskilling-mohammed-private-subnet-2 and then so the avilability zone put down ireland 1b and then for IPv4 subnet CIDR block you put down private cidr block which is 10.0.3.0/24

![alt text](/Image%20folder/image-48.png)


Next we go to internet gateway and crea te internet gateway

![alt text](/Image%20folder/image-50.png)

then from name of the tag devops-upskilling-mohammed-vpc-ig-2 and then create internet gateway

next we go to attach to a vpc and then find your avilable vpc's and attach devops-upskilling-mohammed-vpc-2 andn click attached

![alt text](/Image%20folder/image-51.png)

Next you go to route tables 
![alt text](/Image%20folder/image-52.png)

we go create route table

![alt text](/Image%20folder/image-53.png)


next you add your nameing conventions

![alt text](/Image%20folder/image-54.png)

![alt text](/Image%20folder/image-55.png)

next step is go subnet assosciation and edit subnet assosciation and click public and save assosciation

![alt text](/Image%20folder/image-56.png)



![alt text](/Image%20folder/image-57.png)


![alt text](/Image%20folder/image-58.png)

![alt text](/Image%20folder/image-59.png)

![alt text](/Image%20folder/image-60.png)





![alt text](/Image%20folder/image-61.png)








![alt text](/Image%20folder/image-62.png)

![alt text](/Image%20folder/image-63.png)

![alt text](/Image%20folder/image-64.png)














 
 The problem so far: database not secure
 
 bindIp set to 0.0.0.0
 port 27017 open with no authentication
 
 Architecture diagram
 
 ![alt text](/Image%20folder/image-17.png)
 archdiagram
 
 database has no route to internet traffic
 have to SSH in through app VM
 everything has to be installed already as no access to internet
 Note: Public IP is associated with app VM specifically, not VPC as a whole
 
 Note: CIDR blocks:
 
 10.0.0.0/16
 
 first 4 numbers: each have 8 bits, range 0-255
 /16 means the first 16 bits are locked
 e.g. 10.0
 so the last 0.0 can be used/changed to give possible available IP addresses
 Create custom VPC
 
 VPCs
 
 Create VPC (make sure Ireland region)
 VPC only
 
 name: tech508-mohammed-2tier-first-vpc
 IPv4 CIDR: 10.0.0.0/16
 name tag already filled in
 Create VPC vpcsettings
 Create subnets
 Subnets (left dashboard under Virtual private cloud)
 Create subnet
 select VPC from above
 name: tech508-mohammed-public-subnet
 Availability Zone: 1a
 IPv4 subnet CIDR block: 10.0.2.0/24 subnets
 Add new subnet
 name: tech508-mohammed-private-subnet
 Availability Zone: 1b
 IPv4 subnet CIDR block: 10.0.3.0/24
 Create subnet
 Create internet gateway
 Internet gateways
 Create internet gateway
 name: tech508-mohammed-2tier-first-vpc-ig
 Create internet gateway
 Attach to a VPC (or Actions -> Attach to VPC)
 select VPC
 Attach internet gateway
 Create route tables
 Route tables
 Create route table
 name: tech508-mohammed-2tier-first-vpc-public-rt
 select VPC
 Create route table
 Actions -> Edit subnet associations
 check public subnet
 Save associations
 Edit routes
 Add route
 Destination: 0.0.0.0/0
 Target: Internet Gateway
 igw-: select internet gateway ID from dropdown
 Save changes
 Create VMs
 Launch instance from database AMI
 
 name: tech508-mohammed-in-2tier-vpc-sparta-app-db
 instance type, key pair same as usual
 Network settings:
 Edit
 select VPC
 select private subnet
 Auto-assign public IP: Disable
 Create security group:
 name: tech508-mohammed-2tier-vpc-sparta-app-db-sg-allow-27017
 remove SSH
 Add security group rule: custom tcp, 27017, anywhere (or can add CIDR block)
 Launch instance
 Launch instance from app AMI
 
 name: tech508-mohammed-in-2tier-vpc-sparta-app
 
 Network settings:
 
 select VPC
 select public subnet
 Auto-assign public IP: Enable
 Create security group:
 name: tech508-mohammed-2tier-vpc-sparta-app-allow-SSH-HTTP
 SSH Source type: My IP
 Add security group rule: HTTP - from anywhere
 User data (make sure to use IP of database instance just created):
 
 #!/bin/bash
 cd repo/app
 export DB_HOST=mongodb://10.0.3.96:27017/posts
 pm2 start app.js
 Launch instance
 
 Clean up
 Terminate instances
 Delete VPC
 route table, security groups and subnets will be deleted along with it
 
 
 
 VPC 
 1) Go to VPC on AWS
  
 2) Create VPC
  
 3) VPC ONLY
  
 4) Name tag: tech508-mohammed-2tier-first-vpc
  
 5) IPv4 CIDR: 10.0.0.0/16
  
 vpc is done now we are doing subnets
  
 6) click subnets on left
  
 7) create subnet
  
 8) VPC ID: enter your vpc name
  
 9) Subnet name: tech508-mohammed-public-subent
  
 10) Availability Zone: 
 Europe (Ireland) / euw1-az3 (eu-west-1a)
  
 11) IPv4 subnet CIDR block
 10.0.2.0/24
  
  
  
  
 12) Add subnet
  
 13) and enter the following
  
  
  
 14) create subnet
  
 we are now creating internet gateway
  
 15) On the left click internet gateways
  
 16) click create internet gateway
  
 17) Name tag: tech508-mohammed-2tier-first-vpc-ig
  
 18) click create internet gateway
  
 19) Click attach to a VPC
  
 20) select your vpc
  
 21) attach internet gateway
  
 22) click on the left side: route tables
  
 23) click create route table
  
 24) name tag: tech508-mohammed-2tier-first-vpc-public-rt
  
 25) select your VPC
  
 26) Click create route table
  
 Now we need associate our route a specific subnet
  
 27) Click actions > edit subnet associations
  
 28) select public and save
  
 29) click edit routes
  
 30) click add route
  
 31) Destination 0.0.0.0/0
  
 32) Target: internet gateway 
  
 33) select that field igw again and select first option on drop down
  
 34) save
 
 
