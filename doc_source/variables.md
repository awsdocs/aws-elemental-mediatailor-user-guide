# Dynamic Ad Variables in AWS Elemental MediaTailor<a name="variables"></a>

The AWS Elemental MediaTailor request to the ad decision server \(ADS\) includes information about the current viewing session, which helps the ADS choose the best ads to provide in its response\. In your ADS request configuration, you specify the query parameters to use to convey the viewing session information\. 

The query parameters take the following forms:
+ **Static values** – values that don't change from one session to the next\. For example, the response type that MediaTailor expects from the ADS\.
+ **Session data** – dynamic values that are provided by MediaTailor for each session, for example, the session ID\. For details, see [Session Data](variables-session.md)\. 
+ **Player data** – dynamic values that are provided by the player for each session\. These describe the content viewer and help the ADS to determine which ads MediaTailor should stitch into the stream\. For details, see [Player Data](variables-player.md)\.

## Passing Parameters to the ADS<a name="passing-paramters-to-the-ads"></a>

**To pass session and player information to the ADS**

1. Work with the ADS to determine the information that it needs so that it can respond to an ad query from AWS Elemental MediaTailor\.

1. Create a configuration in MediaTailor that uses a template ADS request URL that satisfies the ADS requirements\. In the URL, include static parameters and include placeholders for dynamic parameters\. Enter your template URL in the configuration's **Ad decision server** field\. 

   In the following example template URL, `correlation` provides session data, and `user` provides player data:

   ```
   https://my.ads.server.com/path?correlation=[session.id]&user=[player_params.userID]
   ```

1. On the player, configure the session initiation request for AWS Elemental MediaTailor to provide parameters for the player data\. Include your parameters in the session initiation request, and omit them from subsequent requests for the session\. 

   The type of call that the player makes to initialize the session determines whether the player \(client\) or MediaTailor \(server\) provides ad\-tracking reporting for the session\. For information about these two options, see [Ad Tracking Reporting in AWS Elemental MediaTailor](ad-reporting.md)\. 

   Make one of the following types of calls, depending on whether you want server\- or client\-side ad\-tracking reporting\. In both of the example calls, `userID` is intended for the ADS and `auth_token` is intended for the origin:
   + \(Option\) Call for server\-side ad\-tracking reporting – Prefix parameters for the ADS with `ads` and omit the `ads` prefix in the parameters that you want MediaTailor to send to the origin server:   
**Example**  

     ```
     GET master.m3u8?ads.userID=xyzuser&auth_token=kjhdsaf7gh
     ```
   + \(Option\) Call for client\-side ad\-tracking reporting – Provide parameters for the ADS inside an `adsParams` object\. Provide parameters that you want MediaTailor to send to the origin server as top\-level objects: 

     ```
     POST master.m3u8
         {
             "adsParams": {
                "userID": "xyzuser"
            }
            "auth_token": "kjhdsaf7gh"
         }
     ```

When the player initiates a session, AWS Elemental MediaTailor replaces the variables in the template ADS request URL with the player `ads` data and the session data\. It passes the remaining parameters from the player to the origin server\.

The following examples show calls from AWS Elemental MediaTailor that correspond to the preceding player's session initialization call examples\. MediaTailor calls the ADS with session data and the player's user ID, and calls the origin server with the player's authorization token:

```
https://my.ads.server.com/path?correlation=896976764&user=xyzuser
```

```
https://my.origin.server.com/master.m3u8?auth_token=kjhdsaf7gh
```

The following sections provide details for configuring session and player data\.