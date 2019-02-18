# HLS Live Manifest Examples<a name="manifest-hls-example"></a>

The following example shows a valid live master manifest as input to AWS Elemental MediaTailor:

```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-STREAM-INF:BANDWIDTH=878612,RESOLUTION=640x360,CODECS="avc1.4D4029,mp4a.40.2"
scte35_1.m3u8
#EXT-X-STREAM-INF:BANDWIDTH=2628628,RESOLUTION=1280x720,CODECS="avc1.4D4029,mp4a.40.2"
scte35_2.m3u8
#EXT-X-STREAM-INF:BANDWIDTH=1128660,RESOLUTION=854x480,CODECS="avc1.4D4029,mp4a.40.2"
scte35_3.m3u8
```

The following example shows a media manifest as input to AWS Elemental MediaTailor\. Note the `EXT-X-CUE-OUT` and `EXT-X-CUE-IN` tags that describe ad break opportunities:

```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-TARGETDURATION:4
#EXT-X-MEDIA-SEQUENCE:6719391
#EXTINF:4.000,
scte35_3_6719391.ts?m=1492714662
#EXTINF:3.533,
scte35_3_6719392.ts?m=1492714662
#EXT-OATCLS-SCTE35:/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXT-X-CUE-OUT:47.000
#EXTINF:0.467,
scte35_3_6719393.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=0.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:4.000,
scte35_3_6719394.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=4.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:4.000,
scte35_3_6719395.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=8.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:4.000,
scte35_3_6719396.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=12.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:4.000,
scte35_3_6719397.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=16.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:4.000,
scte35_3_6719398.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=20.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:4.000,
scte35_3_6719399.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=24.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:4.000,
scte35_3_6719400.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=28.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:4.000,
scte35_3_6719401.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=32.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:4.000,
scte35_3_6719402.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=36.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:4.000,
scte35_3_6719403.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=40.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:4.000,
scte35_3_6719404.ts?m=1492714662
#EXT-X-CUE-OUT-CONT:ElapsedTime=44.453,Duration=47.000,SCTE35=/DAlAAALkmP0AP/wFAXwAlXbf+//4dg/yP4AQItwAAEBAQAAhT3BsQ==
#EXTINF:2.533,
scte35_3_6719405.ts?m=1492714662
#EXT-X-CUE-IN
#EXTINF:1.467,
scte35_3_6719406.ts?m=1492714662
```