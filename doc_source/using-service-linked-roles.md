# Using service\-linked roles for MediaTailor<a name="using-service-linked-roles"></a>

AWS Elemental MediaTailor uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to MediaTailor\. Service\-linked roles are predefined by MediaTailor and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up MediaTailor easier because you don’t have to manually add the necessary permissions\. MediaTailor defines the permissions of its service\-linked roles, and unless defined otherwise, only MediaTailor can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your MediaTailor resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for MediaTailor<a name="slr-permissions"></a>

MediaTailor uses the service\-linked role named **AWSServiceRoleForMediaTailor** – MediaTailor uses this service\-linked role to invoke CloudWatch to create and manage log groups, log streams, and log events\. This service\-linked role is attached to the following managed policy: `AWSMediaTailorServiceRolePolicy`\.

The AWSServiceRoleForMediaTailor service\-linked role trusts the following services to assume the role:
+ `mediatailor.amazonaws.com`

The role permissions policy allows MediaTailor to complete the following actions on the specified resources:
+ Action: `logs:PutLogEvents` on `arn:aws:logs:*:*:log-group:/aws/MediaTailor/*:log-stream:*`
+ Action: `logs:CreateLogStream, logs:CreateLogGroup, logs:DescribeLogGroups, logs:DescribeLogStreams` on `arn:aws:logs:*:*:log-group:/aws/MediaTailor/*`

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for MediaTailor<a name="create-slr"></a>

You don't need to manually create a service\-linked role\. When you enable session logging in the AWS Management Console, the AWS CLI, or the AWS API, MediaTailor creates the service\-linked role for you\. 

**Important**  
This service\-linked role can appear in your account if you completed an action in another service that uses the features supported by this role\. Also, if you were using the MediaTailor service before September 15, 2021, when it began supporting service\-linked roles, then MediaTailor created the AWSServiceRoleForMediaTailor role in your account\. To learn more, see [A New Role Appeared in My IAM Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html#troubleshoot_roles_new-role-appeared)\.

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you enable session logging, MediaTailor creates the service\-linked role for you again\. 

You can also use the IAM console to create a service\-linked role with the **MediaTailor** use case\. In the AWS CLI or the AWS API, create a service\-linked role with the `mediatailor.amazonaws.com` service name\. For more information, see [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. If you delete this service\-linked role, you can use this same process to create the role again\.

## Editing a service\-linked role for MediaTailor<a name="edit-slr"></a>

MediaTailor does not allow you to edit the AWSServiceRoleForMediaTailor service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for MediaTailor<a name="delete-slr"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up the resources for your service\-linked role before you can manually delete it\.

**Note**  
If the MediaTailor service is using the role when you try to clean up the resources, then the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

**To clean up MediaTailor resources used by the AWSServiceRoleForMediaTailor**
+ Before you can delete the service\-linked role created by MediaTailor for the log configuration, you must first *deactivate* all of the log configurations in your account\. To deactivate a log configuration, set the **percent enabled** value to **0**\. This turns off all session logging the corresponding playback configuration\. For more information, see [Deactivating a log configuration](log-configuration.md#deactivating-logging-configuration)\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete the AWSServiceRoleForMediaTailor service\-linked role\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported Regions for MediaTailor service\-linked roles<a name="slr-regions"></a>

MediaTailor supports using service\-linked roles in all of the regions where the service is available\. For more information, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/mediatailor.html#mediatailor_region)\.