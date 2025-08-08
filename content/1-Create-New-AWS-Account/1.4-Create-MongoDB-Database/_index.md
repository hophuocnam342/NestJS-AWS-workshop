---
title : "Configure database with MongoDB"
date : "2025-06-08"
weight : 4
chapter : false
pre : " <b> 1.4 </b> "
---

#### Cluster Configuration

1. Account Configuration

- Access MongoDB homepage: [MongoDB](https://account.mongodb.com/)
- Create Mongo DB account
- After creating the account, on the main interface select **Cluster**
- Select **Build a Cluster**

![Create Account](/NestJS-AWS-workshop/images/1/DB.PNG)

2. Cluster Configuration

- Select Cluster type: select **Free**

![Create Account](/NestJS-AWS-workshop/images/1/DB1.PNG)

- Select Name: name appropriately, or leave the default as **Cluster0**
- Select Provider: Check the **AWS** box
- Select Region: Select Region along with **Region of EC2 and other services on AWS** to avoid problems
- Check the information again, and select **Create Deployment**

![Create Account](/NestJS-AWS-workshop/images/1/DB2.PNG)

3. Create Cluster connection

- After completing the Cluster configuration, we come to the step of creating CLuster connection.
- On the Set up connection page, after granting permissions, the configurations can be left as default

![Create Account](/NestJS-AWS-workshop/images/1/DB3.png)

- Go to the **Choose a connection method** page, in the **Connect to your application** section, select **Drivers**

![Create Account](/NestJS-AWS-workshop/images/1/DB4.png)

- In section 3. , copy the link to the Cluster, in the form **"mongodb+srv://<db_username>:<db_password>@cluster0..."**, replace <db_username> and <db_password> with **username** and **password** on the **Set up connection** page above. We need this link to connect to the database and **EC2**.

![Create Account](/NestJS-AWS-workshop/images/1/DB5.png)
