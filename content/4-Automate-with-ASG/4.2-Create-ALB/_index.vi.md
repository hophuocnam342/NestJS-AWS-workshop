---
title : "Tạo Application Load Balancer"
date :  "2025-08-06"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

#### Tạo Target Group

Ở phần giao diện quản lý **EC2**, ở bảng điều hướng bên trái, hãy kéo xuống phần **Load Balancing**:
- Chọn Target Groups
- Chọn Create target group

![Create Account](/NestJS-AWS-workshop/images/4/TG.png)

Xuất hiện bảng **Specify group details**, hãy cấu hình như sau:
Ở phần **Basic configuration**:
- **Choose a target type**: Instances
- **Target group name**: ecourse

![Create Account](/NestJS-AWS-workshop/images/4/TG1.PNG)

Tiếp tục trong phần **Basic configuration**:
- **Protocol**: Port: HTTP, 3000
- **IP address type**: IPv4
- **VPC**: ecourse-VPC
- **Protocol version**: HTTP1
- Nhấn **Next**

![Create Account](/NestJS-AWS-workshop/images/4/TG2.PNG)

Tiếp theo chúng ta tiến hành **Register targets**:

Ở phần **Available instances**:
- Chọn **instance ecourse-backend**
- **Ports for the selected instances**: 3000
- Chọn **Include as pending below**

Ở phần **Review targets**:
- Kiểm tra target đã được đăng ký.
- Chọn **Create target group**.

#### Tạo Application Load Balancer
Ở giao diện quản lý EC2, ở bảng điều hướng bên trái:

- Chọn **Load Balancers**
- Nhấn vào nút **Create Load Balancer**

![Create Account](/NestJS-AWS-workshop/images/4/ALB.png)

Xuất hiện bảng **Compare and select load balancer type**:

Ở phần **Load balancer types**:
- Tại phần **Application Load Balancer**
- Chọn **Create**

![Create Account](/NestJS-AWS-workshop/images/4/ALB1.png)

Trong bảng **Create Application Load Balancer**:
Ở phần **Basic configuration**:
- **Load balancer name**: ecourse-ALB
- **Scheme**: Internet-facing
- **Load balancer IP address type**: IPv4

![Create Account](/NestJS-AWS-workshop/images/4/ALB2.PNG)

Ở phần **Network mapping**:
- VPC: ecourse-VPC
- Subnet: Chọn Public Subnet ở us-east-1a, us-east-1b.

![Create Account](/NestJS-AWS-workshop/images/4/ALB3.PNG)

Ở phần Security groups:
- Chọn **ecourse-backend**
Ở phần Listeners and routing:
- **Default action**: ecourse

![Create Account](/NestJS-AWS-workshop/images/4/ALB4.PNG)

Ở phần **Summary**, kiểm tra lại các thông tin đã cấu hình cho Load Balancer
Nhấn vào nút **Create Load Balancer**