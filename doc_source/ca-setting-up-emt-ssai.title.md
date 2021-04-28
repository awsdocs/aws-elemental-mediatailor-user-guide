# Setting up MediaTailor ad insertion<a name="ca-setting-up-emt-ssai.title"></a>

To use MediaTailor to insert personalized ads into your channel's stream, use your channel's endpoint URL as the content source for MediaTailor\.

 Before you proceed, you must meet the following requirements: 
+ You must have an ad decision server \(ADS\)\.
+ You must have configured **Ad break** settings when you created your program\. For more information, see [](channel-assembly-adding-programs.md#channel-assembly-programs-ad-breaks) procedure\.<a name="ca-integrating-ssai-procedure"></a>

**To add personalized ads to your channel's stream using MediaTailor**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Configurations**\.

1. Under **Required settings**, enter the basic required information about your configuration:
   + **Name**: The name of your configuration\.
   + **Content source**: Enter the playback URL from your channel's output, minus the filename and extension\. For advanced information about MediaTailor configuration, see [Required settings](configurations-create.md#configurations-create-main)\.
   + **Add decision server**: Enter the URL for your ADS\.

1. You can optionally configure the **Configuration aliases**, **Personalization details**, and **Advanced settings**\. For information about those settings, see [Optional configuration settings](configurations-create.md#configurations-create-addl)\.

1. On the navigation bar, choose **Create configuration**\.