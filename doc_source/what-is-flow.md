# How AWS Elemental MediaTailor Works<a name="what-is-flow"></a>

AWS Elemental MediaTailor serves personalized content to viewers while maintaining broadcast quality\-of\-service in over\-the\-top \(OTT\) applications\.

The general MediaTailor processing flow is as follows:

1. A player or content distribution network \(CDN\) such as Amazon CloudFront sends a request for live or video\-on\-demand \(VOD\) HLS content to MediaTailor\. The request includes parameters from the player that include information about the viewer\. Later, the ad decision server \(ADS\) uses these parameters to determine which ads are included in the MediaTailor response to the content request\. The format of the request varies depending on whether you use server\-side or client\-side reporting to track how much of an ad the viewer watches\. 

   For information about how the requests differ between the two reporting methods, see [Ad Tracking Reporting in AWS Elemental MediaTailor](ad-reporting.md)\. For information about configuring the ad targeting parameters, see [Dynamic Ad Variables in AWS Elemental MediaTailor](variables.md)\.

1. MediaTailor pulls the fully formed template manifest from the content origin server \(such as AWS Elemental MediaPackage\)\. This manifest includes ad markers so that MediaTailor knows where to perform an ad insertion or ad replacement\.

1. Additionally, MediaTailor sends a request to the ad decision server \(ADS\), including the player parameters from the content request\. 

1. The ADS provides a VAST or VMAP response that includes the ads to be played back, based on viewer information gathered from the parameters that MediaTailor passed through, and current ad campaigns\. 

1. MediaTailor manipulates the manifest to include the URLs for the appropriate ads from the VAST or VMAP response\. For the logic behind how ads are inserted, see [Ad Behavior in AWS Elemental MediaTailor](ad-behavior.md)\.

1. MediaTailor provides the fully customized manifest to the requesting CDN or player\. 

1. As playback progresses, either MediaTailor or the video player reports how much of an ad is played\. By default, MediaTailor uses server\-side reporting, meaning that the service sends ad viewing reports to the ad tracking URL directly, with no input required from you\. If you require more control, you can instead perform client\-side ad reporting, where MediaTailor proxies the ad tracking URL to the player for it to perform ad tracking activities\. 

1. As the player requests ad segments throughout content playback, if the ad is not already transcoded in a format that matches the video content, MediaTailor transcodes the ad at the time of the ad segment request\. If an ad is not already transcoded, the service doesn't present it for playback at the first request\. 

## Mixed Content Requests<a name="mixed-content-requests"></a>

Content requests are mixed when some requests are sent over HTTPS, while others are sent over HTTP\. Player requests for manifests and ad segments from MediaTailor are always sent over HTTPS\. If the origin server only accepts HTTP requests, playback might fail at the player\. To avoid playback issues, do one of the following:
+ Use an origin server that supports HTTPS requests\.
+ Use a content distribution network \(CDN\) to enforce HTTPS requests\. For more information, see [Using HTTPS in Amazon CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-https.html)\. 

## Manifest Response Latency<a name="latency-note"></a>

A certain amount of latency is normal for MediaTailor responses to manifests\. Latency mainly occurs for these three reasons:
+ Manifest processing latency – time for MediaTailor to look up entries in databases, and to compute and produce manifests\. Latency is usually less than 100 milliseconds\.
+ ADS latency – time it takes for the ADS to respond to the MediaTailor request\. Latency is variable, but MediaTailor times out if the ADS hasn't sent a response in 1\.5 seconds or less\.
+ Origin server latency – time it takes for the origin server to respond to the MediaTailor request\. Latency is variable, but MediaTailor times out if the origin server hasn't sent a response in 2 seconds or less\.