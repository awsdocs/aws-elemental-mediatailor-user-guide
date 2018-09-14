# AWS Elemental MediaTailor Manifest Handling<a name="manifest"></a>

A manifest is the input to AWS Elemental MediaTailor from an upstream encoder\. When MediaTailor receives a request for content playback, it manipulates the manifest and adds personalized content, tailored for each viewing session\. The following sections describe the expected general behaviors of MediaTailor manifest handling\. For information about ad handling and insertion, see [Ad Behavior in AWS Elemental MediaTailor](ad-behavior.md)\.

**Topics**
+ [Alternate Audio and Subtitles](manifest-audio-captions.md)
+ [HLS \.m3u8 Manifests](manifest-hls.md)