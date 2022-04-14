# How MediaTailor ad insertion works<a name="what-is-flow"></a>

MediaTailor interacts between your content delivery network \(CDN\), origin server, and ad decision server \(ADS\) to stitch personalized ads into live and video on demand content\. 

 Here's an overview of how MediaTailor ad insertion works: 

![\[In the center of the image is the AWS Elemental MediaTailor icon. To the far right are two stacked icons, one for Content Delivery Network (CDN) and the other showing a group of display devices. They share the title "Content Delivery Network (CDN) or display device." A dotted arrow points from "Content Delivery Network (CDN) or display device" to MediaTailor, with the title "request content (viewer params)." Above the arrow is a circle containing the number 1. To the far left of the image, left of MediaTailor, are two icons, one above the other. The top icon is titled Origin Server and the bottom one is titled Ad Decision Server (ADS). A dotted arrow points from MediaTailor to the Origin Server with the title "request content." Just above this arrow is a circle containing the number 2. A solid arrow points back from the Origin Server to MediaTailor with the title "return manifest with ad markers" and an image depicting a media stream with two empty ad marker boxes. Another dotted arrow points from MediaTailor to the ADS with the title "request personalized ads (viewer params)." Just above this arrow is a circle that also contains the number 2. A solid arrow points back from the ADS to MediaTailor with the title "return ad list: A, B, C." In the middle of the image, just below the MediaTailor icon sits the image of the media stream with two empty ad marker boxes and two arrows lettered A and B pointing to the two empty ad marker boxes. Next to the arrows is the title "insert ads." This area has a circle next to it containing the number 3. To the right of MediaTailor a solid arrow points back to the "Content Delivery Network (CDN) or display device" on the far right, with the title "return manifest with ad specs" and an image depicting a media stream superimposed with two ad marker boxes, lettered A and B. Below this solid arrow is a circle containing the number 4.\]](http://docs.aws.amazon.com/mediatailor/latest/ug/images/MediaTailorSSAI_Overview.png)

1. A player or CDN such as Amazon CloudFront sends a request to MediaTailor for HLS or DASH content\. The request contains parameters from the player with information about the viewer, which is used for ad personalization\. 

1. To service the request, MediaTailor retrieves the content manifest and ad specifications:
   + MediaTailor manipulates the manifest to include the ads returned from the ADS, transcoded to match the encoding characteristics of the origin content\.

     If an ad hasn't yet been transcoded to match the content, MediaTailor will skip inserting it and use MediaConvert to prepare the ad so that it's ready for the next request\.
   + MediaTailor sends a request to the ADS that contains the viewer information\. The ADS chooses ads based on the viewer information and current ad campaigns\. It returns the ad URLs to MediaTailor in a VAST or VMAP response\.

1. MediaTailor manipulates the manifest to include the URLs for the ads\. 

1. MediaTailor returns the fully personalized manifest to the requesting CDN or player\. 

The ADS tracks the ads viewed based on viewing milestones such as start of ad, middle of ad, and end of ad\. As playback progresses, the player or MediaTailor sends ad tracking beacons to the ADS ad tracking URL, to record how much of an ad has been viewed\. In the session initialization with MediaTailor, the player indicates whether it or MediaTailor is to send these beacons for the session\. 

 For information about how to get started with ad insertion, see [Getting started with MediaTailor](getting-started.md)\. 