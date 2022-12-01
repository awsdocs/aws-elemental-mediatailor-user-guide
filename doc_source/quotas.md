# Quotas in AWS Elemental MediaTailor<a name="quotas"></a>

MediaTailor resources and operations requests are subject to the following quotas \(formerly referred to as "limits"\)\.

 You can use the AWSService Quotas service to view quotas and request quota increases for MediaTailor, as well as many other AWS services\. For more information, see the [https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html)\.

## Quotas on ad insertion<a name="ad-insertion-quotas"></a>

The following table describes the quotas on AWS Elemental MediaTailor ad insertion\. Unless otherwise noted, the quotas are not adjustable\.


| Name | Default quota value | Description | 
| --- | --- | --- | 
| Ad decision server \(ADS\) length | 25,000  | The maximum number of characters in an ad decision server \(ADS\) specification\.  | 
| Ad decision server \(ADS\) redirects | 5 | The maximum depth of redirects that MediaTailor follows in VAST wrapper tags\. MediaTailor gives up if there are additional redirects\. | 
| Ad decision server \(ADS\) timeout | 3 | The maximum number of seconds that MediaTailor waits before timing out on an open connection to an ad decision server \(ADS\)\. When a connection times out due to no response from the ADS, MediaTailor is unable to fill the ad avail with ads\. | 
| Ad Insertion Requests | 10,000 | The maximum requests per second to make for personalized manifests when performing server side ad insertion\. Ad insertion handles incoming requests for manifests, session initialization, tracking data, and ad segments\. This quota is [adjustable](https://console.aws.amazon.com/servicequotas/home/services/mediatailor/quotas/L-32FBC51C)\. | 
| Configurations | 1,000 | The maximum number of configurations that MediaTailor allows\.  | 
| Content origin length | 512  | The maximum number of characters in a content origin specification\.  | 
| Content origin server timeout | 2 | The maximum number of seconds that MediaTailor waits before timing out on an open connection to the content origin server when requesting template manifests\. Timeouts generate HTTP 504 \(GatewayTimeoutException\) response errors\.  | 
| Manifest size | 2 | The maximum size, in MB, of any origin playback manifest\. To ensure that you stay under the quota, use gzip to compress your input manifests into MediaTailor\. | 
| Prefetch schedules | 25 | The maximum number of active prefetch schedules per playback configuration\. Expired prefetch schedules don't count towards this limit\.  | 
| Server side reporting beacon request timeout | 3 seconds | The maximum number of seconds that MediaTailor waits before timing out on an open connection to the server when firing a beacon for server side reporting\. When a connection times out, MediaTailor is unable to fire the beacon and the service logs an ERROR\_FIRING\_BEACON\_FAILED message to the MediaTailor/AdDecisionServerInteractions log in CloudWatch\. | 
| Session expiration  | 10 times the manifest duration | The maximum amount of time that MediaTailor allows a session to remain inactive before ending the session\. Session activity can be a player request or an advance by the origin server\. When the session expires, MediaTailor returns an HTTP 400 \(Bad Request\) response error\. | 

## Quotas on channel assembly<a name="channel-assembly-quotas"></a>

The following table describes the quotas on AWS Elemental MediaTailor channel assembly\. Unless otherwise noted, the quotas are [adjustable](https://console.aws.amazon.com/servicequotas/home/services/mediatailor/quotas)\.


| Name | Default quota value | Description | 
| --- | --- | --- | 
| Channel Manifest Requests | 50 | The maximum number of manifest requests per second for any single Channel Assembly channel\. This is an account\-level quota\. | 
| Channel outputs | 5 | The maximum number of outputs per channel\. | 
| Channels per account | 100 | The maximum number of channels per account\. | 
| Live Sources | 50 | The maximum number of live sources per source location\. | 
| Manifest Requests | 400 | The maximum number of manifest requests per second across all channels\. This is an account\-level quota\. | 
| Package configurations | 5 | The maximum number of package configurations per source \(whether live or video on demand\)\. | 
| Programs Per Channel | 400 | The maximum number of programs per channel\. | 
| Segment Delivery Configurations Per Source | 5 | The maximum number of segment delivery configurations per source location\. | 
| Source Locations | 100 | The maximum number of source locations per account\. | 
| VOD Sources | 1,000 | The maximum number of video on demand \(VOD\) sources per source location\. | 