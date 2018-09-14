# Ad Behavior in AWS Elemental MediaTailor<a name="ad-behavior"></a>

AWS Elemental MediaTailor can perform ad replacement \(replace content segments with ad content\) or ad insertion \(insert ad content where segments don’t currently exist\)\. The ad behavior depends on the type of content \(VOD or live\), and how the origin server configured the ad breaks\. Additionally, AWS Elemental MediaTailor uses configured slates to fill gaps in ads and manage VPAID ad handling\. 

Generally, the ad flow goes like this:

1. The player requests a master manifest from MediaTailor\.

1. MediaTailor requests a VAST \(or VMAP\) response from the ad decision server \(ADS\) and master manifest from the origin server\.

1. MediaTailor stitches ads into the master manifest based on the response from the ADS\.

   The following sections describe the logic that MediaTailor uses when stitching ads into a manifest\.

**Topics**
+ [VOD Content Ad Behavior](ad-behavior-vod.md)
+ [Live Content Ad Behavior](ad-behavior-live.md)
+ [Slate Management](slate-management.md)