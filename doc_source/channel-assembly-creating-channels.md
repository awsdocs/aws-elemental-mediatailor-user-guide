# Creating channels<a name="channel-assembly-creating-channels"></a>

 The following procedure describes how to create a channel using the MediaTailor console\. <a name="create-channel-procedure"></a>

**To create a channel**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Channels**\.

1. On the navigation bar, choose **Create channel**\.

1. Under **Channel details**, enter details about your channel:
   + **Name**: Enter a name for your channel\.
   + **Tier**: The tier determines what features the channel supports and how much it costs to run the channel\. For more information about pricing, see the [Channel Assembly pricing page](https://aws.amazon.com/mediatailor/pricing/#Channel_Assembly_Pricing)\. MediaTailor supports the following tiers:
     + **Basic** \- The Basic tier supports both the Linear and Loop playback modes, and does not support live sources\.
     + **Standard** \- The Standard tier supports live sources, and requires the Linear playback mode\.
   + **Playback mode**: The playback mode sets the channel's playback behavior\. MediaTailor supports the following playback modes:
     + **Loop** \- The programs in the schedule play back\-to\-back in an endless loop\. After the last program plays in a schedule, playback loops back to the first program\. Playback continues looping until you stop the channel\.
     + **Linear** \- Each program in the schedule plays once, back\-to\-back\.

1. <a name="filler-slate"></a>For **Filler slate**, select the **Source location name** referencing the slate location, and the **VOD source name** to use as slate\. MediaTailor uses the slate to fill gaps between programs in the schedule\. If the duration of the slate is less than the duration of the gap between programs, MediaTailor loops the slate\. You must configure filler slate field is if your channel uses the linear playback mode\. MediaTailor doesn't support filler slate for the loop playback mode\.

1. Choose **Next**\.

1. Under **Output details**, define the settings for this output:
   + **Manifest name**: Enter a manifest name, such as ***index***\. MediaTailor automatically inserts the format extension, such as \.m3u8\.
**Note**  
The manifest name must be unique for each channel output\. For example, you could use index for an HLS output and dash for a DASH output\.
   + **Output type**: Select the streaming format for the channel\. DASH and HLS are supported\.
   + **Source group**: Enter the name of the source group that you created in your package configuration, as described in [Adding VOD sources to your source location](channel-assembly-working-vod-sources.md#channel-assembly-add-vod-source)\.

1. Under **Manifest settings**, enter additional information about your manifest settings:
   + **Manifest window \(sec\)**: The time window \(in seconds\) contained in each manifest\. 

1. If you want to configure multiple channel outputs, under **Outputs** choose **Add**\. Then, configure the details for your output by completing the steps 6 and 7 in this procedure\.

1. Choose **Next**\.

1. Under **Channel policy**, choose your channel's IAM policy settings:
   + **Do not attach channel policy**: Restrict playback to only those who have access to this account's credentials\.
   + **Attach custom policy**: Define your own policy and restrict access to as few or as many as you want\. 
   + **Attach public policy**: Accept all incoming client requests to a channel's output\. You must use this option if you want to use MediaTailor ad insertion\.

1. Choose **Next**\.

1. Review your settings on the **Review and create** pane\.

1. Choose **Create channel**\.
**Note**  
Channels are created in a stopped state\. Your channel won't be active until you start it via the console or the MediaTailor StartChannel API\.