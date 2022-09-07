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

## Creating an admin IAM user<a name="setting-up-create-admin-user"></a>

  When you create an AWS account, you begin with one sign\-in identity that has complete access to all AWS services and resources in the account\. This identity is called the AWS account *root user* and is accessed by signing in with the email address and password that you used to create the account\. We strongly recommend that you do not use the root user for your everyday tasks\. Safeguard your root user credentials and use them to perform the tasks that only the root user can perform\. For the complete list of tasks that require you to sign in as the root user, see [Tasks that require root user credentials](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html#aws_tasks-that-require-root) in the *AWS General Reference*\. 

In the following procedure, you use the AWS account root user to create your first IAM user\. You then add this IAM user to an Administrators group, to ensure that you have access to all services and their resources in your account\. The next time that you access your AWS account, sign in with the credentials for this IAM user\.

To create users with limited permissions, see [Creating a non\-admin IAM user](#setting-up-create-non-admin-user)\.

**To create an administrator user for yourself and add the user to an administrators group \(console\)**

1. Sign in to the [IAM console](https://console.aws.amazon.com/iam/) as the account owner by choosing **Root user** and entering your AWS account email address\. On the next page, enter your password\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user that follows and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane, choose **Users** and then choose **Add users**\.

1. For **User name**, enter **Administrator**\.

1. Select the check box next to **AWS Management Console access**\. Then select **Custom password**, and then enter your new password in the text box\.

1. \(Optional\) By default, AWS requires the new user to create a new password when first signing in\. You can clear the check box next to **User must create a new password at next sign\-in** to allow the new user to reset their password after they sign in\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions**, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** enter **Administrators**\.

1. Choose **Filter policies**, and then select **AWS managed \- job function** to filter the table contents\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.
**Note**  
You must activate IAM user and role access to Billing before you can use the `AdministratorAccess` permissions to access the AWS Billing and Cost Management console\. To do this, follow the instructions in [step 1 of the tutorial about delegating access to the billing console](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_billing.html)\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add metadata to the user by attaching tags as key\-value pairs\. For more information about using tags in IAM, see [Tagging IAM entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users and to give your users access to your AWS account resources\. To learn about using policies that restrict user permissions to specific AWS resources, see [Access management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

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