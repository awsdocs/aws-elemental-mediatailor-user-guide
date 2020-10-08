# Ad behavior in AWS Elemental MediaTailor<a name="ad-behavior"></a>

AWS Elemental MediaTailor replaces or inserts ads depending on how the origin server configures the ad breaks, and whether the content is video on demand \(VOD\) or live\.
+ With **ad replacement**, MediaTailor replaces content segments with ads\. 
+ With **ad insertion**, MediaTailor inserts ad content where segments don't exist\.

 AWS Elemental MediaTailor also uses configured slates to fill gaps in ad breaks and to manage VPAID ad handling, as well as optional bumpers\.

**Topics**
+ [VOD content ad behavior](ad-behavior-vod.md)
+ [Live content ad behavior](ad-behavior-live.md)
+ [Bumpers](bumpers.md)
+ [Slate management](slate-management.md)