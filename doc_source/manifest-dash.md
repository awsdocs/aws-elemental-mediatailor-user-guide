# Integrating an MPEG\-DASH source<a name="manifest-dash"></a>

AWS Elemental MediaTailor supports `.mpd` live and video on demand \(VOD\) manifests that follow the guidelines for the DASH dynamic profile\. MediaTailor accepts multi\-period and single\-period DASH\-compliant manifest inputs, and delivers multi\-period DASH\-compliant manifest outputs\. 

Input manifests must have the following:
+ At least one `Period` element with a `start` attribute\. 
+ SCTE\-35 event streams with splice info settings for either `splice insert `or` time signal`\. The settings can be provided in clear XML or in base64\-encoded binary\. 
+ `Segment templates` with `segment timelines`\. 

For published manifests, MediaTailor requires that updates by the origin server leave the following unchanged: 
+ Period start times, specified in the `start` attribute\. 
+ Values of `presentationTimeOffset` in the segment templates of the period representations\. 

As a best practice, give the ad avails the same `AdaptationSet` and `Representation` settings as the content stream periods\. AWS Elemental MediaTailor uses these settings to transcode the ads to match the content stream, for smooth switching between the two\.

The following sections provide more information about how MediaTailor handles DASH manifests\.

**Topics**
+ [DASH ad markers](dash-ad-markers.md)
+ [DASH ad avail duration](dash-ad-avail-duration.md)
+ [DASH manifest segment numbering](dash-manifest-segment-numbering.md)
+ [DASH manifest examples](dash-manifest-examples.md)
+ [DASH location feature](dash-location-feature.md)