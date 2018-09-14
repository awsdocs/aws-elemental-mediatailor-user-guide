# CDN Integration<a name="integrating-cdn"></a>

We highly recommend that you use a content distribution network \(CDN\) such as Amazon CloudFront to improve the efficiency of the ad stitching workflow between AWS Elemental MediaTailor and your users\. The benefits of a CDN include content and ad caching, consistent domain names across personalized manifests, and CDN DNS resolution\.

When you use a CDN in the AWS Elemental MediaTailor workflow, the request and response flow is as follows:

1. The player requests a master manifest from the CDN with MediaTailor as the manifest origin\. Personalized manifest requests are proxied through the CDN, and the CDN forwards the requests to MediaTailor\.

1. MediaTailor personalizes the manifest and substitutes CDN domain names for the content and ad segment URL prefixes\. MediaTailor sends the personalized manifest as a response to the CDN and consequently to the requesting player\.

1. The player requests segments from the URLs that are provided in the master manifest\. 

1. The CDN translates the segment URLs and forwards content segment requests to the origin server and ad requests to the Amazon CloudFront distribution where MediaTailor stores transcoded ads\.

1. The origin server and MediaTailor respond with the requested segments, and playback begins\.

The following sections describe how to configure MediaTailor and the CDN to perform this flow\.

**Topics**
+ [Integrating MediaTailor and a CDN](integrating-cdn-standard.md)