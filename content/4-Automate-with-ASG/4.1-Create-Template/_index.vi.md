---
title : "Tạo Template"
date :  "2025-08-06"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

#### Tạo Image
Ở trong phần giao diện quản lý EC2, ở bảng điều hướng bên trái:

- Chọn Instances
- Chọn instance **ecourse-backend**
- Chọn **Actions**
- Chọn **Image and templates**
- Ấn **Create image**
Trong bảng cấu hình **Create AMI**, điền các thông tin sau:

- **Image name**: ecourse-AMI
- **Image description**: AMI for ecourse
- Ấn **Create Image**
#### Tạo launch template

Ở giao diện quản lý EC2, ở bảng điều hướng bên trái:

- Chọn **Launch Templates**
- Chọn **Create launch template**

![Create Account](/NestJS-AWS-workshop/images/4/TP.png)

Trong bảng Create launch template, điền các thông tin sau:

Ở phần **Launch template name and description**:
- **Launch template name**: ecourse-template
- **Template version description**: Template for preventive instance

![Create Account](/NestJS-AWS-workshop/images/4/TP1.PNG)

Ở phần **Application and OS Image (Amazon Machine Image)**:
- Chọn **My AMIs**
- Chọn **Owned by me**
- Chọn AMI đã tạo **ecourse-AMI**
Ở phần Instance type:
- Chọn loại **Instance t2.micro**

![Create Account](/NestJS-AWS-workshop/images/4/TP2.PNG)

Ở phần Key pair (login):
- Chọn Key pair name có tên là ecourse-key
Ở phần **Network settings**:
- Chọn **subnet public** ecourse-public-ap-southeast-1a
- Chọn **Select existing security group**
- Chọn **security group**: ecourse-SG
- Cuối cùng, chọn **Create launch template**

![Create Account](/NestJS-AWS-workshop/images/4/TP3.PNG)