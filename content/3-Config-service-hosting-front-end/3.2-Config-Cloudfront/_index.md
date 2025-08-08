---
title : "Cloudfront Configuration"
date : "2025-06-08"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

1. In the **S3 bucket** interface
- Select **Permissions**
- Currently, the **Block all public access** function is **Off** because we turned it on and off in the previous section
- Select **Edit**
- Select **Block all public access**
- Select **Save changes**
- Another window appears to confirm the edit, fill in **confirm**
- Select **confirm**

![Create Account](/images/03/cf.png?featherlight=false&width=90pc)

![Create Account](/images/03/cf1.png?featherlight=false&width=90pc)

![Create Account](/images/03/cf2.png?featherlight=false&width=90pc)

2. In the search bar, search for **Cloudfront** and select it

- After the **Cloudfront** interface appears, select **Create a CloudFront distribution**.

![Create Account](/images/03/cf3.png?featherlight=false&width=90pc)

3. Specify the following settings for the **distribution**:

- In the **Distribution name** field, enter the name **cloudfront-ecourse**.

- **Decripstion**, you can omit it or fill in the description.

- Select **Single website or app**.

- Select **Next**.

![Create Account](/images/03/cf4.png?featherlight=false&width=90pc)
![Create Account](/images/03/cf5.png?featherlight=false&width=90pc)

- Select **Amazon S3** for the **Origin type** field
- In the Origin S3 field, select the S3 bucket you created.

![Create Account](/images/03/cf6.png?featherlight=false&width=90pc)

- In **Setting**, check **Allow private S3 access to Cloudfront**
- In **Origin setting**, select **Use recommended origin setting**, then select **Next**

![Create Account](/images/03/cf7.png?featherlight=false&width=90pc)

- In **Enable security**, select **Do not enable security protections**
- Check the information again and select **Create distribution**

![Create Account](/images/03/cf8.png?featherlight=false&width=90pc)

4. Connect to **S3 bucket**
- After creating, switch to the **Origin** tab and select **Edit**
- Select **Use website endpoint**
- In the **Origin domain** field, select the **S3 bucket** you created.

- **Origin access** field, select **Origin access control setting**

![Create Account](/images/03/cf9.png?featherlight=false&width=90pc)

- **Origin access control** section, select **e-course-OAC**
- Next, a notification board appears, select **Copy policy**, then click **go to S3 bucket permissions** to edit **policy** (usually it will be automatically edited).

![Create Account](/images/03/cf10.png?featherlight=false&width=90pc)
![Create Account](/images/03/cf11.png?featherlight=false&width=90pc)

- Finally, select **Create origin**
5. Update new content
- Every time you update new content on **S3 bucket**, to have **Cloudfront** also update, go to the **invalidation** tab, select **Create invalidation**.
- In **Add object path**, write **/** to update all new content on the bucket.
- Then, select **Create invalidation**

![Create Account](/images/03/cf12.png?featherlight=false&width=90pc)
![Create Account](/images/03/cf13.png?featherlight=false&width=90pc)

6. Check the website
- To check if the website is active on **Cloudfront** or not, select the **Distribution** just created, switch to the **General** tab, copy the **Distribution domain name** and run it on the browser.

![Create Account](/images/03/cf14.png?featherlight=false&width=90pc)
![Create Account](/images/03/cf15.png?featherlight=false&width=90pc)