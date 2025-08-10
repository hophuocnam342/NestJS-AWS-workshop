---
title : "Monitoring Activities"
date : "2025-06-08"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

We will perform the following steps to delete the resources we created in this lab.

1. Delete **Auto Scaling Group**:
- Go to **EC2 Management Console**
- On the left navigation bar, select **Auto Scaling Groups**
- Select the relevant **Auto Scaling Group**.
- Click **Delete**
- Type **delete** in the empty box and click **delete**.

2. Delete **Load Balancer**:
- Go to **EC2 Management Console**
- On the left navigation bar, select **Load Balancers**
- Select the relevant **Load Balancer**.
- Click **Actions**.
- Click **Delete**.

3. Delete **Target Group**:
- Go to **EC2 Management Console**
- On the left navigation bar, select **Target Groups**
- Select the relevant **Target Group**.
- Click **Actions**.
- Click **Delete**.
- Click **Yes, delete**

4. Delete **Launch Template**:
- Go to **EC2 Management Console**
- On the left navigation bar, select **Launch Templates**
- Select the relevant **Launch Template**.
- Click **Actions**.
- Click **Delete template**
- Type **delete** in the blank box and press **delete**

5. Delete **AMI**:
- Go to **EC2 Management Console**
- On the left navigation bar, select **AMIs**
- Select the relevant **AMI**.
- Click **Actions**.
- Click **Deregister**.
- Click **Continue**.

6. Terminate **EC2 instance**:
- Go to **EC2 Management Console**
- On the left navigation bar, select **Intances**
- Select all relevant **EC2 Instances**.
- Click **Actions**.
- Click **Manage Instance State**.
- Select **Terminate**.
- Click **Change State**

7. Delete **VPC + subnet + security group**:
- Go to **VPC Management Console**
- On the left navigation bar, select **Your VPCs**
- Select the relevant **VPC**.
- Click **Action**
- Click **Delete VPC**
- Type **delete** in the blank box
- Click **Delete**

8. Delete **Cloudfront Distribution**
- Go to **Cloudfront Management Console**
- On the left navigation bar, select **Distribution**
- Select the relevant **Distribution**.
- Select **Delete**

9. Delete **S3 Bucket**
- Go to **AWS S3**.
- In the **Bucket name** list, select the relevant bucket.
- Select **Empty**.
- On the **Empty bucket** page, confirm and select **Empty**.
- Select **Delete bucket**.

10. Delete **Cloudtrail**
- Go to **AWS CloudTrail**
- In the **Trail** section, select the relevant **Trail**.
- Select **Delete**

11. Delete **AWS SNS**
- Go to **Amazon Simple Notification Service**
- Select the **Topic** section
- Select the relevant **Topic**
- Select **Delete**.
- Enter **delete me** and select **delete**.