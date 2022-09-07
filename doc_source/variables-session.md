# Using session variables<a name="variables-session"></a>

To configure AWS Elemental MediaTailor to send session data to the Ad Decision Server \(ADS\), in the template ADS URL, specify one or more of the variables listed in this section\. You can use individual variables, and you can concatenate multiple variables to create a single value\. MediaTailor generates some values and obtains the rest from sources like the manifest and the player's session initialization request\. 

The following table describes the session data variables you can use in your template ADS request URL configuration\. The section numbers listed in the table correspond to the 2019a version of the Society of Cable Telecommunications Engineers \(SCTE\)\-35 specification, [Digital Program Insertion Cueing Message For Cable](https://webstore.ansi.org/Standards/SCTE/ansiscte352019a), For details about ad prefetch, see [Prefetching ads](prefetching-ads.md)\.


| Name | Available for ad prefetch | SCTE\-35 specification section | Description | 
| --- | --- | --- | --- | 
| \[avail\.index\] | Yes |  | A number that represents the position of an ad avail in an index\. At the start of a playback session, MediaTailor creates an index of all the ad avails in a manifest and stores the index for the remainder of the session\. When MediaTailor makes a request to the ADS to fill the avail, it includes the ad avail index number\. This parameter enables the ADS to improve ad selection by using features like competitive exclusion and frequency capping\. | 
| \[avail\.random\] | Yes |  | A random number between 0 and 10,000,000,000, as a long number, that MediaTailor generates for each request to the ADS\. Some ad servers use this parameter to enable features such as separating ads from competing companies\. | 
| \[scte\.archive\_allowed\_flag\] | Yes | 10\.3\.3\.1 | An optional Boolean value\. When this value is 0, recording restrictions are asserted on the segment\. When this value is 1, recording restrictions are not asserted on the segment\. | 
| \[scte\.avail\_num\] | Yes | 9\.7\.2\.1 | The value parsed by MediaTailor from the SCTE\-35 field avail\_num, as a long number\. MediaTailor can use this value to designate linear ad avail numbers\. | 
| \[scte\.avails\_expected\] | Yes | 9,7\.2\.1 | An optional long value that gives the expected count of avails within the current event\. | 
| \[scte\.delivery\_not\_restricted\_flag\] | Yes | 10\.3\.3\.1 | An optional Boolean value\. When this value is 0, the next five bits are reserved\. When this value is 1, the next five bits take on the meanings as described in the SCTE\-35 specification\. | 
| \[scte\.device\_restrictions\] | Yes | 10\.3\.3\.1 | An optional integer value that signals three pre\-defined, independent, and non\-hierarchical groups of devices\. For more information about this variable, see the segments\_expected description in the SCTE\-35 specification\. | 
| \[scte\.event\_id\] | Yes | 9\.1 and 9\.7\.2\.1 | The value parsed by MediaTailor from the SCTE\-35 field splice\_event\_id, as a long number\. MediaTailor uses this value to designate linear ad avail numbers or to populate ad server query strings, like ad pod positions\. | 
| \[scte\.no\_regional\_blackout\_flag\] | Yes | 10\.3\.3\.1 | An optional Boolean value\. When this value is 0, regional blackout restrictions apply to the segment\. When this value is 1, regional blackout restrictions do not apply to the segment\. | 
| \[scte\.segment\_num\] | Yes | 10\.3\.3\.1 | An optional integer value that numbers segments within a collection of segments\. For more information about this variable, see the segment\_num description in the SCTE\-35 specification\. | 
| \[scte\.segmentation\_event\_id\]  | Yes | 10\.3\.3\.1 | MediaTailor exposes this variable as [scte.event_id](#scte.event_id)\. | 
| \[scte\.segmentation\_type\_id\] | Yes | 10\.3\.3\.1 | An optional 8\-bit integer value that specifies the segmentation type\. For more information about this variable, see the segmentation\_type\_id description in the SCTE\-35 specification\. | 
| \[scte\.segmentation\_upid\] |  `segmentation_upid_type`: Yes `private_data`: Yes  |  **segmentation\_upid**: 10\.3\.3\.1 Managed Private UPID: 10\.3\.3\.3  |  Corresponds to the SCTE\-35 `segmentation_upid` element\. The `segmentation_upid` element contains `segmentation_upid_type` and `segmentation_upid_length`\. MediaTailor supports the following `segmentation_upid` types: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/mediatailor/latest/ug/variables-session.html)   | 
| \[scte\.segmentation\_upid\.assetId\] | Yes |  | Used in conjunction with the Managed Private UPID \(0xC\) segmentation\_ upid\_type for podbuster workflows\. MediaTailor derives this value from the assetId parameter in the MPU's private\_data JSON structure\. For more information, see [](#podbuster-workflow)\. | 
| \[scte\.segmentation\_upid\.cueData\.key\] | Yes |  | Used in conjunction with the Managed Private UPID \(0xC\) segmentation\_ upid\_type for podbuster workflows\. MediaTailor derives this value from the cueData\.key parameter in the MPU's private\_data JSON structure\. For more information, see [](#podbuster-workflow)\. | 
| \[scte\.segmentation\_upid\.cueData\.value\] | Yes |  | Used in conjunction with the Managed Private UPID \(0xC\) segmentation\_ upid\_type for podbuster workflows\. MediaTailor derives this value from the cueData\.key parameter in the MPU's private\_data JSON structure\. For more information, see [](#podbuster-workflow)\. | 
| \[scte\.segments\_expected\] | Yes | 10\.3\.3\.1 | An optional integer value that gives the expected count of individual segments within a collection of segments\. For more information about this variable, see the segments\_expected description in the SCTE\-35 specification\. | 
| \[scte\.sub\_segment\_num\] | Yes | 10\.3\.3\.1 | An optional integer value that identifies a particular sub\-segment within a collection of sub\-segments\. For more information about this variable, see the sub\_segment\_num description in the SCTE\-35 specification\. | 
| \[scte\.sub\_segments\_expected\] | Yes | 10\.3\.3\.1 | An optional integer value that gives the expected count of individual sub\-segments within a collection of sub\-segments\. For more information about this variable, see the sub\_segments\_expected description in the SCTE\-35 specification\. | 
| \[scte\.unique\_program\_id\] | Yes | 9\.7\.2\.1 | The integer value parsed by MediaTailor from the SCTE\-35 splice\_insert field unique\_program\_id\. The ADS uses the unique program ID \(UPID\) to provide program\-level ad targeting for live linear streams\. If the SCTE\-35 command is not splice insert, MediaTailor sets this to an empty value\. | 
| \[session\.avail\_duration\_ms\] | Yes |  |  The duration in milliseconds of the ad availability slot\. The default value is 300,000 ms\. AWS Elemental MediaTailor obtains the duration value from the input manifest as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/mediatailor/latest/ug/variables-session.html)  | 
| \[session\.avail\_duration\_secs\] | Yes |  | The duration in seconds of the ad availability slot, or ad avail, rounded to the nearest second\. MediaTailor determines this value the same way it determines \[session\.avail\_duration\_ms\]\. | 
| \[session\.client\_ip\] | No |  | The remote IP address that the MediaTailor request came from\. If the X\-forwarded\-for header is set, then that value is what MediaTailor uses for the client\_ip\. | 
| \[session\.id\] | No |  | A unique numeric identifier for the current playback session\. All requests that a player makes for a session have the same id, so it can be used for ADS fields that are intended to correlate requests for a single viewing\. | 
| \[session\.referer\] | No |  | Usually, the URL of the page that is hosting the video player\. MediaTailor sets this variable to the value of the Referer header that the player used in its request to MediaTailor\. If the player doesn't provide this header, MediaTailor leaves the \[session\.referer\] empty\. If you use a content delivery network \(CDN\) or proxy in front of the manifest endpoint and you want this variable to appear, proxy the correct header from the player here\. | 
| \[session\.user\_agent\] | No |  | The User\-Agent header that MediaTailor received from the player's session initialization request\. If you're using a CDN or proxy in front of the manifest endpoint, you must proxy the correct header from the player here\. | 
| \[session\.uuid\] | No |  |  Alternative to **\[session\.id\]**\. This is a unique identifier for the current playback session, such as the following: <pre>e039fd39-09f0-46b2-aca9-9871cc116cde</pre>  | 

**Example**  
If the ADS requires a query parameter named `deviceSession` to be passed with the unique session identifier, the template ADS URL in AWS Elemental MediaTailor could look like the following:  

```
https://my.ads.server.com/path?deviceSession=[session.id]
```
AWS Elemental MediaTailor automatically generates a unique identifier for each stream, and enters the identifier in place of `session.id`\. If the identifier is `1234567`, the final request that MediaTailor makes to the ADS would look something like this:  

```
https://my.ads.server.com/path?deviceSession=1234567
```
If the ADS requires several query parameters to be passed, the template ADS URL in AWS Elemental MediaTailor could look like the following:  

```
https://my.ads.server.com/sample?e=[scte.avails_expected]&f=[scte.segment_num]&g=[scte.segments_expected]&h=[scte.sub_segment_num]&j=[scte.sub_segments_expected]&k=[scte.segmentation_type_id]
```
The following DASH marker example XML fragment shows how to use `scte35:SpliceInsert`:  

```
<Period start="PT444806.040S" id="123456" duration="PT15.000S">
  <EventStream timescale="90000" schemeIdUri="urn:scte:scte35:2013:xml">
    <Event duration="1350000">
      <scte35:SpliceInfoSection protocolVersion="0" ptsAdjustment="180832" tier="4095">
        <scte35:SpliceInsert spliceEventId="1234567890" spliceEventCancelIndicator="false" outOfNetworkIndicator="true" spliceImmediateFlag="false" uniqueProgramId="1" availNum="1" availsExpected="1">
          <scte35:Program><scte35:SpliceTime ptsTime="5672624400"/></scte35:Program>
          <scte35:BreakDuration autoReturn="true" duration="1350000"/>
        </scte35:SpliceInsert>
      </scte35:SpliceInfoSection>
```
The following DASH marker example XML fragment shows how to use `scte35:TimeSignal`:  

```
<Period start="PT346530.250S" id="123456" duration="PT61.561S">
  <EventStream timescale="90000" schemeIdUri="urn:scte:scte35:2013:xml">
    <Event duration="5310000">
      <scte35:SpliceInfoSection protocolVersion="0" ptsAdjustment="183003" tier="4095">
        <scte35:TimeSignal>
          <scte35:SpliceTime ptsTime="3442857000"/>
        </scte35:TimeSignal>
        <scte35:SegmentationDescriptor segmentationEventId="1234567" segmentationEventCancelIndicator="false" segmentationDuration="8100000" segmentationTypeId="52" segmentNum="0" segmentsExpected="0">
          <scte35:DeliveryRestrictions webDeliveryAllowedFlag="false" noRegionalBlackoutFlag="false" archiveAllowedFlag="false" deviceRestrictions="3"/>
          <scte35:SegmentationUpid segmentationUpidType="12" segmentationUpidLength="2">0100</scte35:SegmentationUpid>
        </scte35:SegmentationDescriptor>
      </scte35:SpliceInfoSection>
    </Event>
```
The following DASH marker example XML fragment shows how to use `scte35:Binary`:  

```
<Period start="PT444806.040S" id="123456" duration="PT15.000S">
  <EventStream schemeIdUri="urn:scte:scte35:2014:xml+bin" timescale="1">
    <Event presentationTime="1541436240" duration="24" id="29">
      <scte35:Signal xmlns="http://www.scte.org/schemas/35/2016">
        <scte35:Binary>/DAhAAAAAAAAAP/wEAUAAAHAf+9/fgAg9YDAAAAAAAA25aoh</Binary>
      </scte35:Signal>
    </Event>
    <Event presentationTime="1541436360" duration="24" id="30">
      <scte35:Signal xmlns="http://www.scte.org/schemas/35/2016">
        <scte35:Binary>QW5vdGhlciB0ZXN0IHN0cmluZyBmb3IgZW5jb2RpbmcgdG8gQmFzZTY0IGVuY29kZWQgYmluYXJ5Lg==</Binary>
      </scte35:Signal>
    </Event>
```
The following HLS tag example shows how to use `EXT-X-DATERANGE`:  

```
#EXT-X-DATERANGE:ID="splice-6FFFFFF0",START-DATE="2014-03-05T11:
15:00Z",PLANNED-DURATION=59.993,SCTE35-OUT=0xFC002F0000000000FF0
00014056FFFFFF000E011622DCAFF000052636200000000000A0008029896F50
000008700000000
```
The following HLS tag example shows how to use `EXT-X-CUE-OUT`:  

```
#EXT-OATCLS-SCTE35:/DA0AAAAAAAAAAAABQb+ADAQ6QAeAhxDVUVJQAAAO3/PAAEUrEoICAAAAAAg+2UBNAAANvrtoQ==  
#EXT-X-ASSET:CAID=0x0000000020FB6501  
#EXT-X-CUE-OUT:201.467
```
The following HLS tag example shows how to use `EXT-X-SPLICEPOINT-SCTE35`:  

```
#EXT-X-SPLICEPOINT-SCTE35:/DA9AAAAAAAAAP/wBQb+uYbZqwAnAiVDVUVJAAAKqX//AAEjW4AMEU1EU05CMDAxMTMyMjE5M19ONAAAmXz5JA==
```
The following example shows how to use `scte35:Binary` decode:  

```
{
  "table_id": 252,
  "section_syntax_indicator": false,
  "private_indicator": false,
  "section_length": 33,
  "protocol_version": 0,
  "encrypted_packet": false,
  "encryption_algorithm": 0,
  "pts_adjustment": 0,
  "cw_index": 0,
  "tier": "0xFFF",
  "splice_command_length": 16,
  "splice_command_type": 5,
  "splice_command": {
    "splice_event_id": 448,
    "splice_event_cancel_indicator": false,
    "out_of_network_indicator": true,
    "program_splice_flag": true,
    "duration_flag": true,
    "splice_immediate_flag": false,
    "utc_splice_time": {
      "time_specified_flag": false,
      "pts_time": null
    },
    "component_count": 0,
    "components": null,
    "break_duration": {
      "auto_return": false,
      "duration": {
        "pts_time": 2160000,
        "wall_clock_seconds": 24.0,
        "wall_clock_time": "00:00:24:00000"
      }
    },
    "unique_program_id": 49152,
    "avail_num": 0,
    "avails_expected": 0
    "segment_num": 0,
    "segments_expected": 0,
    "sub_segment_num": 0,
    "sub_segments_expected": 0
  },
  "splice_descriptor_loop_length": 0,
  "splice_descriptors": null,
  "Scte35Exception": {
    "parse_status": "SCTE-35 cue parsing completed with 0 errors.",
    "error_messages": [],
    "table_id": 252,
    "splice_command_type": 5
  }
}
```