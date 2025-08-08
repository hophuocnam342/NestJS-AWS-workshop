---
title : "EC2 Instance Configuration and Backend Deployment"
date : "2025-06-08"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

Open CMD:
Enter the following command:
```Bash
AWS Configure
```
- AWS Access Key ID: enter Key ID
- AWs Secret Access Key ID: enter Secret Key ID
- Default region name: select region, here is **us-east-1**
- Default output format: enter **json**

![Create Account](/static/images/3/EC.png)

- Ssh to EC2 with the command
```bash
    ssh -i "C:\path\to\key.pem" ubuntu@[EC2-PUBLIC-IP]
```
- Replace **C:\path\to\ecourse-key.pem** with the actual path to the key file.

- Replace **[EC2-PUBLIC-IP]** with EC2's public IP.

![Create Account](/static/images/3/EC1.png)

After SSH is successful, run the following commands:
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

    # Check the installation
    node --version
    npm --version
```
![Create Account](/static/images/3/EC2.png)

![Create Account](/static/images/3/EC3.png)

![Create Account](/static/images/3/EC4.png)

After installing the necessary paths, we will clone the back end project on git to prepare for deployment to EC2.

run the following commands:

```bash 
    # Clone your backend repository 
    git clone [your-backend-repo-url] 
    cd [your-backend-directory] 

    # Install dependencies 
    npm install 

    # Create environment file 
    cat > .env << EOF 
    MONGODB_URI=mongodb+srv://ecourse_user:[password]@ecourse-cluster.xxxxx.mongodb.net/ecourse 
    PORT=3000 
    NODE_ENV=production 
    JWT_SECRET=[your-jwt-secret] 
    EOF
```
- Replace **[your-backend-repo-url]** with the backend repository URL.
- The link **"MONGODB_URI"** has been created in section [**1.4**](/vi/1-create-new-aws-account/1.4-create-mongodb-database/), please review.
- Replace **[your-jwt-secret]** with the secret key for JWT, located in the back end's **.env** file.

![Create Account](/static/images/3/EC5.png)

![Create Account](/static/images/3/EC6.png)

![Create Account](/static/images/3/EC9.png)

After cloning the project, continue running the following commands:

```bash 
    # Build application 
    npm run build 

    # Start with PM2 
    pm2 start dist/main.js --name "ecourse-backend" 

    # Save PM2 configuration 
    pm2 save 
    pm2 startups
```
![Create Account](/static/images/3/EC6.png)

![Create Account](/static/images/3/EC7.png)

![Create Account](/static/images/3/EC8.png)

Then, create Nginx Configuration file

```bash 
    sudo nano /etc/nginx/sites-available/ecourse
```

Enter the following code:

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
- Replace **[EC2-PUBLIC-IP]** with EC2's public IP.

Continue running this command to create Ssl needed when deploying front end to **Cloudfront** because EC2 runs through HTTP method, while Cloudfront runs through HTTPS method:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/ssl/private/selfsigned.key \
-out /etc/ssl/certs/selfsigned.crt
```

{{% notice note %}}
Note: the above command only creates self-signed certificate for development.
The best solution: Register a domain and point it to EC2 IP. Install free SSL with Let's Encrypt for that domain. Then, the frontend calls API via https://api.yourdomain.com/... is the most standard, without security warnings.
{{% /notice %}}

After creating the Nginx file, reload it with the command:

```bash
    sudo nginx -t
    sudo systemctl reload nginx
```

![Create Account](/static/images/3/EC10.png)

After running, test with Postman to check if the backend api is working:

![Create Account](/static/images/3/EC11.png)