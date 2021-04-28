# MediaTailor concepts<a name="what-is-terms"></a>

 Here's an overview of the concepts that are used throughout the *AWS Elemental MediaTailor User Guide*\.

## Ad insertion concepts<a name="what-is-ssai-terms"></a>

Here's an overview of the concepts that are related to ad insertion\.

**Ad decision server \(ADS\)**  
A server that provides advertising spot specifications based on criteria including current advertising campaigns and viewer preferences\. 

**Configuration**  
An object in MediaTailor that you interact with\. The configuration holds location information about the origin server and the ad decision server \(ADS\)\. The configuration also holds endpoints that provide access points in and out of MediaTailor\.

**Dynamic transcoding**  
A process that matches the ad quality and format to the primary video content when content is requested\. Dynamic transcoding reduces storage requirements and ensures that playback seamlessly transitions between the ad and video content\.

**Manifest manipulation**  
The process of rewriting manifests from the origin server so that the manifests reference the appropriate ad and content fragments\. Ads are determined by the VAST response from the ad decision server \(ADS\)\. As playback progresses, MediaTailor performs ad insertion or ad replacement into the content stream\.

**VAST and VMAP**  
Video Ad Serving Template \(VAST\) and Video Multiple Ad Playlist \(VMAP\) are XML responses that the ad decision server sends to ad requests from MediaTailor\. The responses dictate what ads MediaTailor inserts in the manifest\. VMAP also includes timing for ad avails\. For more information about the logic behind MediaTailor ad insertion, see [Understanding MediaTailor ad insertion behavior](ad-behavior.md)\. For more information about how MediaTailor works with VAST, see [VAST, VMAP, and VPAID requirements for ad servers](vast.md)\.

## Channel assembly concepts<a name="what-is-ca-terms"></a>

Here's an overview of the concepts that are related to channel assembly\.

**Channels**  
A channel assembles your source manifests into a linear stream\. Each channel has one or more outputs that contain playback URLs accessed by players\. The channel outputs correspond to the package configuration settings you create for your VOD sources\. A channel contains a schedule, which determines when VOD sources will play in the channel's stream\.

**Package configuration**  
A packager configuration is a representation of your VOD source that contains specific packaged format characteristics\. You associate your package configurations with channel outputs to create playback streams for your VOD source's packaged formats, such as HLS\.

**Schedule**  
Each channel is made up of programs that are arranged into the channel's schedule\. The schedule determines what time the programs will play in the channel's linear stream\.

**Source locations**  
A source location represents the origin server where your assets are stored\. It can be Amazon S3, an HTTP server, a content delivery network, or a packaging infrastructure such as MediaPackage\.

**VOD sources**  
A VOD source represents a single piece of content, such as a movie or an episode of a TV show\. You associate VOD sources with programs to add them to your channel's linear stream\.