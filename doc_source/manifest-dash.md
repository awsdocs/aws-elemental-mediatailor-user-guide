# DASH \.mpd Manifests<a name="manifest-dash"></a>

AWS Elemental MediaTailor supports `.mpd` DASH manifests for live streaming as follows: 
+ Inputs multi\-period DASH\-compliant manifests, including those from AWS Elemental MediaPackage\. 
+ Inputs single\-period DASH\-compliant manifests, for example, those from the Unified Streaming Platform \(USP\)\. 
+ Outputs multi\-period DASH\-compliant manifests\. 
+ Supports manifests for live streaming only\. MediaTailor doesn't currently support VOD\.
+ Follows the guidelines for DASH dynamic profile\.
+ Requires at least one `Period` element with a `start` attribute\. 
+ Supports SCTE\-35 event streams with splice info settings for either splice insert or time signal\. The settings can be provided in clear XML or in base64\-encoded binary\. 
+ Supports segment templates that have segment timelines\. 
+ For published manifests, requires that updates by the origin server leave the following unchanged: 
  + Period start times, specified in the `start` attribute\. 
  + Values of `presentationTimeOffset` in the segment templates of the period representations\. 

As a best practice, give the ad avails the same `AdaptationSet` and `Representation` settings as the content stream periods\. AWS Elemental MediaTailor uses these settings to transcode the ads to match the content stream, for smooth switching between the two\.

**Topics**
+ [DASH Ad Markers](dash-ad-markers.md)
+ [DASH Ad Avail Duration](dash-ad-avail-duration.md)
+ [DASH Manifest Segment Numbering](dash-manifest-segment-numbering.md)
+ [DASH Manifest Examples](dash-manifest-examples.md)
+ [DASH Location Feature](dash-location-feature.md)