# Adding VOD sources to your source location<a name="channel-assembly-add-vod-source"></a>

The following procedure explains how to add VOD sources to your source location and set up package configurations using the MediaTailor console\. For information about how to add VOD sources using the MediaTailor API, see [CreateVodSource](https://docs.aws.amazon.com/mediatailor/latest/apireference/sourcelocation-sourcelocationname-vodsource-vodsourcename.html) in the *AWS Elemental MediaTailor API Reference*\.

**Important**  
Before you add your VOD sources, make sure that they meet these requirements:   
Source variants must all have the same length, as determined by the source manifest\. 
Within a package configuration, each source must have the same number of child streams\. 
Because of these requirements, we don't support per title or automated ABR, because these encoding methods can produce varying manifest lengths and child streams\.   
We recommend that you use an encoding template that includes a minimum segment length to ensure that your encoded sources meet these requirements\.<a name="add-vod-sources-procedure"></a>

**To add VOD sources to your source locations**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Source locations**\.

1. In the **Source locations** pane, choose the source location that you created in the [To create a source location](channel-assembly-creating-source-locations.md#create-source-location-procedure) procedure\.

1. Choose **Add VOD source**\.

1. Under **VOD source details**, enter a name for your VOD source:
   + **Name**: An identifier for your VOD source, such as **my\-example\-video**\. 

1. Under **Package configurations** > *source\-group\-name* enter information about the package configuration:
**Note**  
Your source's package configurations must all have the same duration, as determined by the source's manifest\. And, all of the sources within a package configuration must have the same number of child streams\. To meet these requirements, we recommend that you use an encoding template for your assets\. We recommend that you use an encoding template with a minimum segment length of one second\. MediaTailor doesn't support per title or automated adaptive bitrate streaming \(ABR\) because these encoding methods violate these requirements\.
   + **Source group**: Enter a source group name that describes this package configuration, such as HLS\-4k\. Make a note of this name; you'll reference it when you create your channel's output\. For more information, see [Using source groups with your channel's outputs](channel-assembly-source-groups.md)\.
   + **Type**: Select the packaged format for this configuration\. MediaTailor supports HLS and DASH\.
   + **Relative path**: The relative path from the source location's **Base HTTP URL** to the manifest\. For example, **/my/path/index\.m3u8**\.
**Note**  
MediaTailor automatically imports all of the closed captions and child streams contained within a parent manifest\. You don't need to create separate package configurations for each of your sources renditions \(DASH\) or variant streams \(HLS\)\.

    For more information about package configurations, see [Using package configurations](channel-assembly-package-configurations.md)\. 

1. Choose **Add VOD source**\.

   If you want to add more VOD sources, repeat steps 4\-7 in the procedure\.