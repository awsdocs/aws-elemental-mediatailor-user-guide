# DASH Ad Markers<a name="dash-ad-markers"></a>

A `Period` in a DASH manifest is eligible for ad replacement by MediaTailor when the first event in its event stream has splice insert or time signal cue out markers\. You can provide the markers in clear XML or in a base64\-encoded binary:
+ **Clear XML** – the event stream `schemeIdUri` must be set to `urn:scte:scte35:2013:xml`, and the first event must have `scte35:SpliceInfoSection` markers containing one of the following: 
  + `scte35:SpliceInsert` with `outOfNetworkIndicator` set to `true`

    The following example shows this option, with the required markers in bold: 

    ```
      <Period start="PT444806.040S" id="123586" duration="PT15.000S">
        <EventStream timescale="90000" schemeIdUri="urn:scte:scte35:2013:xml">
          <Event duration="1350000">
            <scte35:SpliceInfoSection protocolVersion="0" ptsAdjustment="180832" tier="4095">
              <scte35:SpliceInsert spliceEventId="4026531855" spliceEventCancelIndicator="false" outOfNetworkIndicator="true" spliceImmediateFlag="false" uniqueProgramId="1" availNum="1" availsExpected="1">
                <scte35:Program><scte35:SpliceTime ptsTime="5672624400"/></scte35:Program>
                <scte35:BreakDuration autoReturn="true" duration="1350000"/>
              </scte35:SpliceInsert>
            </scte35:SpliceInfoSection>
          </Event>
        </EventStream>
    ```
  + `scte35:TimeSignal` accompanied by `scte35:SegmentationDescriptor` `scte35:SegmentationUpid` with `segmentationTypeId` set to one of the following cue\-out numbers: 
    + 0x22 \(start break\)
    + 0x30 \(provider advertisement start\)
    + 0x32 \(distributor advertisement start\)
    + 0x34 \(provider placement opportunity start\)
    + 0x36 \(distributor placement opportunity start\)

    The following example shows this option, with the required markers in bold\. The `segmentationTypeId` in this example is set to 52, equivalent to 0x34: 

    ```
      <Period start="PT346530.250S" id="178443" duration="PT61.561S">
        <EventStream timescale="90000" schemeIdUri="urn:scte:scte35:2013:xml">
          <Event duration="5310000">
            <scte35:SpliceInfoSection protocolVersion="0" ptsAdjustment="183003" tier="4095">
              <scte35:TimeSignal>
                <scte35:SpliceTime ptsTime="3442857000"/>
              </scte35:TimeSignal>
              <scte35:SegmentationDescriptor segmentationEventId="1414668" segmentationEventCancelIndicator="false" segmentationDuration="8100000">
                <scte35:DeliveryRestrictions webDeliveryAllowedFlag="false" noRegionalBlackoutFlag="false" archiveAllowedFlag="false" deviceRestrictions="3"/>
                <scte35:SegmentationUpid segmentationUpidType="12" segmentationUpidLength="2" segmentationTypeId="52" segmentNum="0" segmentsExpected="0">0100</scte35:SegmentationUpid>
              </scte35:SegmentationDescriptor>
            </scte35:SpliceInfoSection>
          </Event>
        </EventStream>
    ```

    With time signal markers, MediaTailor only uses the duration settings and ignores all other settings\. 
+ **Base64\-encoded binary** – the event stream `schemeIdUri` must be set to `urn:scte:scte35:2014:xml+bin` and the first event must have `scte35:Signal` `scte35:Binary` that contains a base64\-encoded binary\. The decoded binary must provide the same set of information in its `splice_info_section` as the clear XML would provide in a `scte35:SpliceInfoSection` element\. The command type must be either `splice_insert()` or `time_signal()`, and the additional settings must comply with those described previously for clear XML delivery\. 

  The following example shows this option, with the required markers in bold\. With this example period, AWS Elemental MediaTailor uses only the first `Event` element and disregards the second `Event` element:

  ```
    <Period start="PT444806.040S" id="123586" duration="PT15.000S">
      <EventStream schemeIdUri="urn:scte:scte35:2014:xml+bin" timescale="1">
        <Event presentationTime="1541436240" duration="24" id="29">
          <scte35:Signal xmlns="http://www.scte.org/schemas/35/2016">
            <scte35:Binary>VGVzdCBzdHJpbmcgZm9yIGVuY29kaW5nIHRvIEJhc2U2NCBlbmNvZGVkIGJpbmFyeS4=</Binary>
          </scte35:Signal>
        </Event>
        <Event presentationTime="1541436360" duration="24" id="30">
          <scte35:Signal xmlns="http://www.scte.org/schemas/35/2016">
            <scte35:Binary>QW5vdGhlciB0ZXN0IHN0cmluZyBmb3IgZW5jb2RpbmcgdG8gQmFzZTY0IGVuY29kZWQgYmluYXJ5Lg==</Binary>
          </scte35:Signal>
        </Event>
      <EventStream>
  ```

MediaTailor considers only the first `Event` in an event stream to determine ad replacement markers\. It ignores any additional `Event` markers in the stream\. 