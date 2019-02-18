# DASH \.mpd Manifests<a name="manifest-dash"></a>

AWS Elemental MediaTailor supports `.mpd` DASH manifests for live streaming as follows: 
+ Supports multi\-period DASH\-compliant manifests, including those from MediaPackage\.
+ Supports manifests for live streaming only\. MediaTailor doesn't currently support VOD\.
+ Follows the guidelines for DASH dynamic profile\.
+ Requires at least one `Period` element with a `start` attribute\. 
+ Supports SCTE\-35 event streams with splice info settings for either splice insert or time signal\. The settings can be provided in clear XML or in base64\-encoded binary\. 
+ Supports segment templates that have segment timelines\. 
+ For published manifests, requires that updates by the origin server leave the following unchanged: 
  + Period start times, specified in the `start` attribute\. 
  + Values of `presentationTimeOffset` in the segment templates of the period representations\. 

As a best practice, give the ad break periods the same `AdaptationSet` and `Representation` settings as the content stream periods\. MediaTailor uses these settings to transcode the ads to match the content stream, for smooth switching between the two\.

**Topics**
+ [DASH Ad Markers](dash-ad-markers.md)
+ [DASH Ad Avail Duration](dash-ad-avail-duration.md)
+ [DASH Manifest Examples](dash-manifest-examples.md)
+ [DASH Location Feature](dash-location-feature.md)