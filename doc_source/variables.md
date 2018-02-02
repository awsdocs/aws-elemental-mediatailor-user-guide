# Dynamic Ad Variables in AWS Elemental MediaTailor<a name="variables"></a>

The AWS Elemental MediaTailor request to the ad decision server \(ADS\) carries information about the current viewing session\. Use query parameters in the ADS request URL to convey this information and to help the ADS configure an appropriate response to the AWS Elemental MediaTailor request\. Parameters take the following forms:

+ Static – values do not change from one session to the next\. Static parameters typically capture information such as the response type that AWS Elemental MediaTailor expects from the ADS\.

+ Dynamic from AWS Elemental MediaTailor \(session data\) – AWS Elemental MediaTailor supplies unique parameter values for each session\. The session ID is a common dynamic variable that AWS Elemental MediaTailor provides\.

+ Dynamic from the player \(player data\) – the player supplies unique parameter values for each session\. Player\-supplied values describe the viewer and help the ADS to determine which ads AWS Elemental MediaTailor stitches into the stream\.

**To pass session and player information to the ADS**

1. Work with the ADS to determine the information that it needs to respond to an ad query from AWS Elemental MediaTailor\.

1. Create a configuration in AWS Elemental MediaTailor using a template ADS request URL that includes static parameters and placeholders for dynamic parameters\. Session data is represented in `session` parameters, and player data is represented in `player_params` parameters\. Use this template URL in the AWS Elemental MediaTailor **Ad decision server** field\.   
**Example**  

   In the following example, `correlation` is session data \(the session ID\), and `user` is player data \(the user ID\):

   ```
   https://my.ads.server.com/path?correlation=[session.id]&user=[player_params.userID]
   ```

1. Configure the request to AWS Elemental MediaTailor from the player to include the necessary player data\. To identify the parameters for the ADS, use the `ads.` prefix before all ADS information\. AWS Elemental MediaTailor passes any variables that are not preceded with `ads.` to the origin server\. 

   Include parameters in the session initiation request only\. They're not needed in subsequent requests to AWS Elemental MediaTailor for this session\.  
**Example**  

   In the following example request to AWS Elemental MediaTailor, `userID` goes to the ADS and `auth_token` goes to the origin server: 

   ```
   GET master.m3u8?ads.userID=xyzuser&auth_token=kjhdsaf7gh
   ```

1. When the player initiates a session, AWS Elemental MediaTailor replaces the variables in the template ADS request URL with session and player data\. The remaining parameters are passed to the origin server\.  
**Example**  

   In the following examples, session and player data is sent to the ADS\. The authorization token is sent to the origin server:

   ```
   https://my.ads.server.com/path?correlation=896976764&user=xyzuser
   ```

   ```
   https://my.origin.server.com/master.m3u8?auth_token=kjhdsaf7gh
   ```

The following sections describe how to configure session and player data\.


+ [Session Data](variables-session.md)
+ [Player Data](variables-player.md)