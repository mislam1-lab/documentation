
**user date**
The reason why we create the Database first is because we need the private ip address in order to create the front end otherwise it wouldnt work and give the private ip to the front end.

# Automate User Data Level 3

## 1. Launch a New App Database Instance
- **Name**: `tech508-mohammed-sparta-app-db`
- **AMI**: Ubuntu 22.04 LTS
- **Key Pair**: Use your designated key pair
- **Security Group**: `tech508-mohammed-sparta-app-db-SSH`
  - **Inbound Rules**:
    - SSH (22) from `0.0.0.0/0`
    - Custom TCP (Port Range: 27017, Description: SPARTA APP) from `0.0.0.0/0`
- **Advanced Settings**:
  - Scroll to bottom and paste `prov-db.sh` script
- **Launch the Instance**
![alt text](image-94.png)


### Connect to DB Instance
```bash
# Open Git Bash terminal
cd .ssh
chmod 400 "tech508-mohammed-aws.pem"
ssh -i "tech508-mohammed-aws.pem" ubuntu@ec2-3-248-213-130.eu-west-1.compute.amazonaws.com
sudo systemctl status mongod  # Check if MongoDB is running
```

- Go back to instance and copy the **private IP**.

---

## 2. Launch a New App Instance
- **Name**: `tech508-mohammedtest-sparta-app`
- **AMI**: Ubuntu 22.04 LTS
- **Key Pair**: Use your designated key pair
- **Security Group**: `tech508-mohammed-sparta-app-SSH`
  - **Inbound Rules**:
    - SSH (22) from `0.0.0.0/0`
    - HTTP (80) from `0.0.0.0/0`
    - Custom TCP (Port Range: 3000, Description: SPARTA APP) from `0.0.0.0/0`
- **Advanced Settings**:
  - Scroll to bottom and paste `prov-app.sh` script
  - **IMPORTANT**: Paste the **private IP** from the **DB instance** into the `export DB_HOST` section of the script
- **Launch the Instance**

### Connect to App Instance
```bash
# Open Git Bash terminal
cd .ssh
chmod 400 "tech508-mohammed-aws.pem"
ssh -i "tech508-mohammed-aws.pem" ubuntu@ec2-34-244-208-198.eu-west-1.compute.amazonaws.com
sudo systemctl status nginx  # Check if NGINX is running
```

- Copy **public IP** and paste it into browser (start with `http://`)
- Append `/posts` to the URL to check functionality

---