# Live Content Ad Behavior<a name="ad-behavior-live"></a>

In live streams, AWS Elemental MediaTailor always performs ad replacement, preserving the total time between the `CUE-OUT` and `CUE-IN` ad markers as closely as possible\. The `CUE-OUT` can optionally include the `DURATION` attribute, which indicates the duration of the ad avail\. Every `CUE-OUT` indicator must have a matching `CUE-IN` indicator in live workflows\. 

MediaTailor performs ad replacement for HLS and DASH live content\. For information on how MediaTailor calculates ad avail placement and timing, see [HLS Supported Ad Markers](hls-ad-markers.md) and [DASH Ad Markers](dash-ad-markers.md)\. 

## Ad Selection and Replacement<a name="ad-behavior-live-ad-selection"></a>

AWS Elemental MediaTailor includes ads from the ADS VAST response as follows: 
+ If a duration is specified, MediaTailor selects a set of ads that fit into the duration and includes them\. 
+ If no duration is specified, MediaTailor plays as many ads as it can until it encounters a cue\-in indicator\.

AWS Elemental MediaTailor adheres to the following guidelines during live ad replacement: 
+ MediaTailor tries to play complete ads, without clipping or truncation\.
+ Whenever MediaTailor encounters a cue\-in indicator, it returns to the underlying content\. This can mean truncating an ad that is currently playing\. 
+ At the end of the duration, MediaTailor returns to the underlying content\.
+ If MediaTailor runs out of ads to play for the duration indicated, it plays the slate, if one is configured, or it returns to the underlying content\. This happens most commonly when the ads that are available don't completely fill up the duration\. This can also happen when the ADS response doesn't provide enough ads to fill the ad avail\.

## Examples<a name="ad-behavior-live-examples"></a>
+ If the ad avail has a duration set to 70 seconds and the ADS response contains two 40\-second ads, AWS Elemental MediaTailor plays one of the 40\-second ads\. In the time left over, it switches to the configured slate or underlying content\. At any point during this process, if MediaTailor encounters a cue\-in indicator, it cuts immediately to the underlying content\. 
+ If the ad avail has a duration set to 30 seconds and the shortest ad provided by the ADS response is 40 seconds, MediaTailor plays no ads\. If an ad slate is configured, MediaTailor plays that for 30 seconds or until it encounters a cue\-in indicator\. Otherwise, MediaTailor plays the underlying content\.