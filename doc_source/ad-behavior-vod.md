# VOD Content Ad Behavior<a name="ad-behavior-vod"></a>

AWS Elemental MediaTailor inserts or replaces ads in VOD streams, based on how the origin server configured the CUE\-OUT/CUE\-IN \(or SCTE\-OUT/SCTE\-IN\) markers in the master manifest, or whether the ad decision server \(ADS\) sends VMAP responses\.

For ad behavior by marker configuration, see the following sections\.

**Topics**
+ [No XX\-OUT/XX\-IN Markers](#no-markers)
+ [XX\-OUT/XX\-IN Markers Are Present](#markers-present)

## No XX\-OUT/XX\-IN Markers<a name="no-markers"></a>

Although CUE\-OUT/IN \(or SCTE\-OUT/IN\) markers are the preferred way of signaling ad breaks in a live manifest, the markers are not required for VOD content\. If the manifest doesn't contain ad markers, MediaTailor makes a single call to the ad decision server \(ADS\) and creates ad breaks based on the response:
+ If the ADS sends a VAST response, then MediaTailor inserts all ads from the response in an ad break at the start of the manifest\. This is a pre\-roll\.
+ If the ADS sends a VMAP response, then MediaTailor uses the ad break time offsets to create breaks and insert them throughout the manifest at the specified times \(pre\-roll, mid\-roll, or post\-roll\)\. MediaTailor uses all ads from each ad break in the VMAP response for each ad break in the manifest\.
**Tip**  
If you want to create mid\-roll breaks but your ADS doesn't support VMAP, then ensure that there are CUE\-OUT \(or SCTE\-OUT\) markers in the manifest\. MediaTailor inserts ads at the markers, as described in the following sections\.

## XX\-OUT/XX\-IN Markers Are Present<a name="markers-present"></a>

CUE\-OUT/IN \(or SCTE\-OUT/IN\) markers allow MediaTailor to insert ads throughout the manifest\. If the manifest contains markers, and the CUE\-IN marker immediately follows the CUE\-OUT marker \(there are no segments between them\), this informs MediaTailor that it is an ad insertion request\.

The CUE\-OUT markers should have no duration \(or a duration of 0\) specified, such as `#EXT-X-CUE-OUT:0`\.

For post\-rolls, CUE\-OUT/IN markers must precede the last content segment\. This is because the HLS spec requires tag decorators to be explicitly declared before a segment\. 

For example, for the following declaration: 

```
#EXT-X-CUE-OUT: 0
#EXT-X-CUE-IN
#EXTINF:4.000,
Videocontent.ts
#EXT-X-ENDLIST
```

MediaTailor inserts a post\-roll like the following:

```
#EXTINF:4.000,
Videocontent.ts
#EXT-X-DISCONTINUITY
#EXTINF:3.0,
Adsegment1.ts
#EXTINF:3.0,
Adsegment2.ts 
#EXTINF:1.0,
Adsegment3.ts
#EXT-X-ENDLIST
```

You cannot use multiple CUE\-OUT/IN tags in succession to mimic ad pod behavior\. This is because CUE\-OUT/IN tags must be explicitly attached to a segment\. 

For example, the following declaration is invalid:

```
#EXT-X-CUE-OUT: 0
#EXT-X-CUE-IN
#EXT-X-CUE-OUT: 0
#EXT-X-CUE-IN
#EXT-X-CUE-OUT: 0
#EXT-X-CUE-IN
#EXTINF:4.000,
Videocontent.ts
```

The following declaration is valid:

```
#EXT-X-CUE-OUT: 0
#EXT-X-CUE-IN
#EXTINF:4.000,
Somecontent1.ts
#EXT-X-CUE-OUT: 0
#EXT-X-CUE-IN
#EXTINF:4.000,
Somecontent2.ts
#EXT-X-CUE-OUT: 0
#EXT-X-CUE-IN
#EXTINF:4.000,
Videocontent.ts
```

The above declaration results in an output like the following: 

```
Ad 1
Somecontent.ts
Ad 2
Somecontent2.ts
Videocontent.ts
Post-Roll Ad 3
```