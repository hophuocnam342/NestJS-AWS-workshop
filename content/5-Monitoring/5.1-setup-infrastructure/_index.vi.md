---
title : "Thiết lập Infrastructure và Môi trường"
date : "2025-06-08"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

#### Thiết lập Infrastructure và Môi trường

#### Tổng quan
Trong lab này, bạn sẽ thiết lập toàn bộ infrastructure cần thiết cho AWS Database Benchmarking Suite. Chúng ta sẽ tạo VPC, security groups, IAM roles, và các database instances cần thiết cho việc testing.

#### Mục tiêu
- Tạo VPC và networking infrastructure
- Thiết lập security groups cho database access
- Tạo IAM roles và policies
- Deploy các database instances (RDS, DynamoDB, ElastiCache)
- Thiết lập CloudWatch monitoring

#### Thời gian: 30 phút

#### Bước 1: Tạo VPC và Networking

1. **Đăng nhập vào AWS Console**
   - Truy cập [AWS Console](https://console.aws.amazon.com)
   - Chọn region gần nhất (ví dụ: ap-southeast-1)

2. **Tạo VPC**
   ```bash
   # Tạo VPC cho benchmark environment
   aws ec2 create-vpc \
     --cidr-block 10.0.0.0/16 \
     --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=benchmark-vpc}]'
   ```

3. **Tạo Subnets**
   ```bash
   # Tạo public subnet
   aws ec2 create-subnet \
     --vpc-id vpc-xxxxxxxxx \
     --cidr-block 10.0.1.0/24 \
     --availability-zone ap-southeast-1a \
     --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=benchmark-public-subnet}]'

   # Tạo private subnet
   aws ec2 create-subnet \
     --vpc-id vpc-xxxxxxxxx \
     --cidr-block 10.0.2.0/24 \
     --availability-zone ap-southeast-1b \
     --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=benchmark-private-subnet}]'
   ```

#### Bước 2: Thiết lập Security Groups

1. **Tạo Security Group cho Database Access**
   ```bash
   # Tạo security group cho RDS
   aws ec2 create-security-group \
     --group-name benchmark-db-sg \
     --description "Security group for database benchmarking" \
     --vpc-id vpc-xxxxxxxxx

   # Thêm rule cho MySQL/PostgreSQL
   aws ec2 authorize-security-group-ingress \
     --group-id sg-xxxxxxxxx \
     --protocol tcp \
     --port 3306 \
     --cidr 10.0.0.0/16

   aws ec2 authorize-security-group-ingress \
     --group-id sg-xxxxxxxxx \
     --protocol tcp \
     --port 5432 \
     --cidr 10.0.0.0/16
   ```

2. **Tạo Security Group cho EC2 Benchmark Runner**
   ```bash
   # Tạo security group cho EC2
   aws ec2 create-security-group \
     --group-name benchmark-ec2-sg \
     --description "Security group for benchmark runner" \
     --vpc-id vpc-xxxxxxxxx

   # Thêm SSH access
   aws ec2 authorize-security-group-ingress \
     --group-id sg-xxxxxxxxx \
     --protocol tcp \
     --port 22 \
     --cidr 0.0.0.0/0
   ```

#### Bước 3: Tạo IAM Roles và Policies

1. **Tạo IAM Policy cho Benchmark Runner**
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "rds:DescribeDBInstances",
           "rds:DescribeDBClusters",
           "dynamodb:DescribeTable",
           "dynamodb:Scan",
           "dynamodb:Query",
           "elasticache:DescribeCacheClusters",
           "cloudwatch:PutMetricData",
           "cloudwatch:GetMetricStatistics",
           "s3:PutObject",
           "s3:GetObject",
           "s3:ListBucket"
         ],
         "Resource": "*"
       }
     ]
   }
   ```

2. **Tạo IAM Role**
   ```bash
   # Tạo role
   aws iam create-role \
     --role-name BenchmarkRunnerRole \
     --assume-role-policy-document '{
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": {
             "Service": "ec2.amazonaws.com"
           },
           "Action": "sts:AssumeRole"
         }
       ]
     }'

   # Attach policy
   aws iam attach-role-policy \
     --role-name BenchmarkRunnerRole \
     --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
   ```

#### Bước 4: Deploy Database Instances

1. **Tạo RDS MySQL Instance**
   ```bash
   aws rds create-db-instance \
     --db-instance-identifier benchmark-mysql \
     --db-instance-class db.t3.micro \
     --engine mysql \
     --engine-version 8.0.28 \
     --master-username admin \
     --master-user-password YourPassword123! \
     --allocated-storage 20 \
     --vpc-security-group-ids sg-xxxxxxxxx \
     --db-subnet-group-name benchmark-subnet-group \
     --backup-retention-period 7 \
     --deletion-protection
   ```

2. **Tạo RDS PostgreSQL Instance**
   ```bash
   aws rds create-db-instance \
     --db-instance-identifier benchmark-postgres \
     --db-instance-class db.t3.micro \
     --engine postgres \
     --engine-version 13.7 \
     --master-username admin \
     --master-user-password YourPassword123! \
     --allocated-storage 20 \
     --vpc-security-group-ids sg-xxxxxxxxx \
     --db-subnet-group-name benchmark-subnet-group \
     --backup-retention-period 7 \
     --deletion-protection
   ```

3. **Tạo DynamoDB Table**
   ```bash
   aws dynamodb create-table \
     --table-name benchmark-table \
     --attribute-definitions AttributeName=id,AttributeType=S \
     --key-schema AttributeName=id,KeyType=HASH \
     --billing-mode PAY_PER_REQUEST
   ```

4. **Tạo ElastiCache Redis Cluster**
   ```bash
   aws elasticache create-cache-cluster \
     --cache-cluster-id benchmark-redis \
     --engine redis \
     --cache-node-type cache.t3.micro \
     --num-cache-nodes 1 \
     --vpc-security-group-ids sg-xxxxxxxxx \
     --subnet-group-name benchmark-subnet-group
   ```

#### Bước 5: Thiết lập CloudWatch Monitoring

1. **Tạo CloudWatch Dashboard**
   ```bash
   aws cloudwatch put-dashboard \
     --dashboard-name BenchmarkDashboard \
     --dashboard-body '{
       "widgets": [
         {
           "type": "metric",
           "properties": {
             "metrics": [
               ["AWS/RDS", "CPUUtilization", "DBInstanceIdentifier", "benchmark-mysql"],
               ["AWS/RDS", "DatabaseConnections", "DBInstanceIdentifier", "benchmark-mysql"]
             ],
             "period": 300,
             "stat": "Average",
             "region": "ap-southeast-1",
             "title": "RDS MySQL Metrics"
           }
         }
       ]
     }'
   ```

2. **Tạo CloudWatch Alarms**
   ```bash
   # CPU Utilization Alarm
   aws cloudwatch put-metric-alarm \
     --alarm-name benchmark-cpu-alarm \
     --alarm-description "CPU utilization high" \
     --metric-name CPUUtilization \
     --namespace AWS/RDS \
     --statistic Average \
     --period 300 \
     --threshold 80 \
     --comparison-operator GreaterThanThreshold \
     --evaluation-periods 2 \
     --dimensions Name=DBInstanceIdentifier,Value=benchmark-mysql
   ```

#### Bước 6: Tạo S3 Bucket cho Results

```bash
# Tạo S3 bucket cho benchmark results
aws s3 mb s3://benchmark-results-$(date +%s) \
  --region ap-southeast-1

# Tạo bucket policy
aws s3api put-bucket-policy \
  --bucket benchmark-results-xxxxxxxxx \
  --policy '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "AllowBenchmarkRunner",
        "Effect": "Allow",
        "Principal": {
          "AWS": "arn:aws:iam::ACCOUNT-ID:role/BenchmarkRunnerRole"
        },
        "Action": [
          "s3:PutObject",
          "s3:GetObject",
          "s3:ListBucket"
        ],
        "Resource": [
          "arn:aws:s3:::benchmark-results-xxxxxxxxx",
          "arn:aws:s3:::benchmark-results-xxxxxxxxx/*"
        ]
      }
    ]
  }'
```

#### Bước 7: Tạo EC2 Instance cho Benchmark Runner

```bash
# Tạo EC2 instance
aws ec2 run-instances \
  --image-id ami-xxxxxxxxx \
  --count 1 \
  --instance-type t3.medium \
  --key-name your-key-pair \
  --security-group-ids sg-xxxxxxxxx \
  --subnet-id subnet-xxxxxxxxx \
  --iam-instance-profile Name=BenchmarkRunnerRole \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=benchmark-runner}]'
```

#### Kiểm tra và Validation

1. **Kiểm tra tất cả resources đã được tạo**
   ```bash
   # Kiểm tra VPC
   aws ec2 describe-vpcs --filters "Name=tag:Name,Values=benchmark-vpc"

   # Kiểm tra RDS instances
   aws rds describe-db-instances --db-instance-identifier benchmark-mysql
   aws rds describe-db-instances --db-instance-identifier benchmark-postgres

   # Kiểm tra DynamoDB table
   aws dynamodb describe-table --table-name benchmark-table

   # Kiểm tra ElastiCache cluster
   aws elasticache describe-cache-clusters --cache-cluster-id benchmark-redis
   ```

2. **Test connectivity**
   ```bash
   # SSH vào EC2 instance
   ssh -i your-key.pem ec2-user@your-ec2-ip

   # Test connection đến MySQL
   mysql -h benchmark-mysql.xxxxxxxxx.ap-southeast-1.rds.amazonaws.com -u admin -p

   # Test connection đến PostgreSQL
   psql -h benchmark-postgres.xxxxxxxxx.ap-southeast-1.rds.amazonaws.com -U admin -d postgres
   ```

#### Cleanup (Quan trọng!)

Sau khi hoàn thành workshop, hãy xóa tất cả resources:

```bash
# Xóa EC2 instance
aws ec2 terminate-instances --instance-ids i-xxxxxxxxx

# Xóa RDS instances
aws rds delete-db-instance --db-instance-identifier benchmark-mysql --skip-final-snapshot
aws rds delete-db-instance --db-instance-identifier benchmark-postgres --skip-final-snapshot

# Xóa DynamoDB table
aws dynamodb delete-table --table-name benchmark-table

# Xóa ElastiCache cluster
aws elasticache delete-cache-cluster --cache-cluster-id benchmark-redis

# Xóa S3 bucket
aws s3 rb s3://benchmark-results-xxxxxxxxx --force

# Xóa VPC
aws ec2 delete-vpc --vpc-id vpc-xxxxxxxxx
```

#### Tóm tắt

Trong lab này, bạn đã:
- ✅ Tạo VPC và networking infrastructure
- ✅ Thiết lập security groups
- ✅ Tạo IAM roles và policies
- ✅ Deploy các database instances
- ✅ Thiết lập CloudWatch monitoring
- ✅ Tạo S3 bucket cho results
- ✅ Deploy EC2 benchmark runner

**Bước tiếp theo**: [Thiết kế Benchmark Methodology](5.2-benchmark-methodology/) 