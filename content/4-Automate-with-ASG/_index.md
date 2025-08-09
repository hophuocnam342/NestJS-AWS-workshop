---
title : "Automation with Auto Scaling Group"
date : "2025-08-06"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

#### Why use Auto scaling group?

When our application is put into operation, the number of visitors will change over time, so we need to regularly change (scaling) the number of instances to improve availability and save costs. To automate and be flexible in scaling work, we will have the solution of Auto Scaling Group.

#### Overview of **Auto Scaling Group**

**Amazon EC2 Auto Scaling Group (ASG)** helps automatically adjust the number of EC2 instances according to the needs of the application. ASG can automatically expand (scale out) when traffic increases, or shrink (scale in) when traffic decreases, helping to optimize resources and reduce costs. It also helps ensure high availability by distributing instances across multiple Availability Zones to maintain continuous operations even if one part of the system fails.

#### Types of Scaling in ASG

In this content, we will learn about the following types of Scaling:

**Manual Scaling**: Users manually adjust the number of EC2 instances in the Auto Scaling Group based on demand. This is a manual method, not automatically based on specific metrics.

**Dynamic Scaling**: Automatically adjusts the number of instances based on real-time metrics such as CPU utilization, network traffic, or custom metrics from Amazon CloudWatch. Dynamic scaling has three main policies:

**Target Tracking Scaling**: Maintains a target value for a specific metric

**Step Scaling**: Adjusts based on a metric threshold with different increment/decrement steps

**Simple Scaling**: Adjusts based on a simple threshold

**Scheduled Scaling**: Allows us to configure specific times to automatically scale up or down instances, such as increasing the number of instances during peak hours or reducing them during off-peak hours. Suitable for cases where we already know the traffic pattern.

**Predictive Scaling**: Uses machine learning to predict activity by analyzing historical load data to find daily or weekly patterns in traffic flows. It uses this information to forecast future capacity needs so that Amazon EC2 Auto Scaling can proactively increase the capacity of the Auto Scaling Group to match the forecast.

#### Launch Template
**Launch Template** is a configuration that contains the parameters needed to launch EC2 instances. It stores details such as instance type, AMI (Amazon Machine Image), key pair, network settings, security groups, and other information about the EC2 configuration. It simplifies instance creation, and assists in automatically creating new instances in ASG.

- **Launch Templates** support version management, allowing you to maintain multiple versions of your configuration and tightly control changes, helping to enhance security and compliance.

#### Elastic Load Balancer
**Elastic Load Balancer (ELB)** is a service that helps distribute workloads (traffic) evenly across multiple servers or instances to ensure system stability and avoid overloading any one server. It helps optimize performance, increase availability, and ensure that if one server fails, traffic is redirected to other servers without affecting users.

AWS provides three types of Load Balancers:

- **Application Load Balancer (ALB)**: Optimized for HTTP/HTTPS traffic, operating at the application layer (Layer 7)
- **Network Load Balancer (NLB)**: Handles traffic at the transport layer (Layer 4), suitable for applications requiring extremely high performance
- **Gateway Load Balancer (GWLB)**: Used to deploy and manage virtual network devices
#### Target Group
**Target Group** is a component of **Elastic Load Balancer (ELB)**, used to identify and manage the EC2 instances, IP addresses, Lambda functions, or containers to which the Load Balancer will distribute traffic. Target Groups also define health checks to ensure that traffic is only sent to healthy targets.

{{% notice warning %}}
Inappropriate health check configuration can result in healthy instances being removed or problematic instances being retained in the Target Group. Make sure to set timeouts, intervals, and thresholds that are appropriate for your application.
{{% /notice %}}