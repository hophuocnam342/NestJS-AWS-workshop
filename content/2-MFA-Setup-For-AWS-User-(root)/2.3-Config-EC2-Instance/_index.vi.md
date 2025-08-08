---
title : "Cấu hình EC2 Instance và deploy backend"
date :  "2025-06-08"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

Mở CMD:
Nhập dòng lệnh sau:
```Bash
    AWS Configure
```
- AWS Access Key ID: nhập Key ID 
- AWs Secret Access Key ID: nhập Key ID bí mật
- Default region name: chọn region, ở đây là **us-east-1**
- Default output format: nhập **json**

![Create Account](/NestJS-AWS-workshop/images/3/EC.PNG)

- Ssh đến EC2 bằng câu lệnh
```bash
    ssh -i "C:\path\to\key.pem" ubuntu@[EC2-PUBLIC-IP]
```
- Thay **C:\path\to\ecourse-key.pem** bằng đường dẫn thực tế đến file key.
- Thay **[EC2-PUBLIC-IP]** bằng IP public của EC2.

![Create Account](/NestJS-AWS-workshop/images/3/EC1.png)

Sau khi SSH thành công, chạy các lệnh sau:
```bash
    # Update system
    sudo apt update && sudo apt upgrade -y

    # Install Node.js 18.x
    curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
    sudo apt-get install -y nodejs

    # Install PM2 (process manager)
    sudo npm install -g pm2

    # Install Nginx (web server)
    sudo apt install nginx -y

    # Install Git
    sudo apt install git -y

    # Kiểm tra cài đặt
    node --version
    npm --version
```
![Create Account](/NestJS-AWS-workshop/images/3/EC2.png)

![Create Account](/NestJS-AWS-workshop/images/3/EC3.png)

![Create Account](/NestJS-AWS-workshop/images/3/EC4.png)

sau khi cài đặt các path cần thiết, chúng ta sẽ clone dự án back end trên git về để chuẩn bị triển khai lên EC2.

chạy các lệnh sau:

```bash
    # Clone repository backend của bạn
    git clone [your-backend-repo-url]
    cd [your-backend-directory]

    # Install dependencies
    npm install

    # Tạo file environment
    cat > .env << EOF
    MONGODB_URI=mongodb+srv://ecourse_user:[password]@ecourse-cluster.xxxxx.mongodb.net/ecourse
    PORT=3000
    NODE_ENV=production
    JWT_SECRET=[your-jwt-secret]
    EOF
```
- Thay **[your-backend-repo-url]** bằng URL repository backend. 
- Đường link **"MONGODB_URI"** đã được tạo trong mục [**1.4**](/vi/1-create-new-aws-account/1.4-create-mongodb-database/), vui lòng xem lại.
- Thay **[your-jwt-secret]** bằng secret key cho JWT, nằm trong file **.env** của back end.

![Create Account](/NestJS-AWS-workshop/images/3/EC5.png)

![Create Account](/NestJS-AWS-workshop/images/3/EC6.PNG)

![Create Account](/NestJS-AWS-workshop/images/3/EC9.png)

Sau khi clone dự án xong, tiếp tục chạy các lệnh sau:

```bash
    # Build application
    npm run build

    # Start với PM2
    pm2 start dist/main.js --name "ecourse-backend"

    # Save PM2 configuration
    pm2 save
    pm2 startup
```
![Create Account](/NestJS-AWS-workshop/images/3/EC7.png)

![Create Account](/NestJS-AWS-workshop/images/3/EC8.png)

Sau đó, tạo file Nginx Configuration

```bash
    sudo nano /etc/nginx/sites-available/ecourse
```

Nhập đoạn mã sau:

```nginx
    server {
    listen 80;
    server_name [EC2_Public_IP];

    # Redirect all HTTP to HTTPS
    return 301 https://$host$request_uri;
    }

    server {
    listen 443 ssl;
    server_name [EC2_Public_IP];

    ssl_certificate /etc/ssl/certs/selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/selfsigned.key;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
        }
    }
```
- Thay **[EC2-PUBLIC-IP]** bằng IP public của EC2.

Tiếp tục chạy lệnh này để tạo Ssl cần khi triển khai front end lên **Cloudfront** vì EC2 chạy thông qua phương thức HTTP, còn Cloudfront chạy thông qua phương thức HTTPS:

```bash
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/selfsigned.key \
    -out /etc/ssl/certs/selfsigned.crt
```

{{% notice note %}}
Lưu ý: lệnh trên chỉ tạo self-signed certificate nhằm cho quá trình phát triển.
Giải pháp tốt nhất: Đăng ký domain và trỏ về IP EC2. Cài SSL miễn phí với Let's Encrypt cho domain đó. Khi đó, frontend gọi API qua https://api.yourdomain.com/... là chuẩn nhất, không bị cảnh báo bảo mật.
{{% /notice %}}

Sau khi tạo xong file Nginx, reload lại bằng lệnh:

```bash
    sudo nginx -t
    sudo systemctl reload nginx
```

![Create Account](/NestJS-AWS-workshop/images/3/EC10.png)

Sau khi chạy xong, test với Postman để kiểm tra xem backend api đã hoạt động chưa:

![Create Account](/NestJS-AWS-workshop/images/3/EC11.PNG)

Thông báo cho thấy API Backend đã kết nối thành công