# DASH Ad Avail Duration<a name="dash-ad-avail-duration"></a>

During playback, when AWS Elemental MediaTailor encounters a period that is marked for ad replacement, it replaces some or all of the period with ads\. MediaTailor starts ad replacement at the beginning of the period and includes ads as follows: 
+ If the period specifies a duration for the event, AWS Elemental MediaTailor includes as many ads as it can into the duration without going over\. 
+ If no duration is provided, MediaTailor includes ads until it reaches an end\-of\-period indicator\. MediaTailor doesn't play ads past the end of the period and, when it encounters a period end, truncates the ad instead of going over\. 

**How AWS Elemental MediaTailor looks for the period duration**  
AWS Elemental MediaTailor searches for a duration setting in the following order: 

1. `Event` `duration`

1. For splice insert, the `scte35:BreakDuration` `duration`

1. For time signal, the `scte35:SegmentationDescriptor` `segmentationDuration`

If AWS Elemental MediaTailor doesn't find any of these settings, it manages ad inclusion without a duration\. 

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

The following example shows a period designated for ad replacement with no duration specified\. The `Event` element has no `duration` and the `scte35:SpliceInsert` element doesn't contain a `scte35:BreakDuration` child element:

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