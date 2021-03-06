# Quotas in AWS Elemental MediaTailor<a name="quotas"></a>

The following sections provide information about the quotas \(formerly known as *limits*\) in AWS Elemental MediaTailor\. For information about requesting an increase to soft quotas, see [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\. Hard quotas can't be changed\.

## Soft Quotas<a name="soft-quotas"></a>

The following table describes quotas in AWS Elemental MediaTailor that can be increased\. For information about changing quotas, see [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\. 


| Quota | Default setting | Description | 
| --- | --- | --- | 
| Transactions | 10,000 | The maximum number of concurrent transactions per second across all request types\. This is an account\-level quota\. Your transaction count depends largely on how often players request updated manifests and the number of players\. Each player request counts as a transaction\.  | 

## Hard Quotas<a name="hard-quotas"></a>

The following table describes quotas in AWS Elemental MediaTailor that can't be increased\.


| Quota | Setting | Description | 
| --- | --- | --- | 
| Ad decision server \(ADS\) length | 25,000  | The maximum number of characters in an ad decision server \(ADS\) specification\.  | 
| Ad decision server \(ADS\) redirects | 5 | The maximum depth of redirects that MediaTailor follows in VAST wrapper tags\.  | 
| Ad decision server \(ADS\) timeout | 3 | The maximum number of seconds that MediaTailor waits before timing out on an open connection to an ad decision server \(ADS\)\. When a connection times out, MediaTailor is unable to fill the ad avail with ads due to no response from the ADS\. | 
| Channels egress requests | 4,000 | The maximum number of channels egress requests\. | 
| Channels per account | 500 | The maximum number of channels per account\. | 
| Channel outputs | 5 | The maximum number of outputs per channel\. | 
| Configurations | 1,000 | The maximum number of configurations that MediaTailor allows\.  | 
| Content origin length | 512  | The maximum number of characters in a content origin specification\.  | 
| Content origin server timeout | 2 | The maximum number of seconds that MediaTailor waits before timing out on an open connection to the content origin server when requesting template manifests\. Timeouts generate HTTP 504 \(GatewayTimeoutException\) response errors\.  | 
| Manifest size | 2 | The maximum size, in MB, of any playback manifest, whether in input or output\. To ensure that you stay under the quota, use gzip to compress your input manifests into MediaTailor\.  | 
| Package configurations | 5 | The maximum number of package configurations per video on demand \(VOD\) source\. | 
| Programs per channel | 400 | The maximum number of programs per channel\. | 
| Session expiration  | 10 times the manifest duration  | The maximum amount of time that MediaTailor allows a session to remain inactive before ending the session\. Session activity can be a player request or an advance by the origin server\. When the session expires, MediaTailor returns an HTTP 400 \(Bad Request\) response error\.  | 
| Source locations | 100 | The maximum number of source locations per account\. | 
| VOD sources | 1,000 | The maximum number of video on demand \(VOD\) sources per source location\. | 