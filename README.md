
###  Provisioning Guide: Sparta Tech App & MongoDB (Ubuntu)
This guide walks you through how to get a simple full-stack app running. The app has two parts:

A frontend – what users see when they go to a website
A backend/database – where the app stores and retrieves data
You will set up two virtual machines (VMs) using AWS EC2:

One for the Sparta frontend app
One for the MongoDB backend database

**Why Are We Doing This?**
This isn’t just about running an app. It’s about learning how to deploy software properly, using manual steps first, then moving toward automation with scripts, user data, and AMIs.

Think of it like learning to ride a bike:

First, you go step by step.
Then you add speed and control.
Eventually, you automate and refine the process.
What Does the Sparta App Need?
You will deploy a full-stack application that includes:

A frontend app (user interface)
A MongoDB backend (database)
Required Tools & Configurations:
Frontend EC2 Instance:
Ubuntu 24.04 LTS
Node.js v20
NGINX (for reverse proxy)
PM2 (to keep the app running)
Git (to clone the repo)
Port 3000 open
Security Group allowing SSH, HTTP, and TCP:3000
Backend EC2 Instance:
Ubuntu 22.04 LTS
MongoDB 7.0 or 8.0
Port 27017 open
bindIp set to 0.0.0.0 (for external access)
Security Group allowing SSH and TCP:27017


**Why Manual First?**
Before jumping into automation, it’s best to do things manually to:

Understand each step
Spot and fix issues
Learn how the app works
Lay the groundwork for automation
<img width="1453" height="615" alt="image" src="https://github.com/user-attachments/assets/6c750206-493e-40cb-b68f-ca4d36a80cf9" />

 A **Sparta Tech Node.js app** with NGINX as reverse proxy
- A **MongoDB 7.0 database**
- All provisioning steps are via terminal commands on Ubuntu
 
---
 ##  Part 1: Setup the Sparta Tech App
 Open the launch instance from AWS: https://statics.teams.cdn.office.net/evergreen-assets/safelinks/2/atp-safelinks.html

- Start new instance with name:tech508-mohammed-test-sparta-app
- Select supported app AMI version: 22.04 LTS
- Select Ubuntu

![alt text](/Image%20folder/image-81.png)

- Enter your key pair: tech508-mohammed-aws
- Enter security group with allows for SSH & HTTP
   click edit
   Source type anywhere (so any one can access port); tech508-YOURNAME- sparta-app
   Type: SSH on port 22 (for connecting) after cick on add security
         HTTP on port 80 (for website access)
after click on add security
Custom TCP on port 3000 (to run the app)
-  Launch instance

![alt text](/Image%20folder/image-82.png)

- Click connect & copy ssh command
 
 ![alt text](/Image%20folder/image-83.png)

 
 

 
### 1. Update Your System
 
 Open git bash terminal
 you need to cd into .ssh folder then paste the ec2 instance command to the instance.  

 Next command would be 

- `sudo apt update`

- `sudo apt upgrade`
 
 Next we will do installation for nginx by doing 
 
- `sudo apt install nginx`
- `sudo apt-get update`
 
- `sudo apt-get upgrade -y`
- `sudo apt-get install nginx -y`
 
#### configure reverse proxy using nginx
 
#### Installing NodesJs v20
- `sudo DEBIAN_FRONTEND=noninteractive bash -c "curl -fsSL https://deb.nodesource.com/setup_20.x | bash -" && \
sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs`
 
##### check node version
- `node -v`
 
#### git clone command
- `git clone https://github.com/mislam1-lab/sparta-test-app-cicd.git repo`
 
once done that you do cd repo/app


#### set env var for connections to database
 
- export DB_HOST=mongodb://172.31.24.151:27017/posts
 
##### cd into app folder
- cd repo/app
 
#### Install package for the app
- npm install
 
#### Start app
- npm start
 
![alt text](/Image%20folder/image-49.png)


<<<<<<< HEAD

### Script for the provisioning data base ###
![alt text](/Image%20folder/image-110.png)
 
- Follow the same steps to create the new vm named -tech508-mohammed-sparta-db
- 
 ![alt text](/Image%20folder/image-65.png)
=======
##  Part 2: Setup the Database & Script for the provisioning data base
![alt text](/Image%20folder/image-110.png)
 
- Follow the same steps to create the new vm named -tech508-mohammed-sparta-db
 ![alt text](/Image%20folder/image-65.png)
>>>>>>> a185690a1eead9216e0e18546c62f85756e2869e

Next we connect and get the ssh command to then connect to the git bash terminal where with first cd into .ssh then paste the code to connect. 
### 1. Update Your System
Next we go  
 
<<<<<<< HEAD
- sudo apt-get update
 ![alt text](/Image%20folder/image-67.png)
- sudo apt-get upgrade -y
- ![alt text](/Image%20folder/image-68.png)
=======
- `sudo apt-get update`
  
 ![alt text](/Image%20folder/image-67.png)
- `sudo apt-get upgrade -y`
  
 ![alt text](/Image%20folder/image-68.png)
>>>>>>> a185690a1eead9216e0e18546c62f85756e2869e
 
### Download Mongodb 22.4 version 7 from the browser and follow the steps given
 
#### Import PUblic key
 
- `curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor`

 ![alt text](/Image%20folder/image-69.png)

### create list file
- echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

![alt text](/Image%20folder/image-70.png)

#### Reload the package database
<<<<<<< HEAD
- sudo apt-get update
 ![alt text](/Image%20folder/image-71.png)
=======

- `sudo apt-get update`
  
 ![alt text](/Image%20folder/image-71.png)

>>>>>>> a185690a1eead9216e0e18546c62f85756e2869e
##### install mongodb
- `sudo apt-get install -y \
   mongodb-org=7.0.22 \
   mongodb-org-database=7.0.22 \
   mongodb-org-server=7.0.22 \
   mongodb-mongosh \
   mongodb-org-shell=7.0.22 \
   mongodb-org-mongos=7.0.22 \
   mongodb-org-tools=7.0.22 \
<<<<<<< HEAD
   mongodb-org-database-tools-extra=7.0.22"
 ![alt text](/Image%20folder/image-72.png)

#### Check the status:
- sudo systemctl status mongod ( you can see the status in not enabled now )
 ![alt text](/Image%20folder/image-73.png)
=======
   mongodb-org-database-tools-extra=7.0.22`
  
 ![alt text](/Image%20folder/image-72.png)

#### Check the status:
- `sudo systemctl status mongod` ( you can see the status in not enabled now )
  
 ![alt text](/Image%20folder/image-73.png)
 
>>>>>>> a185690a1eead9216e0e18546c62f85756e2869e
####  Take a back up file for mongod.configure
The next step is go to root directory which is cd / and then cd into etc and do ls to list the file which you mongod.comnf

-`sudo cp mongod.conf mongod.conf.bk`
 
#### Create an enviorment variable inside mongod.conf
 
- `cd /etc`
- `sudo nano mongod.conf`
from there we change the binip to 0.0.0.0 in order to allow exernal connection to happen

- `export DB_HOST=mongodb://172.31.24.86:27017/posts`
 
#### Now run the systemctl command :
- `sudo systemctl start mongod`
 
- `sudo systemctl enable mongod`
 
![alt text](/Image%20folder/image-78.png)
 
#### Run, stop, re-run Sparta app in the background with &
 
- To start app: Instead of npm start in your app script, replace with:
nohup npm start &
- To stop app: find process ID (node ...) - this process will be engaging 3000
- kill it
- make sure you can re-run it
 
 
<<<<<<< HEAD
![alt text](/Image%20folder/Sparta_app.png)
=======
<img width="914" height="481" alt="image" src="https://github.com/user-attachments/assets/2c40256f-4d32-45dd-ba19-070bb547615e" />

>>>>>>> a185690a1eead9216e0e18546c62f85756e2869e








 
 
 
 
 
=======
# sparta-cloud-docs
>>>>>>> 96c7a85907ad53b177527ac1c1fd0c386ee8626f
