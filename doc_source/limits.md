# Limits in AWS Elemental MediaTailor<a name="limits"></a>

The following sections provide information about the limits in AWS Elemental MediaTailor\. For information about requesting an increase to soft limits, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\. Hard limits can't be changed\.

## Soft Limits<a name="soft-limits"></a>

The following table describes limits in AWS Elemental MediaTailor that can be increased\. For information about changing limits, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\. 


| Limit | Default Setting | Description | 
| --- | --- | --- | 
| Transactions | 3,000 | The maximum number of concurrent transactions per second across all request types\. This is an account\-level limit\. Your transaction count depends largely on how often players request updated manifests and the number of players\. Each player request counts as a transaction\.  | 

## Hard Limits<a name="hard-limits"></a>

The following table describes limits in AWS Elemental MediaTailor that can't be increased\.


| Limit | Setting | Description | 
| --- | --- | --- | 
| Ad decision server \(ADS\) length | 25,000  | The maximum number of characters in an ad decision server \(ADS\) specification\.  | 
| Ad decision server \(ADS\) redirects | 3 | The maximum depth of redirects that MediaTailor follows in VAST wrapper tags\.  | 
| Ad decision server \(ADS\) timeout | 1\.5  | The maximum number of seconds that MediaTailor waits before timing out on an open connection to an ad decision server \(ADS\)\. When a connection times out, MediaTailor is unable to fill the ad avail with ads due to no response from the ADS\. | 
| Configurations | 500 | The maximum number of configurations that MediaTailor allows\.  | 
| Content origin length | 512  | The maximum number of characters in a content origin specification\.  | 
| Content origin server timeout | 2 | The maximum number of seconds that MediaTailor waits before timing out on an open connection to the content origin server when requesting template manifests\. Timeouts generate HTTP 504 \(GatewayTimeoutException\) response errors\.  | 
| Manifest size | 2 | The maximum size, in MB, of any playback manifest, whether in input or output\. To ensure that you stay under the limit, use gzip to compress your input manifests into MediaTailor\.  | 
| Session expiration  | 10 times the manifest duration  | The maximum amount of time that MediaTailor allows a session to remain inactive before ending the session\. Session activity can be a player request or an advance by the origin server\. When the session expires, MediaTailor returns an HTTP 400 \(Bad Request\) response error\.  | 