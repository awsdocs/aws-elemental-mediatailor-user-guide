# DASH Manifest Requirements<a name="manifest-dash-manifest-requirements"></a>

AWS Elemental MediaTailor requires the following of the manifests that it receives from the origin server: 
+ The manifest must contain at least one `Period` element with a `start` attribute\. 
+ After a manifest is published, subsequent updates to the manifest must leave the following unchanged: 
  + Period start times, specified in the `start` attribute\. 
  + Values of `presentationTimeOffset` in the segment templates of the period representations\. 
+ To mark a period as an ad break, the first `Event` in the period must specify `scte35:SpliceInsert` markings with `outOfNetworkIndicator` set to `true`\. 

As a best practice, give the ad break periods the same `AdaptationSet` and `Representation` markings as the content stream periods\. MediaTailor uses these settings to transcode the ads to match the content stream, for smooth switching between the two\.