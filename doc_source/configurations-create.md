# Creating a Configuration<a name="configurations-create"></a>

Create a configuration to start receiving content streams and to provide an access point for downstream playback devices to request content\.

**To add a configuration \(console\)**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. On the **Configurations** page, choose **Create configuration**\.

1.  For **Configuration name**, enter a unique name that describes the configuration\. The name is the primary identifier for the configuration\. The maximum length allowed is 512 characters\.

1. For **Video content source**, enter the URL prefix for the manifest for this stream, minus the asset ID\. The maximum length is 512 characters\.

   For example, the URL prefix `http://origin-server.com/a/` is valid for an HLS master manifest URL of `http://origin-server.com/a/master.m3u8` and for a DASH manifest URL of `http://origin-server.com/a/dash.mpd`\. Alternatively, you can enter a shorter prefix such as `http://origin-server.com`, but the `/a/` must be included in the asset ID in the player request for content\. 
**Note**  
If your content origin uses HTTPS, its certificate must be from a well\-known certificate authority\. \(It can't be a self\-signed certificate\.\) Otherwise, AWS Elemental MediaTailor fails to connect to the content origin and can't serve manifests in response to player requests\.

1.  For **Ad decision server**, enter the URL for your ad decision server \(ADS\)\. This is either the URL with variables as described in [Step 3: Configure ADS Request URL and Query Parameters](getting-started.md#getting-started-configure-request), or the static VAST URL that you are using for testing purposes\. The maximum length is 25,000 characters\.
**Note**  
If your ADS uses HTTPS, its certificate must be from a well\-known certificate authority\. \(It can't be a self\-signed certificate\.\) The same also applies to mezzanine ad URLs returned by the ADS\. Otherwise, AWS Elemental MediaTailor can't retrieve and stitch ads into the manifests from the content origin\.

1. Choose **Additional configuration**\. More configuration settings appear\.

1. For **Slate ad**, enter the URL for a high\-quality MP4 asset to transcode and use to fill in time that's not used by ads\. AWS Elemental MediaTailor shows the slate to fill in gaps in media content\. Configuring the slate is optional for non\-VPAID configurations\. For VPAID, you must configure a slate, which MediaTailor provides in the slots designated for dynamic ad content\. The slate must be a high\-quality MP4 asset that contains both audio and video\. For more information, see [Slate Management](slate-management.md)\.
**Note**  
If the server that hosts your slate uses HTTPS, its certificate must be from a well\-known certificate authority\. \(It can't be a self\-signed certificate\.\) Otherwise, AWS Elemental MediaTailor can't retrieve and stitch the slate into the manifests from the content origin\.

1. \(Optional\) The **CDN content segment prefix** enables AWS Elemental MediaTailor to create manifests with URLs to your CDN path for content segments\. Before you do this step, set up a rule in your CDN to pull segments from your origin server\. For **CDN content segment prefix**, enter the CDN prefix path\.

   For more information about integrating MediaTailor with a CDN, see [CDN Integration with AWS Elemental MediaTailor](integrating-cdn.md)\.

1. \(Optional\) The **CDN ad segment prefix** enables AWS Elemental MediaTailor to create manifests with URLs to your own CDN path for ad segments\. By default, MediaTailor serves ad segments from an internal Amazon CloudFront distribution with default cache settings\. Before you can complete the **CDN ad segment prefix** field, you must set up a rule in your CDN to pull ad segments from the following origin, like in the following example\.

   ```
   https://segments.mediatailor.<region>.amazonaws.com
   ```

   \(Optional\) For **CDN ads segment prefix**, enter the name of your CDN prefix in the configuration\.

   For more information about integrating MediaTailor with a CDN, see [CDN Integration with AWS Elemental MediaTailor](integrating-cdn.md)\.

1. \(Optional as needed for DASH\) For **DASH mpd location**, choose **DISABLED** if you have CDN routing rules set up for accessing MediaTailor manifests and you are either using client\-side reporting or your players support sticky HTTP redirects\. 

   For more information about the **Location** feature, see [DASH Location Feature](dash-location-feature.md)\.

1. \(Optional\) For **DASH origin manifest type**, if your origin server produces single\-period DASH manifests, open the dropdown list and choose **SINGLE\_PERIOD**\. By default, MediaTailor handles DASH manifests as multi\-period manifests\. For more information, see [DASH \.mpd Manifests](manifest-dash.md)\.

1. Choose **Create configuration**\.

   AWS Elemental MediaTailor displays the new configuration in the table on the **Configurations** page\.

1. \(Optional, but recommended\) You can use the configuration playback URLs to set up a CDN with AWS Elemental MediaTailor for manifests and reporting\.

   For information about setting up a CDN for manifest and reporting requests, see [Integrating AWS Elemental MediaTailor and a CDN](integrating-cdn-standard.md)\.