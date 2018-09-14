# Creating a Configuration<a name="configurations-create"></a>

Create a configuration to start receiving content streams and to provide an access point for downstream playback devices to request content\.

**To add a configuration \(console\)**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. On the **Configurations** page, choose **Create configuration**\.

1.  For **Configuration name**, type a unique name that describes the configuration\. The name is the primary identifier for the configuration\. The maximum length allowed is 512 characters\.

1.  For **Video content source**, type the URL prefix for the master playlist for this stream, minus the asset ID\. For example, if the master playlist URL is `http://origin-server.com/a/master.m3u8`, you would type `http://origin-server.com/a/`\. Alternatively, you can type a shorter prefix such as `http://origin-erver.com` but the `/a/` must be included in the asset ID in the player request for content\. The maximum length is 512 characters\.
**Note**  
If your content origin uses HTTPS, its certificate must be from a well\-known certificate authority \(it cannot be a self\-signed certificate\)\. Otherwise, MediaTailor fails to connect to the content origin and can't serve manifests in response to player requests\.

1.  For **Ad decision server**, type the URL for your ad decision server \(ADS\)\. This is either the URL with variables as described in [Step 3: Configure ADS Request URL and Query Parameters](getting-started.md#getting-started-configure-request), or the static VAST URL that you are using for testing purposes\. The maximum length is 25000 characters\.
**Note**  
If your ADS uses HTTPS, its certificate must be from a well\-known certificate authority \(it cannot be a self\-signed certificate\)\. The same also applies to mezzanine ad URLs returned by the ADS\. Otherwise, MediaTailor can't retrieve and stitch ads into the manifests from the content origin\.

1. For **Slate ad**, type the URL for a high\-quality MP4 asset to transcode and use to fill in time that's not used by ads\. MediaTailor shows the slate to fill in gaps in media content\. Configuring the slate is optional for non\-VPAID configurations\. For VPAID, you must configure a slate, which MediaTailor provides in the slots designated for dynamic ad content\. The slate must be a high\-quality MP4 asset that contains both audio and video\. For more information, see [Slate Management](slate-management.md)\.
**Note**  
If the server that hosts your slate uses HTTPS, its certificate must be from a well\-known certificate authority \(it cannot be a self\-signed certificate\)\. Otherwise, MediaTailor can't retrieve and stitch the slate into the manifests from the content origin\.

1. \(Optional\) The **CDN content segment prefix** enables MediaTailor to create manifests with URLs to your CDN path for content segments\. Before you do this step, set up a rule in your CDN to pull segments from your origin server\. For **CDN content segment prefix**, type the CDN prefix path\.

   For more information about integrating MediaTailor with a CDN, see [CDN Integration](integrating-cdn.md)\.

1. \(Optional\) The **CDN ad segment prefix** enables MediaTailor to create manifests with URLs to your own CDN path for ad segments\. By default, MediaTailor serves ad segments from an internal Amazon CloudFront distribution with default cache settings\. Before you can complete the **CDN ad segment prefix** field, you must set up a rule in your CDN to pull ad segments from the following origin:

   ```
   https://ad.mediatailor.<region>.amazonaws.com
   ```

   For **CDN ads segment prefix**, type the name of your CDN prefix in the configuration\.

   For more information about integrating MediaTailor with a CDN, see [CDN Integration](integrating-cdn.md)\.

1. Choose **Create configuration**\.

   MediaTailor displays the new configuration in the table on the **Configurations** page\.

1. \(Optional, but recommended\) You can use the configuration playback URLs to set up a CDN with MediaTailor for manifests and reporting\.

   For information about setting up a CDN for manifest and reporting requests, see [Integrating MediaTailor and a CDN](integrating-cdn-standard.md)\.