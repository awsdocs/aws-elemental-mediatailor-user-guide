# Step 2: Create Policies<a name="setting-up-non-admin-policies"></a>

Create two policies for AWS Elemental MediaTailor: one to provide read/write access, and one to provide read\-only access\. 

**To create policies for AWS Elemental MediaTailor**

1. In the navigation pane of the IAM console, choose **Policies**\.

1. On the **Policies** page, create a policy named **MediaTailorAllAccess** that allows all actions on all resources in MediaTailor:

   1. Choose **Create policy**\.

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

   1. Choose **Review policy**\.

   1. On the **Review policy** page, for **Name**, enter **MediaTailorAllAccess**, and then choose **Create policy**\.

1. On the **Policies** page, create a read\-only policy named **MediaTailorReadOnlyAccess** for MediaTailor:

   1. Choose **Create policy**\.

   1. Choose the **JSON** tab and paste the following read\-only policy:

      ```
      {
          "Version": "2012-10-17",
          "Statement": [
              {
                  "Effect": "Allow",
                  "Action": [
                      "mediatailor:GetPlaybackConfiguration",
                      "mediatailor:ListPlaybackConfigurations",
                      "mediatailor:ListTagsForResource"
                  ],
                  "Resource": "*"
              }
          ]
      }
      ```

   1. Choose **Review policy**\.

   1. On the **Review policy** page, for **Name**, enter **MediaTailorReadOnlyAccess**, and then choose **Create policy**\.