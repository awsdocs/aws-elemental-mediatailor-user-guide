# Monitoring AWS Elemental MediaTailor with Amazon CloudWatch<a name="monitoring-cloudwatch"></a>

You can monitor MediaTailor using CloudWatch, which collects raw data and processes it into readable, near real\-time metrics\. These statistics are kept for 15 months, so that you can access historical information and gain a better perspective on how your web application or service is performing\. You can also set alarms that watch for certain thresholds, and send notifications or take actions when those thresholds are met\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

**To view metrics using the CloudWatch console**  
Metrics are grouped first by the service namespace, and then by the various dimension combinations within each namespace\.

1. Open the CloudWatch console at [https://console.aws.amazon.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Under **All metrics**, choose the **MediaTailor** namespace\. 

1. Select the metric dimension to view the metrics \(for example, originID\)\.

**To view metrics using the AWS CLI**  
At a command prompt, use the following command\.

```
aws cloudwatch list-metrics --namespace "AWS/MediaTailor"
```

## AWS Elemental MediaTailor CloudWatch Metrics<a name="metrics"></a>

The AWS Elemental MediaTailor namespace includes the following metrics\. These metrics are published by default to your account\.


| Metric | Description | 
| --- | --- | 
| AdDecisionServer\.Ads |  The count of ads included in ad decision server \(ADS\) responses for the time period that you specified\. | 
| `AdDecisionServer.Duration` | The total duration, in milliseconds, of all ads that MediaTailor received from the ad decision server \(ADS\) in the time period that you specified\. | 
| AdDecisionServer\.Errors |  The number of non\-HTTP 200 status code responses, empty responses, and timed\-out responses that MediaTailor received from the ADS in the time period that you specified\. | 
| AdDecisionServer\.FillRate | The simple average of the rates at which the responses from the ad decision server \(ADS\) fill the time available for the corresponding ad breaks\. Each rate is defined as \(AdDecisionServer\.Duration\)/\(Avails\.Duration\)\. This number can be higher than 100%\.For information about the simple average, see [Simple Average Explanation](#metrics-simple-average)\.For example, for a single ad break, if the ADS returns 120 seconds of ads and the ad break is 180 seconds, then the `AdDecisionServer.FillRate` for this single ad break is 67% \(120/180\)\. The best `Avails.FillRate` that MediaTailor can attain for this ad break is 67%\.If your ADS returns 120 seconds of ads and the ad break is 90 seconds, then the `AdDecisionServer.FillRate` is 133% \(120/90\)\. The best `Avails.FillRate` that MediaTailor can attain for this ad break is therefore 100%\. | 
| AdDecisionServer\.Timeouts |  The number of timed\-out requests to the ADS in the time period that you specified\.  | 
| AdNotReady |  The number of times that the ADS pointed at an ad that wasn't yet transcoded by the internal transcoder service\. A high value for this metric might contribute to a low overall `Avails.FillRate`\.  | 
| Avails\.Duration | The total duration, in milliseconds, of all ad breaks that MediaTailor encountered in the time period that you specified\.  | 
| Avails\.FilledDuration | The total duration, in milliseconds, of all ad breaks that MediaTailor filled in the time period that you specified\.This metric is calculated as \(number of concurrent sessions\) x \(ad break duration filled\)\. | 
| Avails\.FillRate |  The simple average of the rate at which MediaTailor filled ad breaks\. The rate is defined as \(`Avails.FilledDuration`\) / \(`Avails.Duration`\)\. For information about the simple average, see [Simple Average Explanation](#metrics-simple-average)\. For example, if your ADS returns 90 seconds of ads and the ad break is 120 seconds, then the `AdDecisionServer.FillRate` for this single ad is 75% \(90/120\)\.  If the `Avails.FillRate` is low, look at the `AdDecisionServer.FillRate` compared to the `Avails.FillRate`\. If the `AdDecisionServer.FillRate` is low \(50% or lower\) and `Avails.FillRate` is also low, your ADS might be returning only enough ads for half of a typical break duration\. The maximum `Avails.FillRate` that MediaTailor can attain is bounded by the `AdDecisionServer.FillRate`\.  | 
| GetManifest\.Errors | The number of errors received when MediaTailor is generating manifests\. | 
| `Origin.Errors` |  The number of non\-HTTP 200 status code responses and timed\-out responses that MediaTailor received from the origin server in the time period that you specified\. | 
| Origin\.Timeouts |  The number of timed\-out requests to the origin server in the time period that you specified\.  | 

### Simple Average Explanation<a name="metrics-simple-average"></a>

Simple average means that the durations aren't weighted in the `FillRate` calculation\. The `FillRate` for each ad break is simply averaged together\. This gives an overall view of how successful ad insertions are across all of your ad breaks, independent of how long each ad break is\. To get a weighted average, calculate the sum of your **total duration** and divide by the **total filled duration** in a time period\. 

**Example**

You have the following two ad breaks:
+ Ad break A: 90 seconds total duration, 45 seconds filled \(50% filled\)
+ Ad break B: 120 seconds total duration, 120 seconds filled \(100% filled\)

The `FillRate` metric reported is 75% \( \[50% \+ 100%\] / 2\)\.

The actual weighted average `FillRate` is 79% \( \[45 seconds filled \+ 120 seconds filled\] / \[90 total seconds \+ 120 total seconds\] \)\.

## AWS Elemental MediaTailor CloudWatch Dimensions<a name="dimensions"></a>

You can filter the MediaTailor data using the following dimensions\.


| Dimension | Description | 
| --- | --- | 
| `Configuration Name` | Indicates the configuration that the metric belongs to\. | 