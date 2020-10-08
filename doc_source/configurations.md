# Working with configurations in AWS Elemental MediaTailor<a name="configurations"></a>

A configuration is an object that you interact with in AWS Elemental MediaTailor\. The configuration holds the mapping information for the origin server and the ad decision server \(ADS\)\. You can also define a default playback for MediaTailor to use when an ad isn't available or doesn't fill the entire ad avail\.

If you use a content distribution network \(CDN\) with MediaTailor, you must set up the behavior rules in the CDN before you add CDN information to the configuration\. For more information about setting up your CDN, see [Integrating AWS Elemental MediaTailor and a CDN](integrating-cdn-standard.md) \.

**Topics**
+ [Creating a configuration](configurations-create.md)
+ [Viewing a configuration](configurations-view.md)
+ [Editing a configuration](configurations-edit.md)
+ [Deleting a configuration](configurations-delete.md)