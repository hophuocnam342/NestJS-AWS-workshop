---
title : "Khởi tạo EC2 Instance"
date :  "2025-06-08"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

#### Tạo EC2 Instance

Truy cập vào [AWS Management Console](https://ap-southeast-1.console.aws.amazon.com/):

- Tìm **EC2**
- Chọn **EC2**

![Create Account](/NestJS-AWS-workshop/images/2/EC.png)

Trong giao diện **EC2**:

- Chọn **Launch instances**

![Create Account](/NestJS-AWS-workshop/images/2/EC1.png)

#### Cấu hình thông tin cơ bản

- Đặt tên cho instance, nhập **ecourse-backend**.
- Chọn **AMI**, chọn **Ubuntu**

![Create Account](/NestJS-AWS-workshop/images/2/EC3.PNG)

- Mục **Instance type**, chọn **t2.micro**

{{% notice note %}}
Bạn nên chọn những Instance type thuộc **Free tier eligible** để tránh tốn phí!!!
{{% /notice %}}

- Mục **Key pair(login)**, chọn key pair vừa tạo ở mục [**1.3**](/vi/1-create-new-aws-account/1.3-create-key-pair/)

![Create Account](/static/images/2/EC4.PNG)

- Mục **Network settings**, chọn **Select existing security group**.
- Chọn Sercurity group  đã tạo ở mục [**1.2**](/vi/1-create-new-aws-account/1.2-create-security-group-for-ec2/)
- Chọn **Launch ínstance**

![Create Account](/NestJS-AWS-workshop/images/2/EC5.PNG)
