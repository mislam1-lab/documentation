## 3. Documentation for AMI Creation
An AMI (Amazon Machine Image) is a preconfigured virtual machine image to launch new instances from. They include:
OS
software installed
any other data
of the VM the image was taken from.

The app script has already been run through user data in the original VM and will not be run again -> everything has been installed and configured but the app itself will not be told to start up.

A small script should be given in user data of the new VM being made from the image including:

#!/bin/bash
cd into app folder
set environment variable for database private IP
pm2 start app.js
Creating VMs from AMIs
create database and app VMs with previous scripts in user data, make sure the app works exactly as it should
on each VM:
Actions -> Create image
naming convention: tech508-tabitha-test-sparta-app-ready-to-run-database/app
Add new tag
"Name" - name as above
Create image
when status is Available, Launch instance from AMI
database as usual
app needs short script in user data:
see run-app-only.sh
#!/bin/bash to define bash script
export DB_HOST=mongodb://<privIP>:27017/posts set env var for database IP
make sure to change to the new db instance IP
cd repo/app navigate to app folder
pm2 stop all stop any previous processes
pm2 start app.js start the app






### Create DB AMI
1. First we go to **Instances > Actions > Image and Templates > Create Image**
![alt text](/Image%20folder/image-75.png)
1. Name: `tech508-mohammed-test-sparta-app-ready-to-run-database`
![alt text](/Image%20folder/image-76.png)
1. Add Tag:
   - **Key**: `Name`
   - **Value**: `tech508-mohammed-test-sparta-app-ready-to-run-database`
2. Click **Create Image**

![alt text](/Image%20folder/image-77.png)

### Create App AMI
1. Go to **Instances > Actions > Image and Templates > Create Image**
2. Name: `tech508-mohammed-test-sparta-app-ready-to-run-app`
3. Add Tag:
   - **Key**: `Name`
   - **Value**: `tech508-mohammed-test-sparta-app-ready-to-run-app`
4. Click **Create Image**

---

## 4. Launch Instances from AMIs

### Database Image
1. Go to **Images > AMIs**
2. Select **database AMI** and launch an instance
3. **Name**: `tech508-mohammed-test-sparta-app-db-image`
4. **Security Group**: `tech508-mohammed-sparta-app-db-Image-SSH`
   - **Inbound Rules**:
     - SSH (22) from `0.0.0.0/0`
     - Custom TCP (27017, Description: SPARTA APP) from `0.0.0.0/0`

### App Image
1. Select **app AMI** and launch an instance
2. **Name**: `tech508-mohammed-test-sparta-app-image`
3. **Key Pair**: Use your designated key pair
4. **Security Group**: `tech508-mohammed-sparta-app-SSH`
   - **Inbound Rules**:
     - SSH (22) from `0.0.0.0/0`
     - HTTP (80) from `0.0.0.0/0`
     - Custom TCP (3000, Description: SPARTA APP) from `0.0.0.0/0`

### Advanced Settings - User Data Script
```bash
#!/bin/bash
export DB_HOST=mongodb://172.31.21.64:27017/posts  # Replace with private IP of DB instance
cd repos/app
npm install
pm2 stop all
pm2 start app.js
```

- **IMPORTANT**: Make sure to update `DB_HOST` with the actual **private IP** of the **DB instance**
- Copy **public IP** and check in browser using `http://<public-ip>/posts`
 

 **Task: Automate app Stage 4 - Create and test app + DB images**
 
 Back end DB Image
 Step 1 on the db instance go to Action and Image and templates then click create image
 
 Step 2: next create a name called tech508-mohammed-test-sparta-app-ready-to-run-database and same with the value optional.
 
 Step 3: in the section below click Add new tag and put Name for key and value optional tech508-mohammed-test-sparta-app-ready-to-run-database then click Create image
  
 Front end DB Image
 Step 1 on the App instance go to Action and Image and templates then click create image
 
 Step 2: next create a name called tech508-mohammed-test-sparta-app-ready-to-run and same with the value optional.
 
 Step 3: in the section below click Add new tag and put Name for key and value optional tech508-mohammed-test-sparta-app-ready-to-run then click Create image
 
 
 
 
 
 
 
 
 
 
 
 
 
 ubuntu@ip-172-31-29-198:~$ lsof -i :3000
 ubuntu@ip-172-31-29-198:~$ ls
 prov-db.sh
 ubuntu@ip-172-31-29-198:~$ ./prov-db.sh
 
