# CDN Integration with AWS Elemental MediaTailor<a name="integrating-cdn"></a>

We highly recommend that you use a content distribution network \(CDN\) such as Amazon CloudFront to improve the efficiency of the ad stitching workflow between AWS Elemental MediaTailor and your users\. The benefits of a CDN include content and ad caching, consistent domain names across personalized manifests, and CDN DNS resolution\.

When you use a CDN in the AWS Elemental MediaTailor workflow, the request and response flow is as follows:

1. The player requests a manifest from the CDN with MediaTailor as the manifest origin\. The CDN forwards the request to MediaTailor\.

1. MediaTailor personalizes the manifest and substitutes CDN domain names for the content and ad segment URL prefixes\. MediaTailor sends the personalized manifest as a response to the CDN, which forwards it to the requesting player\.

1. The player requests segments from the URLs that are provided in the manifest\. 

1. The CDN translates the segment URLs\. It forwards content segment requests to the origin server and forwards ad requests to the Amazon CloudFront distribution where MediaTailor stores transcoded ads\.

1. The origin server and MediaTailor respond with the requested segments, and playback begins\.

The following sections describe how to configure AWS Elemental MediaTailor and the CDN to perform this flow\.

## How AWS Elemental MediaTailor Handles BaseURLs for DASH<a name="baseurls-for-dash"></a>

With server\-side ad insertion, the content segments and ad segments come from different locations\. In your DASH manifests, AWS Elemental MediaTailor manages URL settings based on your CDN configuration and the URLs specified in the manifest\. MediaTailor uses the rules in the following list to manage the `BaseURL` settings in your DASH manifests for your content segments and ad segments\. 

AWS Elemental MediaTailor behavior for content segments:
+ If you specify a **CDN content segment prefix** in your configuration, then MediaTailor makes sure that there is exactly one `BaseURL`, with your specified prefix, defined at the `MPD` level\.
+ If you do not specify a **CDN content segment prefix**, then MediaTailor uses the origin template manifest as follows: 
  + If the origin template manifest contains one or more `BaseURL` settings at the `MPD` level, MediaTailor leaves them unmodified\.
  + If the origin template manifest does not contain any `BaseURL` settings at the `MPD` level, MediaTailor adds one that is based on the origin `MPD` URL\. 

For ad segments, AWS Elemental MediaTailor does the following:
+ If you specify a **CDN ad segment prefix** in your configuration, then MediaTailor ensures that each ad period has exactly one `BaseURL` setting, populated with the configured prefix\. 
+ If you do not specify a **CDN ad segment prefix**, then MediaTailor adds exactly one `BaseURL` setting to each ad period that points to the ad content server that is set up by MediaTailor for serving ad segments\.