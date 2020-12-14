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

1. Click [this link](https://console.aws.amazon.com/iam/home#/usersnew) to add user.
2. Enter `<user name>` in User name, and then choose both **Programmatic access** and **AWS Management Console access**. Next, enter `<password>` in **Console password**,
In last, uncheck **Require password reset**.

  ![](/analytics-on-aws/images/iam-user.png)


5. Click the **\[Next: Permissions\]** button, select **Attach existing policies directly**, and add **AdministratorAccess** privileges.

  ![](/analytics-on-aws/images/iam-user-policy.png)


6. Click the **\[Next: Review\]** button, check the information, and click the **Create user** button.
7. Click the **Download.csv** button to download the new user's information. This file is essential for setting up EC2, so save it in a location that is easy to remember.
  ![](/analytics-on-aws/images/iam-user-download.png)
