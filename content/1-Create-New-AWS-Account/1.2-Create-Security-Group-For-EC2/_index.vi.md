---
title : "Tạo Sercurity Group cho EC2"
date : "2025-06-08"
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---


#### Tạo VPC Security group cho Amazon EC2
1. Trong giao diện VPC
- Chọn **Security Group**
- Chọn **Create security group**

![Create Account](/NestJS-AWS-workshop/images/1/SG.png)

2. Tiến hành cấu hình
- **Security group name**, nhập Ecourse-SG
- **Description**, nhập Security Group for Ecourse
- Chọn **VPC** đã tạo

![Create Account](/NestJS-AWS-workshop/images/1/SG2.png)

3. Cấu hình **Inbound rule**
- Để thêm rule, chọn **Add rule**
- **Custom TCP** chọn port 3000
- **SSH** cổng 22 dùng để kết nối với máy local. Source chọn **My IP**
- **HTTP** cổng 80 và source là **Anywhere IPv4**
- **HTTPS** cổng 443 và source là **Anywhere IPv4**
- Chọn **Create security group**

![Create Account](/NestJS-AWS-workshop/images/1/SG1.png)
