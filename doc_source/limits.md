# Limits in AWS Elemental MediaTailor<a name="limits"></a>

The following sections provide information about the limits in AWS Elemental MediaTailor\. For information about requesting an increase to soft limits, see [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\. Hard limits cannot be changed\.

## Soft Limits<a name="soft-limits"></a>

The following table describes limits in AWS Elemental MediaTailor that can be increased\. For information about changing limits, see [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\. 


| Resource or Operation | Default Limit | 
| --- | --- | 
| Transactions | 3,000 concurrent transactions per second across all request types \(such as manifest requests and tracking requests for client\-side reporting\)\. This is an account\-level limit\.Your transactions per second are largely dependent on how often the player requests updated manifests\. For example, a player with eight second segments might update the manifest every eight seconds\. The player, then, generates 0\.125 transactions per second\.To request a limit increase, create a case with [AWS Support](https://console.aws.amazon.com/support/home)\.  | 

## Hard Limits<a name="hard-limits"></a>

The following table describes limits within AWS Elemental MediaTailor that can't be increased\.


| Resource or Operation | Default Limit | 
| --- | --- | 
| Configurations | 50 | 
| Characters per field | Content origin: 512**Ad decision server**: 25,000 | 
| Ad decision server \(ADS\) timeout | AWS Elemental MediaTailor waits for 1\.5 seconds before timing out on an open connection to an ad server\. When a connection times out, AWS Elemental MediaTailor is unable to fill the ad break with ads due to no response from the ADS\. | 
| Origin server timeout | AWS Elemental MediaTailor waits for two seconds before timing out on an open connection to the origin server when requesting template manifests\. Timeouts generate HTTP 504 \(Gateway Time\-out\) response errors\.  | 
| ADS redirect | AWS Elemental MediaTailor follows a maximum of three redirects in VAST wrapper tags\.  | 
| Sessions becoming stale | Sessions expire after 10 times the manifest duration if there are no requests during that timeframe, or if the origin server does not advance in that timeframe\. For example, if a manifest has one minute's worth of segments, the player must make a request or the origin server must advance within 10 minutes\. Otherwise, AWS Elemental MediaTailor starts returning HTTP 400 \(Bad Request\) response errors \(bad request for expired sessions\)\. | 
| Manifest size | The size of any playback manifest, input or output, is limited to a maximum of 1 MB\. Please gzip your input manifest into AWS Elemental MediaTailor to ensure that you stay under this limit\.  | 