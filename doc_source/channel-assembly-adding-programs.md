# Creating and updating programs<a name="channel-assembly-adding-programs"></a>

 The following procedure describes how to create and update programs within your channel's schedule using the MediaTailor console\. It also describes how to configure ad breaks, which are optional\. For information about how to create programs using the MediaTailor API, see [CreateProgram](https://docs.aws.amazon.com/mediatailor/latest/apireference/channel-channelname-program-programname.html) in the *AWS Elemental MediaTailor API Reference*\. <a name="add-programs-procedure"></a>

**To add and update programs**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Channels**\.

1. In the **Channels** pane, choose the channel that you created in the [To create a channel](channel-assembly-creating-channels.md#create-channel-procedure) procedure\.

1. 
**Important**  
If you modify the program list for a program that is scheduled within the next 10 minutes, the edit won't become apparent until the next loop\.

   Under **Program details**, enter details about your program:
   + **Name**: This is the name of the program that you add to your channel\.
   + **Source location name**: The source location to be associated with the program\.

     If you choose **Select an existing source location**, select a source location name from the **Select a source location** drop\-down menu\. You can alternatively search for your source location by name\. This is helpful if you have a large number of source locations\.

     If you choose **Enter the source location name**, search for your source location by name\.
   + **VOD source name**: The name of the VOD source to be associated with the program\.

     If you choose **Select an existing VOD source**, select a VOD source name from the list of VOD sources that are associated with your account\. You can alternatively search for your VOD source by name\. This is helpful if you have a large number of VOD sources\.

     If you choose **Search by name**, search for your VOD source by name\.

1. Under **Playback configuration**, define how and when a program is inserted in your channel's schedule:
   + **Transition type**: This value is fixed at **Relative**\. **Relative** transition types indicate that the program occurs relative to other programs within a program list\. This setting does not apply if this is the first program in your channel's schedule\. 
   + **Relative position**: Choose where in the program list to append the program\. You can select **Before program** or **After program**\. This setting does not apply if this is the first program in your channel's schedule\.
   + **Relative program**: The name of the program to be used to insert the new program before or after\. This setting does not apply if this is the first program in your channel's schedule\.

     If you choose **Select an existing program**, select the program name from a predefined list of the next 100 programs played by the channel in the **Use existing program** drop\-down menu\.

     If you choose **Search for a program by name**, enter name of an existing program in your channel\.

    If you'd like to add ad breaks to your program, continue to the next step\. 

1. <a name="channel-assembly-programs-ad-breaks"></a>Select **Add ad break**\. Under **Ad breaks**, configure the settings for the ad break:
   + **Slate source location name**: Choose **Select an existing source location** and choose the source location where your slate is stored that you created earlier in this tutorial\.
   + **VOD source name**: Choose **Select an existing VOD source** and choose the VOD source you're using for slate that you added earlier in this tutorial\. The duration of the slate determines the duration of the ad break\.
   + For **Offset in milliseconds**: This value determines the ad break start time in milliseconds, as an offset relative to the beginning of the program\. You can enter any value that's less than the duration of the VOD source, and within 100ms of a segment boundary, otherwise the ad break will be skipped\. For example, if you enter **0**, this creates a pre\-roll ad break that plays before the program begins\.
   + For **Avail number**, this is written to `splice_insert.avail_num`, as defined in section 9\.7\.3\.1\. of the SCTE\-35 specification\. The default value is `0`\. Values have to be between `0` and `256`, inclusive\.

     For **Avail expected**, this is written to `splice_insert.avails_expected`, as defined in section 9\.7\.3\.1\. of the SCTE\-35 specification\. The default value is `0`\. Values have to be between `0` and `256`, inclusive\.

     For **Splice event ID**, this is written to `splice_insert.splice_event_id`, as defined in section 9\.7\.3\.1\. of the SCTE\-35 specification\. The default value is `1`\.

     For **Unique program ID**, this is written to `splice_insert.unique_program_id`, as defined in section 9\.7\.3\.1\. of the SCTE\-35 specification\. The default value is `0`\. Values have to be between `0` and `256`, inclusive\.

1. Choose **Add program**\.

    For more advanced information using MediaTailor to personalize your ad breaks, see [Using MediaTailor ad insertion with your channel](channel-assembly-integrating-mediatailor-ssai.md)\. 