# HLS Manifest Tag Handling<a name="manifest-hls-tags"></a>

This section describes how AWS Elemental MediaTailor manages tags in the personalized output manifest\. MediaTailor outputs all unknown and custom tags into the personalized output manifest except for the ad marker tags listed in [HLS Ad Markers](hls-ad-markers.md)\. 

AWS Elemental MediaTailor requires HLS `EXT-X-VERSION` `3` or later as the input manifest\.

**EXT\-X\-CUE Tags**  
To identify ad creative boundaries, AWS Elemental MediaTailor converts `EXT-X-CUE-OUT`, `EXT-X-CUE-OUT-CONT`, and `EXT-X-CUE-IN` tags from the input manifest to `EXT-X-DISCONTINUITY` tags in the output manifest\. MediaTailor inserts an `EXT-X-DISCONTINUITY` tag at the start and end of every ad, including the following boundaries:
+ Where content transitions to an ad
+ Where one ad transitions to another ad
+ Where an ad transitions back to content

**EXT\-X\-KEY Value**  
When the origin server enables encryption or digital rights management \(DRM\) on the content stream, the manifest includes `EXT-X-KEY` tags\. Ads aren't encrypted, so AWS Elemental MediaTailor sets the `EXT-X-KEY` tag to `NONE` for ad breaks\. When playback returns to the content stream, MediaTailor re\-enables the `EXT-X-KEY` tag\.