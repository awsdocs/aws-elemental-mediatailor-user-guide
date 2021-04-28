# Working with VOD sources<a name="channel-assembly-working-vod-sources"></a>

A VOD source represents a single piece of content, such as a video or an episode of a podcast, that you add to your source location\. You add one or more VOD sources to your source location, and then associate each VOD source with a program after you create your channel\.

Each VOD source must have at least one *package configuration*\. A package configuration specifies a package format, manifest location, and source group for your VOD source\. When you create your channel, you use the package configuration's source groups to create the corresponding outputs on your channel\. For example, if your source is packaged in two different formats—HLS and DASH—then you'd create two package configurations, one for DASH and one for HLS\. Then, you would create two channel outputs, one for each package configuration\. Each channel output provides an endpoint that's used for playback requests\. So, using the preceding example, the channel would provide an endpoint for HLS playback requests and an endpoint for DASH playback requests\. 

**Topics**
+ [Adding VOD sources to your source location](channel-assembly-add-vod-source.md)
+ [Using package configurations](channel-assembly-package-configurations.md)