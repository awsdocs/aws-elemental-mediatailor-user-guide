# Server\-side Reporting<a name="ad-reporting-server-side"></a>

AWS Elemental MediaTailor defaults to server\-side reporting: the service sends reports to the ad tracking URL directly when the player requests an ad URL from the playlist manifest\. After the player initializes a playback session with AWS Elemental MediaTailor, no further input is required from you or the player to perform server\-side reporting\. As ads are played back, AWS Elemental MediaTailor sends beacons to the ad server to report how much of the ad is viewed\. Beacons track the start of an ad, ad progression in quartiles \(first, midpoint, and third\), and when an ad is viewed to completion\.

**To perform server\-side ad reporting**

1. From the player, initialize a new AWS Elemental MediaTailor playback session using a request in the following format:

   ```
   GET <mediatailorURL>/v1/master/<hashed-account-ID>/<originID>/<assetID>?ads.<key-value-pairs>
   ```

   where `<key-value-pairs>` are the targeting parameters for ad tracking\. For information about adding parameters to the request, see [Adding Query Parameters](variables-player.md#adding-query-params)\.

1. AWS Elemental MediaTailor responds to the request with the master manifest URL\. The master manifest includes URLs for the media playlists\. Links for ad segment requests are embedded in the media playlists\.

1. When the player requests playback from an ad segment URL \(`/v1/segment` path\), AWS Elemental MediaTailor sends the appropriate beacon \(start, complete, and quartiles\) to the ad server through the ad tracking URLs\. At the same time, the service issues a redirect to the actual \*\.ts ad segment either in the Amazon CloudFront distribution where AWS Elemental MediaTailor stores transcoded ads, or in the content distribution network \(CDN\) where you have cached the ad\. 

   AWS Elemental MediaTailor sends a beacon each time a player makes a request to the `/v1/segment` URL\. If the player has to make multiple requests to the same URL \(in conditions such as network degradation\), the service also sends multiple beacons\. To avoid this duplication, use a CDN in front of AWS Elemental MediaTailor to cache the `/v1/segment` URL path \(as described in [Integrating AWS Elemental MediaTailor and a CDN](integrating-cdn-standard.md)\), or consider client\-side reporting \(as described in [Client\-side Reporting](ad-reporting-client-side.md)\)\.