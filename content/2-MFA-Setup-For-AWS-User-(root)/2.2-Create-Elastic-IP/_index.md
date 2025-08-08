---
title : "Initialize Elastic IP"
date : "2025-06-08"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

{{% notice note %}}
Elastic IP is intended to provide a public IP for EC2 Instance, which will remain the same regardless of whether EC2 is restarted or not.

{{% /notice %}}

In the **Network & Security** section of the **EC2** interface:

- Select **Elastic IPs**
- Select **Allocate Elastic IP address**

![Create Account](/static/images/2/IP.png)

- In the **Public IPv4 address pool** section, select **Amazon's pool of IPv4 addresses**.

- In the **Network border group** section, select the region you want to use, here is **us-east-1**.
- Select **Allocate**.

![Create Account](/static/images/2/IP2.png)

After creating, in the Elastic IPs interface:

- Select the newly created IP, select the **Actions** button, select **Associate Elastic IP address**

![Create Account](/static/images/2/IP3.png)

- In the **Resource type** section, select **Instance**

- Select the newly created Instance
- Select **Associate**

![Create Account](/static/images/2/IP4.png)