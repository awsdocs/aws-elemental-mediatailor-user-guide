# Step 2: Create User Groups<a name="setting-up-non-admin-groups"></a>

Create a user group for each of the policies that you created in step 1\. This way, when you create additional users you can add the users to a group rather than attaching individual policies to each user\. 

**To create groups for users who need access to AWS Elemental MediaTailor**

1. In the navigation pane of the IAM console, choose **Groups**, and then choose **Create New Group**\.

1. On the **Set Group Name** page, type a name for the group, such as **MediaTailorAdmins**\. Choose **Next Step**\.

1. On the **Attach Policy** page, for **Filter**, choose **Customer Managed**\.

1. In the policy list, choose the **MediaTailorAllAccess** policy that you created\.

1. On the **Review** page, verify that the correct policy is added to this group, and then choose **Create Group**\.

1. On the **Groups** page, repeat the steps in this section to create a user group that has read\-only permissions\. In step 4, choose **MediaTailorReadOnlyAccess**\.