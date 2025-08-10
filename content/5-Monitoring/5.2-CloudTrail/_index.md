---
title : "Setting up CloudTrail"
date : "2025-06-08"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

Go to **AWS Console** > Find **CloudTrail** > Select **CloudTrail**
![Create Account](/NestJS-AWS-workshop/images/4/CT.png)
- Select **Trails** > **Create trail**
- Trail name: Name **ecourse-trail**
- **Apply trail to all regions**: Should be selected to track all activities in all regions
- **Management events**: Select “All” (track both read and write)
- **Data events**: Can choose S3, Lambda if you want to track in detail (increase cost if enabled multiple)
- **Storage location**: Select S3 bucket to save logs (can create new or select existing bucket)
- **Log file SSE-KMS encryption**: Leave it as default if you don't need high security
![Create Account](/NestJS-AWS-workshop/images/4/CT1.PNG)
![Create Account](/NestJS-AWS-workshop/images/4/CT2.PNG)
- Once created, **CloudTrail** will automatically record all activities (who created, deleted, modified resources, etc.)
![Create Account](/NestJS-AWS-workshop/images/4/CT3.png)
- View CloudTrail logs: Go to **CloudTrail** > **Event history** to see recent events (who did what, when, where). Filter by user, service, resource, etc.