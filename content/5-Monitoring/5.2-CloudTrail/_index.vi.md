---
title : "Thiết lập CloudTrail"
date : "2025-06-08"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

Vào **AWS Console** > Tìm **CloudTrail** > Chọn **CloudTrail**
![Create Account](/NestJS-AWS-workshop/images/4/CT.png)
- Chọn **Trails** > **Create trail**
- Trail name: Đặt tên **ecourse-trail**
- **Apply trail to all regions**: Nên chọn nhằm theo dõi toàn bộ hoạt động trên mọi region
- **Management events**: Chọn “All” (theo dõi cả read và write)
- **Data events**: Có thể chọn S3, Lambda nếu muốn theo dõi chi tiết (tăng chi phí nếu bật nhiều)
- **Storage location**: Chọn S3 bucket để lưu log (có thể tạo mới hoặc chọn bucket hiện có)
- **Log file SSE-KMS encryption**: Để mặc định nếu chưa cần bảo mật cao
![Create Account](/NestJS-AWS-workshop/images/4/CT1.PNG)
![Create Account](/NestJS-AWS-workshop/images/4/CT2.PNG)
- Sau khi tạo, **CloudTrail** sẽ tự động ghi lại mọi hoạt động (ai tạo, xoá, sửa tài nguyên, v.v.)
![Create Account](/NestJS-AWS-workshop/images/4/CT3.png)
- Xem log CloudTrail: Vào **CloudTrail** > **Event history** để xem các sự kiện gần đây (ai làm gì, lúc nào, ở đâu). Có thể filter theo user, service, resource, v.v.
