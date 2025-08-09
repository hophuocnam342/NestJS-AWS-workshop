---
title : "Create Template"
date : "2025-08-06"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

#### Create Image
In the EC2 management interface, in the left navigation panel:

- Select Instances
- Select instance **ecourse-backend**
- Select **Actions**
- Select **Image and templates**
- Click **Create image**
In the **Create AMI** configuration panel, fill in the following information:

- **Image name**: ecourse-AMI
- **Image description**: AMI for ecourse
- Click **Create Image**
#### Create launch template

In the EC2 management interface, in the left navigation panel:

- Select **Launch Templates**
- Select **Create launch template**

![Create Account](/NestJS-AWS-workshop/images/4/TP.png)

In the panel Create launch template, fill in the following information:

In the **Launch template name and description** section:
- **Launch template name**: ecourse-template
- **Template version description**: Template for preventive instance

![Create Account](/NestJS-AWS-workshop/images/4/TP1.PNG)

In the **Application and OS Image (Amazon Machine Image)** section:
- Select **My AMIs**
- Select **Owned by me**
- Select the created AMI **ecourse-AMI**
In the Instance type section:
- Select the **Instance t2.micro** type

![Create Account](/NestJS-AWS-workshop/images/4/TP2.PNG)

In the Key pair (login) section:
- Select the Key pair name named ecourse-key
In the **Network settings** section:
- Select **subnet public** ecourse-public-ap-southeast-1a
- Select **Select existing security group**
- Select **security group**: ecourse-SG
- Finally, select **Create launch template**

![Create Account](/NestJS-AWS-workshop/images/4/TP3.PNG)