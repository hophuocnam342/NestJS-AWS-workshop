---
title : "Create Auto Scaling Group"
date : "2025-08-06"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---

In the EC2 management interface, drag the left selection panel to the bottom:

Select **Auto Scaling Groups**
Press **Create Auto Scaling group**

![Create Account](/NestJS-AWS-workshop/images/4/ASG.png)

In the **Auto Scaling group** creation interface, fill in the following information:

- **Name**: ecourse-ASG
- In Launch template:
- **Launch template**: select ecourse-template
- **Version**: Default (1)
- Click **Next**

![Create Account](/NestJS-AWS-workshop/images/4/ASG1.PNG)

In the Network section, configure as follows:

- **VPC**: select VPC ecourse-VPC created previously
- **Availability Zones and subnets**: select 2 public subnets created
- Click **Next**

![Create Account](/NestJS-AWS-workshop/images/4/ASG2.PNG)

#### Set up Load Balancer
**Load balancing**: select **Attach to an existing load balancer**
**Attach to an existing load balancer**: select **Choose from your load balancer target group**
**Existing load balancer target group**: select ecourse | HTTP

![Create Account](/NestJS-AWS-workshop/images/4/ASG3.PNG)

Next, in the **Health checks** section:

Check **Turn on Elastic Load Balancing health checks**
Keep the remaining settings as default

![Create Account](/NestJS-AWS-workshop/images/4/ASG4.png)

In the **Additional settings** section, in the **Monitoring** section:

- Check **Enable group metrics collection within CloudWatch**
- Click **Next**

![Create Account](/NestJS-AWS-workshop/images/4/ASG5.png)

#### Set Size and Scaling Policies

In the **Group size** section:
- Desired capacity: 1
In the **Scaling limits** section:
- Min desired capacity: 1
- Max desired capacity: 3

![Create Account](/NestJS-AWS-workshop/images/4/ASG6.png)

In **Automatic scaling - optional**: select **No scaling policies** (temporarily do not set up scaling policies for ASG)

![Create Account](/NestJS-AWS-workshop/images/4/ASG7.png)

In **Instance maintenance policy**: select **No policy**

![Create Account](/NestJS-AWS-workshop/images/4/ASG8.png)

#### Set up notifications (AWS SNS)

Configure notifications:
- **Send a notification to**: asg-topic (create a new topic)
- **With these recipients**: enter the email you want to receive notifications
- **Event types**: select all
- Click **Next**

![Create Account](/NestJS-AWS-workshop/images/4/ASG9.PNG)

Finally select **Create Auto Scaling group**

![Create Account](/NestJS-AWS-workshop/images/4/ASG10.png)

You will then receive an email notification confirming receipt of notifications from **AWS SNS**

![Create Account](/NestJS-AWS-workshop/images/4/ASG12.png)
![Create Account](/NestJS-AWS-workshop/images/4/ASG11.png)