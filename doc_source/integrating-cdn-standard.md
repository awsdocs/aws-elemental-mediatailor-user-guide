# Integrating MediaTailor and a CDN<a name="integrating-cdn-standard"></a>

The following steps show how to integrate MediaTailor with your content distribution network \(CDN\)\. Depending on the CDN that you use, some terminology might differ from what is used in these steps\.

## Step 1: \(CDN\) Create Routing Behaviors<a name="integrating-cdn-standard-cdn-routing"></a>

In the CDN, create behaviors and rules that route content segment requests to the origin server and ad segment requests to MediaTailor, as follows:
+ Create one behavior that routes *content segment* requests to the *origin server * based on a rule that includes a phrase to differentiate content segment requests from ad segment requests\.

  For example, the CDN routes player requests to `https://CDN_Hostname/subdir/content.ts` to the origin server path `http://origin.com/contentpath/subdir/content.ts` based on the keyword **subdir** in the request\.
+ Create one behavior that routes *ad segment* requests to the internal Amazon CloudFront distribution where MediaTailor stores transcoded ads, based on a rule that includes a phrase to differentiate ad segment requests from content segment requests\.

   These are the default Amazon CloudFront distributions that MediaTailor uses for storing ads:
  + `https://ads.mediatailor.us-east-1.amazonaws.com`
  + `https://ads.mediatailor.eu-west-1.amazonaws.com`

  This step is optional because MediaTailor provides a default CDN configuration for ad serving\.

## Step 2: \(MediaTailor\) Create a Configuration with CDN Mapping<a name="integrating-cdn-standard-config"></a>

Create an MediaTailor configuration that maps the domains of the CDN routing behaviors to the origin server and to the location where the ads are stored\. Type the domain names in the configuration as follows:
+ For **CDN content segment prefix**, type the CDN domain from the behavior that you created to route content requests to the origin server\. In the master manifest, MediaTailor replaces the content segment URL prefix with the CDN domain\.

  For example,
  + If the full content file path is `http://origin.com/contentpath/subdir/content.ts,`
  + then the **Video content source** in the MediaTailor configuration is `http://origin.com/contentpath/`, 
  + and the **CDN content segment prefix** is `https://CDN_Hostname/`,
  + then the content segment advertised in the master manifest that MediaTailor serves is `https://CDN_Hostname/subdir/content.ts`\.
+ For **CDN ad segment prefix**, type the name of the CDN behavior that you created to route ad requests through your CDN\. In the playlist manifest, MediaTailor replaces the Amazon CloudFront distribution with the behavior name\.

## Step 3: \(CDN\) Set up CDN for Manifest and Reporting Requests<a name="integrating-cdn-standard-cache"></a>

Using a CDN for manifest and reporting requests enables additional functionality in your workflow\.

For manifests, referencing a CDN in front of `/v1/master` \(in master playlist requests\) or `/v1/manifest` \(for manifest playlist requests\) lets you use CDN features such as geofencing, and also lets you serve everything from your own domain name\. For this path, do not cache the manifests because they are all personalized\.

For reporting, referencing a CDN in front of `/v1/segment` in ad segment requests helps prevent MediaTailor from sending duplicate ad tracking beacons\. When a player makes a request for a /v1/segment ad, MediaTailor issues a 301 redirect to the actual \*\.ts segment\. When MediaTailor sees that `/v1/segment` request, it issues a beacon call to track the view percentage of the ad\. If the same player makes multiple requests for the same `/v1/segment` in one session, and your ADS can't de\-duplicate requests, then MediaTailor issues multiple requests for the same beacon\. Using a CDN to cache these 301 responses ensures that MediaTailor doesn't make duplicate beacon calls for repeated requests\. For this path, you can use a high or default cache because cache\-keys for these segments are unique\.

To take advantage of these benefits, create behaviors in the CDN that route requests to the MediaTailor configuration endpoint based on rules that differentiate requests for master manifests, media playlists, and reporting\. Requests follow these formats:
+ Master manifests: `https://<playback-endpoint>/v1/master/<hashed-account-id>/<origin-id>/<assetID>.m3u8`  
**Example**  

  ```
  https://a57b77e98569478b83c10881a22b7a24.mediatailor.us-east-1.amazonaws.com/v1/master/a1bc06b59e9a570b3b6b886a763d15814a86f0bb/Demo/assetId.m3u8
  ```
+ Media playlists: `https://<playback-endpoint>/v1/manifest/<hashed-account-id>/<session-id>/<playlistNumber>.m3u8`  
**Example**  

  ```
  https://a57b77e98569478b83c10881a22b7a24.mediatailor.us-east-1.amazonaws.com/v1/manifest/a1bc06b59e9a570b3b6b886a763d15814a86f0bb/c240ea66-9b07-4770-8ef9-7d16d916b407/0.m3u8
  ```
+ Ad reporting requests for server\-side reporting: `https://<playback-endpoint>/v1/segment/<origin-id>/<session-id>/<playlistNum>/<HLSSequenceNum>`  
**Example**  

  ```
  https://a57b77e98569478b83c10881a22b7a24.mediatailor.us-east-1.amazonaws.com/v1/segment/Demo/240ea66-9b07-4770-8ef9-7d16d916b407/0/440384
  ```

In the CDN, create a behavior that routes manifest requests to the MediaTailor configuration endpoint based on a rule that includes a phrase to differentiate the manifest request from segment requests\.

For example, player requests to `https://CDN_Hostname/some/path/asset.m3u8` are routed to the MediaTailor path `https://mediatailor.us-west-2.amazonaws.com/v1/session/configuration/endpoint` based on the keyword **\*\.m3u8** in the request\.