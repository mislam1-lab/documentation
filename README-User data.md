
**Task: Automate app Stage 3 - Automate app deployment with user data**

User data:  instead of manuel deploy the sparta app, this script can manually provision it by itself.

Backend 
Step 1: create instance from virtual machine called tech508-mohammed-sparta-app-db2

Step 2: once created we then add all the previous group and on the bottom of instance we add this script on the instance: 


![alt text](/Image%20folder/image-95.png)

#Scipt for the database
 
# Update the package list to make sure the latest versions are installed

`#!/bin/bash`
- Tells the system to run the script using the Bash shell. 

`echo "update..."`
`sudo DEBIAN_FRONTEND=noninteractive apt-get update`
`echo "update done"`
`echo`

- apt-get update: Refreshes the package index.
- DEBIAN_FRONTEND=noninteractive: Prevents prompts during execution—critical for automation.

`echo "upgrade..."`
`sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y`
`echo "upgrade done"`
`echo`

- apt upgrade -y: Installs newer versions of packages.
- -y: Automatically confirms prompts

So in this section we want to make commands non interactive, so the script can run by it self
 
`sudo DEBIAN_FRONTEND=noninteractive apt-get install gnupg curl`

- gnupg: Needed to handle GPG keys.
- curl: Used to fetch the MongoDB public key.

`echo "import public key"`
`curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \`
`gpg --dearmor | sudo tee /usr/share/keyrings/mongodb-server-7.0.gpg > /dev/null`
`echo "imported public key"`
`echo`

- curl -fsSL: Fetches the key silently.
- gpg --dearmor: Converts ASCII-armored key to binary format.
- tee: Writes the key to a secure location for APT to verify packages.
- > /dev/null: Suppresses output for cleaner logs.

#create list file
`echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list`

 - Adds MongoDB’s official repo for Ubuntu Jammy.
- signed-by: Ensures packages are verified using the imported GPG key

`sudo DEBIAN_FRONTEND=noninteractive apt-get update`

- Required after adding a new repository so APT can find MongoDB packages


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
   
- Installs specific versions of MongoDB components.
- Pinning versions (e.g. =7.0.22) ensures consistency across environments.
- mongodb-mongosh: The new shell replacing the legacy mongo CLI.

   
`echo`
`echo " Configuring MongoDB to Allow External Connections"`
`echo`

- This is a placeholder. You’ll likely want to modify /etc/mongod.conf:

# Backup existing config
`sudo cp /etc/mongod.conf /etc/mongod.conf.bk`

• 	Purpose: Creates a backup of the MongoDB configuration file before making changes.
• 	Why it matters: If your changes break MongoDB or cause startup failures, you can quickly restore the original config

# Update bindIp to 0.0.0.0
`sudo sed -i 's/bindIp: 127.0.0.1/bindIp: 0.0.0.0/' /etc/mongod.conf`
- Replaces the line bindIp: 127.0.0.1 with bindIp: 0.0.0.0 in the config file.
- - By default, MongoDB only listens on 127.0.0.1 (localhost), which means it’s inaccessible from other machines.

`echo "bindIp updated to 0.0.0.0"`
`echo`
- Purpose: Provides feedback in the terminal or logs so you know the change was made.

`echo`
`echo " Starting MongoDB..."`
`echo`
`sudo systemctl start mongod`
`sudo systemctl enable mongod`

- systemctl start mongod: Starts the MongoDB service immediately.
- systemctl enable mongod: Ensures MongoDB starts automatically on system boot.

 
`echo`
`echo`
`echo " MongoDB provisioned and running "`
`echo`

- Purpose: Signals that the setup is complete.
- Operational clarity: Useful for CI/CD logs or cloud-init output to confirm success

**Step 3: Once done create instance and then launch**

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

- apt update: Refreshes the local package index so the system knows about the latest versions.
- DEBIAN_FRONTEND=noninteractive: Prevents any interactive prompts—critical for automation.
- echo statements: Provide visibility in logs or terminal output, useful for debugging and CI traceability.

`echo`
`echo "installing"`
`#FIX! expects user input`
`sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y`

- apt upgrade -y: Upgrades all installed packages to their latest versions.
- -y: Automatically confirms prompts


`#FIX! expects user input`
`sudo DEBIAN_FRONTEND=noninteractive apt-get install nginx -y`

- Installs the Nginx web server.
- Again, DEBIAN_FRONTEND=noninteractive and -y should suppress prompts.


`echo "Back up nginx default file"`
`sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bk`

- If you plan to modify /etc/nginx/sites-available/default, this gives you a rollback path.
- Especially useful in automation where a bad config could break the web server


# configure reverse proxy using nginx
`sudo sed -i 's,try_files $uri $uri/ =404;,proxy_pass http://localhost:3000/;,' /etc/nginx/sites-available/default`
`sudo sudo systemctl restart nginx `
- Replaces the default static file handler with a reverse proxy directive
- By default, Nginx serves static files from /var/www/html

#Installing NodeJs v20 (installs node and npm commands)
`sudo DEBIAN_FRONTEND=noninteractive bash -c "curl -fsSL https://deb.nodesource.com/setup_20.x | bash -" && \`
`sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs`

- Downloads and runs the NodeSource setup script for Node.js v20.
- Installs both node and npm.

#check node version
`node -v`
- Prints the installed Node.js version.
  
#download app code and put in folder repo
`git clone https://github.com/mislam1-lab/tech508-sparta-app.git repo`
-  Downloads your app code into a folder named repo

#cd into app folder
`cd repo/app`
- Moves into the app’s working directory where package.json and source files live

#set env var for connection to database
`export DB_HOST=mongodb://172.31.17.136:27017/posts`
- Sets the DB_HOST environment variable used by the app to connect to MongoDB.

# install packages for app
`npm install`
- Installs all packages listed in package.json.

#start app
npm start &
- Launches the app in the background.
  
Step 3: once we launched the instance we then test the public ip doing http://172.31.17.136/posts 
