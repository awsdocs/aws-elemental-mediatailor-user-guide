# Monitoring channel assembly resources with MediaTailor alerts<a name="channel-assembly-alerts"></a>

MediaTailor creates alerts for issues or potential issues that occur with your channel assembly resources\. The alert describes the issue, when it issue occurred, and the resources that are affected\.\.

You can view the alerts in the AWS Management Console, AWS CLI, AWS SDKs, or programmatically using the MediaTailor [ListAlerts](https://docs.aws.amazon.com/mediatailor/latest/apireference/alerts.html) API\.

**Important**  
Alerts are only available for channel assembly resources created on or after July 14th, 2021\.

## Viewing alerts<a name="channel-assembly-viewing-alerts-procedure"></a>

You can view alerts for any MediaTailor channel assembly resource\. When you view the alerts for channels and programs, MediaTailor includes all of the related resources contained within the channel or program\. For example, when you view the alerts for a specific program, you also see alerts for the source location and VOD sources that the program contains\.

To view alerts, perform the following procedure\.

------
#### [ Console ]

**To view alerts in the console**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. Choose the resource that you want to view alerts for\.

1. Select the **Alerts** tab to view the alerts\.

------
#### [ AWS CLI ]

To list alerts for a channel assembly resource, you need the resource's [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can use the `decribe-resource_type` command in the AWS CLI to get the resource's ARN\. For example, run the [https://docs.aws.amazon.com/cli/latest/reference/mediatailor/describe-channel.html](https://docs.aws.amazon.com/cli/latest/reference/mediatailor/describe-channel.html) command to get a specific channel's ARN:

```
aws mediatailor describe-channel --channel-name MyChannelName
```

Then use the [https://docs.aws.amazon.com/cli/latest/reference/mediatailor/list-alerts.html](https://docs.aws.amazon.com/cli/latest/reference/mediatailor/list-alerts.html) command to list the alerts associated with the resource:

```
aws mediatailor list-alerts --resource-arn arn:aws:mediatailor:region:aws-account-id:resource-type/resource-name
```

------
#### [ API ]

To list alerts for a channel assembly resource, you need the use resource's [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can use the `DescribeResource` operation in the MediaTailor API to get the resource's ARN\. For example, use the `[DescribeChannel](https://docs.aws.amazon.com/mediatailor/latest/apireference/channel-channelname.html#channel-channelname-http-methods)` operation to get a specific channel's ARN\.

Then use the [https://docs.aws.amazon.com/mediatailor/latest/apireference/alerts.html](https://docs.aws.amazon.com/mediatailor/latest/apireference/alerts.html) API to list the alerts for the resource\.

------

## Handling alerts<a name="channel-assembly-handling-alerts"></a>

When an alert occurs, view the alerts in the AWS Management Console, or use the AWS CLI, AWS SDKs, or the MediaTailor Alerts API to determine the possible sources of the issue\.

After you resolve the issue, MediaTailor clears the alert\.