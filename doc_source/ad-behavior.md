# Understanding MediaTailor ad insertion behavior<a name="ad-behavior"></a>

AWS Elemental MediaTailor stiches ads into live or video on demand \(VOD\) content by either replacing or inserting ads into the origin manifest\. Whether ads are inserted or replaced depends on how the ad breaks are configured in the origin manifest, and whether the content is VOD or live\.
+ With **ad replacement**, MediaTailor replaces content segments with ads\. 
+ With **ad insertion**, MediaTailor inserts ad content where segments don't exist\.

For information about how MediaTailor stitches ads into live and VOD content, select the applicable topic\.

**Topics**
+ [Ad stitching behavior for VOD](#ad-behavior-vod)
+ [Live ad stitching behavior](#ad-behavior-live)

## Ad stitching behavior for VOD<a name="ad-behavior-vod"></a>

MediaTailor inserts or replaces ads in VOD content based on how ad markers are configured in the origin manifest, and if the ad decision server \(ADS\) sends VMAP responses\.

For ad behavior by marker configuration, see the following sections\.

### If ad markers are present<a name="markers-present"></a>

AWS Elemental MediaTailor inserts ads where SCTE\-35 ad markers are present in the origin manifest\. Ad markers with an `EXT-X-CUE-OUT` value of `0` duration indicate ad insertion\. 

#### HLS ad marker guidelines<a name="markers-present-hls"></a>

 Follow these guidelines for post\-roll and ad pod SCTE signaling: 

##### Pre\-roll ads<a name="post-roll-ad-markers"></a>

For HLS post\-rolls, `CUE-OUT/IN` markers must precede the last content segment\. This is because the HLS spec requires tag decorators to be explicitly declared before a segment\. 

For example, consider the following declaration\. 

```
#EXT-X-CUE-OUT: 0
#EXT-X-CUE-IN
#EXTINF:4.000,
Videocontent.ts
#EXT-X-ENDLIST
```

AWS Elemental MediaTailor inserts a post\-roll like the following\.

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

**Example 2: Ad pods**  
 `CUE-OUT/IN` tags must be explicitly attached to a segment\. You can't use multiple `CUE-OUT/IN` tags in succession to mimic ad pod behavior\.  
For example, the following declaration is a valid use of `CUE-OUT/IN` to portray an ad pod\.  

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
The preceding declaration results in an output like the following\.   

```
Ad 1
Somecontent.ts
Ad 2
Somecontent2.ts
Videocontent.ts
Post-Roll Ad 3
```
The following declaration is invalid\.  

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

### If no ad markers are present<a name="no-markers"></a>

Ad markers are the recommended way of signaling ad breaks in a live manifest\. However, ad markers aren't required\. If the manifest doesn't contain ad markers, MediaTailor makes a single call to the ADS and creates ad breaks based on the response:
+ If the ADS sends a VAST response, then MediaTailor inserts all ads from the response in an ad break at the start of the manifest\. This is a pre\-roll\.
+ If the ADS sends a VMAP response, then MediaTailor uses the ad break time offsets to create breaks and insert them throughout the manifest at the specified times \(pre\-roll, mid\-roll, or post\-roll\)\. MediaTailor uses all ads from each ad break in the VMAP response for each ad break in the manifest\. 
**Note**  
When a segment overlaps an insertion point with VMAP for VOD content, MediaTailor rounds down to the nearest insertion point\. 
**Tip**  
If you want to create mid\-roll ad breaks but your ADS doesn't support VMAP, then ensure that there are ad markers in the manifest\. MediaTailor inserts ads at the markers, as described in the following sections\.

## Live ad stitching behavior<a name="ad-behavior-live"></a>

In live streams, AWS Elemental MediaTailor always performs ad replacement, preserving the total time between the ad markers as closely as possible\. When ad markers include the `DURATION` attribute, MediaTailor uses the value to determine the duration of the ad break\. Every `CUE-OUT` indicator must have a matching `CUE-IN` indicator in live workflows\. 

MediaTailor performs ad replacement for HLS and DASH live content\. For information on how MediaTailor calculates ad break placement and timing, see [HLS supported ad markers](hls-ad-markers.md) and [DASH ad markers](dash-ad-markers.md)\. 

### Ad selection and replacement<a name="ad-behavior-live-ad-selection"></a>

AWS Elemental MediaTailor includes ads from the ad decision server \(ADS\) VAST response as follows: 
+ If a duration is specified, MediaTailor selects a set of ads that fit into the duration and includes them\. 
+ If no duration is specified, MediaTailor plays as many ads as it can until it encounters an ad marker that indicates a return to the main content\.

AWS Elemental MediaTailor adheres to the following guidelines during live ad replacement: 
+ MediaTailor tries to play complete ads, without clipping or truncation\.
+ Whenever MediaTailor encounters an ad marker that indicates an end to the ad break, it returns to the underlying content\. This can mean shortening an ad that is currently playing\. 
+ At the end of the duration, MediaTailor returns to the underlying content\.
+ If MediaTailor runs out of ads to play for the duration of an ad break it either plays the slate, if one is configured, or resumes playback of the underlying content stream\. This usually happens when there aren't enough transcoded ads to fill up the duration of the ad break\. 

  
**Tip**  
You can define the limit of unfilled ad time allowed in an break with the personalization threshold configuration setting\. For more information, see the [PlaybackConfiguration reference](https://docs.aws.amazon.com/mediatailor/latest/apireference/playbackconfiguration.html#playbackconfiguration-prop-putplaybackconfigurationrequest-personalizationthresholdseconds)\.

### Examples<a name="ad-behavior-live-examples"></a>
+ If the ad break has a duration set to 70 seconds and the ADS response contains two 40\-second ads, AWS Elemental MediaTailor plays one of the 40\-second ads\. In the time left over, it switches to the configured slate or underlying content\. At any point during this process, if MediaTailor encounters a cue\-in indicator, it cuts immediately to the underlying content\. 
+ If the ad break has a duration set to 30 seconds and the shortest ad provided by the ADS response is 40 seconds, MediaTailor plays no ads\. If an ad slate is configured, MediaTailor plays that for 30 seconds or until it encounters a cue\-in indicator\. Otherwise, MediaTailor plays the underlying content\.