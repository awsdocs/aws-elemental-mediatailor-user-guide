# Integrating an HLS source<a name="manifest-hls"></a>

AWS Elemental MediaTailor supports `.m3u8` HLS manifests with an `EXT-X-VERSION` of `3` or higher for live streaming and video on demand \(VOD\)\. When MediaTailor encounters an ad break, it attempts ad insertion or replacement, based on the type of content\. If there aren't enough ads to fill the duration, for the remainder of the ad break, MediaTailor displays the underlying content stream or the configured slate\. For more information about HLS ad behavior based on content type, see [Understanding MediaTailor ad insertion behavior](ad-behavior.md)\.

The following sections provide more information about how MediaTailor handles HLS manifests\.

**Topics**
+ [HLS supported ad markers](hls-ad-markers.md)
+ [Ad marker passthrough](ad-marker-passthrough.md)
+ [HLS manifest tag handling](manifest-hls-tags.md)
+ [HLS manifest examples](manifest-hls-example.md)