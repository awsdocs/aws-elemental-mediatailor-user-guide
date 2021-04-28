# Considerations when using MediaTailor ad insertion<a name="ca-considerations-ssai.title"></a>

Note the following considerations when using MediaTailor ad insertion with channel assembly:

**Use a CDN in between channel assembly and MediaTailor ad insertion**  
The MediaTailor ad insertion service can generate additional origin requests, so it's best practice to configure your CDN to proxy the manifests from channel assembly, then use the CDN prefixed URLs at the content source URL\.

**Prepare your HLS and DASH streams for ad insertion**  
You need to prepare your HLS and DASH streams for MediaTailor ad insertion, if you haven't already\. For information about how to prepare content streams, see [Step 2: Prepare a stream](getting-started-ad-insertion.md#getting-started-prep-stream) in the Getting started with MediaTailor ad insertion topic\.