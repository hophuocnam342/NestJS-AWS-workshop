---
title : "Cấu hình Cloudfront"
date :  "2025-06-08"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

1. Trong giao diện **S3 bucket**
- Chọn **Permissions**
- Hiện tại, chức năng **Block all public access** đang **Off** do chúng ta đã bật tắt tại mục trước
- Chọn **Edit**
- Chọn **Block all public access**
- Chọn **Save changes**
- Một cửa số khác xuất hiện để xác nhận việc chỉnh sửa, điền **confirm**
- Chọn **confirm**

![Create Account](/NestJS-AWS-workshop/images/03/cf.png)

![Create Account](/NestJS-AWS-workshop/images/03/cf1.png)

![Create Account](/NestJS-AWS-workshop/images/03/cf2.png)

2. Trên thanh tìm kiếm, tìm **Cloudfront** và chọn nó
- Sau khi giao diện của **Cloudfront** hiện ra, chọn **Create a CloudFront distribution**.

![Create Account](/NestJS-AWS-workshop/images/03/cf3.png)

3. Chỉ định các cài đặt sau cho bản **distribution**:

- Trong trường **Distribution name**. đặt tên là **cloudfront-ecourse**.
- **Decripstion**, có thể bỏ qua hoặc điền miêu tả.
- Chọn **Single website or app**.
- Chọn **Next**.

![Create Account](/NestJS-AWS-workshop/images/03/cf4.png)
![Create Account](/NestJS-AWS-workshop/images/03/cf5.png)

- Chọn **Amazon S3** cho trường **Origin type**
- Trong trường Origin S3, chọn S3 bucket mà bạn đã tạo.

![Create Account](/NestJS-AWS-workshop/images/03/cf6.png)

- Mục **Setting**, tích vào **Allow private S3 access to Cloudfront**
- Mục **Origin setting**, chọn **Use recommended origin setting**, sau đó chọn **Next**

![Create Account](/NestJS-AWS-workshop/images/03/cf7.png)

- Mục **Enable security**, chọn **Do not enable security protections**
- Kiểm tra lại thông tin và chọn **Create distribution**

![Create Account](/NestJS-AWS-workshop/images/03/cf8.png)

4. Kết nối với **S3 bucket**
- Sau khi tạo xong, chuyển qua tab **Origin** và chọn **Edit**
- Chọn **Use website endpoint**
- Trong trường **Origin domain**, chọn **S3 bucket** mà bạn đã tạo.
- Trường **Origin access**, chọn **Origin access control setting**

![Create Account](/NestJS-AWS-workshop/images/03/cf9.png)

- Mục **Origin access control**, chọn **e-course-OAC**
- Tiếp tục, 1 bảng thông báo hiện ra, chọn **Copy policy**, sau đó click vào **go to S3 bucket permissions** để chỉnh sửa **policy**(thường thì sẽ được tự động chỉnh sửa).

![Create Account](/NestJS-AWS-workshop/images/03/cf10.png)
![Create Account](/NestJS-AWS-workshop/images/03/cf11.png)
- Sau cùng chọn **Create origin**
5. Cập nhật nội dung mới
- Mỗi khi bạn cập nhật nội dung mới trên **S3 bucket**, để **Cloudfront** cũng cập nhật theo, chuyển qua tab **invalidation**, chọn **Create invalidation**.
- Mục **Add object path**, ghi **/** để cập nhật toàn bộ nội dung mới trên bucket.
- Sau đó, chọn **Create invalidation**

![Create Account](/NestJS-AWS-workshop/images/03/cf12.png)
![Create Account](/NestJS-AWS-workshop/images/03/cf13.png)

6. Kiểm tra website
- Để kiểm tra website đã hoạt động trên **Cloudfront** hay chưa, chọn **Distribution** vừa tạo, chuyển qua tab **General**, copy **Distribution domain name** và chạy trên trình duyệt.

![Create Account](/NestJS-AWS-workshop/images/03/cf14.png)
![Create Account](/NestJS-AWS-workshop/images/03/cf15.png)