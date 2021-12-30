# Controlling the volume of ad insertion session logs<a name="log-configuration"></a>

MediaTailor ad insertion session logs are sometimes verbose\. To reduce log costs, you can define the percentage of session logs that MediaTailor sends to Amazon CloudWatch Logs\. For example, if your playback configuration has 1000 ad insertion sessions and you set a percentage enabled value of `60`, MediaTailor sends logs for 600 of the sessions to CloudWatch Logs\. MediaTailor decides at random which of the sessions to send logs for\. If you want to view logs for a specific session, you can use the [debug log mode](debug-log-mode.md)\. 

When you set a logging percentage, MediaTailor automatically creates a service\-linked role that grants MediaTailor the permissions it requires to write CloudWatch Logs to your account\. For information about how MediaTailor uses service\-linked roles, see [Using service\-linked roles for MediaTailor](using-service-linked-roles.md)\.

## Creating a log configuration<a name="creating-log-configuration"></a>

To control the percentage of session logs that MediaTailor writes to CloudWatch Logs, you create a *log configuration* for your playback configuration\. When you create a log configuration, you specify a *playback configuration name*, and a *percent enabled* value\.

------
#### [ Console ]

**To create a log configuration for an *existing* playback configuration**

1. Sign in to the AWS Management Console and open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. On the **Playback configuration** pane, select the playback configuration that you'd like to set the log configuration for\.

1. Choose **Edit**\.

1. Under **Log configuration**, specify a **percent enabled** value\.

**To create a log configuration for a *new* playback configuration**
+ Follow the procedure in [Log configuration](configurations-create.md#configurations-log-configurations)\.

------
#### [ CLI ]

**To create a log configuration for an *existing* playback configuration**

To create a log configuration by using the AWS CLI, run the `[configure\-logs\-for\-playback\-configuration](https://docs.aws.amazon.com/cli/latest/reference/mediatailor/configure-logs-for-playback-configuration.html)` command and specify the appropriate values for the required parameters\.

This example is formatted for Linux, macOS, or Unix, and it uses the backslash \(\\\) line\-continuation character to improve readability\.

```
$ aws mediatailor configure-logs-for-playback-configuration \
--percent-enabled 10 \
--playback-configuration-name MyPlaybackConfiguration
```

This example is formatted for Microsoft Windows, and it uses the caret \(^\) line\-continuation character to improve readability\.

```
C:\> aws mediatailor configure-logs-for-playback-configuration ^
--percent-enabled 10 ^
--playback-configuration-name MyPlaybackConfiguration
```

Where:
+ `percent-enabled` is the percentage of playback configuration session logs that MediaTailor sends to CloudWatch Logs\.
+ `playback-configuration-name` is the name of the playback configuration to set the log configuration settings for\.

If the command runs successfully, you receive output similar to the following\.

```
{
    "PercentEnabled": 10,
    "PlaybackConfigurationName": "MyPlaybackConfiguration"
}
```

**To create a log configuration for a *new* playback configuration**
+ Use the `configure-logs-for-playback-configuration` option for the [https://docs.aws.amazon.com/cli/latest/reference/mediatailor/put-playback-configuration.html](https://docs.aws.amazon.com/cli/latest/reference/mediatailor/put-playback-configuration.html) command\.

------

## Deactivating a log configuration<a name="deactivating-logging-configuration"></a>

After you create a log configuration, you can't delete it—you can only *deactivate* it\. To deactivate the log configuration, set the **percent enabled** value to **0** via the MediaTailor console or API\. This turns off all session logging for that playback configuration\.

If you want to delete the service\-linked role that MediaTailor uses for the log configuration\(s\) in your account, you must first deactivate all of your log configurations\. For information about how to delete the service\-linked role, see [Using service\-linked roles for MediaTailor](using-service-linked-roles.md)\.

------
#### [ Console ]

**To deactivate log configuration on a playback configuration**

1. Sign in to the AWS Management Console and open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. On the **Playback configuration** pane, select the playback configuration that you'd like to deactivate log configuration on\.

1. Choose **Edit**\.

1. Under **Log configuration**, set the **percent enabled** value to `0`\. This turns off all session logging for this playback configuration\.

1. Select **Save**\.

------
#### [ CLI ]

**To deactivate a log configuration**
+ Set the `percent-enabled` value to `0` using the [configure\-logs\-for\-playback\-configuration](https://docs.aws.amazon.com/cli/latest/reference/mediatailor/configure-logs-for-playback-configuration.html) command\.

------