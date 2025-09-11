
**Task: Automate app Stage 3 - Automate app deployment with user data**




Backend 
Step 1: create instance from virtual machine called tech508-mohammed-sparta-app-db2

Step 2: once created we then add all the previous group and on the bottom of instance we add this script on the instance: 


![alt text](/Image%20folder/image-95.png)

`#!/bin/bash`
 
#Scipt for the database
 
# Update the package list to make sure the latest versions are installed
`echo "update..."`
`sudo DEBIAN_FRONTEND=noninteractive apt-get update`
`echo "update done"`
`echo`
 
`echo "upgrade..."`
`sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y`
`echo "upgrade done"`
`echo`
 
`sudo DEBIAN_FRONTEND=noninteractive apt-get install gnupg curl`
 
`echo "import public key"`
`curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \`
`gpg --dearmor | sudo tee /usr/share/keyrings/mongodb-server-7.0.gpg > /dev/null`
`echo "imported public key"`
`echo`
 
#create list file
`echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list`
 
`sudo DEBIAN_FRONTEND=noninteractive apt-get update`
 
`#install mongodb`
`sudo DEBIAN_FRONTEND=noninteractive apt-get install -y \`
   `mongodb-org=7.0.22 \`
   `mongodb-org-database=7.0.22 \`
   `mongodb-org-server=7.0.22 \`
   `mongodb-mongosh \`
   `mongodb-org-shell=7.0.22 \`
   `mongodb-org-mongos=7.0.22 \`
   `mongodb-org-tools=7.0.22 \`
   `mongodb-org-database-tools-extra=7.0.22`
 
   `echo`
`echo " Configuring MongoDB to Allow External Connections"`
`echo`
 
# Backup existing config
`sudo cp /etc/mongod.conf /etc/mongod.conf.bk`
 
# Update bindIp to 0.0.0.0
`sudo sed -i 's/bindIp: 127.0.0.1/bindIp: 0.0.0.0/' /etc/mongod.conf`
`echo "bindIp updated to 0.0.0.0"`
`echo`
 
`echo`
`echo " Starting MongoDB..."`
`echo`
`sudo systemctl start mongod`
`sudo systemctl enable mongod`
 
`echo`
`echo`
`echo " MongoDB provisioned and running "`
`echo`

Step 3: Once done create instance and then launch

Frontend:
Step 1: create instance from virtual machine called tech508-mohammed-test-sparta-app

Step 2: then add all details from security details and even add the script as well:

# provision sparta test app
`#!/bin/bash`
`echo`
`echo "Update..."`
`sudo DEBIAN_FRONTEND=noninteractive apt update`
`echo "Done"`
`echo`

`echo`
`echo "installing"`
`#FIX! expects user input`
`sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y`

`#FIX! expects user input`
`sudo DEBIAN_FRONTEND=noninteractive apt-get install nginx -y`

`echo "Back up nginx default file"`
`sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bk`

# configure reverse proxy using nginx
`sudo sed -i 's,try_files $uri $uri/ =404;,proxy_pass http://localhost:3000/;,' /etc/nginx/sites-available/default`
`sudo sudo systemctl restart nginx `
#Installing NodeJs v20 (installs node and npm commands)
`sudo DEBIAN_FRONTEND=noninteractive bash -c "curl -fsSL https://deb.nodesource.com/setup_20.x | bash -" && \`
`sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs`

#check node version
`node -v`

#download app code and put in folder repo
`git clone https://github.com/mislam1-lab/tech508-sparta-app.git repo`

#cd into app folder
`cd repo/app`

#set env var for connection to database
`export DB_HOST=mongodb://172.31.17.136:27017/posts`

# install packages for app
`npm install`

#start app
npm start &

Step 3: once we launched the instance we then test the public ip doing http://172.31.17.136/posts 
