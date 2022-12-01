# SCTE\-35 messages for ad breaks<a name="ca-scte-35-messages"></a>

With MediaTailor, you can create a content channel based off of source location and VOD source resources\. You can then set up one or more ad breaks for each of the programs on a channel's schedule\. You use messages based on the SCTE\-35 specification to condition the content for ad breaks\. For example, you can use SCTE\-35 messages to provide metadata about the ad breaks\. For more information about the SCTE\-35 specification, see [Digital Program Insertion Cueing Message](https://webstore.ansi.org/Standards/SCTE/ANSISCTE352022)\.

You set up the ad breaks in one of two ways:
+ Attaching a `splice_insert` SCTE\-35 message that provides basic metadata about the ad break\.
+ Attaching a `time_signal` SCTE\-35 message with a `segmentation_descriptor` message\. This `segmentation_descriptor` message contains more advanced metadata fields, like content identifiers, which convey more information about the ad break\. MediaTailor writes the ad metadata to the output manifest as part of the `EXT-X-DATERANGE` \(HLS\) or `EventStream` \(DASH\) ad marker's SCTE\-35 data\.

The following illustration shows the two ways of setting up ad breaks in a channel using SCTE\-35 messages:
+ Use a `splice_insert()` message to set up ad breaks with basic metadata\.
+ Use a `time_signal()` message together with a `segmentation_descriptor()` message to set up ad breaks with more detailed metadata\.

![\[Two ways of setting up ad breaks in a channel using SCTE-35 messages.\]](http://docs.aws.amazon.com/mediatailor/latest/ug/images/scte-35-splice-insert-vs-time-signal-segmentation-descriptor.png)

For information about using `time_signal`, see section 9\.7\.4 of the 2022 SCTE\-35 specification, [Digital Program Insertion Cueing Message](https://webstore.ansi.org/Standards/SCTE/ANSISCTE352022)\.

The ad break information appears in the output `splice_info_section` SCTE\-35 data\. With MediaTailor, you can pair a single `segmentation_descriptor` message together with a single `time_signal` message\.

**Note**  
If you send a `segmentation_descriptor` message, you must send it as part of the `time_signal` message type\. The `time_signal` message contains only the `splice_time` field that MediaTailor constructs using a given timestamp\.

The following table describes the fields that MediaTailor requires for each `segmentation_descriptor` message\. For more information, see section 10\.3\.3\.1 of the 2022 SCTE\-35 specification, which you can purchased at the [ANSI Webstore website](https://webstore.ansi.org/Standards/SCTE/ANSISCTE352022)\.


**Required fields for a `segmentation_descriptor` message**  

| Field | Type | Default value | Description | 
| --- | --- | --- | --- | 
| segmentation\_event\_id | integer | 1 | This is written to segmentation\_descriptor\.segmentation\_event\_id\. | 
| segmentation\_upid\_type | integer | 14 \(0x0E\) | This is written to segmentation\_descriptor\.segmentation\_upid\_type\. The value must be between 0 and 256, inclusive\. | 
| segmentation\_upid | string | "" \(empty string\) | This is written to segmentation\_descriptor\.segmentation\_upid\. The value must be a hexadecimal string, containing characters 0\-9 and A\-F\. | 
| segmentation\_type\_id | integer | 48 \(0x30\) | This is written to segmentation\_descriptor\.segmentation\_type\_id\. The value must be between 0 and 256, inclusive\. | 
| segment\_num | integer | 0 | This is written to segmentation\_descriptor\.segment\_num\. The value must be between 0 and 256, inclusive\. | 
| segments\_expected | integer | 0 | This is written to segmentation\_descriptor\.segments\_expected\. The value must be between 0 and 256, inclusive\. | 
| sub\_segment\_num | integer | null | This is written to segmentation\_descriptor\.sub\_segment\_num\. The value must be between 0 and 256, inclusive\. | 
| sub\_segments\_expected | integer | null | This is written to segmentation\_descriptor\.sub\_segments\_expected\. The value must be between 0 and 256, inclusive\. | 

The following table shows the values that MediaTailor automatically sets for some of the `segmentation_descriptor` message's fields\.


**Values set by MediaTailor for a `segmentation_descriptor` message's fields**  

| Field | Type | Value | 
| --- | --- | --- | 
| segmentation\_event\_cancel\_indicator | Boolean | True | 
| program\_segmentation\_flag | Boolean | True | 
| delivery\_not\_restricted\_flag | Boolean | True | 

MediaTailor always sets the `segmentation_duration_flag` to `True`\. MediaTailor populates the `segmentation_duration` field with the duration, in ticks, of the state content\.

**Note**  
When MediaTailor sends the `time_signal` messages, it sets the `splice_command_type` field in the `splice_info_section` message to 6 \(0x06\)\.

In HLS output, for an `AdBreak` with a `time_signal` message, the output `EXT-X-DATERANGE` tag includes a `SCTE-35` field that is set to the serialized version of the `splice_info_section` message\. For example, the following `EXT-X-DATERANGE` tag shows the serialized version of the `splice_info_section` message:

```
#EXT-X-DATERANGE:ID=\"1\",START-DATE=\"2020-09-25T02:13:20Z\",DURATION=300.0,SCTE35-OUT=0xFC002C00000000000000FFF00506800000000000160214435545490000000100E000019BFCC00E0030000000000000
```

In DASH output, for an `AdBreak` with a `time_signal` message, the output `EventStream` element includes a `scte35:SpliceInfoSection` element with `scte35:TimeSignal` and `scte35:SegmentationDescriptor` elements as its children\. The `scte35:TimeSignal` element has a child `scte35:SpliceTime` element, and the `scte35:SegmentationDescriptor` element has a child `scte35:SegmentationUpid` element\. For example, the following DASH output shows the `EventStream` element's child element structure:

```
<EventStream schemeIdUri="urn:scte:scte35:2013:xml" timescale="90000">
    <Event duration="27000000">
        <scte35:SpliceInfoSection protocolVersion="0" ptsAdjustment="0" tier="4095">
            <scte35:TimeSignal>
                <scte35:SpliceTime ptsTime="0" />
            </scte35:TimeSignal>
            <scte35:SegmentationDescriptor segmentNum="0" segmentationDuration="27000000" segmentationEventCancelIndicator="false" segmentationEventId="1" segmentationTypeId="48" segmentsExpected="0">
                <scte35:SegmentationUpid segmentationUpidFormat="hexBinary" segmentationUpidType="14">012345</scte35:SegmentationUpid>
            </scte35:SegmentationDescriptor>
        </scte35:SpliceInfoSection>
    </Event>
</EventStream>
```

You learned about using SCTE\-35 messages to set up ad breaks in channel assembly, the structure and required fields for those messages, and sample HLS and DASH output that includes the SCTE\-35 messages\.