# VOD Content Ad Behavior<a name="ad-behavior-vod"></a>

AWS Elemental MediaTailor inserts or replaces ads in VOD streams, based on how the origin server configured the CUE\-OUT/CUE\-IN \(or SCTE\-OUT/SCTE\-IN\) markers in the master manifest, or whether the ad decision server \(ADS\) sends VMAP responses\.

For ad behavior by marker configuration, see the following sections\.


+ [No XX\-OUT/XX\-IN Markers](#no-markers)
+ [XX\-OUT/XX\-IN Markers Are Present](#markers-present)

## No XX\-OUT/XX\-IN Markers<a name="no-markers"></a>

Although CUE\-OUT/IN \(or SCTE\-OUT/IN\) markers are the preferred way of signaling ad breaks in a live manifest, the markers are not required for VOD content\. If the manifest doesn't contain ad markers, AWS Elemental MediaTailor makes a single call to the ad decision server \(ADS\) and creates ad breaks based on the response:

+ If the ADS sends a VAST response, then AWS Elemental MediaTailor inserts all ads from the response in an ad break at the start of the manifest\. This is a pre\-roll\.

+ If the ADS sends a VMAP response, then AWS Elemental MediaTailor uses the ad break time offsets to create breaks and insert them throughout the manifest at the specified times \(pre\-roll, mid\-roll, or post\-roll\)\. AWS Elemental MediaTailor uses all ads from each ad break in the VMAP response for each ad break in the manifest\.
**Tip**  
If you want to create mid\-roll breaks but your ADS doesn't support VMAP, then ensure that there are CUE\-OUT \(or SCTE\-OUT\) markers in the manifest\. AWS Elemental MediaTailor inserts ads at the markers, as described in the following sections\.

## XX\-OUT/XX\-IN Markers Are Present<a name="markers-present"></a>

CUE\-OUT/IN \(or SCTE\-OUT/IN\) markers allow AWS Elemental MediaTailor to insert ads throughout the manifest\. If the manifest contains markers, and the CUE\-IN marker immediately follows the CUE\-OUT marker \(there are no segments between them\), this informs AWS Elemental MediaTailor that it is an ad insertion request\.

The CUE\-OUT markers should have no duration \(or a duration of 0\) specified, such as `#EXT-X-CUE-OUT:0`\.