# Configuring MediaTailor as your ad insertion service<a name="configurations"></a>

A configuration is an object that you interact with in AWS Elemental MediaTailor\. The configuration holds the mapping information for the origin server and the ad decision server \(ADS\)\. You can also define a default playback for MediaTailor to use when an ad isn't available or doesn't fill the entire ad avail\.

If you use a content distribution network \(CDN\) with MediaTailor, you must set up the behavior rules in the CDN before you add CDN information to the configuration\. For more information about setting up your CDN, see [Integrating a CDN](integrating-cdn-standard.md)\.

**Topics**
+ [VAST, VMAP, and VPAID requirements for ad servers](vast.md)
+ [Working with MediaTailor configurations](working-with-configurations.md)
+ [Customizing ad break behavior](ad-rules.md)
+ [Integrating a content source](integrating-origin.md)
+ [Reporting ad tracking data](ad-reporting.md)
+ [Using dynamic ad variables in AWS Elemental MediaTailor](variables.md)
+ [Working with CDNs](integrating-cdn.md)
+ [Understanding MediaTailor ad insertion behavior](ad-behavior.md)