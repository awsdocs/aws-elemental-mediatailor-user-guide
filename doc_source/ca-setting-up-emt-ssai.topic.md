# Setting up ad insertion with MediaTailor<a name="ca-setting-up-emt-ssai.topic"></a>

To insert personalized ads into your channel's stream, your channel's endpoint URL is the content source for AWS Elemental MediaTailor\. This guide shows how to set up MediaTailor for ad insertion\.

## Prerequisites<a name="ca-setting-up-emt-ssai-prereqs"></a>

Before you begin, make sure that you meet the following requirements:
+ Prepare your HLS and DASH streams for MediaTailor ad insertion\. 
  + If you haven't prepared content streams already, see [Step 2: Prepare a stream](getting-started-ad-insertion.md#getting-started-prep-stream) in the *Getting started with MediaTailor ad insertion* topic\.
+ Have an ad decision server \(ADS\)\.
+ Configure **Ad break** settings in the program\. For more information, see the [](channel-assembly-adding-programs.md#channel-assembly-programs-ad-breaks) procedure\.

## <a name="considerations"></a>

As a best practice, consider using a content delivery network \(CDN\) in between channel assembly and MediaTailor ad insertion\. The MediaTailor ad insertion service can generate additional origin requests\. Therefore, it's a best practice to configure your CDN to proxy the manifests from channel assembly, then use the CDN prefixed URLs at the content source URL\.

## Configure MediaTailor for ad insertion<a name="name"></a>

The following shows how to configure MediaTailor console settings so that you can insert personalized ads into your channel's stream\.<a name="ca-integrating-ssai-procedure"></a>

**To configure MediaTailor for ad insertion**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Configurations**\.

1. Under **Required settings**, enter the basic required information about your configuration:
   + **Name**: The name of your configuration\.
   + **Content source**: Enter the playback URL from your channel's output, minus the file name and extension\. For advanced information about MediaTailor configuration, see [Required settings](configurations-create.md#configurations-create-main)\.
   + **Ad decision server**: Enter the URL for your ADS\.

1. You can optionally configure the **Configuration aliases**, **Personalization details**, and **Advanced settings**\. For information about those settings, see [Optional configuration settings](configurations-create.md#configurations-create-addl)\.

1. On the navigation bar, choose **Create configuration**\.

Now that you've set up MediaTailor for ad insertion, you can also set up ad breaks\. For detailed instructions, see [Getting started with MediaTailor ad insertion](getting-started-ad-insertion.md)\.