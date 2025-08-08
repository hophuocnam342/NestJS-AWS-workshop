---
title : "Cấu hình database với MongoDB"
date : "2025-06-08"
weight : 4
chapter : false
pre : " <b> 1.4 </b> "
---

#### Cấu hình Cluster

1. Cấu hình tài khoản

- Truy cập trang chủ MongoDB: [MongoDB](https://account.mongodb.com/)
- Tạo tài khoản Mongo DB
- Sau khi tạo xong tài khoản, tại giao diện chính chọn **Cluster**
- Chọn **Build a Cluster**

![Create Account](/NestJS-AWS-workshop/images/1/DB.png)

2. Cấu hình Cluster

- Chọn loại Cluster: chọn **Free**

![Create Account](/NestJS-AWS-workshop/images/1/DB1.png)

- Chọn Name: đặt tên phù hợp, hoặc để mặc định là **Cluster0**
- Chọn Provider: Tích vào ô **AWS**
- Chọn Region: Chọn Region cùng với **Region của EC2 và các dịch vụ khác trên AWS** để tránh sự cố
- Kiểm tra lại thông tin, và chọn **Create Deployment**

![Create Account](/NestJS-AWS-workshop/images/1/DB2.png)

3. Tạo kết nối Cluster

- Sau khi hoàn tất cấu hình Cluster, chúng ta đến với bước tạo kết nối CLuster.
- Ở trang Set up connection, sau khi phân quyền, các cấu hình có thể để mặc định

![Create Account](/NestJS-AWS-workshop/images/1/DB3.png)

- Đến trang **Choose a connection method**, ở mục **Connect to your application**, chọn **Drivers**

![Create Account](/NestJS-AWS-workshop/images/1/DB4.png)

- Ở mục 3. , copy đường link đến Cluster, có dạng **"mongodb+srv://<db_username>:<db_password>@cluster0..."**, thay thế <db_username> và <db_password> bằng **username** và **password** ở trang **Set up connection** ở trên. Chúng ta cần đường link này để kết nối được database và **EC2**.

![Create Account](/NestJS-AWS-workshop/images/1/DB5.png)


