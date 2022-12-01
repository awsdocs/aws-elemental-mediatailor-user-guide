# Setting up AWS Elemental MediaTailor<a name="setting-up"></a>

This section guides you through the steps required to configure users to access AWS Elemental MediaTailor\. For background and additional information about identity and access management for MediaTailor, see [Identity and access management](security-iam.md)\.

To start using AWS Elemental MediaTailor, complete the following steps\.

**Topics**
+ [Signing up for AWS](#setting-up-aws-sign-up)
+ [Creating an admin IAM user](#setting-up-create-admin-user)
+ [Creating a non\-admin IAM user](#setting-up-create-non-admin-user)

## Signing up for AWS<a name="setting-up-aws-sign-up"></a>

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html#aws_tasks-that-require-root)\.

## Creating an admin IAM user<a name="setting-up-create-admin-user"></a>

  When you create an AWS account, you begin with one sign\-in identity that has complete access to all AWS services and resources in the account\. This identity is called the AWS account *root user* and is accessed by signing in with the email address and password that you used to create the account\. We strongly recommend that you do not use the root user for your everyday tasks\. Safeguard your root user credentials and use them to perform the tasks that only the root user can perform\. For the complete list of tasks that require you to sign in as the root user, see [Tasks that require root user credentials](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html#aws_tasks-that-require-root) in the *AWS General Reference*\. 

In the following procedure, you use the AWS account root user to create your first IAM user\. You then add this IAM user to an Administrators group, to ensure that you have access to all services and their resources in your account\. The next time that you access your AWS account, sign in with the credentials for this IAM user\.

To create users with limited permissions, see [Creating a non\-admin IAM user](#setting-up-create-non-admin-user)\.

To create an administrator user, choose one of the following options\.


****  

| Choose one way to manage your administrator | To | By | You can also | 
| --- | --- | --- | --- | 
| In IAM Identity Center \(Recommended\) | Use short\-term credentials to access AWS\.This aligns with the security best practices\. For information about best practices, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-users-federation-idp) in the *IAM User Guide*\. | Following the instructions in [Getting started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) in the AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide\. | Configure programmatic access by [Configuring the AWS CLI to use AWS IAM Identity Center \(successor to AWS Single Sign\-On\)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html) in the AWS Command Line Interface User Guide\. | 
| In IAM \(Not recommended\) | Use long\-term credentials to access AWS\. | Following the instructions in [Creating your first IAM admin user and user group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the IAM User Guide\. | Configure programmatic access by [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the IAM User Guide\. | 

For information about creating users with limited permissions, see [Creating a non\-admin IAM user](#setting-up-create-non-admin-user)\.

## Creating a non\-admin IAM user<a name="setting-up-create-non-admin-user"></a>

Users in the Administrators group for an account have access to all AWS services and resources in that account\. This section describes how to create users with permissions that are limited to AWS Elemental MediaTailor\.

**Topics**
+ [Step 1: Sign in to the IAM console](#sign-in-to-iam-console)
+ [Step 2: Create policies](#setting-up-non-admin-policies)
+ [Step 3: Create user groups](#setting-up-non-admin-groups)
+ [Step 4: Create users](#setting-up-non-admin-users)

### Step 1: Sign in to the IAM console<a name="sign-in-to-iam-console"></a>

Sign in to the IAM console to manage policies and users\.

**To sign in**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   Use your Administrator user credentials to sign in\.

### Step 2: Create policies<a name="setting-up-non-admin-policies"></a>

Create two policies for AWS Elemental MediaTailor: one to provide read/write access, and one to provide read\-only access\. 

**To create policies for AWS Elemental MediaTailor**

1. In the navigation pane of the IAM console, choose **Policies**\.

1. On the **Policies** page, create a policy named **MediaTailorAllAccess** that allows all actions on all resources in MediaTailor:

   1. Choose **Create policy**\.

   1. Choose the **JSON** tab and paste the following policy, replacing the resource ARN for PassRole with the actual ARN\.

      ```
      {
      	"Version": "2012-10-17",
      	"Statement": {
      		"Effect": "Allow",
      		"Action": "mediatailor:*",
      		"Resource": "*"
      	}
      }
      ```

   1. Choose **Review policy**\.

   1. On the **Review policy** page, for **Name**, enter **MediaTailorAllAccess**, and then choose **Create policy**\.

1. On the **Policies** page, create a read\-only policy named **MediaTailorReadOnlyAccess** for MediaTailor:

   1. Choose **Create policy**\.

   1. Choose the **JSON** tab and paste the following read\-only policy\.

      ```
      {
      	"Version": "2012-10-17",
      	"Statement": {
      		"Effect": "Allow",
      		"Action": [
      			"mediatailor:List*",
      			"mediatailor:Describe*",
      			"mediatailor:Get*"
      		],
      		"Resource": "*"
      	}
      }
      ```

   1. Choose **Review policy**\.

   1. On the **Review policy** page, for **Name**, enter **MediaTailorReadOnlyAccess**, and then choose **Create policy**\.

### Step 3: Create user groups<a name="setting-up-non-admin-groups"></a>

Create a user group for each of the policies that you created in [Step 2: Create policies](#setting-up-non-admin-policies)\. This way, when you create additional users you can add the users to a group rather than attaching individual policies to each user\. 

**To create groups for users who need access to AWS Elemental MediaTailor**

1. In the navigation pane of the IAM console, choose **Groups**\.

1. On the **Groups** page, choose **Create New Group** and create an administrator group using the **MediaTailorAllAccess** policy: 

   1. On the **Set Group Name** page, enter a name for the group, such as **MediaTailorAdmins**\. Choose **Next Step**\.

   1. On the **Attach Policy** page, for **Filter**, choose **Customer Managed**\.

   1. In the policy list, choose the **MediaTailorAllAccess** policy that you created\.

   1. On the **Review** page, verify that the correct policy is added to this group, and then choose **Create Group**\.

1. On the **Groups** page, choose **Create New Group** and create a read\-only group using the **MediaTailorReadOnlyAccess** policy: 

   1. On the **Set Group Name** page, enter a name for the group, such as **MediaTailorReadOnly**\. Choose **Next Step**\.

   1. On the **Attach Policy** page, for **Filter**, choose **Customer Managed**\.

   1. In the policy list, choose the **MediaTailorReadOnlyAccess** policy that you created\.

   1. On the **Review** page, verify that the correct policy is added to this group, and then choose **Create Group**\.

### Step 4: Create users<a name="setting-up-non-admin-users"></a>

Create IAM users for the individuals who require access to AWS Elemental MediaTailor\. Next, add each user to the appropriate user group to ensure that they have the right level of permissions\. If you already have users created, skip past the user creation steps to modify the permissions for the users\.

**To create users who can access AWS Elemental MediaTailor**

1. In the navigation pane of the IAM console, choose **Users**, and then choose **Add user**\.

1. For **User name**, enter the name that the user will use to sign in to MediaTailor\.

1. Select the check box next to **AWS Management Console access**, select **Custom password**, and then enter the new user's password in the box\. You can optionally select **Require password reset** to force the user to create a password the next time the user signs in\.

1. Choose **Next: Permissions**\.

1. On the **Set permissions for user** page, choose **Add user to group**\.

1. Modify the permissions for the users in the group list\. Choose the group with the appropriate attached policy\. Remember that permissions levels are as follows:
   + The group with the **MediaTailorAllAccess** policy allows all actions on all resources in MediaTailor\.
   + The group with the **MediaTailorReadOnlyAccess** policy allows read\-only rights for all resources in MediaTailor\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.