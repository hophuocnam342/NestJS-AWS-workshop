---
title : "Triển khai trang web nestjs api lên AWS"
date :  "2025-08-03"
weight : 1 
chapter : false
---

# Triển khai website Nestjs lên AWS

#### Tổng quan

Trong bài lap này, bạn sẽ được hướng dẫn cách triển khai một **trang web chạy backend API Nestjs** lên **nền tảng của AWS**. Với các dịch vụ hỗ trợ một cách tối ưu như **EC2, VPC, S3, Cloudfront,...**, trang web của bạn sẽ hoạt động một cách nhanh chóng, phù hợp với yêu cầu khắt khe của người dùng.
![Deploy](/images/1/Diagram.png)

#### Giới thiệu Amazon Elastic Compute Cloud (EC2)

Amazon EC2 là dịch vụ cung cấp khả năng điện toán đám mây có thể thay đổi kích thước được trên AWS. Một số đặc điểm chính:
- **Amazon EC2** hoạt động tương tự như máy chủ ảo hoặc máy chủ vật lý truyền thống, nhưng với khả năng khởi tạo nhanh chóng, co giãn tài nguyên linh hoạt và quản lý đơn giản.
- **Máy chủ ảo** chia nhỏ máy chủ vật lý thành nhiều máy chủ ảo, giúp tối ưu hóa việc sử dụng tài nguyên phần cứng.

- **Amazon EC2** hỗ trợ đa dạng các workload như: lưu trữ web, ứng dụng, cơ sở dữ liệu, dịch vụ xác thực và bất kỳ tác vụ nào mà máy chủ thông thường có thể thực hiện.

![Create Account](/images/1/h1.jpeg)

#### Giới thiệu Amazon Virtual Private Cloud (Amazon VPC)

**Amazon VPC** là dịch vụ mạng ảo tùy chỉnh nằm trong **AWS Cloud**, cho phép bạn tạo một môi trường mạng riêng biệt và hoàn toàn tách biệt với thế giới bên ngoài. Khái niệm này tương tự như việc thiết kế và triển khai một mạng độc lập trong trung tâm dữ liệu on-premise truyền thống.
**Tính năng chính:**
- Kiểm soát hoàn toàn môi trường mạng ảo
- Khởi tạo và quản lý tài nguyên AWS
- Tùy chỉnh phạm vi địa chỉ IP và phân đoạn mạng
- Cấu hình định tuyến và kết nối mạng linh hoạt
- Hỗ trợ đầy đủ IPv4 và IPv6

#### Giới thiệu Amazon Simple Storage Service (Amazon S3) 

**Amazon Simple Storage Service (Amazon S3)** là một dịch vụ lưu trữ dạng đối tượng cung cấp khả năng mở rộng theo yêu cầu sử dụng, đảm bảo tính khả dụng của dữ liệu, độ bảo mật, và hiệu năng ở mức cao nhất.

S3 được xây dựng để đáp ứng yêu cầu của khách hàng thuộc mọi quy mô và ngành nghề, đều có thể dùng dịch vụ này để lưu trữ và bảo vệ bất kỳ lượng dữ liệu nào.

S3 có thể được dùng cho nhiều trường hợp sử dụng như kho dữ liệu (data warehouse), trang web, ứng dụng di động, sao lưu và khôi phục, lưu trữ, ứng dụng doanh nghiệp, thiết bị IoT và phân tích dữ liệu lớn. Ngoài ra, Amazon S3 còn cung cấp các tính năng quản lý dễ sử dụng, giúp bạn có thể tổ chức dữ liệu và cấu hình các biện pháp kiểm soát truy cập để đáp ứng yêu cầu cụ thể của doanh nghiệp, tổ chức và yêu cầu về tuân thủ.

Amazon S3 được thiết kế để đảm bảo độ bền 99,999999999% (11 9’s) và lưu trữ dữ liệu của hàng triệu ứng dụng cho các công ty trên toàn thế giới.

#### Giới thiệu Amazon Cloudfront

**Amazon CloudFront** là một dịch vụ mạng phân phối nội dung (CDN) của **Amazon Web Services** (AWS). Nó giúp tăng tốc độ phân phối nội dung web, video, ứng dụng và API đến người dùng trên toàn cầu bằng cách phân phối nội dung từ các vị trí trung gian (edge locations) gần người dùng nhất, giảm độ trễ và cải thiện hiệu suất. 
**Tính năng chính:**
- **CDN toàn cầu:** CloudFront có mạng lưới các điểm hiện diện (edge locations) trên toàn thế giới, cho phép phân phối nội dung gần hơn với người dùng, giảm thời gian tải trang và cải thiện trải nghiệm người dùng. 
- **Phân phối nội dung tĩnh và động:** CloudFront hỗ trợ cả nội dung tĩnh (ví dụ: hình ảnh, CSS, JavaScript) và nội dung động (ví dụ: trang web được tạo động). 
- **Tích hợp với AWS:** CloudFront tích hợp tốt với các dịch vụ AWS khác như Amazon S3, Amazon EC2, giúp các công ty đã sử dụng AWS có thể dễ dàng triển khai và quản lý CDN. 
- **Bảo mật:** CloudFront cung cấp các tính năng bảo mật như mã hóa SSL/TLS, kiểm soát truy cập và tích hợp với AWS Shield để bảo vệ chống lại các cuộc tấn công DDoS. 
- **Tối ưu chi phí:** CloudFront giúp tối ưu hóa chi phí bằng cách giảm tải cho các máy chủ gốc, giảm băng thông và cung cấp các tùy chọn định giá linh hoạt. 
- **Tính năng khác:** CloudFront còn hỗ trợ các tính năng như phân phối video, quản lý cache, và giới hạn truy cập theo khu vực địa lý. 

#### Giới thiệu AWS Cognito

**AWS Cognito** cho phép chúng ta dễ dàng xây dựng luồng đăng nhập, đăng ký, xác minh email, đổi mật khẩu, đặt lại mật khẩu, v.v., thay vì phải tự xây dựng DB cho người dùng và thực hiện nhiều thao tác như JWT, băm mật khẩu, gửi email xác minh,... Điều này giúp bạn tập trung phát triển các tính năng khác của ứng dụng. Người dùng có thể đăng nhập trực tiếp bằng tên người dùng và mật khẩu hoặc thông qua bên thứ ba như Facebook, Amazon, Google hoặc Apple.
Hai thành phần chính của **Amazon Cognito** là nhóm người dùng và nhóm danh tính:

- Nhóm người dùng : thư mục người dùng cung cấp tùy chọn đăng ký và đăng nhập cho người dùng web và ứng dụng di động của bạn. Sau khi đăng nhập bằng nhóm người dùng, người dùng ứng dụng có thể truy cập các tài nguyên mà ứng dụng cho phép.
- Nhóm danh tính : cấp cho người dùng của bạn quyền truy cập vào các dịch vụ AWS khác.

#### Giới thiệu Amazon EC2 Auto Scaling Group

**1. Tại sao cần sử dụng Auto scaling group?**

Khi ứng dụng của chúng ta đưa vào hoạt động, lượng người truy cập sẽ thay đổi theo thời gian, do đó chúng ta cần thường xuyên thay đổi (scaling) lượng instance nhằm nâng cao tính sẵn sàng và tiết kiệm chi phí. Để tự động hóa và linh hoạt trong công việc scaling, chúng ta sẽ có giải pháp là **Auto Scaling Group**.

**2. Sơ lược về Auto Scaling Group**

**Amazon EC2 Auto Scaling Group (ASG)** giúp tự động điều chỉnh số lượng EC2 instances theo nhu cầu của ứng dụng. ASG có thể tự động mở rộng (scale out) khi lưu lượng tăng, hoặc thu nhỏ (scale in) khi lưu lượng giảm, giúp tối ưu hóa tài nguyên và giảm chi phí. Nó cũng giúp đảm bảo tính sẵn sàng cao bằng cách phân phối instances qua nhiều Availability Zones để duy trì hoạt động liên tục ngay cả khi một phần của hệ thống gặp sự cố.

#### Giới thiệu Elastic Load Balancer

**Elastic Load Balancer (ELB)** là một dịch vụ giúp phân phối đều tải công việc (traffic) đến nhiều máy chủ hoặc instances để đảm bảo hệ thống hoạt động ổn định và tránh quá tải cho bất kỳ một máy chủ nào. Nó giúp tối ưu hiệu suất, tăng tính sẵn sàng và đảm bảo rằng nếu một máy chủ gặp sự cố, lưu lượng sẽ được chuyển hướng tới các máy chủ khác mà không ảnh hưởng đến người dùng.

AWS cung cấp ba loại Load Balancer:

- Application Load Balancer (ALB): Tối ưu cho HTTP/HTTPS traffic, hoạt động ở tầng ứng dụng (Layer 7)
- Network Load Balancer (NLB): Xử lý traffic ở tầng vận chuyển (Layer 4), phù hợp cho các ứng dụng yêu cầu hiệu suất cực cao
- Gateway Load Balancer (GWLB): Dùng để triển khai và quản lý các thiết bị mạng ảo

Trong bài hướng dẫn này, chúng ta sẽ sử dụng **Application Load Balancer (ALB)** để tối ưu HTTP/HTTPS traffic.

#### Giới thiệu ElastiCache

{{% notice warning %}}
Service này có thể tính phí cao, vui lòng cân nhắc trước khi sử dụng. 
{{% /notice %}}

**ElastiCache** là một dịch vụ của AWS mà cho phép ta tạo một clusters Memcached hoặc Redis một cách dễ dàng thay vì ta phải tự cài đặt và cấu hình nhiều thứ. 
AWS ElastiCache sẽ cover cho ta nhứng thứ sau:

- Installation: khi ta tạo một ElastiCache thì AWS sẽ tự động cài đặt những thứ cần thiết cho Memcached và Redis ở bên dưới của nó, ta chỉ cần đợi nó cài xong và xài.
- Administration: những vấn đề liên quan tới công việc của system admin cho một ElastiCache thì ta không cần phải quan tâm, AWS làm cho ta.
- Monitoring: ElastiCache sẽ push metrics của nó lên trên CloudWatch.
- Backups: AWS có option cho ta tự động backup dữ liệu cache (redis only).

#### Các dịch vụ thông báo

**1. Amazon Cloudtrail**

**AWS CloudTrail** là một dịch vụ của Amazon Web Services (AWS) giúp ghi lại hoạt động của tài khoản AWS của bạn, bao gồm cả các hành động được thực hiện bởi người dùng, vai trò hoặc dịch vụ AWS. Nó hoạt động như một công cụ kiểm toán và giám sát, ghi lại các sự kiện dưới dạng nhật ký, cho phép người dùng xem lại lịch sử hoạt động, phân tích rủi ro và đảm bảo tuân thủ các quy định. 

**2. Amazon Cloudwatch**

**Amazon CloudWatch** là dịch vụ theo dõi và quản lý cung cấp dữ liệu và thông tin định hướng hành động cho tài nguyên cơ sở hạ tầng và ứng dụng AWS, ứng dụng hybrid cũng như ứng dụng on-premises. Bạn có thể thu thập và truy cập tất cả dữ liệu về hiệu năng và hoạt động dưới hình thức logs và metrics trong cùng một nền tảng, thay vì theo dõi riêng lẻ (máy chủ, mạng hoặc cơ sở dữ liệu). CloudWatch cho phép bạn theo dõi end-to-end (ứng dụng, cơ sở hạ tầng và dịch vụ) và tận dụng cảnh báo, logs và dữ liệu sự kiện để tự động hóa các hành động và giảm Mean Time To Resolution (MTTR). Dịch vụ này giúp bạn giải phóng tài nguyên quan trọng và tập trung vào việc xây dựng các ứng dụng và giá trị kinh doanh.

**3. Simple Notification Service(AWS SNS)**

**AWS SNS (Simple Notification Service)** là một dịch vụ nhắn tin của Amazon Web Services (AWS) cho phép các nhà phát triển gửi thông báo (notifications) đến các thuê bao (subscribers) hoặc các ứng dụng khác. Nó hoạt động theo mô hình xuất bản/đăng ký (publish/subscribe), nơi các nhà xuất bản (publishers) gửi tin nhắn đến các chủ đề (topics), và các người đăng ký (subscribers) nhận thông báo từ các chủ đề mà họ quan tâm. 

**4. Amazon Simple Queue Service(AWS SQS)**

**AWS SQS**, hay **Amazon Simple Queue Service**, là một dịch vụ hàng đợi tin nhắn phân tán, được quản lý hoàn toàn bởi AWS, giúp các ứng dụng và hệ thống phân tán có thể giao tiếp với nhau một cách linh hoạt và đáng tin cậy. SQS giúp tách rời các thành phần của ứng dụng, cho phép chúng hoạt động độc lập và tăng khả năng mở rộng, chịu lỗi. 


#### Nội dung chính

1. [Chuẩn bị](1-create-new-aws-account/)
2. [Cài đặt backend Nestjs cho EC2](2-mfa-setup-for-aws-user-(root)/)
3. [Cấu hình dịch vụ triển khai frontend](3-Config-service-hosting-front-end/)
4. [Dọn dẹp tài nguyên](4-verify-new-account/)