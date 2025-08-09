---
title : "Create Application Load Balancer"
date : "2025-08-06"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

#### Create Target Group

In the **EC2** management interface, in the left navigation panel, scroll down to the **Load Balancing** section:
- Select Target Groups
- Select Create target group

![Create Account](/NestJS-AWS-workshop/images/4/TG.png)

The **Specify group details** table appears, configure as follows:
In the **Basic configuration** section:
- **Choose a target type**: Instances
- **Target group name**: ecourse

![Create Account](/NestJS-AWS-workshop/images/4/TG1.PNG)

Continue in the **Basic configuration** section:
- **Protocol**: Port: HTTP, 3000
- **IP address type**: IPv4
- **VPC**: ecourse-VPC
- **Protocol version**: HTTP1
- Click **Next**

![Create Account](/NestJS-AWS-workshop/images/4/TG2.PNG)

Next we proceed to **Register targets**:

In the **Available instances** section:
- Select **instance ecourse-backend**
- **Ports for the selected instances**: 3000
- Select **Include as pending below**

In the **Review targets** section:
- Check that the target has been registered.

- Select **Create target group**.

#### Create Application Load Balancer
In the EC2 management interface, in the left navigation panel:

- Select **Load Balancers**
- Click the **Create Load Balancer** button

![Create Account](/NestJS-AWS-workshop/images/4/ALB.png)

The **Compare and select load balancer type** table appears:

In the **Load balancer types** section:

- In the **Application Load Balancer** section
- Select **Create**

![Create Account](/NestJS-AWS-workshop/images/4/ALB1.png)

In the **Create Application Load Balancer** table:
In the **Basic configuration** section:
- **Load balancer name**: ecourse-ALB
- **Scheme**: Internet-facing
- **Load balancer IP address type**: IPv4

![Create Account](/NestJS-AWS-workshop/images/4/ALB2.PNG)

In the **Network mapping** section:

- VPC: ecourse-VPC
- Subnet: Select Public Subnet in us-east-1a, us-east-1b.

![Create Account](/NestJS-AWS-workshop/images/4/ALB3.PNG)

In the Security groups section:
- Select **ecourse-backend**
In the Listeners and routing section:
- **Default action**: ecourse

![Create Account](/NestJS-AWS-workshop/images/4/ALB4.PNG)

In the **Summary** section, review the configured information for the Load Balancer
Click the **Create Load Balancer** button