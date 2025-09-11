**Auto-Scaling with AWS**
Scaling Overview
Scaling is the process of adjusting computing resources to match workload demand. It ensures applications remain responsive and cost-efficient by dynamically increasing or decreasing capacity.

There are two main types of scaling: vertical and horizontal.

**Types of Scaling**
Vertical Scaling
Vertical scaling increases the capacity of a single virtual machine (VM) by adding more resources such as CPU, RAM, or storage.

The workload is moved from a smaller instance type to a larger one, and the smaller instance is then decommissioned. While effective for boosting performance, scaling down can be more challenging depending on the cloud provider.

Sizing terminology:

Scale up → add more resources to a single VM
Scale down → reduce resources of a single VM
Horizontal Scaling (AWS Preferred Method)
Horizontal scaling increases capacity by adding more instances (VMs) rather than resizing a single one. These instances are typically identical and distributed behind a load balancer.

With auto-scaling, AWS automatically adjusts the number of instances to meet demand, adding capacity during peak times and removing unnecessary instances when demand falls.

Sizing terminology:

Scale out → add more instances
Scale in → remove instances
Deploying the Sparta app with an AWS auto-scaling group
This set of instructions allows for high availability and scalability for the app. It does not provide instructions for using the app with the database.

Basic overview
Create an app VM with your preferred method
Create an app AMI
Create a launch template (including user data containing the script needed to run the app)
Create an auto-scaling group using the launch template
Set up a scaling policy
To avoid a single point of failure
Have the auto-scaler spread the new VMs across availability zones within the region
For the Sparta test app, use the Ireland region and availabiliy zones 1a, 1b, 1c
The load balancer will spread traffic across the created VMs
Set a max, min and desired number of machines
For the sparta app, set the min and desired to 2 and the max to 3
 
 
 
 
 **why use an AWS auto scaling group**
 
 when the App virtual machine has high level of cpu it starts to fall over.
 
 App virtual machine has a cloud watch monitoring the cpu load and if you miss it it goes to dashboard to notify you missed it
 
 Application virtual machine 
 
 Autoscaling stops the cpu to be overloaded and increases the number of instances.
 
 Types of scaling
 
 vertical one side cpu
 if you want a bigger virtual machine
 
 Virtual machine-2 cpu, 1 gig memoery 
 Change the size of the virtual machines
 
 
 Horizontal memoery, cpu
 Create more virtual machines 
 in and out 
 
 How does autoscaling work
 
 App virtual machine- creates-AMI app-launch template- autoscaling group-scaling policy cpu 50% min 2 desired and max 2
 
 What creates high availability- 3 virtual machines if one goes down two will continue on the other is poor health
 
 load balancer- balance load of the virtual machines-http internet 
 helps the scalability if you just one cpu it could be crash but have mutlpple 
 
 Explain High availability and scalability
 you need set up load balancer it can compaseate the two scaling verticval or horizontal
 
**Creating a launch template**
In the EC2 sidebar, go to Instances Launch Templates
In the top right-hand corner, click Create launch template
Give the template a descriptive name using the standard naming convention
It should start with tech508-mohammed
(Optional) Add a description
Under Application and OS Images (Amazon Machine Image), select My AMIs> Owned by me, then search for the appropriate AMI
Select t3.micro from the Instance Type drop-down
Select the correct key pair from the Key pair (login) drop-down
Under Network settings, choose Select existing security group, then search for an appropriate one
For the Sparta app, the security group should allow SSH and HTTP
Paste the script into the User data box under Advanced details which is this:
bash
   #!/bin/bash
   cd repo/app
   pm2 start app.js
To be updated with an option for a database script later
Click Create launch template in the Summary box on the right-hand side
 
 
 **Create Autoscaling groups**
 
 Step 1: Choose launch template or configuration
 
         Autoscaling group name: tech508-mohammed-app-asg-lb
         Launch template: tech508-mohammed-for-asg-app-it
 
 Step 2: Choose instance launch options
         Availability Zones and subnets
         euw1-az3 (eu-west-1a) | subnet-0429d69d55dfad9d2 (DevOpsStudent default 1a)
         euw1-az2 (eu-west-1c) | subnet-013b0b0deea20b0e5 (DevOpsStudent default 1c)
         euw1-az1 (eu-west-1b) | subnet-00ac052b1e40c0164 (DevOpsStudent default 1b)
 
 Step 3: Integrate with other services
 
         Load Balancing- Attach to a new load balancer
         Load balancer type-Application Load Balancer
         Load balancer name-tech508-mohammed-app-asg-lb-1
         Load balancer scheme-internet facing 
         Listeners and routing-Default routing (forward to- New target group name- tech508-mohammed-app-asg-lb-1
         Health checks-Turn on Elastic Load Balancing health checks
         Health check grace period-180 seconda
 
 
          
 
 Step 4: Configure group size and scaling
        
         Desired capacity-2
         Scaling- Min desired capacity-2
         Max desired capacity-3
 
 Step 5: Add notifications
 
        
 Step 6: Add tags
 
         Key-Name
         Value - optional- tech508-mohammed-asg-ha-sc
 
 Step 7: Review
  
         Once you review everything you can Create auto scaling group.
 
 
 
 ![alt text](/Image%20folder/image-6.png)
 
 ![alt text](/Image%20folder/image-5.png)        
 
 
 how to connect the inside the app script into database
 
 In the front end script for you change location by going to export DB_HOST=mongodb://172.31.17.136:27017/posts and you put in the private ip address from the database instance
 
 
 
 and how to deploy of failure application
 
 
 
  #!/bin/bash
 export DEBIAN_FRONTEND=noninteractive
 echo "Updating system..."
 apt-get update -y && apt-get upgrade -y
 echo
 echo "Installing Nginx, Git, Curl..."
 apt-get install nginx git curl -y
 echo
 echo "Configuring NGINX reverse proxy..."
 sed -i 's|try_files $uri $uri/ =404;|proxy_pass http://localhost:3000;|' /etc/nginx/sites-available/default
 systemctl restart nginx
 echo "NGINX configured and restarted."
 echo
 echo "Installing Node.js v20..."
 curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
 apt-get install -y nodejs
 echo "Node.js version: $(node -v)"
 echo
 echo "Installing PM2 globally..."
 npm install -g pm2
 echo
 echo "Cloning your Sparta app repo..."
 git clone https://github.com/ramyadeena/tech508-sparta-app-1.git repo
 cd repo/app
 echo "Repo cloned and moved into app directory."
 echo
 export DB_HOST=mongodb://172.31.28.46:27017/posts
 echo "Environment variable DB_HOST set."
 echo
 echo "Installing app dependencies..."
 npm install
 echo
 echo "Deleting any existing PM2 process named sparta-app (if any)..."
 pm2 delete all || true
 echo
 echo "Starting app with PM2 using npm start..."
 pm2 start npm --name "app.js" -- start
 echo "App started and registered with PM2."
 echo
 echo "Public IP address of this instance:"
 curl -s http://checkip.amazonaws.com
 echo
 echo "App provisioning complete. "
 
 
 
 
 
 
 
 
 
- [](#)
      - [Run, stop, re-run Sparta app in the background with \&](#run-stop-re-run-sparta-app-in-the-background-with-)
- [Automate User Data Level 3](#automate-user-data-level-3)
  - [1. Launch a New App Database Instance](#1-launch-a-new-app-database-instance)
    - [Connect to DB Instance](#connect-to-db-instance)
  - [2. Launch a New App Instance](#2-launch-a-new-app-instance)
    - [Connect to App Instance](#connect-to-app-instance)
  - [3. Documentation for AMI Creation](#3-documentation-for-ami-creation)
    - [Create DB AMI](#create-db-ami)
    - [Create App AMI](#create-app-ami)
  - [4. Launch Instances from AMIs](#4-launch-instances-from-amis)
    - [Database Image](#database-image)
    - [App Image](#app-image)
    - [Advanced Settings - User Data Script](#advanced-settings---user-data-script)
- [Update the package list to make sure the latest versions are installed](#update-the-package-list-to-make-sure-the-latest-versions-are-installed)
- [Backup existing config](#backup-existing-config)
- [Update bindIp to 0.0.0.0](#update-bindip-to-0000)
- [provision sparta test app](#provision-sparta-test-app)
- [configure reverse proxy using nginx](#configure-reverse-proxy-using-nginx-1)
- [install packages for app](#install-packages-for-app)
- [Creating an Auto Scaling group](#creating-an-auto-scaling-group)
  - [Step 1 - Choose launch template or configuration](#step-1---choose-launch-template-or-configuration)
  - [Step 2 - Choose instance launch options](#step-2---choose-instance-launch-options)
  - [Step 3 - Integrate with other services - optional](#step-3---integrate-with-other-services---optional)
  - [Step 4 - Configure group size and scaling - optional](#step-4---configure-group-size-and-scaling---optional)
  - [Step 5 - Add notifications - optional](#step-5---add-notifications---optional)
  - [Step 6 - Add tags - optional](#step-6---add-tags---optional)
  
 # Creating an Auto Scaling group
  
 On aws go to: EC2 > Auto Scaling groups > Create Auto Scaling group
  
 You should already have a working launch template
 ## Step 1 - Choose launch template or configuration
  
 - pick a name for the ASG - for example `tech508-mohammed-app-asg`
 - Pick a launch template - for example `tech508-mohammed-for-asg-app-lt`
  
 The launch template is the template based of the AMI and user data that runs the app without any user input, this means there is minimal manual setup on my end
  
  
 ## Step 2 - Choose instance launch options  
  
 Under the **network** section, leave VPC default but the following [**availability zones**](./images/Screenshot_4.png) are needed
  
 Everything else can be left as default
  
 ## Step 3 - Integrate with other services - optional
  
 Under the **load balancing** section select attach a new load balancer (assuming you dont have one you can already use).
  
 Under **Attach to a new load balancer** configure as following:
  
 - Load balancer type: Application Load Balancer
 - Load blancer name: naming convention - `tech508-mohammed-app-asg-lb`
 - Load balancer scheme: Internet-facing
 - Listeners and routing:
   - port: 80
   - default routing: create a target group
   - set a new target group name: naming convention - `tech508-mohammed-app-asg-lb-tg`
 - Tags:
   - Key: Name
   - Value: tech508-mohammed-app-asg-lb
  
 Under the **Health Checks** section, tick: Turn on Elastic Load Balancing health checks
  
 The other sections can be left as default
  
 The changed settings should look like [This](./images/Screenshot_8.png)
  
 ## Step 4 - Configure group size and scaling - optional
  
 Under the **Group size** section set Desired capacity to 2
  
 Under the **Scaling** section:
  
 - min desired capacity: 2
 - max desired capacity: 3
 - Automatic Scaling: Target tracking scaling policy
     - Scaling policy name: Target Tracking Policy
     - Metric type: Average CPU utilization
     - Target value: 50
     - Instance warmup: 300 seconds
  
 [Here](/Image%20folder/Screenshot_7.png) is a screenshot of what it looks like
  
 Any other sections can be left as defualt
  
 ## Step 5 - Add notifications - optional
  
 Nothing to do here
  
 ## Step 6 - Add tags - optional
  
 Under the **Tags** section add the key pair:
  
 - Key: Name
 - Value: `tech508-mohammed-app-asg-HA-SC`
 
 