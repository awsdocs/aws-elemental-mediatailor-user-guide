# HLS Manifest Tag Handling<a name="manifest-hls-tags"></a>

AWS Elemental MediaTailor outputs all unknown and custom tags into the personalized output manifest with the exception of `EXT-X-CUE-` `OUT/IN` tags, whose handling is described below\. 

To work properly, AWS Elemental MediaTailor requires HLS `EXT-X-VERSION` `3` or higher as the input manifest\.


+ [EXT\-X\-CUE Tags](#ext-x-cue-tags)
+ [EXT\-X\-KEY Value](#ext-x-key-value)

## EXT\-X\-CUE Tags<a name="ext-x-cue-tags"></a>

AWS Elemental MediaTailor converts `EXT-X-CUE-OUT`, `EXT-X-CUE-OUT-CONT`, and `EXT-X-CUE-IN` tags from the input manifest to `EXT-X-DISCONTINUITY` tags in the output manifest to identify discrete ad creative boundaries\. AWS Elemental MediaTailor inserts an `EXT-X-DISCONTINUITY` tag at the start and end of every ad, including the following boundaries:

+ Where content transitions to an ad

+ Where one ad transitions to another ad

+ Where an ad transitions back to content

## EXT\-X\-KEY Value<a name="ext-x-key-value"></a>

If the origin server has enabled encryption or digital rights management \(DRM\) on the content stream, the manifest includes `EXT-X-KEY` tags\. Because ads aren't encrypted, AWS Elemental MediaTailor sets the `EXT-X-KEY` tag to `NONE` for ad breaks\. When playback returns to the content stream, AWS Elemental MediaTailor re\-enables the `EXT-X-KEY` tag\.