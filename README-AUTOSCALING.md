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

**High Avilability**
when a instances fail the autoscaling group would replace it the instance so it front end can continue working. 
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
 **Diagram:**
 <img width="1160" height="837" alt="image" src="https://github.com/user-attachments/assets/d382ced6-17c2-4624-8444-8d0ee7c16f30" />

 
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
 
 
