# Step 1: Create Policies<a name="setting-up-non-admin-policies"></a>

Create two policies for AWS Elemental MediaTailor: one to provide read/write access, and one to provide read\-only access\. Perform these steps one time only for each policy\.

**To create policies for AWS Elemental MediaTailor**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Use your Administrator user credentials to sign in to the IAM console\.

1. In the navigation pane of the console, choose **Policies**, and then choose **Create policy**\.

1. Choose the **JSON** tab and paste the following policy:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "mediatailor:*",
               "Resource": "*"
           }
       ]
   }
   ```

   This policy allows all actions on all resources in AWS Elemental MediaTailor\.

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, type **MediaTailorAllAccess**, and then choose **Create policy**\.

1. On the **Policies** page, repeat the steps in this section to create a read\-only policy\. Use the following policy and call it **MediaTailorReadOnlyAccess**:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "mediatailor:GetConfig",
                   "mediatailor:GetAllConfigs"
               ],
               "Resource": "*"
           }
       ]
   }
   ```