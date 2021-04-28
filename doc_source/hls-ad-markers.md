# HLS supported ad markers<a name="hls-ad-markers"></a>

AWS Elemental MediaTailor identifies ad avail boundaries in an HLS manifest ad markers in the input manifest\. The following sections describe what markers MediaTailor uses\.

## EXT\-x\-ASSET<a name="hls-ad-markers-asset"></a>

The `EXT-X-ASSET` tag contains metadata that's used by the ad decision server \(ADS\) to personalize content for the viewer\. `EXT-X-ASSET` parameters are comma\-separated key\-value pairs\.

To use this tag, you must meet the following requirements:
+ You must URL\-encode the `EXT-X-ASSET` *values* in the origin manifest\. The following example shows the `EXT-X-ASSET` tag with keys and URL\-encoded values\.

  ```
              #EXT-X-ASSET:GENRE=CV,CAID=12345678,EPISODE="Episode%20Name%20Date",SEASON="Season%20Name%20and%20Number",SERIES="Series%2520Name"
  ```
+ You must include the dynamic `[asset.]` variable and the *keys* in your MediaTailor ADS configuration\. The following example shows an MediaTailor ADS configuration using the dynamic `[asset.]` variable and keys\.

  ```
              https://myads.com/stub?c=[asset.GENRE]&g=[asset.CAID]&e=[asset.EPISODE]&s=[asset.SEASON]&k=[asset.SERIES]
  ```

**Example VAST request**  
The following example shows a VAST `GET` request to an ADS\.

```
            https://myads.com/stub?c=CV&g=12345678&e=Episode%20Name%20Date&s=Season%20Name%20and%20Number&k=Series%2520Name
```

## EXT\-x\-CUE\-OUT and EXT\-x\-CUE\-IN<a name="hls-ad-markers-cue"></a>

This type of ad marker is the most common\. The following examples show options for these cue markers\.

```
#EXT-X-CUE-OUT:DURATION=120
    ...
    #EXT-X-CUE-IN
```

```
#EXT-X-CUE-OUT:30.000
    ...
    #EXT-X-CUE-IN
```

```
#EXT-X-CUE-OUT
    ...
    #EXT-X-CUE-IN
```

## EXT\-x\-DATERANGE<a name="hls-ad-markers-range"></a>

With `EXT-X-DATERANGE` ad marker tags, you use `SCTE35-OUT` attributes to specify the timing of the ad avail\. 

**Note**  
AWS Elemental MediaTailor ignores any `START-DATE` attributes that are provided for `EXT-X-DATERANGE` ad markers\. 

You can specify the ad avail in one of the following ways:
+ `EXT-X-DATERANGE` tag with `SCTE35-OUT` and `DURATION` specifications\. 

  Example

  ```
  #EXT-X-DATERANGE:ID="splice-6FFFFFF0",START-DATE="2019-01T00:15:00Z\",DURATION=60.000,SCTE35-OUT=0xF
  ```
+ Paired `EXT-X-DATERANGE` tags, the first with a `SCTE35-OUT` specification and the second with a `SCTE35-IN` specification\. 

  Example

  ```
  #EXT-X-DATERANGE:ID="splice-6FFFFFF0",START-DATE="2019-01T00:15:00Z\",SCTE35-OUT=0xF
      ...
      #EXT-X-DATERANGE:ID="splice-6FFFFFF0",START-DATE="2019-01T00:15:00Z\",SCTE35-IN=0xF
  ```
+ A combination of the prior options\. You specify an `EXT-X-DATERANGE` tag with `SCTE35-OUT` and `DURATION` specifications followed by an `EXT-X-DATERANGE` tag with a `SCTE35-IN` specification\. In this case, MediaTailor uses the earliest cue\-in setting from the two specifications\.

  Example

  ```
  #EXT-X-DATERANGE:ID="splice-6FFFFFF0",START-DATE="2019-01T00:15:00Z\",DURATION=60.000,SCTE35-OUT=0xF
      ...
      #EXT-X-DATERANGE:ID="splice-6FFFFFF0",START-DATE="2019-01T00:15:00Z\",SCTE35-IN=0xF
  ```

## EXT\-x\-SPLICEPOINT\-SCTE35<a name="hls-ad-markers-splice"></a>

You append the `EXT-X-SPLICEPOINT-SCTE35` ad marker tag with a SCTE\-35 payload in base64\-encoded binary\. The decoded binary must provide a SCTE\-35 `splice_info_section` containing the cue\-out marker `0x34`, for provider placement opportunity start, and the cue\-in marker `0x35`, for provider placement opportunity end\. 

The following example shows the splice point specification with base64\-encoded binary payloads that specify the cue\-out and cue\-in markers\. 

```
    #EXT-X-SPLICEPOINT-SCTE35:/DA9AAAAAAAAAP/wBQb+uYbZqwAnAiVDVUVJAAAKqX//AAEjW4AMEU1EU05CMDAxMTMyMjE5M19ONAAAmXz5JA==
    ...
    #EXT-X-SPLICEPOINT-SCTE35:/DA4AAAAAAAAAP/wBQb+tTeaawAiAiBDVUVJAAAKqH+/DBFNRFNOQjAwMTEzMjIxOTJfTjUAAIiGK1s=
```