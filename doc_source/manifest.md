# AWS Elemental MediaTailor Manifest Handling<a name="manifest"></a>

A manifest is the input to AWS Elemental MediaTailor from an upstream encoder\. When MediaTailor receives a request for content playback, it manipulates the manifest and adds personalized content, tailored for the viewing session\. This section describes how MediaTailor handles manifests\. For information about ad handling and insertion, see [Ad Behavior in AWS Elemental MediaTailor](ad-behavior.md)\.

When AWS Elemental MediaTailor stitches in the ads that are specified by the ad decision server \(ADS\) in the VAST response, it checks whether the ads have already been transcoded: 
+ If an ad has been transcoded, MediaTailor uses the ad in the ad avail\. 
+ If it hasn't been transcoded, MediaTailor transcodes it for future use, but doesn't use it for the current request\. 

If there are multiple ads in the VAST response, AWS Elemental MediaTailor evaluates them sequentially and uses the ones that are already transcoded\. If no ads are transcoded yet, MediaTailor plays the underlying content \(or ad slate\) instead of the ad\.