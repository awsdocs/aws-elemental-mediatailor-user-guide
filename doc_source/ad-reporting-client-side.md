# Client\-side Reporting<a name="ad-reporting-client-side"></a>

With client\-side reporting, AWS Elemental MediaTailor proxies the ad tracking URL to the client player\. The player then performs all ad\-tracking activities\. Client\-side reporting enables functionality like trick play for VOD \(players display visual feedback during fast forward and rewind\) and other advanced playback behavior during ad breaks that requires player development \(like no skip\-forward and countdown timers on ad breaks\)\.

**To perform client\-side ad reporting**

1. From the player, initialize a new AWS Elemental MediaTailor playback session using a request in the following JSON format:

   ```
   POST <mediatailorURL>/v1/session/<hashed-account-ID>/<originID>/<assetID>
       {
         
           adsParams: {
              param1: "value1",
              param2: "value2",
              param3: "value3",
          }
       }
   ```

   where:

   + `adsParams` are values that AWS Elemental MediaTailor has to use in the request to the ADS\. Define the `adsParams` parameters as `[player_params.param]` in the ADS template URL in the AWS Elemental MediaTailor configuration, as described in [Step 3: Configure ADS Request URL and Query Parameters](getting-started.md#getting-started-configure-request)\.

   + any other query parameters are forwarded to your origin server\.

1. AWS Elemental MediaTailor responds to the request with two URLs, one for the manifest and one for the tracking endpoint:

   + Manifest – used to retrieve content playlists and ad segments

     Example: `<mediatailorURL>/v1/master/<hashed-account-id>/<originID>/<assetID>?aws.sessionID=<session>`

   + Tracking – used to poll for upcoming ad breaks

     Example: `<mediatailorURL>/v1/tracking/<hashed-account-id>/<originID>/<assetID>/<session>`

1. The player should periodically poll the tracking URL\. When an ad is coming, the AWS Elemental MediaTailor response to the player's request to the tracking URL contains a JSON object with the time offsets for the ad breaks\. These offsets are relative to when the player initiated the session\. You can use them when programming specific behaviors in the player \(such as preventing the viewer from skipping past the ads\)\. The response also includes duration, timing, and identification information\. These are the values included in the response:

   + `adID`: HLS sequence number associated with the beginning of this ad

   + `duration`: length in ISO 8601 seconds format\. The response includes durations for the entire ad break, as well as for each ad and beacon \(though beacon durations are always zero\)\.

   + `durationInSeconds`: length in seconds format\. The response includes durations for the entire ad break, as well as for each ad and beacon \(though beacon durations are always zero\)\.

   + `startTime`: time position in ISO 8601 seconds format, relative to the beginning of the playback session\. The response includes start times for the entire ad break, as well as for each ad and beacon\.

   + `startTimeInSeconds`: time position in seconds format, relative to the beginning of the playback session\. The response includes start times for the entire ad break, as well as for each ad and beacon\.

   + `beaconUrls`: where each beacon is sent

   + `eventId`: HLS sequence number associated with the beacon

   + `eventType`: type of beacon

   + `availId`: HLS sequence number associated with the start of the ad break

   Here is an example response:

   ```
   {
     "avails": [
       {
         "ads": [
           {
             "adId": "8104385",
             "duration": "PT15.100000078S",
             "durationInSeconds": 15.1,
             "startTime": "PT17.817798612S",
             "startTimeInSeconds": 17.817,
             "trackingEvents": [
   		  {
                 "beaconUrls": [
                   "http://<mediatailorELB>/tracking?event=impression"
                 ],
                 "duration": "PT15.100000078S",
                 "durationInSeconds": 15.1,
                 "eventId": "8104385",
                 "eventType": "impression",
                 "startTime": "PT17.817798612S",
                 "startTimeInSeconds": 17.817
               },
               {
                 "beaconUrls": [
                   "http://<mediatailorELB>/tracking?event=start"
                 ],
                 "duration": "PT0S",
                 "durationInSeconds": 0.0,
                 "eventId": "8104385",
                 "eventType": "start",
                 "startTime": "PT17.817798612S",
                 "startTimeInSeconds": 17.817
               },
   			{
                 "beaconUrls": [
                   "http://<mediatailorELB>/tracking?event=firstQuartile"
                 ],
                 "duration": "PT0S",
                 "durationInSeconds": 0.0,
                 "eventId": "8104386",
                 "eventType": "firstQuartile",
                 "startTime": "PT21.592798631S",
                 "startTimeInSeconds": 21.592
               },
   			 {
                 "beaconUrls": [
                   "http://<mediatailorELB>/tracking?event=midpoint"
                 ],
                 "duration": "PT0S",
                 "durationInSeconds": 0.0,
                 "eventId": "8104387",
                 "eventType": "midpoint",
                 "startTime": "PT25.367798651S",
                 "startTimeInSeconds": 25.367
               },
               {
                 "beaconUrls": [
                   "http://<mediatailorELB>/tracking?event=thirdQuartile"
                 ],
                 "duration": "PT0S",
                 "durationInSeconds": 0.0,
                 "eventId": "8104388",
                 "eventType": "thirdQuartile",
                 "startTime": "PT29.14279867S",
                 "startTimeInSeconds": 29.142
               },
               {
                 "beaconUrls": [
                   "http://<mediatailorELB>/tracking?event=complete"
                 ],
                 "duration": "PT0S",
                 "durationInSeconds": 0.0,
                 "eventId": "8104390",
                 "eventType": "complete",
                 "startTime": "PT32.91779869S",
                 "startTimeInSeconds": 32.917
               }
             ]
           }
         ],
         "availId": "8104385",
         "duration": "PT15.100000078S",
         "durationInSeconds": 15.1,
         "meta": null,
         "startTime": "PT17.817798612S",
         "startTimeInSeconds": 17.817
       }
     ]
   }
   ```