# Creating a configuration<a name="configurations-create"></a>

Create a configuration to start receiving content streams and to provide an access point for downstream playback devices to request content\.

You can use the AWS Elemental MediaTailor console, the AWS CLI, or the MediaTailor API to create a configuration\. For information about creating a configuration through the AWS CLI or MediaTailor API, see the [https://docs.aws.amazon.com/mediatailor/latest/apireference/what-is.html](https://docs.aws.amazon.com/mediatailor/latest/apireference/what-is.html)\.

When you're creating a configuration, do not put sensitive identifying information like customer account numbers into free\-form fields such as the **Configuration name** field\. This includes when you work with MediaTailor using the console, REST API, AWS CLI, or AWS SDKs\. Any data that you enter into MediaTailor might get picked up for inclusion in diagnostic logs or Amazon CloudWatch Events\.

**To add a configuration \(console\)**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. On the **Configurations** page, choose **Create configuration**\. 

1. Complete the configuration and additional configuration fields as described in the following topics:
   +  [Required settings](#configurations-create-main) 
   +  [Optional configuration settings](#configurations-create-addl) 

1. Choose **Create configuration**\. 

   AWS Elemental MediaTailor displays the new configuration in the table on the **Configurations** page\.

1. \(Optional, but recommended\) You can use the configuration playback URLs to set up a CDN with AWS Elemental MediaTailor for manifests and reporting\.

   For information about setting up a CDN for manifest and reporting requests, see [Integrating a CDN](integrating-cdn-standard.md)\.

## Required settings<a name="configurations-create-main"></a>

When you create a configuration, you must include the following required settings\.

**Name**  
Enter a unique name that describes the configuration\. The name is the primary identifier for the configuration\. The maximum length allowed is 512 characters\.

**Content source**  
 Enter the URL prefix for the manifest for this stream, minus the asset ID\. The maximum length is 512 characters\.  
For example, the URL prefix `http://origin-server.com/a/` is valid for an HLS parent manifest URL of `http://origin-server.com/a/master.m3u8` and for a DASH manifest URL of `http://origin-server.com/a/dash.mpd`\. Alternatively, you can enter a shorter prefix such as `http://origin-server.com`, but the `/a/` must be included in the asset ID in the player request for content\.   
If your content origin uses HTTPS, its certificate must be from a well\-known certificate authority\. \(It can't be a self\-signed certificate\.\) Otherwise, AWS Elemental MediaTailor fails to connect to the content origin and can't serve manifests in response to player requests\.

**Ad decision server**  
 Enter the URL for your ad decision server \(ADS\)\. This is either the URL with variables as described in [Step 3: Configure ADS request URL and query parameters](getting-started-ad-insertion.md#getting-started-configure-request) , or the static VAST URL that you are using for testing purposes\. The maximum length is 25,000 characters\.  
If your ADS uses HTTPS, its certificate must be from a well\-known certificate authority\. \(It can't be a self\-signed certificate\.\) The same also applies to mezzanine ad URLs returned by the ADS\. Otherwise, AWS Elemental MediaTailor can't retrieve and stitch ads into the manifests from the content origin\.

## Optional configuration settings<a name="configurations-create-addl"></a>

You can optionally configure ** configuration aliases**, ** personalization details**, and **advanced settings** in the console, MediaTailor API, or AWS CLI\.

### Configuration aliases<a name="configurations-configuration-aliases"></a>

The following are optional configuration aliases that you can configure in the MediaTailor console, or via the AWS CLI, or MediaTailor API\.

**Player parameter variable**  
Add one or more player parameter variables to use for dynamic domain configuration during session initialization\.  
For more information about using player parameter variables to dynamically configure domains, see [Using domain variables](variables-domains.md)\.

### Personalization details<a name="configurations-personalization-details"></a>

The following are optional personalization detail settings that you can configure in the MediaTailor console, or via the AWS CLI, or MediaTailor API\.

**Slate ad**  
Enter the URL for a high\-quality MP4 asset to transcode and use to fill in time that's not used by ads\. AWS Elemental MediaTailor shows the slate to fill in gaps in media content\. Configuring the slate is optional for non\-VPAID configurations\. For VPAID, you must configure a slate, which MediaTailor provides in the slots designated for dynamic ad content\. The slate must be a high\-quality MP4 asset that contains both audio and video\. For more information, see [Inserting slate](slate-management.md) \.  
If the server that hosts your slate uses HTTPS, its certificate must be from a well\-known certificate authority\. \(It can't be a self\-signed certificate\.\) Otherwise, AWS Elemental MediaTailor can't retrieve and stitch the slate into the manifests from the content origin\.

**Start bumper**  
The URL of the start bumper asset location\. Bumpers are short video or audio clips that play at the beginning or end of an ad break\. They can be stored on Amazon's S3, or a different storage service\. To learn more about bumpers, see [Inserting bumpers](bumpers.md)\.

**End bumper**  
The URL of the end bumper asset location\. Bumpers are short video or audio clips that play at the beginning or end of an ad break\. They can be stored on Amazon's S3, or a different storage service\. To learn more about bumpers, see [Inserting bumpers](bumpers.md)\.

**Personalization threshold**  
Defines the maximum duration of underfilled ad time \(in seconds\) allowed in an ad break\. If the duration of underfilled ad time exceeds the personalization threshold, then the personalization of the ad break is abandoned and the underlying content is shown\. For example, if the personalization threshold is 3 seconds and there would be 4 seconds of slate in an ad break, then personalization of the ad break is abandoned and the underlying content is shown\. This feature applies to *ad replacement* in live and VOD streams, rather than ad insertion, because it relies on an underlying content stream\. For more information about ad break behavior, including ad replacement and insertion, see [Understanding MediaTailor ad insertion behavior](ad-behavior.md)\.

**Live pre\-roll ad decision server**  
To insert ads at the start of a live stream before the main content starts playback, enter the URL for the ad pre\-roll from the ad decision server \(ADS\)\. This is either the URL with variables as described in [Step 3: Configure ADS request URL and query parameters](getting-started-ad-insertion.md#getting-started-configure-request), or the static VAST URL that you are using for testing purposes\. The maximum length is 25,000 characters\.  
If your ADS uses HTTPS, its certificate must be from a well\-known certificate authority\. \(It can't be a self\-signed certificate\.\) The same also applies to mezzanine ad URLs returned by the ADS\. Otherwise, AWS Elemental MediaTailor can't retrieve and stitch ads into the manifests from the content origin\.
For information about how pre\-roll works, see [Inserting pre\-roll ads](ad-behavior-preroll.md)\.

**Live pre\-roll maximum allowed duration**  
When you're inserting ads at the start of a live stream, enter the maximum allowed duration for the pre\-roll ad avail\. MediaTailor won't go over this duration when inserting ads\. If the response from the ADS contains more ads than will fit in this duration, MediaTailor fills the avail with as many ads as possible, without going over the duration\. For more details about how MediaTailor fills avails, see [Live ad stitching behavior](ad-behavior.md#ad-behavior-live) \.

**Avail suppression mode**  
Sets the mode for avail suppression, also known as ad suppression\. By default, ad suppression is off and all ad breaks are filled by MediaTailor with ads or slate\. When the mode is set to `BEHIND_LIVE_EDGE`, ad suppression is active and MediaTailor won't fill ad breaks on or behind the avail suppression value time in the manifest lookback window\.

**Avail suppression value**  
The avail suppression value is a live edge offset time in `HH:MM:SS`\. MediaTailor won't fill ad breaks on or behind this time in the manifest lookback window\.

### Advanced settings<a name="configurations-advanced-settings"></a>

The following are optional advanced settings that you can configure in the MediaTailor console, or via the AWS CLI, or MediaTailor API\.

**CDN content segment prefix**  
Enables AWS Elemental MediaTailor to create manifests with URLs to your CDN path for content segments\. Before you do this step, set up a rule in your CDN to pull segments from your origin server\. For **CDN content segment prefix**, enter the CDN prefix path\.  
For more information about integrating MediaTailor with a CDN, see [Working with CDNs](integrating-cdn.md)\.

**CDN ad segment prefix**  
Enables AWS Elemental MediaTailor to create manifests with URLs to your own CDN path for ad segments\. By default, MediaTailor serves ad segments from an internal Amazon CloudFront distribution with default cache settings\. Before you can complete the **CDN ad segment prefix** field, you must set up a rule in your CDN to pull ad segments from the following origin, like in the following example\.  

```
https://segments.mediatailor.<region>.amazonaws.com
```
For **CDN ad segment prefix**, enter the name of your CDN prefix in the configuration\.  
For more information about integrating MediaTailor with a CDN, see [Working with CDNs](integrating-cdn.md)\.

**DASH origin manifest type**  
If your origin server produces single\-period DASH manifests, open the dropdown list and choose **SINGLE\_PERIOD**\. By default, MediaTailor handles DASH manifests as multi\-period manifests\. For more information, see [Integrating an MPEG\-DASH source](manifest-dash.md)\.

**DASH mpd location**  
\(Optional as needed for DASH\) Choose **DISABLED** if you have CDN routing rules set up for accessing MediaTailor manifests and you are either using client\-side reporting or your players support sticky HTTP redirects\.   
For more information about the **Location** feature, see [DASH location feature](dash-location-feature.md)\.

**Transcode profile name**  
The name that associates this configuration with a custom transcode profile\. This name overrides the dynamic transcoding defaults of MediaTailor\. Complete this field only if you have already set\-up custom profiles with the help of AWS Support\.

**Ad marker passthrough**  
For HLS, enables or disables ad marker passthrough\. When ad marker passthrough is enabled, MediaTailor passes through `EXT-X-CUE-IN`, `EXT-X-CUE-OUT`, and `EXT-X-SPLICEPOINT-SCTE35` ad markers from the origin manifest to the MediaTailor personalized manifest\. No logic is applied to the ad marker values; they are passed from the origin manifest to the personalized manifest as\-is\. For example, if `EXT-X-CUE-OUT` has a value of `60` in the origin manifest, but no ads are placed, MediaTailor won't change the value to `0` in the personalized manifest\.