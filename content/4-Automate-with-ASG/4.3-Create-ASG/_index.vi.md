---
title : "Tạo Auto Scaling Group"
date :  "2025-08-06"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---

Ở giao diện quản lý EC2, kéo bảng lựa chọn bên trái xuống cuối:

Chọn **Auto Scaling Groups**
Ấn **Create Auto Scaling group**

![Create Account](/NestJS-AWS-workshop/images/4/ASG.png)

Trong giao diện tạo **Auto Scaling group**, điền các thông tin sau:
- **Name**: ecourse-ASG
- Trong Launch template:
- **Launch template**: chọn ecourse-template
- **Version**: Default (1)
- Nhấn **Next**

![Create Account](/NestJS-AWS-workshop/images/4/ASG1.PNG)

Trong phần Network, cấu hình như sau:

- **VPC**: chọn VPC ecourse-VPC đã tạo trước đó
- **Availability Zones and subnets**: chọn 2 public subnets đã tạo
- Ấn **Next**

![Create Account](/NestJS-AWS-workshop/images/4/ASG2.PNG)

#### Thiết lập Load Balancer
**Load balancing**: chọn **Attach to an existing load balancer**
**Attach to an existing load balancer**: chọn **Choose from your load balancer target group**
**Existing load balancer target group**: chọn ecourse | HTTP

![Create Account](/NestJS-AWS-workshop/images/4/ASG3.PNG)

Tiếp theo, ở phần **Health checks**:

Tích chọn **Turn on Elastic Load Balancing health checks**
Giữ các thiết lập còn lại theo mặc định

![Create Account](/NestJS-AWS-workshop/images/4/ASG4.png)

Trong phần **Additional settings**, ở mục **Monitoring**:

- Tích chọn **Enable group metrics collection within CloudWatch**
- Ấn **Next**

![Create Account](/NestJS-AWS-workshop/images/4/ASG5.png)

#### Thiết lập Size và Scaling Policies

Trong phần **Group size**:
- Desired capacity: 1
Trong phần **Scaling limits**:
- Min desired capacity: 1
- Max desired capacity: 3

![Create Account](/NestJS-AWS-workshop/images/4/ASG6.png)

Trong **Automatic scaling - optional**: chọn **No scaling policies** (tạm thời chưa thiết lập chính sách scaling cho ASG)

![Create Account](/NestJS-AWS-workshop/images/4/ASG7.png)

Trong **Instance maintenance policy**: chọn **No policy**

![Create Account](/NestJS-AWS-workshop/images/4/ASG8.png)

#### Thiết lập thông báo( AWS SNS)

Cấu hình thông báo:
- **Send a notification to**: asg-topic (tạo topic mới)
- **With these recipients**: nhập email bạn muốn nhận thông báo
- **Event types**: chọn tất cả
- Ấn **Next**

![Create Account](/NestJS-AWS-workshop/images/4/ASG9.PNG)

Sau cùng chọn **Create Auto Scaling group**

![Create Account](/NestJS-AWS-workshop/images/4/ASG10.png)

Sau đó, bạn sẽ nhận được thông báo trong email về xác nhận việc nhận thông báo từ **AWS SNS**

![Create Account](/NestJS-AWS-workshop/images/4/ASG12.png)
![Create Account](/NestJS-AWS-workshop/images/4/ASG11.png)

