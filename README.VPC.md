

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

<img width="934" height="716" alt="image" src="https://github.com/user-attachments/assets/5dfd31c7-1883-410d-8ac0-2f0d894f1183" />

