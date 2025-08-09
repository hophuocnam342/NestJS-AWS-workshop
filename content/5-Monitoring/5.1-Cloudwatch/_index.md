---
title : "Setting up CloudWatch on EC2"
date : "2025-06-08"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

Open CMD, and SSH into EC2

```bash
ssh -i "ecourse-key.pem" ubuntu@<EC2-PUBLIC-IP>
```
Enter the following commands to install **CLoudƯatch**

- Download the **CloudWatch Agent** installation file from AWS:

```bash
wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
```
- Install the .deb file

```bash
sudo dpkg -i amazon-cloudwatch-agent.deb
```
- Check the actual file thi

```bash
ls /opt/aws/amazon-cloudwatch-agent/bin/
```

![Create Account](/NestJS-AWS-workshop/images/4/CW.png)

![Create Account](/NestJS-AWS-workshop/images/4/CW1.png)

- Run **config wizard**
```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```
![Create Account](/NestJS-AWS-workshop/images/4/CW2.png)

- After running, the wizard will ask questions about configuration options, you can choose 1 to choose the default.

![Create Account](/NestJS-AWS-workshop/images/4/CW3.png)

![Create Account](/NestJS-AWS-workshop/images/4/CW4.png)

![Create Account](/NestJS-AWS-workshop/images/4/CW5.png)

![Create Account](/NestJS-AWS-workshop/images/4/CW6.png)

- When you get to the **log file path** section, this is the path you want to send to **CloudÚatch** for monitoring

- Enter the 3 log file paths of the system, nginx and app:

- System
```bash
/var/log/syslog
```
- Nginx
```bash
/var/log/nginx/access.log
/var/log/nginx/error.log
```
- App
```bash
/home/ubuntu/.pm2/logs/ecourse-backend-out.log
/home/ubuntu/.pm2/logs/ecourse-backend-error.log
```
- After importing each file, press ***Enter** to continue following AWS's default commands for uploading log files to **Cloudwatch**, after completing each file, the wizard will ask a question as shown:

![Create Account](/NestJS-AWS-workshop/images/4/CW7.png)

- Select 1 to continue importing other log files, select 2 when all necessary path files have been imported.

- After importing the log file, the wizard will ask 2 questions about using **X-Ray trace** and **X-ray Deamon**, because our website is a small project, so we choose 2(no) because it is not necessary.

![Create Account](/NestJS-AWS-workshop/images/4/CW8.png)

- Finally, choose to store the config file just now, name it **CloudÚatch** and choose the domain.

- In the **Do you want to store the config in the SSM parameter store** section, choose 1 to store it for new EC2s later.

- In the **parameter stored name** section: leave it as default, avoid name errors.

- In the Region section: choose **us-east-1**.

- Enter the **Access key** and **Secret key** to authenticate.

![Create Account](/NestJS-AWS-workshop/images/4/CW9.png)

- Restart **Cloudwatch Agent**
```bash
sudo systemctl start amazon-cloudwatch-agent
sudo systemctl enable amazon-cloudwatch-agent
```