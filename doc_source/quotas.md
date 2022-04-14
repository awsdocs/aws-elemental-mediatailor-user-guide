# Quotas in AWS Elemental MediaTailor<a name="quotas"></a>

MediaTailor resources and operations requests are subject to the following quotas \(formerly referred to as "limits"\)\.

 You can use the AWSService Quotas service to view quotas and request quota increases for MediaTailor, as well as many other AWS services\. For more information, see the [https://docs.aws.amazon.com/servicequotas/latest/userguide//latest/userguide/intro.html](https://docs.aws.amazon.com/servicequotas/latest/userguide//latest/userguide/intro.html)\.

## Quotas on ad insertion<a name="ad-insertion-quotas"></a>

The following table describes the quotas on AWS Elemental MediaTailor ad insertion\.


| Name | Default quota value | Description | Adjustable | 
| --- | --- | --- | --- | 
| Ad decision server \(ADS\) length | 25,000  | The maximum number of characters in an ad decision server \(ADS\) specification\.  | No | 
| Ad decision server \(ADS\) redirects | 5 | The maximum depth of redirects that MediaTailor follows in VAST wrapper tags\.  | No | 
| Ad decision server \(ADS\) timeout | 3 | The maximum number of seconds that MediaTailor waits before timing out on an open connection to an ad decision server \(ADS\)\. When a connection times out, MediaTailor is unable to fill the ad avail with ads due to no response from the ADS\. | No | 
| Configurations | 1,000 | The maximum number of configurations that MediaTailor allows\.  | No | 
| Content origin length | 512  | The maximum number of characters in a content origin specification\.  | No | 
| Content origin server timeout | 2 | The maximum number of seconds that MediaTailor waits before timing out on an open connection to the content origin server when requesting template manifests\. Timeouts generate HTTP 504 \(GatewayTimeoutException\) response errors\.  | No | 
| Manifest size | 2 | The maximum size, in MB, of any playback manifest, whether in input or output\. To ensure that you stay under the quota, use gzip to compress your input manifests into MediaTailor\.  | No | 
| Prefetch schedules | 25 | The maximum number of active prefetch schedules per playback configuration\. Expired prefetch schedules don't count towards this limit\.  | No | 
| Session expiration  | 10 times the manifest duration  | The maximum amount of time that MediaTailor allows a session to remain inactive before ending the session\. Session activity can be a player request or an advance by the origin server\. When the session expires, MediaTailor returns an HTTP 400 \(Bad Request\) response error\.  | No | 
| Server side reporting beacon request timeout | 3 seconds | The maximum number of seconds that MediaTailor waits before timing out on an open connection to the server when firing a beacon for server side reporting\. When a connection times out, MediaTailor is unable to fire the beacon and the service logs an ERROR\_FIRING\_BEACON\_FAILED message to the MediaTailor/AdDecisionServerInteractions log in CloudWatch\. | No | 
| Transactions | 10,000 | The maximum number of concurrent transactions per second across all request types\. This is an account\-level quota\. Your transaction count depends largely on how often players request updated manifests and the number of players\. Each player request counts as a transaction\.  | [Yes](https://console.aws.amazon.com/servicequotas/home/services/mediatailor/quotas/L-32FBC51C) | 

## Quotas on channel assembly<a name="channel-assembly-quotas"></a>

The following table describes the quotas on AWS Elemental MediaTailor channel assembly\.


| Name | Default quota value | Description | Adjustable | 
| --- | --- | --- | --- | 
| Channels egress requests | 4,000 | The maximum number of channels egress requests\. | No | 
| Channels per account | 100 | The maximum number of channels per account\. | Yes | 
| Channel outputs | 5 | The maximum number of outputs per channel\. | No | 
| Package configurations | 5 | The maximum number of package configurations per video on demand \(VOD\) source\. | No | 
| Programs per channel | 400 | The maximum number of programs per channel\. | Yes | 
| Source locations | 100 | The maximum number of source locations per account\. | No | 
| VOD sources | 1,000 | The maximum number of video on demand \(VOD\) sources per source location\. | Yes | 