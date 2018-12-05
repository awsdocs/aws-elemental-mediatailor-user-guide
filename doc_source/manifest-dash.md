# DASH \.mpd Manifests<a name="manifest-dash"></a>

AWS Elemental MediaTailor supports `.mpd` DASH manifests for live streaming\. A `Period` in a DASH manifest is eligible for ad replacement by MediaTailor when its `Event` contains `scte35:SpliceInsert` markings with `outOfNetworkIndicator` set to `true`\. MediaTailor considers only the first `Event` in an event stream, ignoring any additional `Event` markings\. 

The following example shows the main markings used for ad replacement in a DASH MPD: 

```
<Period ...
   <EventStream ...
     <Event>
       <scte35:SpliceInfoSection ...
         <scte35:SpliceInsert ... outOfNetworkIndicator="true" ...
```

During playback, when AWS Elemental MediaTailor encounters a period that is marked for ad replacement, it replaces some or all of the period with ads\. 

MediaTailor starts ad replacement at the beginning of the period\. If the first `Event` in the period has a `duration` specified, MediaTailor includes as many ads as it can into the duration without going over\. 

The following example shows a period that is configured with an `Event` `duration`: 

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
        ...
```

If no event duration is specified, MediaTailor includes ads until it reaches an end\-of\-period indicator\. MediaTailor doesn't play ads past the end of the period and truncates an ad rather than going over, when it encounters a period end\. 

The following example shows a period designated for ad replacement with no event duration specified:

```
  <Period start="PT444836.720S" id="123597" duration="PT12.280S">
    <EventStream timescale="90000" schemeIdUri="urn:scte:scte35:2013:xml">
      <Event>
        <scte35:SpliceInfoSection protocolVersion="0" ptsAdjustment="180832" tier="4095">
          <scte35:SpliceInsert spliceEventId="4026531856" spliceEventCancelIndicator="false" outOfNetworkIndicator="true" spliceImmediateFlag="false" uniqueProgramId="1" availNum="1" availsExpected="1">
            <scte35:Program><scte35:SpliceTime ptsTime="5675385600"/></scte35:Program>
          </scte35:SpliceInsert>
        </scte35:SpliceInfoSection>
      </Event>
      ...
```