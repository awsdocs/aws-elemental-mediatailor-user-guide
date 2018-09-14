# Live Content Ad Behavior<a name="ad-behavior-live"></a>

In live streams, AWS Elemental MediaTailor always performs ad replacement, with the total time between XX\-OUT and XX\-IN markers preserved as closely as possible\. Note the following about live ad insertion:
+ MediaTailor always prioritizes the content stream over ad content\. If MediaTailor encounters an early CUE\-IN before the ad break time has elapsed, the ad might be truncated\.
+ If there aren't enough ads in the VAST response to fill the ad break in the manifest, MediaTailor plays the underlying stream \(or the ad slate if one was provided in the ADS and origin server configuration\)\.
+ If the fragment duration for individual ads exceeds the ad break in the manifest, MediaTailor fits as many complete ads as it can\. When it can't fit any more complete ads, MediaTailor plays the slate or underlying stream\. 

  If the ad break is for 70 seconds but the VAST response includes two ads, each of which is 40 seconds, MediaTailor plays one ad for a total of 40 seconds and then displays the configured ad slate or underlying content stream for the remaining 30 seconds of the ad break\.

Ad behavior is further refined by the length of the CUE\-OUT duration, as described in the following sections\.

## XX\-OUT Duration Greater Than Zero<a name="greater-0-duration"></a>

If the CUE\-OUT \(or SCTE\-OUT\) duration is greater than zero, MediaTailor replaces as many ads that fit in the ad break without truncation:
+ If the VAST response includes a single ad and the ad break duration is less than the ad creative duration, MediaTailor doesn't splice any ads into the content stream\. Instead, the service displays the configured ad slate or underlying content stream for the duration of the break\.
+ If the CUE\-IN is presented earlier than expected, MediaTailor honors the CUE\-IN and returns to the content stream, possibly cutting off some of the ad\. 
+ If the CUE\-IN is not encountered by the time the CUE\-OUT duration is reached, MediaTailor ends the ad break and the stream returns to the content stream\.

## XX\-OUT Duration Equal to Zero<a name="zero-duration"></a>

If the CUE\-OUT \(or SCTE\-OUT\) duration is zero, MediaTailor splices in all ads from the ADS response until it encounters a CUE\-IN marker\. No CUE\-IN markers in a live scenario is an error state that requires attention\.