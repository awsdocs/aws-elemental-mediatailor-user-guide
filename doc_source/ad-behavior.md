# Ad Behavior in AWS Elemental MediaTailor<a name="ad-behavior"></a>

AWS Elemental MediaTailor can perform ad replacement \(replace content segments with ad content\) or ad insertion \(insert ad content where segments don’t currently exist\)\. The ad behavior depends on the type of content \(VOD or live\), and how the origin server configured the ad breaks\. Generally, the flow goes like this:

1. The player requests a master manifest from AWS Elemental MediaTailor\.

1. AWS Elemental MediaTailor requests a VAST \(or VMAP\) response from the ad decision server \(ADS\) and master manifest from the origin server\.

1. AWS Elemental MediaTailor stitches ads into the master manifest based on the response from the ADS\.

   The following sections describe the logic that AWS Elemental MediaTailor uses when stitching ads into a manifest\.


+ [VOD Content Ad Behavior](ad-behavior-vod.md)
+ [Live Content Ad Behavior](ad-behavior-live.md)