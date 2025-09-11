

**Jenkins and CI/CD**
What is Jenkins?
Jenkins is an open-source automation tool that helps teams build, test, and deploy software automatically.
Instead of doing everything manually, Jenkins can:

Pull code from a repository (e.g., GitHub)
Build the application
Run automated tests
Deploy the application to servers
This makes software delivery faster, more reliable, and less error-prone.

**What is CI/CD?**
CI/CD stands for Continuous Integration and Continuous Delivery/Deployment. It is a modern way of delivering software efficiently.

**Continuous Integration (CI)**:
Every time developers push new code, it is automatically built and tested.
Goal: Catch errors early and ensure new code works with the project.

**Continuous Delivery (CD)**:
After passing tests, the code is packaged and prepared for release.
Goal: Code is always ready to be deployed with one click.

**Continuous Deployment (CD)**:
The process goes one step further — the code is automatically deployed to production.
Goal: Fully automated release pipeline.

Together, CI/CD ensures that updates are delivered quickly, consistently, and safely.

**Why Use Jenkins with AWS?**
When Jenkins is combined with Amazon Web Services (AWS), it becomes even more powerful.
AWS provides the cloud infrastructure, while Jenkins handles the automation.

Typical integrations include:

Deploying applications to AWS EC2 servers
Storing build files in S3 buckets
Running scalable apps with ECS or EKS
Automating infrastructure with Terraform or CloudFormation
This creates a smooth pipeline where code moves from development → testing → deployment in the cloud.

CI/CD Pipeline with Jenkins and AWS
Below is a simple diagram of how Jenkins integrates with AWS services in a CI/CD pipeline:
<img width="1470" height="861" alt="image" src="https://github.com/user-attachments/assets/da421245-6e51-487c-8cd5-88ebffec8564" />


**Creation of Jenkins Server on AWS**
Login to your aws and click launch instance

Give a name tag "mohammed-jenkins-server"

-Select UBUNTU as your AMI
-Select Ubuntu server 22.04 LTS

For the instance type select t3.small well depends on how big is your project
Select your key pair login

Click on allow HTTP and create your security group if you haven't "mohammed-jenkins-allow-http" and description "mohammed-jenkins-allow-http-this-is-jenkins"
Select the inbound security group rule to :

SSH
Type: SSH

Port: 22

Source: your IP only

Jenkins Web UI

Type: Custom TCP

Port: 8080

Source: your IP only

Storage: 30–40 GB gp3 is comfortable.

Click Launch instance → wait until Instance state = Running.

<img width="1617" height="693" alt="image" src="https://github.com/user-attachments/assets/b2545a24-e284-4164-8822-b7791d1523eb" />


Open gitbash cd to your ssh and connect to your jenkins ec2 you have created
Click you jenkins ec2 and click connect and copypaste into your gitbash command follows :
`chmod 400 "tech508-mohammed-aws.pem`
`ssh -i "tech508-mohammed-aws.pem" ubuntu@ec2-108-130-144-5.eu-west-1.compute.amazonaws.com`

You must do the Update system apt-get update && sudo apt-get -y upgrade adn after sudo timedatectl set-timezone Europe/London   # optional, set correct timezone
Install Java (required by Jenkins) sudo apt-get install -y openjdk-17-jre
Check Java version java -version and should give you version 17 this is the output you should get:

**Task: 3-job Jenkins pipeline to deploy Sparta test app**

<img width="560" height="219" alt="image" src="https://github.com/user-attachments/assets/e1ac12c1-cc2e-4bc0-bfd8-75df6e390a8d" />


**Creating and testing a basic project (get date and time)**
Creation
Click New Item in the sidebar
Enter a descriptive project name
For this example, use rubaet-get-date-and-time
Select Freestyle Project, then click OK
It will take you to the configure page
Enter a description in the box type "testing jenkins"
Tick the box next to Discard old builds, then specify a maximum number of old builds to keep
For this example, choose 5
Scroll down to Build Steps
Click Add build steps, and select Execute shell from the drop-down
inside the Execute Shell write "uname -a " to find linux name .
Click Save
You can check console output to see output


Pre-requisites: You already have:

Job 1 <yourname>-job1-ci-test working

![alt text](/Image%20folder/image-9.png)
Steps:

Step 1: Get Job 2 working:

        
        Name Job 2: similar to project-tech508-mohammed-job2-ci-merge


If you haven't already, create a dev branch on your local git repo sparta-test-app-cicd
Make a change to dev branch and push the code to GitHub - the webhook needs to trigger the pipeline


**Step 1:** on Jenkins github project is the same
 
source code management > 
Repository URL
git@github.com:mislam1-lab/sparta-test-app-cicd.git

Credentials: mohammeds-jenkins-to-github-key(to read/write repo)

Branches to build > */dev

![alt text](/Image%20folder/image-11.png)


 
**additional behaviour > merge before build > use link**

![alt text](/Image%20folder/image-12.png)
 
Build triggers > mohammed-tech508-job1-ci-test > 
Trigeer only if build is stable
![alt text](/Image%20folder/image-13.png)
 
build environment > ssh agent > add your credentials
 in your git bash you do ls command and you find your key then you do $ cat mohammed-jenkins-to-github-key.pub
then copy and paste these credentials you add it in your ssh agent.

build steps > execute shell > find out the code for these steps

 
post build actions > push only if build succeds and merge results 

Branch to push > main 

**Target remote name > origin**


![alt text](/Image%20folder/image-14.png)

**Next is got to post build and put git publisher**


![alt text](/Image%20folder/image-15.png)




Step 5: go to git bash and cd tech508-sparta-test-app-cicd:

        then you do nano README.md  within that you write Test webhook 12:17 then save and exit
        The next command is doing git add . 
        The next Command is git commit -m "change readme 12:17"
        The next command is git push origin main
        git branch dev
        git switch dev 
        git push origin dev
        
        Name Job 2: similar to project-tech508-mohammed-job2-ci-merge



Next you go back to Jenkins to see if its merges the first job and second job

![alt text](/Image%20folder/image-18.png)










        


**Get Job 3 working:**
Step 1: Name Job 3: similar to <yourname>-job3-cd-deploy

Copy the updated & tested code from Jenkins to the AWS EC2 instance

Jenkins will need to SSH into the EC2 instance to update the code & re-run the app
Each person will need to add the key/SSH credentials/pem file on Jenkins for Job 3 to access your EC2 instance
To copy the code from Jenkins to the EC2, use the scp or rsync commands from Jenkins
DO NOT: git clone from main branch and push to production

**You will need an EC2 instance already running**

the next step is to get your Deploy key working and you mention mohammed-jenkins-to-github-key


go into my .ssh file and do ls where i find my key which is tech508-mohammed-aws.pem and would do this command 
$ cat tech508-mohammed-aws.pem and copy and paste the private key credentials.


key/SSH credentials/pem file on Jenkins for Job 3 to access your EC2 instance

![alt text](/Image%20folder/image-90.png)


Step 5: go to git bash and cd tech508-sparta-test-app-cicd:

        then you do sudo nano INDEX.js within that you write Test webhook 12:17 then save and exit
        The next command is doing git add . 
        The next Command is git commit -m "change readme"
        The next command is git push origin dev
        

The next step is to check if its working or not





![alt text](/Image%20folder/image-7.png)

![alt text](/Image%20folder/image-8.png)




![alt text](/Image%20folder/image-23.png)






        
