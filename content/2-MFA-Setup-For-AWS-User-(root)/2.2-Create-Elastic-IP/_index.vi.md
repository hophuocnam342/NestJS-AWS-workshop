---
title : "Khởi tạo Elastic IP"
date :  "2025-06-08"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

{{% notice note %}}
Elastic IP nhằm mục đích cung cấp một public Ip cho EC2 Instance, nó luôn giữ nguyên dù cho EC2 có khởi động lại hay không.
{{% /notice %}}

Trong mục **Network & Security** của giao diện **EC2**:
- Chọn **Elastic IPs**
- Chọn **Allocate Elastic IP address**

![Create Account](/images/2/IP.png?featherlight=false&width=90pc)

- Mục **Public IPv4 address pool**, chọn **Amazon's pool of IPv4 addresses**.
- Mục **Network border group**, chọn region mà bạn muốn sử dụng, ở đây là **us-east-1**.
- Chọn **Allocate**.

![Create Account](/images/2/IP2.png?featherlight=false&width=90pc)

Sau khi tạo xong, ở giao diện Elastic IPs:
- Chọn Ip vừa tạo, chọn nút **Actions**, chọn **Associate Elastic IP address**

![Create Account](/images/2/IP3.png?featherlight=false&width=90pc)

- Ở mục **Resource type**, chọn **Instance**
- Chọn Instance vừa tạo
- Chọn **Associate**

![Create Account](/images/2/IP4.png?featherlight=false&width=90pc)