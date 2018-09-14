# HLS \.m3u8 Manifests<a name="manifest-hls"></a>

AWS Elemental MediaTailor supports HLS manifests \(\*\.m3u8\) for live streaming and video on demand \(VOD\)\. Ad markers such as SCTE\-IN/OUT and CUE\-IN/OUT indicate ad breaks\. The duration of the ad breaks is determined by the value in the `EXT-X-CUE-OUT` tag or by `EXT-X-DATERANGE Duration`\. When MediaTailor encounters an ad break, it attempts ad insertion or replacement, based on the type of content\. If there aren't enough ads to fill the duration, MediaTailor displays the underlying content stream or the configured slate for the remainder of the ad break\. For more information about HLS ad behavior based on content type \(live or VOD\), see [Ad Behavior in AWS Elemental MediaTailor](ad-behavior.md)\.

When MediaTailor stitches in ads, it first checks to see if the ads returned in the VAST response of the ad decision server \(ADS\) have been transcoded\. If an ad has been transcoded, MediaTailor uses the ad in the ad break\. If it hasn't been transcoded, MediaTailor transcodes it and stores it for future use\. If there are multiple ads in the VAST response, MediaTailor evaluates them sequentially and attempts to fill in subsequent ad creatives if the ads are already transcoded\. If no ads are transcoded yet, MediaTailor plays the underlying content \(or ad slate\) instead of the ad\.

**Topics**
+ [HLS Live Manifest Examples](manifest-hls-example.md)
+ [HLS Manifest Tag Handling](manifest-hls-tags.md)