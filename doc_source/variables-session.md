# Using session variables<a name="variables-session"></a>

To configure AWS Elemental MediaTailor to send session data to the ADS, in the template ADS URL, specify one or more of the variables listed in this section\. You can use individual variables, and you can concatenate multiple variables to create a single value\. MediaTailor generates some values and obtains the rest from sources like the manifest and the player's session initialization request\. 

You can use the following session data variables in your template ADS request URL configuration: 
+ **\[avail\.index\]** – A number that represents the position of an ad avail in an index\. At the start of a playback session, MediaTailor creates an index of all the ad avails in a manifest and stores the index for the remainder of the session\. When MediaTailor makes a request to the ADS to fill the avail, it includes the ad avail index number\. This parameter enables the ADS to improve ad selection by using features like competitive exclusion and frequency capping\.
+ **\[avail\.random\]** – A random number between 0 and 10,000,000,000 that MediaTailor generates for each request to the ADS\. Some ad servers use this parameter to enable features such as separating ads from competing companies\.
+ **\[scte\.avail\_num\]** – The value parsed by MediaTailor from the SCTE\-35 field `avail_num`\. MediaTailor can use this value to designate linear ad avail numbers\.
+ **\[scte\.event\_id\]** – The value parsed by MediaTailor from the SCTE\-35 field `splice_event_id`, as a long number\. MediaTailor uses this value to designate linear ad avail numbers or to populate ad server query strings, like ad pod positions\.
+ **\[scte\.segmentation\_upid\]** – Corresponds to the SCTE\-35 `segmentation_upid` element\. The `segmentation_upid` element contains `segmentation_upid_type` and `segmentation_upid_length`\.

  MediaTailor supports the following `segmentation_upid` types:
  + **ADS Information \(0xE\)** \- Advertising information\. For more information, see the **segmentation\_upid** description in section 10\.3\.3\.1 of the SCTE\-35 specification\.
  + **Managed Private UPID \(0xC\) ** \- The Managed Private UPID \(MPU\) structure as defined in SCTE\-35, section 10\.3\.3\.3\. MediaTailor supports binary or DASH XML SCTE representations\.

    You can use this structure in a podbuster workflow\. To do so, specify `0xC` \(MPU\) as the `segmentation_upid_type`, and include the following parameters in the `private_data` attribute:

    ```
    0xC{"assetId":"my_program","cueData":{"cueType":"theAdType","key":"pb","value":"123456"}}
    ```

     MediaTailor parses the values from the preceding JSON, and passes them into the `scte.segmentation_upid.assetId`, `scte.segmentation_upid.cueData.key`, and `scte.segmentation_upid.cueData.value` dynamic variables\. 
  + **User Defined \(0x1\) ** \- A user defined structure\. For more information, see the **segmentation\_upid** description in section 10\.3\.3\.1 of the SCTE\-35 specification\.
+ **\[scte\.segmentation\_upid\.assetId\]** – Used in conjunction with the Managed Private UPID \(0xC\) `segmentation_ upid_type` for podbuster workflows\. MediaTailor derives this value from the `assetId` parameter in the MPU's `private_date` JSON structure\. For more information, see [](#podbuster-workflow)\.
+ **\[scte\.segmentation\_upid\.cueData\.key\]** – Used in conjunction with the Managed Private UPID \(0xC\) `segmentation_ upid_type` for podbuster workflows\. MediaTailor derives this value from the `cueData.key` parameter in the MPU's `private_date` JSON structure\. For more information, see [](#podbuster-workflow)\.
+ **\[scte\.segmentation\_upid\.cueData\.value\]** – Used in conjunction with the Managed Private UPID \(0xC\) `segmentation_ upid_type` for podbuster workflows\. MediaTailor derives this value from the `cueData.key` parameter in the MPU's `private_date` JSON structure\. For more information, see [](#podbuster-workflow)\.
+ **\[scte\.unique\_program\_id\]** – The value parsed by MediaTailor from the SCTE\-35 `splice_insert` field `unique_program_id`\. The ADS uses the unique program ID \(UPID\) to provide program\-level ad targeting for live linear streams\. If the SCTE\-35 command is not splice insert, MediaTailor sets this to an empty value\.
+ **\[session\.avail\_duration\_ms\]** – The duration in milliseconds of the ad availability slot\. The default value is 300,000 ms\. AWS Elemental MediaTailor obtains the duration value from the input manifest as follows: 
  + For HLS, MediaTailor obtains the duration from the `#EXT-X-CUE-OUT: DURATION` or from values in the `#EXT-X-DATERANGE` tag\. If the input manifest has a null, invalid, or 0 duration for the ad avail in those tags, MediaTailor uses the default\. 
  + For DASH, MediaTailor obtains the duration value from the event duration, if one is specified\. Otherwise, it uses the default value\. 
+ **\[session\.avail\_duration\_secs\]** – The duration in seconds of the ad availability slot, or ad avail\. MediaTailor determines this value the same way it determines `[session.avail_duration_ms]`\.
+ **\[session\.client\_ip\]** – The remote IP address that the MediaTailor request came from\. If the `X-forwarded-for` header is set, then that value is what MediaTailor uses for the `client_ip`\.
+ **\[session\.id\]** – A unique numeric identifier for the current playback session\. All requests that a player makes for a session have the same id, so it can be used for ADS fields that are intended to correlate requests for a single viewing\.
+ **\[session\.referer\]** – Usually, the URL of the page that is hosting the video player\. MediaTailor sets this variable to the value of the `Referer` header that the player used in its request to MediaTailor\. If the player doesn't provide this header, MediaTailor leaves the **\[session\.referer\]** empty\. If you use a CDN or proxy in front of the manifest endpoint and you want this variable to appear, proxy the correct header from the player here\.
+ **\[session\.user\_agent\]** – The `User-Agent` header that MediaTailor received from the player's session initialization request\. If you're using a CDN or proxy in front of the manifest endpoint, you must proxy the correct header from the player here\.
+ **\[session\.uuid\]** – Alternative to **\[session\.id\]**\. This is a unique identifier for the current playback session, such as the following: 

  ```
  e039fd39-09f0-46b2-aca9-9871cc116cde
  ```

**Examples**  
If the ADS requires a query parameter named `deviceSession` to be passed with the unique session identifier, the template ADS URL in AWS Elemental MediaTailor could look like the following:  

```
https://my.ads.server.com/path?deviceSession=[session.id]
```
AWS Elemental MediaTailor automatically generates a unique identifier for each stream, and enters the identifier in place of `session.id`\. If the identifier is `1234567`, the final request that MediaTailor makes to the ADS would look something like this:  

```
https://my.ads.server.com/path?deviceSession=1234567
```