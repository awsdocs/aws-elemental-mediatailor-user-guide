# Getting started with MediaTailor channel assembly<a name="channel-assembly-getting-started"></a>

This Getting Started tutorial shows you how to perform the following tasks:
+ Create a source location, and add source content to it
+ Create a channel
+ Create a program list to play your channel's content on a schedule
+ Add personalized ads to the channel stream using AWS Elemental MediaTailor ad insertion

When you're finished, you'll be able to open a browser, enter the playback URL for your channel, and view your channel's stream containing personalized ads\.

This tutorial walks you through the basic steps to get started with MediaTailor channel assembly\. For more advanced information, see [Using MediaTailor to create linear assembled streams](channel-assembly.md)\.

**Estimated cost**
+ The fee for an active channel is $0\.10 per hour\. You aren't charged for channels that are inactive\.

**Topics**
+ [Prerequisites](#ca-getting-started-prerequisites)
+ [Step 1: Create a source location](#ca-getting-started-create-source-location)
+ [Step 2: Add VOD sources to your source location](#ca-getting-started-add-sources)
+ [Step 3: Create a channel](#ca-getting-started-create-channel)
+ [Step 4: Add programs to your channel's schedule](#ca-getting-started-create-programs)
+ [Step 5 \(*optional*\): Use MediaTailor to insert personalized ads into your stream](#ca-getting-started-integrate-mediatailor-ssai)
+ [Step 6: Start your channel](#ca-getting-started-start-channel)
+ [Step 7: Test your channel](#ca-getting-started-test-channel)
+ [Step 8: Clean up](#ca-getting-started-clean-up)

## Prerequisites<a name="ca-getting-started-prerequisites"></a>

 Before you begin this tutorial, you must complete these requirements: 
+ Be sure that you've completed the steps in [Setting up AWS Elemental MediaTailor](setting-up.md)\.
+ You must have assets available for both VOD source content and ad slate\. You must know the path to the manifests for the assets\.
**Note**  
If you are using automated adaptive bitrate \(ABR\) or per title encoding, you must encode your assets so that all variants are the same length and have the same number of child tracks\. We recommend that you use an encoding template with a minimum segment length of one second\.

## Step 1: Create a source location<a name="ca-getting-started-create-source-location"></a>

A source location represets the origin server where your content is stored\. It can be Amazon S3, a standard web server, a content delivery network \(CDN\), or a packaging origin, such as AWS Elemental MediaPackage\.

 MediaTailor fetches the content manifests from your source location, and uses them to assemble a live sliding manifest window that references the underlying content segments\. 

To create a source location, perform the following procedure\.<a name="ca-getting-started-create-source-location-procedure"></a>

**To create a source location**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Source locations**\.

1. On the navigation bar, choose **Create source location**\.

1. Under **Source location configuration**, enter an identifier and the location of your source content:
   + **Name**: An identifier for your source location, such as **my\-origin**\. 
   + **Base URL**: The base URL of the origin server where your content is hosted, such as **https://111111111111\.cloudfront\.net**\. The URL must be in a standard HTTP URL format, prefixed with **http://** or **https://**\.

1. Choose **Create source location**\.

## Step 2: Add VOD sources to your source location<a name="ca-getting-started-add-sources"></a>

 Now that you've defined one or more source locations for your channel, you can add one or more *VOD sources*\. Each VOD source represents a single piece of content, such as a single movie, an episode of a TV show, or a highlight clip\.

 You must create at least one *package configuration* for your VOD source\. Each package configuration contains the packaged format and manifest settings for your VOD sources\. You then add your package configurations to your channel to create outputs\.

You can use multiple package configurations to create different channel outputs\. For example, if your VOD source is packaged as both HLS and DASH, you can create two package configurations for each format\. You can then use the package configuration's source groups to create two channel outputs: one for HLS, one for DASH\.<a name="ca-getting-started-add-sources-procedure"></a>

**To add VOD sources and create package configurations**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Source locations**\.

1. In the **Source locations** pane, choose the source location that you created in the [To create a source location](#ca-getting-started-create-source-location-procedure) procedure\.

1. Choose **Add VOD source**\.

1. Under **VOD source details**, enter a **Name** for your VOD source, such as **my\-example\-video**\.

1. Under **Package configurations** > *source\-group\-name* enter information about the package configuration:
**Note**  
Your source's package configurations must all have the same duration, as determined by the source's manifest\. And, all of the sources within a package configuration must have the same number of child streams\. To meet these requirements, we recommend that you use an encoding template for your assets\. We recommend that you use an encoding template with a minimum segment length of one second\. MediaTailor doesn't support per title or automated adaptive bitrate streaming \(ABR\) because these encoding methods violate these requirements\.
   + **Source group**: Enter a source group name that describes this package configuration, such as HLS\-4k\. Make a note of this name; you'll reference it when you create your channel's output\. For more information, see [Using source groups with your channel's outputs](channel-assembly-source-groups.md)\.
   + **Type**: Select the packaged format for this configuration\. MediaTailor supports HLS and DASH\.
   + **Relative path**: The relative path from the source location's **Base HTTP URL** to the manifest\. For example, **/my/path/index\.m3u8**\.

1. Choose **Add source**\.

1. Repeat steps 4\-7 in this procedure to add the VOD source for your ad slate\.

## Step 3: Create a channel<a name="ca-getting-started-create-channel"></a>

 A channel assembles your sources into a live linear stream\. Each channel contains one or more outputs that correspond to your VOD source's package configurations\.

 First you create a channel, then you add your VOD sources to the channel's schedule by creating programs\. <a name="ca-gsg-create-channel-procedure"></a>

**To create a channel**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Channels**\.

1. On the navigation bar, choose **Create channel**\.

1. Under **Channel details**, enter details about your channel:
   + **Name**: Enter a name for your channel\.
   + **Playback mode**: Determines what kind of program transitions are allowed and what happens to a program after it finishes\. Use the default loop mode\.

1. Choose **Next**\.

1. Under **Output details**, define the settings for this output:
   + **Manifest name**: Enter a manifest name, such as ***index***\. MediaTailor will append the format extension, such as \.m3u8 for HLS\.
**Note**  
You must enter a unique manifest name per channel output\.
   + **Format type**: Select the streaming format for the channel\. DASH and HLS are supported\. Choose the format that corresponds to the package configuration that you created in [Step 1: Create a source location](#ca-getting-started-create-source-location)\.
   + **Source group**: Enter the name of the source group that you created in [Step 1: Create a source location](#ca-getting-started-create-source-location)\.

1. Under **Manifest settings**, enter additional information about your manifest settings:
   + **Manifest window \(sec\)**: The time window \(in seconds\) contained in each manifest\. The minimum value is 30 seconds, and the maximum value is 3600 seconds\.

1. Choose **Next**\.

1. Under **Channel policy**, select **Do not attach channel policy**\. This option restricts playback to only those who have access to your AWS account's credentials\.

1. Choose **Next**\.

1. Review your settings on the **Review and create** pane\.

1. Choose **Create channel**\.
**Note**  
Channels are created in a stopped state\. Your channel won't be active until you start it\.

## Step 4: Add programs to your channel's schedule<a name="ca-getting-started-create-programs"></a>

 Now that you have a channel, you'll add *programs* to the channel's schedule\. Each program contains a VOD source from a source location in your account\. The channel schedule determines the order that your programs will play in the channel's stream\.

 Each program can have one or more ad breaks\. You insert an ad break, by specifying a VOD source to use as an ad slate\. The duration of the ad break is determined by the duration of the slate\. You can optionally use a server\-side ad insertion server, such as MediaTailor ad insertion, to personalize your ad breaks\. <a name="ca-getting-started-add-programs"></a>

**To add programs to your channel's schedule**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Channels**\.

1. In the **Channels** pane, choose the channel that you created in the [Step 3: Create a channel](#ca-getting-started-create-channel) procedure\.

1. Under **Program details**, enter details about your program:
   + **Name**: This is the name of the program to add to your channel's schedule\.
   + **Source location name**: Choose **Select an existing source location**, and select the source location that you created in the [Step 1: Create a source location](#ca-getting-started-create-source-location) from the **Select a source location** drop\-down menu\.
   + **VOD source name**: Choose **Select an existing VOD source** and select the VOD source that you created earlier in this tutorial\.

1. Under **Playback configuration**, define how and when a program is inserted in a channel's schedule:
   + **Transition type**: This value is fixed at **Relative**\. The relative transition type indicates that this program occurs relative to other programs within the program list\.
   + **Relative position**: If this is the first program in your channel's schedule, you can skip this setting\. If it's not the first program in your channel's schedule, then choose where in the program list to append the program\. You can select **Before program** or **After program**\.
   + **Relative program**: If this is the first program in your schedule, you can skip this setting\. If it's not the first program in your channel's schedule, the choose **Use existing program**, select the program name that you created in [To add programs to your channel's schedule](#ca-getting-started-add-programs)\.

1. <a name="ad-breaks"></a>Select **Add ad break**\. Under **Ad breaks**, configure the settings for the ad break:
   + **Slate source location name**: Choose **Select an existing source location** and choose the source location where your slate is stored that you created earlier in this tutorial\.
   + **VOD source name**: Choose **Select an existing VOD source** and choose the VOD source you're using for slate that you added earlier in this tutorial\. The duration of the slate determines the duration of the ad break\.
   + For **Offset in milliseconds**: This value determines the ad break start time in milliseconds, as an offset relative to the beginning of the program\. Enter any value that's less than the duration of the VOD source, and that aligns with a segment boundary on all tracks within the program's VOD source \(all audio, video and closed caption tracks\), otherwise the ad break will be skipped\. For example, if you enter **0**, this creates a pre\-roll ad break that plays before the program begins\.

1. Choose **Add program**\.

   For more information about programs, see [](channel-assembly-adding-programs.md#channel-assembly-programs-ad-breaks)\.

    For more advanced information about using ads with your linear stream, see [Optional configuration settings](configurations-create.md#configurations-create-addl)\. 

## Step 5 \(*optional*\): Use MediaTailor to insert personalized ads into your stream<a name="ca-getting-started-integrate-mediatailor-ssai"></a>

 You now have a channel with programs\. If you want, you can use MediaTailor to insert personalized ads into the ad breaks in your programs in the channel's stream\. 

 **Prerequisites** 

 Before you proceed, you must meet the following requirements: 
+ You must have an ad decision server \(ADS\)\.
+ You must have configured **Ad break** settings in the [Working with programs](channel-assembly-programs.md) procedure\.<a name="ca-getting-started-ssai-procedure"></a>

**To add personalized ads to your channel's stream using MediaTailor**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Configurations**\.

1. Under **Required settings**, enter the basic required information about your configuration:
   + **Name**: The name of your configuration\.
   + **Content source**: Enter the playback URL from your channel's output, minus the file name and extension\. For advanced information about MediaTailor configuration, see [Required settings](configurations-create.md#configurations-create-main)\.
   + **Add decision server**: Enter the URL for your ADS\.

1. You can optionally configure the **Configuration aliases**, **Personalization details**, and **Advanced settings**\. For information about those settings, see [Optional configuration settings](configurations-create.md#configurations-create-addl)\.

1. On the navigation bar, choose **Create configuration**\.

 For more advanced information about using MediaTailor ad insertion, see [Configuring MediaTailor as your ad insertion service](configurations.md)\. 

## Step 6: Start your channel<a name="ca-getting-started-start-channel"></a>

 You now have a channel\. But before you can access the channel's stream, you need to start your channel\. If you try to access a channel before it is active, MediaTailor returns an HTTP `4xx` error code\. <a name="ca-getting-started-create-program-list"></a>

**Start your channel**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Channels**\.

1. On the navigation bar, choose **Start**\.

## Step 7: Test your channel<a name="ca-getting-started-test-channel"></a>

 To verify that your channel is working correctly, open a web browser and enter the URL from your channel's output\. You should see your channel's stream\.

 In some cases, you might need to clear the cache to see the expected behavior\. 

## Step 8: Clean up<a name="ca-getting-started-clean-up"></a>

 After you've finished with the channel that you created for this tutorial, you should clean up by deleting the channel\.

 You'll stop incurring charges for that channel as soon as the channel status changes to stopped\. To keep your channel for later, but not incur charges, you can stop the channel now and then start it again later\. <a name="ca-getting-started-delete-channel"></a>

**To delete your channel**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Channels**\.

1. Select the channel that you want to delete\.

1. If your channel is running, from the **Actions** drop\-down menu, choose **Stop**\. You must stop your channel before you can delete it\.

1. When your channel is stopped, from the **Actions** drop\-down menu, choose **Delete**\.