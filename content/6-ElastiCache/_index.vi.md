---
title : "Các hoạt động giám sát"
date : "2025-06-08"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

{{% notice warning %}}
Dịch vụ này có thể tính phí cao, vui lòng cân nhắc trước khi sử dụng. 
{{% /notice %}}

#### Tạo subnet group
 Truy cập **ElastiCache**
 - Trên thanh điều hướng bên trái, chọn **Subnet groups**
 - Chọn **Create Subnet group**
 - Trường **Name**, nhập **ecourse-cache-subnet-group**.
 - Trường **Decription**, nhập **Subnet group for Reddis Cache**
 - Chọn **VPC** liên quan.

{{% notice note %}}
ElastiCache Serverless yêu cầu ít nhất 3 subnets trong VPC.
{{% /notice %}}

![Create Account](/NestJS-AWS-workshop/images/4/EC.PNG)

#### Tạo Sercurity group
Truy cập **VPC**
- Chọn **Security Group**
- Chọn **Create security group**
- **Security group name**, nhập Ecourse-Cache-SG
- **Description**, nhập Security Group for Ecourse Cache
- Chọn **VPC** liên quan.
- Cấu hình **Inbound Rule**, chọn **Custom TCP**, port **6379**, **Source** chọn **Security group** đang sử dụng trước đó.
- Cấu hình **Outbound Rule**, chọn **All traffic**.

#### Tạo ElastiCache
Truy cập **ElastiCache**
Chọn **Valkey Cache** bên thanh điều hướng bên trái, chọn **Create Cache**
- **Engine**, chọn **Valkey**
- Chọn **Design your own Cache**
- Chọn **Cluster Cache**