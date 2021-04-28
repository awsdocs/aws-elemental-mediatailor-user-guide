# Inserting pre\-roll ads<a name="ad-behavior-preroll"></a>

 * Pre\-roll ads are only available for live workflows\. * 

MediaTailor can insert ads at the beginning of a playback session, before the main content begins\. These are *pre\-roll* ads\. 

To insert pre\-roll ads, complete the **Live pre\-roll ad decision server** and **Live pre\-roll maximum allowed duration** fields in the **Additional** settings on your configuration, as described in [Optional configuration settings](configurations-create.md#configurations-create-addl)\. 

1. When MediaTailor receives a playback request, it sends a request to for pre\-roll ads based on the following fields in the MediaTailor playback configuration:
   + **Live pre\-roll ad decision server** is the ad decision server \(ADS\) URL where MediaTailor sends the request for pre\-roll ads\. 
   + **Live pre\-roll maximum allowed duration** is the total maximum length of time for the pre\-roll ads\. MediaTailor takes the following action based on the maximum allowed duration:
     + If the total duration of the ads in the ADS response is *less* than the value you gave in **Live pre\-roll maximum allowed duration**, MediaTailor inserts all of the ads\. When the last ad is complete, MediaTailor immediately returns to the underlying content\.
     + If the total duration of the ads in the ADS response is *more* than the value you gave in **Live pre\-roll maximum allowed duration**, MediaTailor selects a set of ads that fit into the duration without going over\. MediaTailor inserts these ads without clipping or truncation\. MediaTailor returns to the underlying content when the last selected ad completes\.

1. When MediaTailor receives the pre\-roll response from the ADS, it manipulates the manifest to add links to the pre\-roll ads\. MediaTailor calculates the start time of the pre\-roll ad break as follows:
   + For DASH, the formula is `(publishTime - breakabilityStartTime) - max(suggestedPresentationDelay, minBufferTime)`\.
   + For HLS, the formula is `max(2*EXT-X-TARGETDURATION, EXT-X-START:TIMEOFFSET)`\.

1. MediaTailor determines what action to take on any ad breaks that aren't pre\-rolls\. If the pre\-roll overlaps another ad break, MediaTailor doesn't personalize the overlapped portion of the ad break\. 