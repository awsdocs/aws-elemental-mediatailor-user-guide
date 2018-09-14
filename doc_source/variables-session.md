# Session Data<a name="variables-session"></a>

AWS Elemental MediaTailor generates data about each playback session\. It uses this data when making a request to the ADS, to provide values for `session` and `avail` query parameters in the template ADS request URL\. You can also concatenate multiple variables to create a value\. 

You can use the following variables in the template ADS request URL:
+ **\[session\.avail\_duration\_ms\]** – the duration in milliseconds of the ad availability slot that is being requested\. MediaTailor obtains the duration value from the input manifest’s `#EXT-X-CUE-OUT: DURATION` or from values in the `#EXT-X-DATERANGE` tag\. If the input manifest has a null, invalid, or 0 duration for the ad break in those tags, MediaTailor uses a default value of 300,000 ms\.
+ **\[session\.avail\_duration\_secs\]** – the duration in seconds of the ad availability slot that is being requested\. MediaTailor obtains the duration value from the input manifest’s `#EXT-X-CUE-OUT: DURATION` or from values in the `#EXT-X-DATERANGE` tag\. If the input manifest has a null, invalid, or 0 duration for the ad break in those tags, MediaTailor uses a default value of 300 seconds\.
+ **\[session\.client\_ip\]** – the remote IP address that the MediaTailor request came from\. If the `X-forwarded-for` header is set, then that value is what MediaTailor uses for the `client_ip`\.
+ **\[session\.id\]** – a unique numeric identifier for the current playback session\. All requests that a player makes during a session have the same value for this field, so it can be used for ADS fields that are intended to correlate requests for a single viewing\.
+ **\[session\.referer\]** – usually, the URL of the page that is hosting the video player\. This variable is set to the value of the `Referer` header that the player uses in its request to MediaTailor\. If the player doesn't include this header, the **\[session\.referer\]** value is empty\. If you're using a CDN or proxy in front of the manifest endpoint, you must proxy the correct header from the player here\.
+ **\[session\.user\_agent\]** – the `User-Agent` header that MediaTailor received from the player’s session initialization request\. If you're using a CDN or proxy in front of the manifest endpoint, you must proxy the correct header from the player here\.
+ **\[session\.uuid\]** – alternative to **\[session\.id\]**\. This is a unique identifier for the current playback session, such as the following: 

  ```
  e039fd39-09f0-46b2-aca9-9871cc116cde
  ```
+ **\[avail\.random\]** – a random number between 0 and 10,000,000,000 that MediaTailor generates for each request to the ADS\. Some ad servers use this parameter to enable features such as separating ads from competing companies\.
+ **\[event\_id\]** – the value parsed from the SCTE\-35 field `splice_event_id` as a long number\. MediaTailor uses this value to designate linear ad break numbers or to populate ad server query strings, like ad pod positions\.
+ **\[avail\_num\]** – the value parsed from the SCTE\-35 field `avail_num`\. MediaTailor can use this value to designate linear ad break numbers\.

**Example Examples**  
If the ADS requires a query parameter named `deviceSession` to be passed with the unique session identifier, the template ADS URL in AWS Elemental MediaTailor could look like the following:  

```
https://my.ads.server.com/path?deviceSession=[session.id]
```
AWS Elemental MediaTailor automatically generates a unique identifier for each stream, and enters the identifier in place of `session.id`\. If the identifier is `1234567`, the final request that MediaTailor makes to the ADS would look something like this:  

```
https://my.ads.server.com/path?deviceSession=1234567
```