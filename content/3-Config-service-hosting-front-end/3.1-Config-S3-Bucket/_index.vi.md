---
title : "Cấu hình S3 Bucket"
date :  "2025-06-08"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

1. Vào **AWS Console**
- Tìm **S3** 
- Chọn và giao diện **S3** sẽ xuất hiện.
- Nhấn **Create Bucket**

![Create Account](/images/03/S3.png)

2. Tại giao diện khởi tạo **Bucket**
- Tại **Bucket name**, đặt tên là **ecourse-frontend**.
- Tại **Object Ownership**, chọn **ACLs disable**.
- Bỏ chọn **Block all public access**.
- **Bucket Versioning**. chọn **disable**.
- Những mục khác có thể giữ nguyên, sau đó chọn **Create Bucket**

![Create Account](/images/03/S32.png)
![Create Account](/images/03/S33.png)
![Create Account](/images/03/S34.png)
![Create Account](/images/03/S35.png)

3. Sau khi tạo xong **Bucket**, chúng ta cần bật **Static Website Hosting**
- Click vào bucket vừa tạo.
- Vào tab Properties.
- Kéo xuống phần Static website hosting → nhấn Edit.
- Chọn Enable.
- Index document: **index.html**
- Error document: **error.html** (nếu có, hoặc để trống)
- Nhấn Save changes.

![Create Account](/images/03/S36.png)

4. Cấu hình **Bucket policy**

- Vào tab **Permissions** của **bucket**.
- Kéo xuống phần **Bucket policy**, nhấn **Edit**.
- Dán đoạn sau:

```js
   {
      "Version": "2012-10-17",
      "Statement": [
         {
               "Sid": "PublicReadGetObject",
               "Effect": "Allow",
               "Principal": "*",
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::ecourse-frontend/*"
         }
      ]
   }
```

- Sau đó nhấn **Save changes**
![Create Account](/images/03/S37.png)

5. Upload folder và file frontend lên Bucket

- Vào tab **Objects**.
- Nhấn Upload → Add files hoặc Add folder.
- Chọn toàn bộ file/folder frontend (HTML, CSS, JS, img, ...).
- Nhấn **Upload**.
 {{% notice note %}}
Khi upload, bạn nên up toàn bộ các file con trong thư mục cha của dự án frontend, tránh lỗi không tìm thấy file **index.html** hay **error.html**.
{{% /notice %}}

![Create Account](/images/03/S38.png)
![Create Account](/images/03/S39.png)

6. Kiểm tra website
- Vào lại tab **Properties**, kéo xuống phần **Static website hosting**.
- Copy **Bucket website endpoint** và mở trên trình duyệt.
- Nếu thấy website hiện ra là thành công!

![Create Account](/images/03/S310.png)

![Create Account](/images/03/S311.png)