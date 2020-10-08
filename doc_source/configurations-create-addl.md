# Optional configuration settings<a name="configurations-create-addl"></a>

You can optionally configure ** personalization details** and **advanced settings** in the console, MediaTailor API, or AWS CLI\.

## Personalization details<a name="configurations-personalization-details"></a>

The following are optional personaliaztion detail settings that you can configure in the MediaTailor console, or via the AWS CLI, or MediaTailor API\.\.

**Slate ad**  
Enter the URL for a high\-quality MP4 asset to transcode and use to fill in time that's not used by ads\. AWS Elemental MediaTailor shows the slate to fill in gaps in media content\. Configuring the slate is optional for non\-VPAID configurations\. For VPAID, you must configure a slate, which MediaTailor provides in the slots designated for dynamic ad content\. The slate must be a high\-quality MP4 asset that contains both audio and video\. For more information, see [Slate management](slate-management.md) \.  
If the server that hosts your slate uses HTTPS, its certificate must be from a well\-known certificate authority\. \(It can't be a self\-signed certificate\.\) Otherwise, AWS Elemental MediaTailor can't retrieve and stitch the slate into the manifests from the content origin\.

**Start bumper**  
The URL of the start bumper asset location\. Bumpers are short video or audio clips that play at the beginning or end of an ad break\. They can be stored on Amazon's S3, or a different storage service\. To learn more about bumpers, see [Bumpers](https://docs.aws.amazon.com/mediatailor/latest/ug/bumpers.html)\.

**End bumper**  
The URL of the end bumper asset location\. Bumpers are short video or audio clips that play at the beginning or end of an ad break\. They can be stored on Amazon's S3, or a different storage service\. To learn more about bumpers, see [Bumpers](https://docs.aws.amazon.com/mediatailor/latest/ug/bumpers.html)\.

**Personalization threshold**  
Defines the maximum duration of underfilled ad time \(in seconds\) allowed in an ad break\. If the duration of underfilled ad time exceeds the personalization threshold, then the personalization of the ad break is abandoned and the underlying content is shown\. For example, if the personalization threshold is 3 seconds and there would be 4 seconds of slate in an ad break, then personalization of the ad break is abandoned and the underlying content is shown\. This feature applies to *ad replacement* in live and VOD streams, rather than ad insertion, because it relies on an underlying content stream\. For more information about ad break behavior, including ad replacement and insertion, see [Ad behavior in AWS Elemental MediaTailor](https://docs.aws.amazon.com/mediatailor/latest/ug/ad-behavior.html)\.

**Live pre\-roll ad decision server**  
To insert ads at the start of a live stream before the main content starts playback, enter the URL for the ad pre\-roll from the ad decision server \(ADS\)\. This is either the URL with variables as described in [Step 3: Configure ADS request URL and query parameters](getting-started.md#getting-started-configure-request) , or the static VAST URL that you are using for testing purposes\. The maximum length is 25,000 characters\.  
If your ADS uses HTTPS, its certificate must be from a well\-known certificate authority\. \(It can't be a self\-signed certificate\.\) The same also applies to mezzanine ad URLs returned by the ADS\. Otherwise, AWS Elemental MediaTailor can't retrieve and stitch ads into the manifests from the content origin\.
For information about how pre\-roll works, see [Pre\-Roll ads](ad-behavior-live.md#ad-behavior-preroll) \.

**Live pre\-roll maximum allowed duration**  
When you're inserting ads at the start of a live stream, enter the maximum allowed duration for the pre\-roll ad avail\. MediaTailor won't go over this duration when inserting ads\. If the response from the ADS contains more ads than will fit in this duration, MediaTailor fills the avail with as many ads as possible, without going over the duration\. For more details about how MediaTailor fills avails, see [Live content ad behavior](ad-behavior-live.md) \.

**Avail Suppression Mode**  
Sets the mode for avail suppression, also known as ad suppression\. By default, ad suppression is off and all ad breaks are filled by MediaTailor with ads or slate\. When the mode is set to `BEHIND_LIVE_EDGE`, ad suppression is active and MediaTailor won't fill ad breaks on or behind the avail suppression value time in the manifest lookback window\.

**Avail Suppression Value**  
The avail suppression value is a live edge offset time in `HH:MM:SS`\. MediaTailor won't fill ad breaks on or behind this time in the manifest lookback window\.

## Advanced settings<a name="configurations-advanced-settings"></a>

The following are optional advanced settings that you can configure in the MediaTailor console, or via the AWS CLI, or MediaTailor API\.

**CDN content segment prefix**  
Enables AWS Elemental MediaTailor to create manifests with URLs to your CDN path for content segments\. Before you do this step, set up a rule in your CDN to pull segments from your origin server\. For **CDN content segment prefix**, enter the CDN prefix path\.  
For more information about integrating MediaTailor with a CDN, see [CDN integration with AWS Elemental MediaTailor](integrating-cdn.md)\.

**CDN ad segment prefix**  
Enables AWS Elemental MediaTailor to create manifests with URLs to your own CDN path for ad segments\. By default, MediaTailor serves ad segments from an internal Amazon CloudFront distribution with default cache settings\. Before you can complete the **CDN ad segment prefix** field, you must set up a rule in your CDN to pull ad segments from the following origin, like in the following example\.  

```
https://segments.mediatailor.<region>.amazonaws.com
```
For **CDN ad segment prefix**, enter the name of your CDN prefix in the configuration\.  
For more information about integrating MediaTailor with a CDN, see [CDN integration with AWS Elemental MediaTailor](integrating-cdn.md)\.

**DASH origin manifest type**  
If your origin server produces single\-period DASH manifests, open the dropdown list and choose **SINGLE\_PERIOD**\. By default, MediaTailor handles DASH manifests as multi\-period manifests\. For more information, see [DASH \.mpd manifests](manifest-dash.md)\.

**DASH mpd location**  
\(Optional as needed for DASH\) Choose **DISABLED** if you have CDN routing rules set up for accessing MediaTailor manifests and you are either using client\-side reporting or your players support sticky HTTP redirects\.   
For more information about the **Location** feature, see [DASH location feature](dash-location-feature.md)\.

**Transcode Profile Name**  
The name that associates this configuration with a custom transcode profile\. This name overrides the dynamic transcoding defaults of MediaTailor\. Complete this field only if you have already set\-up custom profiles with the help of AWS Support\.