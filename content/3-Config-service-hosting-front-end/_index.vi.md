---
title : "Cấu hình dịch vụ triển khai frontend"
date :  "2025-08-06"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

Chúng ta sẽ sử dụng 2 dịch vụ phổ biển cho việc triển khai website tĩnh là **S3 bucket** và **Cloudfront** bới các tính năng sau:

**Với S3 bucket**:

- S3 được xây dựng để đáp ứng yêu cầu của khách hàng thuộc mọi quy mô và ngành nghề, đều có thể dùng dịch vụ này để lưu trữ và bảo vệ bất kỳ lượng dữ liệu nào.

- S3 có thể được dùng cho nhiều trường hợp sử dụng như kho dữ liệu (data warehouse), trang web, ứng dụng di động, sao lưu và khôi phục, lưu trữ, ứng dụng doanh nghiệp, thiết bị IoT và phân tích dữ liệu lớn. Ngoài ra, Amazon S3 còn cung cấp các tính năng quản lý dễ sử dụng, giúp bạn có thể tổ chức dữ liệu và cấu hình các biện pháp kiểm soát truy cập để đáp ứng yêu cầu cụ thể của doanh nghiệp, tổ chức và yêu cầu về tuân thủ.

- Amazon S3 được thiết kế để đảm bảo độ bền 99,999999999% (11 9’s) và lưu trữ dữ liệu của hàng triệu ứng dụng cho các công ty trên toàn thế giới.

**Với Cloudfront**:

- **CDN toàn cầu:** CloudFront có mạng lưới các điểm hiện diện (edge locations) trên toàn thế giới, cho phép phân phối nội dung gần hơn với người dùng, giảm thời gian tải trang và cải thiện trải nghiệm người dùng. 
- **Phân phối nội dung tĩnh và động:** CloudFront hỗ trợ cả nội dung tĩnh (ví dụ: hình ảnh, CSS, JavaScript) và nội dung động (ví dụ: trang web được tạo động). 
- **Tích hợp với AWS:** CloudFront tích hợp tốt với các dịch vụ AWS khác như Amazon S3, Amazon EC2, giúp các công ty đã sử dụng AWS có thể dễ dàng triển khai và quản lý CDN. 
- **Bảo mật:** CloudFront cung cấp các tính năng bảo mật như mã hóa SSL/TLS, kiểm soát truy cập và tích hợp với AWS Shield để bảo vệ chống lại các cuộc tấn công DDoS. 
- **Tối ưu chi phí:** CloudFront giúp tối ưu hóa chi phí bằng cách giảm tải cho các máy chủ gốc, giảm băng thông và cung cấp các tùy chọn định giá linh hoạt. 
- **Tính năng khác:** CloudFront còn hỗ trợ các tính năng như phân phối video, quản lý cache, và giới hạn truy cập theo khu vực địa lý. 