# HLS Live Manifest Example<a name="manifest-hls-example"></a>

The following example shows a simplified representation of an HLS live manifest, where each segment is four seconds long:

```
Segment1.ts
#EXT-X-CUE-OUT: 8
Segment2.ts
Segment3.ts
#EXT-X-CUE-IN
Segment4.ts
```