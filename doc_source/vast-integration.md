# VAST Integration<a name="vast-integration"></a>

To integrate your ad server with AWS Elemental MediaTailor, your ad server must send XML that conforms to the IAB specifications for the supported versions of VAST and VMAP\. You can use a public VAST validator to ensure that these tags are well\-formed\.

Make sure that your ad server's VAST response contains IAB compliant `TrackingEvents` elements and standard event types like `impression`\. If you don't include standard tracking events, AWS Elemental MediaTailor rejects the VAST response and doesn't provide an ad fill for the break\.

VAST 3\.0 introduced support for ad pods, which is the delivery of a set of sequential linear ads\. With AWS Elemental MediaTailor if a specific ad in an ad pod is not available, AWS Elemental MediaTailor logs an error on CloudWatch, in the interactions log of the ad decision servers, and tries to insert the next ad in the pod\. In this way, AWS Elemental MediaTailor iterates through the ads in the pod until it finds one that it can use\.

## Targeting<a name="targeting"></a>

To target specific players for your ads, you can create templates for your ad tags and URLs\. For more information, see [Dynamic Ad Variables in AWS Elemental MediaTailor](variables.md)\.

AWS Elemental MediaTailor proxies the player's `user-agent` and `x-forwarded-for` headers when it sends the ad server VAST request and when it makes the server\-side tracking calls\. Make sure that your ad server can handle these headers\. Alternatively, you can use `[session.user_agent]` or `[session.client_ip]` and pass these values in query strings on the ad tag and ad URL\. For more information, see [Session Data](variables-session.md)\.

## Ad Calls<a name="ad-calls"></a>

AWS Elemental MediaTailor calls your VAST ads URL as defined in your configuration, substituting any player\-specific or session\-specific parameters when making the ad call\. AWS Elemental MediaTailor follows up to three levels of VAST wrappers and redirects in the VAST response\. In live streaming scenarios, AWS Elemental MediaTailor makes ad calls simultaneously at ad break start for connected players\. In practice, due to jitter, these ad calls can be spread out over a few seconds\. Make sure that your ad server can handle the number of concurrent connections this type of calling requires\. AWS Elemental MediaTailor does not currently support pre\-fetching VAST responses\.

## Creative Handling<a name="creative-handling"></a>

When AWS Elemental MediaTailor receives the ADS VAST response, for each creative it identifies the highest bit rate `MediaFile` for transcoding and uses this as its source\. It sends this file to the on\-the\-fly transcoder for transformation into renditions that fit the player's master manifest bit rates and resolutions\. For best results, make sure that your highest bit rate media file is a high\-quality MP4 asset with valid manifest presets\. When manifest presets are not valid, the transcode jobs fail, resulting in no ad shown\. Examples of presets that are not valid include unsupported input file formats, like ProRes, and certain rendition specifications, like the resolution 855X481\. 

**Creative Indexing**  
AWS Elemental MediaTailor uniquely indexes each creative by the value of the `id` attribute provided in the `<Creative>` element\. If a creative's ID is not specified, AWS Elemental MediaTailor uses the media file URL for the index\.

The following example declaration shows the creative ID:

```
<Creatives>
    <Creative id="57859154776" sequence="1">
```

If you define your own creative IDs, use a new, unique ID for each creative\. Do not reuse creative IDs\. AWS Elemental MediaTailor stores creative content for repeated use, and finds each by its indexed ID\. When a new creative comes in, the service first checks its ID against the index\. If the ID is present, AWS Elemental MediaTailor uses the stored content, rather than reprocessing the incoming content\. If you reuse a creative ID, AWS Elemental MediaTailor uses the older, stored ad and does not play your new ad\. 