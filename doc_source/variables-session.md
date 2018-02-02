# Session Data<a name="variables-session"></a>

AWS Elemental MediaTailor generates data about each playback session\. AWS Elemental MediaTailor replaces the `session` and `avail` query parameters in the template ADS request URL with this data when making a request to the ADS\. You can also concatenate multiple variables together to achieve the value that you want\. 

You can use the following variables in the template ADS request URL:

+ **\[session\.id\]** – unique numeric identifier for the current playback session\. All requests that a player makes during a session have the same value for this field, so it can be used for ADS fields that are intended to correlate requests for a single viewing\.

+ **\[session\.uuid\]** – alternative to **\[session\.id\]**\. This is a unique identifier for the current playback session, such as the following: 

  `e039fd39-09f0-46b2-aca9-9871cc116cde`

+ **\[session\.referer\]** – usually, the URL of the page that is hosting the video player\. This variable is set to the value of the `Referer` header that the player uses in its request to AWS Elemental MediaTailor\. If the player doesn't include this header, the **\[session\.referer\]** value is empty\. If you're using a CDN or proxy in front of the manifest endpoint, you must proxy the correct header from the player here\.

+ **\[session\.user\_agent\]** – the `User-Agent` header that AWS Elemental MediaTailor received from the player’s session initialization request\. If you're using a CDN or proxy in front of the manifest endpoint, you must proxy the correct header from the player here\.

+ **\[session\.client\_ip\]** – the remote IP address that the AWS Elemental MediaTailor request came from\. If the `X-forwarded-for` header is set, then that value is what AWS Elemental MediaTailor uses for the `client_ip`\.

+ **\[session\.avail\_duration\_secs\]** – the duration in seconds of the ad availability slot that is being requested\.

+ **\[session\.avail\_duration\_ms\]** – the duration in milliseconds of the ad availability slot that is being requested\.

+ **\[avail\.random\]** – a random number between 0 and 10000000000 that AWS Elemental MediaTailor generates for each request to the ADS\. Some ad servers use this parameter to enable features such as separating ads from competing companies\.

+ **\[avail\_num\]** – the value parsed from the SCTE\-35 field `avail_num`\. AWS Elemental MediaTailor can use this value to designate linear ad break numbers\.

**Example**  
If the ADS requires a query parameter named `correlator` to be passed with the unique session identifier, the template ADS URL in AWS Elemental MediaTailor could look like this:  
**https://my\.ads\.server\.com/path?correlator=\[session\.id\]**  
AWS Elemental MediaTailor automatically generates a unique identifier for each stream, and enters the identifier in place of `session.id`\. If the identifier is 1234567, the final request that AWS Elemental MediaTailor makes to the ADS would look something like this:  
**https://my\.ads\.server\.com/path?correlator=1234567**