# VAST, VMAP, and VPAID requirements for ad servers<a name="vast"></a>

To integrate your ad server with AWS Elemental MediaTailor, your ad server must send XML that conforms to the IAB specifications for the supported versions of VAST and VMAP\. You can use a public VAST validator to ensure that your tags are well\-formed\.

AWS Elemental MediaTailor supports VAST and VMAP responses from ad decision servers\. AWS Elemental MediaTailor also supports the proxying of VPAID metadata through our client\-side reporting API for client\-side ad insertion\. For information about client\-side reporting, see [Client\-side tracking](ad-reporting-client-side.md)\.

MediaTailor supports the following versions of VAST, VMAP, and VPAID:
+ [VAST 2\.0 and 3\.0](https://www.iab.com/guidelines/digital-video-ad-serving-template-vast-3-0/)
+ [VMAP 1\.0](https://www.iab.com/guidelines/digital-video-multiple-ad-playlist-vmap-1-0-1/)
+ [VPAID 2\.0](https://www.iab.com/guidelines/digital-video-player-ad-interface-definition-vpaid-2-0/)

## VAST requirements<a name="vast-integration"></a>

Your ad server's VAST response must contain IAB compliant `TrackingEvents` elements and standard event types, like `impression`\. If you don't include standard tracking events, AWS Elemental MediaTailor rejects the VAST response and doesn't provide an ad for the avail\.

VAST 3\.0 introduced support for ad pods, which is the delivery of a set of sequential linear ads\. If a specific ad in an ad pod is not available, AWS Elemental MediaTailor logs an error on CloudWatch, in the interactions log of the ADS\. It then tries to insert the next ad in the pod\. In this way, MediaTailor iterates through the ads in the pod until it finds one that it can use\.

### Targeting<a name="targeting"></a>

To target specific players for your ads, you can create templates for your ad tags and URLs\. For more information, see [Using dynamic ad variables in AWS Elemental MediaTailor](variables.md)\.

AWS Elemental MediaTailor proxies the player's `user-agent` and `x-forwarded-for` headers when it sends the ad server VAST request and when it makes the server\-side tracking calls\. Make sure that your ad server can handle these headers\. Alternatively, you can use `[session.user_agent]` or `[session.client_ip]` and pass these values in query strings on the ad tag and ad URL\. For more information, see [Using session variables](variables-session.md)\.

### Ad calls<a name="ad-calls"></a>

AWS Elemental MediaTailor calls your VAST ads URL as defined in your configuration\. It substitutes any player\-specific or session\-specific parameters when making the ad call\. MediaTailor follows up to five levels of VAST wrappers and redirects in the VAST response\. In live streaming scenarios, MediaTailor makes ad calls simultaneously at the ad avail start for connected players\. In practice, due to jitter, these ad calls can be spread out over a few seconds\. Make sure that your ad server can handle the number of concurrent connections this type of calling requires\. MediaTailor supports prefetching VAST responses for live workflows\. For more information, see [Prefetching ads](prefetching-ads.md)\.

### Creative handling<a name="creative-handling"></a>

When AWS Elemental MediaTailor receives the ADS VAST response, for each creative it identifies the highest bitrate `MediaFile` for transcoding and uses this as its source\. It sends this file to the on\-the\-fly transcoder for transformation into renditions that fit the player's master manifest bitrates and resolutions\. For best results, make sure that your highest bitrate media file is a high\-quality MP4 asset with valid manifest presets\. When manifest presets aren't valid, the transcode jobs fail, resulting in no ad shown\. Examples of presets that aren't valid include unsupported input file formats, like ProRes, and certain rendition specifications, like the resolution 855X481\. 

**Creative Indexing**  
AWS Elemental MediaTailor uniquely indexes each creative by the value of the `id` attribute provided in the `<Creative>` element\. If a creative's ID is not specified, MediaTailor uses the media file URL for the index\.

The following example declaration shows the creative ID\.

```
<Creatives>
        <Creative id="57859154776" sequence="1">
```

If you define your own creative IDs, use a new, unique ID for each creative\. Don't reuse creative IDs\. AWS Elemental MediaTailor stores creative content for repeated use, and finds each by its indexed ID\. When a new creative comes in, the service first checks its ID against the index\. If the ID is present, MediaTailor uses the stored content, rather than reprocessing the incoming content\. If you reuse a creative ID, MediaTailor uses the older, stored ad and doesn't play your new ad\. 

## VPAID requirements<a name="vpaid"></a>

VPAID allows publishers to serve highly interactive video ads and to provide viewability metrics on their monetized streams\. For information about VPAID, see the [VPAID specification](https://www.iab.com/guidelines/digital-video-player-ad-interface-definition-vpaid-2-0/)\.

AWS Elemental MediaTailor supports a mix of server\-side\-stitched VAST MP4 linear ads and client\-side\-inserted VPAID interactive creatives in the same ad avail\. It preserves the order in which they appear in the VAST response\. MediaTailor follows VPAID redirects through a maximum of five levels of wrappers\. The client\-side reporting response includes the unwrapped VPAID metadata\.

To use VPAID, follow these guidelines:
+ Configure an MP4 slate for your VPAID creatives\. AWS Elemental MediaTailor fills the VPAID ad slots with your configured slate, and provides VPAID ad metadata for the client player to use to run the interactive ads\. If you don't have a slate configured, when a VPAID ad appears, MediaTailor provides the ad metadata through client\-side reporting as usual\. It also logs an error in CloudWatch about the missing slate\. For more information, see [Inserting slate](slate-management.md) and [Creating a configuration](configurations-create.md)\. 
+ Use client\-side reporting\. AWS Elemental MediaTailor supports VPAID through our client\-side reporting API\. For more information, see [Client\-side tracking](ad-reporting-client-side.md)\. 

  It is theoretically possible to use the default server\-side reporting mode with VPAID\. However, if you use server\-side reporting, you lose any information about the presence of the VPAID ad and the metadata surrounding it, because that is available only through the client\-side API\. 
+ In live scenarios, make sure that your ad avails, denoted by `EXT-X-CUE-OUT: Duration`, are large enough to accommodate any user interactivity on VPAID\. For example, if the VAST XML specifies a VPAID ad that is 30 seconds long, implement your ad avail to be more than 30 seconds, to accommodate the ad\. If you don't do this, you lose the VPAID metadata, because the remaining duration in the ad avail is not long enough to accommodate the VPAID ad\.

