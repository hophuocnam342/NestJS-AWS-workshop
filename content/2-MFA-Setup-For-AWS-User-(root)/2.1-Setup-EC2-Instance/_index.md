---
title : "Launch EC2 Instance"
date : "2025-06-08"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

#### Create EC2 Instance

Go to [AWS Management Console](https://ap-southeast-1.console.aws.amazon.com/):

- Search for **EC2**
- Select **EC2**

![Create Account](/images/2/EC.png)

In the **EC2** interface:

- Select **Launch instances**

![Create Account](/images/2/EC1.png)

#### Configure basic information

- Name the instance, enter **ecourse-backend**.

- Select **AMI**, select **Ubuntu**

![Create Account](/images/2/EC3.png)

- In **Instance type**, select **t2.micro**

{{% notice note %}}
You should choose Instance types belonging to **Free tier eligible** to avoid fees!!!

{{% /notice %}}

- In **Key pair(login)**, select the key pair just created in [**1.3**](/vi/1-create-new-aws-account/1.3-create-key-pair/)

![Create Account](/images/2/EC4.png?featherlight=false&width=90pc)

- In **Network settings**, select **Select existing security group**.
- Select the Security group created in [**1.2**](/vi/1-create-new-aws-account/1.2-create-security-group-for-ec2/)
- Select **Launch instance**

![Create Account](/images/2/EC5.png?featherlight=false&width=90pc)