# Slate management<a name="slate-management"></a>

With AWS Elemental MediaTailor, you can designate a *slate ad* for ad breaks\. A slate is a default MP4 asset that is inserted into a stream, such as a still image or looped video, that plays instead of the live or video on demand \(VOD\) content\.

AWS Elemental MediaTailor shows a slate in the following situations:
+ To fill in time that's not fully used by an ad replacement
+ If the ad decision server \(ADS\) responds with a empty VAST or VMAP response
+ For error conditions, such as ADS timeout
+ If the duration of ads is longer than the ad break
+ If an ad isn't available

## Configuring the slate<a name="configuring-the-slate"></a>

You designate the slate in the ****additional configuration**** pane in the [ MediaTailor console](https://console.aws.amazon.com/console/home?nc2=h_ct&src=header-signin)\. MediaTailor downloads the slate from the URL that you specify, and transcodes it to the same renditions as your content\. You can control the maximum amount of time that a slate will be shown through the optional **personalization threshold** configuration in the MediaTailor console\. For more information, see [Optional configuration settings](configurations-create-addl.md)\.

## VPAID requirements<a name="vpaid-requirements"></a>

Configuring a slate is required if you use VPAID\. For VPAID, MediaTailor inserts the slate for the duration of the VPAID ad\. This duration might be slightly higher than the duration of the VPAID ad as reported by VAST to accommodate user interactivity\. The video player then handles the VPAID ad based on the client\-side reporting metadata that MediaTailor returns\. For information about client\-side reporting, see [Client\-side reporting](ad-reporting-client-side.md)\. For information about VPAID, see [VPAID handling](vpaid.md)\.

If you aren't using VPAID, if you don't configure a slate then MediaTailor defaults to the underlying content stream\.