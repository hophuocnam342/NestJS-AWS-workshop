---
title : "S3 Bucket Configuration"
date : "2025-06-08"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

1. Go to **AWS Console**
- Search for **S3**
- Select and the **S3** interface will appear.
- Click **Create Bucket**

![Create Account](/NestJS-AWS-workshop/images/03/S3.png)

2. At the **Bucket** initialization interface
- At **Bucket name**, name it **ecourse-frontend**.

- At **Object Ownership**, select **ACLs disable**.

- Uncheck **Block all public access**.

- **Bucket Versioning**. select **disable**.

- Other items can be kept as is, then select **Create Bucket**

![Create Account](/NestJS-AWS-workshop/images/03/S32.PNG)
![Create Account](/NestJS-AWS-workshop/images/03/S33.PNG)
![Create Account](/NestJS-AWS-workshop/images/03/S34.PNG)
![Create Account](/NestJS-AWS-workshop/images/03/S35.PNG)

3. After creating **Bucket**, we need to enable **Static Website Hosting**
- Click on the bucket just created.
- Go to the Properties tab.
- Scroll down to the Static website hosting section → click Edit.
- Select Enable.
- Index document: **index.html**
- Error document: **error.html** (if available, or leave blank)
- Click Save changes.

![Create Account](/NestJS-AWS-workshop/images/03/S36.PNG)

4. Configure **Bucket policy**

- Go to the **Permissions** tab of **bucket**.

- Scroll down to the **Bucket policy** section, click **Edit**.
- Paste the following:

```js
{
"Version": "2012-10-17",
"Statement": [
{
"Sid": "PublicReadGetObject",
"Effect": "Allow",
"Principal": "*",
"Action": "s3:GetObject",
"Resource": "arn:aws:s3:::ecourse-frontend/*"
}
]
}
```

- Then click **Save changes**
![Create Account](/NestJS-AWS-workshop/images/03/S37.png)

5. Upload the folder and frontend files to the Bucket

- Go to the **Objects** tab.

- Click Upload → Add files or Add folder.

- Select all frontend files/folders (HTML, CSS, JS, img, ...).

- Click **Upload**.
{{% notice note %}}
When uploading, you should upload all the sub-files in the parent folder of the frontend project, to avoid errors such as not finding the **index.html** or **error.html** file.
{{% /notice %}}

![Create Account](/NestJS-AWS-workshop/images/03/S38.png)
![Create Account](/NestJS-AWS-workshop/images/03/S39.png)

6. Check the website
- Go back to the **Properties** tab, scroll down to the **Static website hosting** section.

- Copy **Bucket website endpoint** and open it on the browser.

- If you see the website appear, it is successful!

![Create Account](/NestJS-AWS-workshop/images/03/S310.png)

![Create Account](/NestJS-AWS-workshop/images/03/S311.png)