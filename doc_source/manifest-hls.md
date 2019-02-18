# HLS \.m3u8 Manifests<a name="manifest-hls"></a>

AWS Elemental MediaTailor supports `.m3u8` HLS manifests for live streaming and video on demand \(VOD\)\. Ad markers such as `SCTE-IN/OUT` and `CUE-IN/OUT` indicate ad breaks\. The duration of the ad breaks is set in the `EXT-X-CUE-OUT` tag or the `EXT-X-DATERANGE Duration` tag\. When MediaTailor encounters an ad break, it attempts ad insertion or replacement, based on the type of content\. If there aren't enough ads to fill the duration, for the remainder of the ad break, MediaTailor displays the underlying content stream or the configured slate\. For more information about HLS ad behavior based on content type \(live or VOD\), see [Ad Behavior in AWS Elemental MediaTailor](ad-behavior.md)\.

When AWS Elemental MediaTailor stitches in the ads that are specified in the VAST response, it checks if the ads have already been transcoded\. If an ad has been transcoded, MediaTailor uses the ad in the ad break\. If it hasn't been transcoded, MediaTailor transcodes it for future use, but doesn't use it for the current request\. If there are multiple ads in the VAST response, MediaTailor evaluates them sequentially and uses the ones that are already transcoded\. If no ads are transcoded yet, MediaTailor plays the underlying content \(or ad slate\) instead of the ad\.

**Topics**
+ [HLS Live Manifest Examples](manifest-hls-example.md)
+ [HLS Manifest Tag Handling](manifest-hls-tags.md)