---
title : "Create Security Group for EC2"
date : "2025-06-08"
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---

# Create Security Group for EC2

#### Create VPC Security group for Amazon EC2
1. In the VPC interface
- Select **Security Group**
- Select **Create security group**

![Create Account](/images/1/SG.png)

2. Proceed to configure
- **Security group name**, enter Ecourse-SG
- **Description**, enter Security Group for Ecourse
- Select **VPC** created

![Create Account](/images/1/SG2.png)

3. Configure **Inbound rule**
- To add a rule, select **Add rule**
- **Custom TCP** select port 3000
- **SSH** port 22 used to connect to local machine. Source select **My IP**
- **HTTP** port 80 and source is **Anywhere IPv4**
- **HTTPS** port 443 and source is **Anywhere IPv4**
- Select **Create security group**

![Create Account](/images/1/SG1.png)