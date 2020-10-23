---
title: "Creating an IAM User"
weight: 21
pre: "<b>2-1 </b>"
tag:
  - IAM_User
---

{{% notice info %}}
Let's create an IAM User to use during the lab.
{{% /notice %}}

1. Log in to the AWS Management Console and access the IAM service.
2. Select **Users** from the left menu.
3. Click the **Add user** button to enter the Add User page.
4. Enter `<user name>` in User name, and then choose both **Programmatic access** and **AWS Management Console access**. Next, enter `<password>` in **Console password**,
In last, uncheck **Require password reset**.
5. Click the **\[Next: Permissions\]** button, select **Attach existing policies directly**, and add **AdministratorAccess** privileges.
6. Click the **\[Next: Review\]** button, check the information, and click the **Create user** button.
7. Click the **Download.csv** button to download the new user's information. This file is essential for setting up EC2, so save it in a location that is easy to remember.