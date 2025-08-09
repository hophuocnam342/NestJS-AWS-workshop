---
title : "Các hoạt động giám sát"
date : "2025-06-08"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

Chúng ta sẽ sử dụng 2 dịch vụ sau cho việc giám sát:

**AWS CloudTrail** là một dịch vụ của Amazon Web Services (AWS) giúp ghi lại hoạt động của tài khoản AWS của bạn, bao gồm cả các hành động được thực hiện bởi người dùng, vai trò hoặc dịch vụ AWS. Nó hoạt động như một công cụ kiểm toán và giám sát, ghi lại các sự kiện dưới dạng nhật ký, cho phép người dùng xem lại lịch sử hoạt động, phân tích rủi ro và đảm bảo tuân thủ các quy định. 

->**CloudTrail**: Theo dõi mọi thao tác (API call, tạo/xoá resource, v.v.) trên tài khoản AWS.

**Amazon CloudWatch** là dịch vụ theo dõi và quản lý cung cấp dữ liệu và thông tin định hướng hành động cho tài nguyên cơ sở hạ tầng và ứng dụng AWS, ứng dụng hybrid cũng như ứng dụng on-premises. Bạn có thể thu thập và truy cập tất cả dữ liệu về hiệu năng và hoạt động dưới hình thức logs và metrics trong cùng một nền tảng, thay vì theo dõi riêng lẻ (máy chủ, mạng hoặc cơ sở dữ liệu). CloudWatch cho phép bạn theo dõi end-to-end (ứng dụng, cơ sở hạ tầng và dịch vụ) và tận dụng cảnh báo, logs và dữ liệu sự kiện để tự động hóa các hành động và giảm Mean Time To Resolution (MTTR). Dịch vụ này giúp bạn giải phóng tài nguyên quan trọng và tập trung vào việc xây dựng các ứng dụng và giá trị kinh doanh.

-> **CloudWatch**: Giám sát log, metric, cảnh báo cho EC2/backend.