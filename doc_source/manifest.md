# AWS Elemental MediaTailor manifest handling<a name="manifest"></a>

A manifest is the input to AWS Elemental MediaTailor from an upstream encoder\. When MediaTailor receives a request for content playback, it manipulates the manifest and adds personalized content, tailored for the viewing session\. This section describes how MediaTailor handles manifests\. For information about ad handling and insertion, see [Ad behavior in AWS Elemental MediaTailor](ad-behavior.md)\.

**Topics**
+ [Ad transcoding](manifest-transcoding.md)
+ [Alternate audio and subtitles](manifest-audio-captions.md)
+ [HLS \.m3u8 manifests](manifest-hls.md)
+ [DASH \.mpd manifests](manifest-dash.md)