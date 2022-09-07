# Working with live sources<a name="channel-assembly-working-live-sources"></a>

A *live source* represents a single live stream, such as a live football game or news broadcast, that you add to your source location\. After you create your channel, you add one or more live sources to your source location, and then associate each live source with a program\.

Each live source must have at least one package configuration\. A *package configuration* specifies a package format, manifest location, and source group for your live source\. When you create your channel, you use the package configuration's source groups to create the corresponding outputs on your channel\. For example, if your source is packaged in two different formats—HLS and DASH—then you'd create two package configurations, one for DASH and one for HLS\. Then, you'd create two channel outputs, one for each package configuration\. Each channel output provides an endpoint that's used for playback requests\. In this example, the channel provides an endpoint for HLS playback requests and an endpoint for DASH playback requests\.

## Adding live sources to your source location<a name="channel-assembly-add-live-sources"></a>

The following procedure explains how to use the MediaTailor console to add live sources to your source location and set up package configurations\. For information about how to add live sources using the MediaTailor API, see [CreateLiveSource](https://docs.aws.amazon.com/mediatailor/latest/apireference/sourcelocation-sourcelocationname-livesource-livesourcename.html) in the *AWS Elemental MediaTailor API Reference*\.

**Important**  
Before you add your live sources, make sure that within a package configuration, each source has the same number of child streams\.<a name="add-live-sources-procedure"></a>

**To add live sources to your source locations**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Source locations**\.

1. In the **Source locations** pane, choose the source location that you created in the [To create a source location](channel-assembly-creating-source-locations.md#create-source-location-procedure) procedure\.

1. On the **Live sources** tab, choose **Add live source**\.

1. Under **live source details**, enter a name for your live source:
   + **Name**: An identifier for your live source, such as **my\-example\-video**\. 

1. Under **Package configurations** > *source\-group\-name* enter information about the package configuration:
**Note**  
Within a package configuration, all of the VOD sources and live sources must have the same number of child streams\. We recommend that you configure your source streams the same way\.
   + **Source group**: Enter a source group name that describes this package configuration, such as HLS\-4k\. Make a note of this name; you'll reference it when you create your channel's output\. For more information, see [Using source groups with your channel's outputs](channel-assembly-source-groups.md)\.
   + **Type**: Select the packaged format for this configuration\. MediaTailor supports HLS and DASH\.
   + **Relative path**: The relative path from the source location's **Base HTTP URL** to the manifest\. For example, **/my/path/index\.m3u8**\.
**Note**  
MediaTailor automatically imports all of the closed captions and child streams contained within a parent manifest\. You don't need to create separate package configurations for each of your sources renditions \(DASH\) or variant streams \(HLS\)\.

    For more information about package configurations, see [Using package configurations](channel-assembly-package-configurations.md)\. 

1. Choose **Add live source**\.

   If you want to add more live sources, repeat steps 4\-6 in the procedure\.