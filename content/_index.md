---
title : "Deploy Nestjs website to AWS"
date :  "2025-06-08"
weight : 1 
chapter : false
---

# Deploy Nestjs website to AWS

#### Overview

In this lab, you will be guided on how to deploy a **website running the Nestjs API backend** to **AWS platform**. With optimal support services such as **EC2, VPC, S3, Cloudfront,...**, your website will operate quickly, meeting the strict requirements of users.
![Deploy](/images/1/Diagram.png)

#### Introducing Amazon Elastic Compute Cloud (EC2)

Amazon EC2 is a service that provides scalable cloud computing capabilities on AWS. Some key features:

- **Amazon EC2** operates similarly to traditional virtual or physical servers, but with the ability to quickly initialize, flexibly scale resources and simple management.
- **Virtual Server** divides a physical server into multiple virtual servers, optimizing the use of hardware resources.

- **Amazon EC2** supports a variety of workloads such as: web hosting, applications, databases, authentication services, and any task that a regular server can perform.

![Create Account](/images/1/h1.jpeg)

#### Introducing Amazon Virtual Private Cloud (Amazon VPC)

**Amazon VPC** is a custom virtual network service within **AWS Cloud**, allowing you to create a separate and completely isolated network environment from the outside world. This concept is similar to designing and deploying a standalone network in a traditional on-premises data center.
**Key Features:**
- Full control over your virtual network environment
- Instantiate and manage AWS resources
- Customize IP address ranges and network segments
- Flexible routing and networking configuration
- Full IPv4 and IPv6 support

#### Introducing Amazon Simple Storage Service (Amazon S3)

**Amazon Simple Storage Service (Amazon S3)** is an object storage service that provides on-demand scalability, ensuring the highest levels of data availability, security, and performance.

S3 is built to meet the needs of customers of all sizes and industries, who can use it to store and protect any amount of data.

S3 can be used for a variety of use cases such as data warehouses, websites, mobile applications, backup and restore, archiving, enterprise applications, IoT devices, and big data analytics. Additionally, Amazon S3 provides easy-to-use management features that help you organize your data and configure access controls to meet your specific business, organizational, and compliance requirements.

Amazon S3 is designed to provide 99.999999999% (11 9’s) durability and stores data for millions of applications for companies worldwide.

#### Introducing Amazon Cloudfront

**Amazon CloudFront** is a content delivery network (CDN) service from **Amazon Web Services** (AWS). It accelerates the delivery of web, video, application, and API content to users globally by serving content from edge locations closest to users, reducing latency and improving performance.

**Key Features:**
- **Global CDN:** CloudFront has a network of edge locations around the world, enabling content delivery closer to users, reducing page load times and improving user experience.
- **Static and dynamic content delivery:** CloudFront supports both static content (e.g. images, CSS, JavaScript) and dynamic content (e.g. dynamically generated web pages).
- **Integration with AWS:** CloudFront integrates well with other AWS services such as Amazon S3, Amazon EC2, making it easy for companies already using AWS to deploy and manage a CDN.
- **Security:** CloudFront provides security features such as SSL/TLS encryption, access control, and integration with AWS Shield to protect against DDoS attacks.

- **Cost Optimization:** CloudFront helps optimize costs by offloading origin servers, reducing bandwidth, and providing flexible pricing options.

- **Other Features:** CloudFront also supports features such as video delivery, cache management, and geographic access restrictions.

#### Introducing AWS Cognito

**AWS Cognito** allows us to easily build login, registration, email verification, password change, password reset, etc. flows, instead of having to build DBs for users and perform many operations such as JWT, password hashing, sending verification emails, etc. This helps you focus on developing other features of the application. Users can log in directly with username and password or through third parties such as Facebook, Amazon, Google, or Apple.
The two main components of **Amazon Cognito** are user groups and identity groups:

- User groups: a user directory that provides sign-up and sign-in options for your web and mobile app users. Once signed in with a user group, app users can access the resources the app allows.

- Identity groups: give your users access to other AWS services.

#### Introducing Amazon EC2 Auto Scaling Group

**1. Why use Auto scaling group?**

When our application is put into operation, the number of visitors will change over time, so we need to regularly change (scaling) the number of instances to improve availability and save costs. To automate and be flexible in scaling work, we will have the solution of **Auto Scaling Group**.

**2. Overview of Auto Scaling Group**

**Amazon EC2 Auto Scaling Group (ASG)** helps automatically adjust the number of EC2 instances according to the needs of the application. ASG can automatically scale up (scale out) when traffic increases, or scale down (scale in) when traffic decreases, helping to optimize resources and reduce costs. It also helps ensure high availability by distributing instances across multiple Availability Zones to maintain continuous operation even if part of the system fails.

#### Introducing Elastic Load Balancer

**Elastic Load Balancer (ELB)** is a service that helps distribute workloads (traffic) evenly across multiple servers or instances to ensure stable system operation and avoid overloading any one server. It helps optimize performance, increase availability, and ensure that if a server fails, traffic will be redirected to other servers without affecting users.

AWS provides three types of Load Balancers:

- Application Load Balancer (ALB): Optimized for HTTP/HTTPS traffic, operating at the application layer (Layer 7)
- Network Load Balancer (NLB): Handles traffic at the transport layer (Layer 4), suitable for applications requiring extremely high performance
- Gateway Load Balancer (GWLB): Used to deploy and manage virtual network devices

In this tutorial, we will use **Application Load Balancer (ALB)** to optimize HTTP/HTTPS traffic.

#### Introducing ElastiCache

{{% notice warning %}}
This service may be expensive, please consider before using.

{{% /notice %}}

**ElastiCache** is an AWS service that allows us to create a Memcached or Redis cluster easily instead of having to install and configure many things ourselves.

AWS ElastiCache will cover the following for us:

- Installation: when we create an ElastiCache, AWS will automatically install the necessary things for Memcached and Redis below it, we just need to wait for it to finish installing and use it.

- Administration: we don't need to worry about the system admin's work for an ElastiCache, AWS will do it for us.

- Monitoring: ElastiCache will push its metrics to CloudWatch.

- Backups: AWS has an option for us to automatically backup cache data (redis only).

#### Notification Services

**1. Amazon Cloudtrail**

**AWS CloudTrail** is an Amazon Web Services (AWS) service that records activity in your AWS account, including actions taken by users, roles, or AWS services. It acts as an auditing and monitoring tool, recording events as logs, allowing users to review activity history, analyze risks, and ensure regulatory compliance.

**2. Amazon Cloudwatch**

**Amazon CloudWatch** is a monitoring and management service that provides actionable data and insights for AWS infrastructure resources and applications, hybrid applications, and on-premises applications. You can collect and access all performance and operational data in the form of logs and metrics in one platform, instead of monitoring them individually (server, network, or database). CloudWatch enables you to monitor end-to-end (applications, infrastructure, and services) and leverage alerts, logs, and event data to automate actions and reduce Mean Time To Resolution (MTTR). This service frees up critical resources and allows you to focus on building applications and business value.

**3. Simple Notification Service(AWS SNS)**

**AWS SNS (Simple Notification Service)** is a messaging service from Amazon Web Services (AWS) that allows developers to send notifications to subscribers or other applications. It operates on a publish/subscribe model, where publishers send messages to topics, and subscribers receive notifications from topics they are interested in.

**4. Amazon Simple Queue Service(AWS SQS)**

**AWS SQS**, or **Amazon Simple Queue Service**, is a fully managed, distributed message queuing service provided by AWS that enables applications and distributed systems to communicate with each other in a flexible and reliable way. SQS helps decouple application components, allowing them to operate independently and increase scalability and fault tolerance.

#### Main content

1. [Preparation](1-create-new-aws-account/)
2. [Install Nestjs backend for EC2](2-mfa-setup-for-aws-user-(root)/)
3. [Frontend Deployment Service Configuration](3-Config-service-hosting-front-end/)
4. [Clean up resources](4-verify-new-account/)