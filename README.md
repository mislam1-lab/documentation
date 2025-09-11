<<<<<<< HEAD
###  Provisioning Guide: Sparta Tech App & MongoDB (Ubuntu)

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
   Type: Custom TCP :choose port : 3000
-  Launch instance

![alt text](/Image%20folder/image-82.png)

- Click connect & copy ssh command
 
 ![alt text](/Image%20folder/image-83.png)

 
 

 
### 1. Update Your System
 
 Open git bash terminal
 you need to cd into .ssh folder then paste the ec2 instance command to the instance.  

 Next command would be 

 `sudo apt update`

 `sudo apt upgrade`
 
 install nginx by doing 
 
 `sudo apt install nginx`



 
- echo "Updating system..."
- `sudo apt-get update`
- echo "Done"
 
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



### Script for the provisioning data base ###
![alt text](/Image%20folder/image-110.png)
 
- Follow the same steps to create the new vm named -tech508-mohammed-sparta-db
- 
 ![alt text](/Image%20folder/image-65.png)

Next we connect and get the ssh command to then connect to the git bash terminal where with first cd into .ssh then paste the code to connect. 
### 1. Update Your System
Next we go  
 
- sudo apt-get update
 ![alt text](/Image%20folder/image-67.png)
- sudo apt-get upgrade -y
- ![alt text](/Image%20folder/image-68.png)
 
### Download Mongodb 22.4 version 7 from the browser and follow the steps given
 
#### Import PUblic key
 
- curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor

 ![alt text](/Image%20folder/image-69.png)

### create list file
- echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

![alt text](/Image%20folder/image-70.png)

#### Reload the package database
- sudo apt-get update
 ![alt text](/Image%20folder/image-71.png)
##### install mongodb
- "sudo apt-get install -y \
   mongodb-org=7.0.22 \
   mongodb-org-database=7.0.22 \
   mongodb-org-server=7.0.22 \
   mongodb-mongosh \
   mongodb-org-shell=7.0.22 \
   mongodb-org-mongos=7.0.22 \
   mongodb-org-tools=7.0.22 \
   mongodb-org-database-tools-extra=7.0.22"
 ![alt text](/Image%20folder/image-72.png)

#### Check the status:
- sudo systemctl status mongod ( you can see the status in not enabled now )
 ![alt text](/Image%20folder/image-73.png)
####  Take a back up file for mongod.configure
The next step is go to root directory which is cd / and then cd into etc and do ls to list the file which you mongod.comnf

-sudo cp mongod.conf mongod.conf.bk
 
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
 
 
![alt text](/Image%20folder/Sparta_app.png)








 
 
 
 
 
=======
# sparta-cloud-docs
>>>>>>> 96c7a85907ad53b177527ac1c1fd0c386ee8626f
