---
title : "Các hoạt động giám sát"
date : "2025-06-08"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

Chúng ta sẽ tiến hành các bước sau để xóa các tài nguyên chúng ta đã tạo trong bài thực hành này.

1. Xóa **Auto Scaling Group**:
- Truy cập **EC2 Management Console**
- Trên thanh điều hướng bên trái, chọn **Auto Scaling Groups**
- Chọn **Auto Scaling Group** liên quan.
- Click **Delete**
- Gõ **delete** vào ô trống và nhấn **delete**.

2. Xóa **Load Balancer**:
- Truy cập **EC2 Management Console**
- Trên thanh điều hướng bên trái, chọn **Load Balancers**
- Chọn **Load Balancer** liên quan.
- Click **Actions**.
- Click **Delete**.

3. Xóa **Target Group**:
- Truy cập **EC2 Management Console**
- Trên thanh điều hướng bên trái, chọn **Target Groups**
- Chọn **Target Group** liên quan.
- Click **Actions**.
- Click **Delete**.
- Click **Yes, delete**

4. Xóa **Launch Template**:
- Truy cập **EC2 Management Console**
- Trên thanh điều hướng bên trái, chọn **Launch Templates**
- Chọn **Launch Template** liên quan.
- Click **Actions**.
- Click **Delete template**
- Gõ **delete** vào ô trống và nhấn **delete**

5. Xóa **AMI**:
- Truy cập **EC2 Management Console**
- Trên thanh điều hướng bên trái, chọn **AMIs**
- Chọn **AMI** liên quan.
- Click **Actions**.
- Click **Deregister**.
- Click **Continue**.

6. Terminate **EC2 instance**:
- Truy cập **EC2 Management Console**
- Trên thanh điều hướng bên trái, chọn **Intances**
- Chọn tất cả **EC2 Instance** liên quan.
- Click **Actions**.
- Click **Manage Instance State**.
- Chọn **Terminate**.
- Click **Change State**

7. Xóa **VPC + subnet + security group**:
- Truy cập **VPC Management Console**
- Trên thanh điều hướng bên trái, chọn **Your VPCs**
- Chọn **VPC** liên quan.
- Click chọn **Action**
- Click **Delete VPC**
- Gõ **delete** vào ô trống
- Click **Delete**

8. Xóa **Cloudfront Distribution**
- Truy cập **Cloudfront Management Console**
- Trên thanh điều hướng bên trái, chọn **Distribution**
- Chọn **Distribution** liên quan.
- Chọn **Delete**

9. Xóa **S3 Bucket**
- Truy cập **AWS S3**.
- Trong danh sách **Bucket name**, chọn bucket liên quan.
- Chọn **Empty**.
- Trong trang **Empty bucket**, xác nhận và chọn **Empty**.
- Chọn **Delete bucket**.

10. Xóa **Cloudtrail**
- Truy cập **AWS CloudTrail**
- Trong mục **Trail**, chọn **Trail** liên quan.
- Chọn **Delete**

11. Xóa **AWS SNS**
- Truy cập **Amazon Simple Notification Service**
- Chọn mục **Topic**
- Chọn **Topic** liên quan
- Chọn **Delete**.
- Nhập **delete me** và chọn **delete**.