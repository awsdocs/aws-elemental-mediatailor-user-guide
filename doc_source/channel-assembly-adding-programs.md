# Creating programs<a name="channel-assembly-adding-programs"></a>

 The following procedure describes how to create a program within your channel's schedule using the MediaTailor console\. It also describes how to configure ad breaks, which are optional\. For information about how to create programs using the MediaTailor API, see [CreateProgram](https://docs.aws.amazon.com/mediatailor/latest/apireference/channel-channelname-program-programname.html) in the *AWS Elemental MediaTailor API Reference*\. <a name="add-programs-procedure"></a>

**To add a program**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Channels**\.

1. In the **Channels** pane, choose the channel that you created in the [To create a channel](channel-assembly-creating-channels.md#create-channel-procedure) procedure\.

1. 
**Important**  
For looping channels, if you modify the program list for a program that is scheduled within the next 10 minutes, the edit won't become apparent until the next loop\.

   Under **Program details**, enter details about your program:
   + **Name**: This is the name of the program that you add to your channel\.
   + **Source type**: Determines what type of source the program plays\. This option is only available for Standard channels\.
     + **VOD** \- The program plays a VOD source, such as a pre\-recorded TV episode\.
     + **Live** \- The program plays a live source, such as a live news broadcast\.
   + **Source location name**: The source location to be associated with the program\.

     If you choose **Select an existing source location**, select a source location name from the **Select a source location** drop\-down menu\. You can alternatively search for your source location by name\. This is helpful if you have a large number of source locations\.

     If you choose **Enter the source location name**, search for your source location by name\.
   + **VOD source name**: The name of the VOD source to be associated with the program\.

     If you choose **Select an existing VOD source**, select a VOD source name from the list of VOD sources that are associated with your account\. You can alternatively search for your VOD source by name\. This is helpful if you have a large number of VOD sources\.

     If you choose **Search by name**, search for your VOD source by name\.
   + **Live source name**: The name of the live source to be associated with the program\. This option is only available if you selected **Live** as the source type\.

     If you choose **Select an existing live source**, select a live source name from the list of live sources that are associated with your account\. You can alternatively search for your live source by name\. This is helpful if you have a large number of live sources\.

     If you choose **Search by name**, search for your live source by name\.

1. Under **Playback configuration**, define when a program plays in your channel's schedule:
   + **Duration in milliseconds**: Defines the duration of the program in milliseconds\. This option is only available for programs that use live sources\.
   + **Transition type**: Defines the transitions from program to program in the schedule\.
     + **Relative** \- The program plays either before or after another program in the schedule\. This option is only available for programs that use VOD sources\.
     + **Absolute** \- The program plays at a specific wall clock time\. MediaTailor makes a best effort to play the program at the clock time that you specify\. We start playback of the program on a common segment boundary between the preceding program or slate\. This option is only available for channels configured to use the [linear playback mode](channel-assembly-creating-channels.md#linear-playback-mode)\.
**Note**  
Be aware of the following behavior for absolute transition types:  
If the preceding program in the schedule has a duration that extends beyond the wall clock time, MediaTailor truncates the preceding program on the common segment boundary closest to the wall clock time\.
If there are gaps between programs in the schedule, MediaTailor plays [filler slate](channel-assembly-creating-channels.md#filler-slate)\. If the duration of the slate is less than the duration of the gap, MediaTailor loops the slate\.
   + **Program start time** \- For absolute transition types, the wall clock time when the program is scheduled to play\. If you are adding this program to a running linear channel, you must enter a start time that's 15 minutes or later from the current time\.
   + **Relative position**: Choose where to insert the program into schedule relative to another program\. You can select **Before program** or **After program**\. This setting does not apply if this is the first program in your channel's schedule\.
   + **Relative program**: The name of the program to be used to insert the new program before or after\. This setting does not apply if this is the first program in your channel's schedule\.

     If you choose **Select an existing program**, select the program name from a predefined list of the next 100 programs played by the channel in the **Use existing program** drop\-down menu\.

     If you choose **Search for a program by name**, enter name of an existing program in your channel\.

   If you'd like to add ad breaks to your program, continue to the next step\. Ad breaks are only configurable for programs that use VOD sources\. For live sources, ad breaks in DASH manifests and ad breaks in HLS manifests that use the `EXT-X-DATERANGE` tag are passed through automatically\.

1. Select **Add ad break**\. Under **Ad breaks**, configure the settings for the ad break:<a name="channel-assembly-programs-ad-breaks"></a>
   + **Slate source location name**: Choose **Select an existing source location** and choose the source location where your slate is stored that you created earlier in this tutorial\.
   + **VOD source name**: Choose **Select an existing VOD source** and choose the VOD source you're using for slate that you added earlier in this tutorial\. The duration of the slate determines the duration of the ad break\.
   + For **Offset in milliseconds**: This value determines the ad break start time in milliseconds, as an offset relative to the beginning of the program\. Enter any value that's less than the duration of the VOD source, and that aligns with a segment boundary on all tracks within the program's VOD source \(all audio, video and closed caption tracks\), otherwise the ad break will be skipped\. For example, if you enter **0**, this creates a pre\-roll ad break that plays before the program begins\.
   + For **Avail number**, this is written to `splice_insert.avail_num`, as defined in section 9\.7\.3\.1\. of the SCTE\-35 specification\. The default value is `0`\. Values have to be between `0` and `256`, inclusive\.

     For **Avail expected**, this is written to `splice_insert.avails_expected`, as defined in section 9\.7\.3\.1\. of the SCTE\-35 specification\. The default value is `0`\. Values have to be between `0` and `256`, inclusive\.

     For **Splice event ID**, this is written to `splice_insert.splice_event_id`, as defined in section 9\.7\.3\.1\. of the SCTE\-35 specification\. The default value is `1`\.

     For **Unique program ID**, this is written to `splice_insert.unique_program_id`, as defined in section 9\.7\.3\.1\. of the SCTE\-35 specification\. The default value is `0`\. Values have to be between `0` and `256`, inclusive\.

1. Choose **Add program**\.

    For more advanced information using MediaTailor to personalize your ad breaks, see [Insert personalized ads and ad breaks in a channel stream](channel-assembly-integrating-mediatailor-ssai.md)\.