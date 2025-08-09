---
title : "Thiết lập CloudWatch trên EC2"
date : "2025-06-08"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

Mở CMD, và SSH vào EC2

```bash
  ssh -i "ecourse-key.pem" ubuntu@<EC2-PUBLIC-IP>
```
Nhập lần lượt những lệnh sau để cài đặt **CLoudƯatch**

- Tải file cài đặt **CloudWatch Agent** từ AWS:

```bash
  wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
```
- Cài đặt file .deb

```bash
  sudo dpkg -i amazon-cloudwatch-agent.deb
```
- Kiểm tra lại file thực thi

```bash
  ls /opt/aws/amazon-cloudwatch-agent/bin/
```

![Create Account](/NestJS-AWS-workshop/images/4/CW.png)

![Create Account](/NestJS-AWS-workshop/images/4/CW1.png)

- Chạy **config wizard**
```bash
  sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```
![Create Account](/NestJS-AWS-workshop/images/4/CW2.png)

- Sau khi chạy xong, wizard sẽ đưa các câu hỏi về lựa chọn cấu hình, có thể chọn 1 để chọn mặc định.

![Create Account](/NestJS-AWS-workshop/images/4/CW3.png)

![Create Account](/NestJS-AWS-workshop/images/4/CW4.png)

![Create Account](/NestJS-AWS-workshop/images/4/CW5.png)

![Create Account](/NestJS-AWS-workshop/images/4/CW6.png)

- Khi đến phần **log file path**, đây là phần đường dẫn mà bạn muốn gửi lên **CloudƯatch** để giám sát

- Hãy nhập 3 log file path của hệ thống, nginx và app lên:

- Hệ thống
```bash
  /var/log/syslog
```
- Nginx
```bash
  /var/log/nginx/access.log
  /var/log/nginx/error.log
```
- App
```bash
  /home/ubuntu/.pm2/logs/ecourse-backend-out.log
  /home/ubuntu/.pm2/logs/ecourse-backend-error.log
```
- Nhập xong từng file thì nhấn ***Enter** để tiếp tục theo các lệnh mặc định của AWS cho việc tải file log lên **Cloudwatch**, sau khi hoàn tất từng file, wizard sẽ đưa câu hỏi như hình:

![Create Account](/NestJS-AWS-workshop/images/4/CW7.png)

- Chọn 1 để tiếp tục nhập các file log khác, chọn 2 khi đã nhập xong tất cả các file path cần thiết.
- Sau khi nhập xong file log, wizard sẽ đưa 2 câu hỏi về việc sử dụng **X-Ray trace** và **X-ray Deamon**, do website chúng ta là một dự án nhỏ, cho nên chúng ta chọn 2(no) vì không cần thiết.

![Create Account](/NestJS-AWS-workshop/images/4/CW8.png)

- Sau cùng là lựa chọn lưu trữ file config vừa rồi, đặt tên cho **CloudƯatch** và chọn miền.
- Mục **Do you want to store the config in the SSM parameter store**, chọn 1 để lưu trữ cho các EC2 mới sau này.
- Mục **parameter stored name**: để mặc định, tránh việc lỗi tên.
- Mục Region: chọn **us-east-1**.
- Nhập các **Access key** và **Secret key** để xác thực.

![Create Account](/NestJS-AWS-workshop/images/4/CW9.png)

- Khởi động lại **Cloudwatch Agent**
```bash
  sudo systemctl start amazon-cloudwatch-agent
  sudo systemctl enable amazon-cloudwatch-agent
```