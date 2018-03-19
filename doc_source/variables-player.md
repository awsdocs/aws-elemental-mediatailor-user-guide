# Player Data<a name="variables-player"></a>

To send data from the player to the ADS, use `player_params.<query_parameter_name>` variables in the template ADS URL\. For example, if the player sends a query parameter named `user_id` in its request to AWS Elemental MediaTailor and you need to pass that data in the ADS request, then include `[player_params.user_id]` anywhere in the ADS URL configuration\. 

When AWS Elemental MediaTailor receives a manifest request from the player, it URL\-decodes the values of the query parameters in the player request once and substitutes the values of the parameters into the variables in the ADS request URL\. If your ADS is expecting a URL\-encoded value as the query parameter \(instead of a URL\-decoded value\), then you must URL\-encode the value from the player twice\.

This functionality allows you to control the query parameters that are included in the ADS request\. The most common methods of control are as follows:

+ Dynamically adding query parameters to the ADS request

+ Passing arbitrary key\-value pairs as the value of a special query parameter that your ADS recognizes

The following sections provide more information about these methods\.

## Adding Query Parameters<a name="adding-query-params"></a>

To take data from a query parameter provided by the player and include it as a query parameter in the ADS request, do the following:

1. URL\-encode the key\-value pairs on the player\.

1. Pass the pairs to AWS Elemental MediaTailor as the value of a single query parameter\.

1. Reference the player parameter in the ADS request URL configuration\.

The following example shows how this is done\.

**Note**  
For client\-side reporting, you don't need to URL\-encode query strings in the JSON object of the session initiation request\. Query strings are passed through as\-is from the JSON object\.

Pairs:

+ *param1* with a value of *value1:*

+ *param2* with a value of *value2:*

**To add query parameters**

1. URL\-encode the pairs\.

   The decoded representation of the values that must be sent to the ADS is `param1=value1:&param2=value2:`, so the URL\-encoded representation is `param1=value1%3A&param2=value2%3A`\.

1. Pass the URL\-encoded pairs to AWS Elemental MediaTailor\.

   The request is some variation of the following:

   ```
   <masterAssetID>.m3u8?ads.param1=value1%3A&ads.param2=value2%3A
   ```

1. In AWS Elemental MediaTailor, make sure the ADS request URL references the parameter, such as the following:

   ```
   https://my.ads.com/path?param1=[player_params.param1]&param2=[player_params.param2]
   ```

1. AWS Elemental MediaTailor decodes the parameter when the player request is received\. AWS Elemental MediaTailor sends the following request to the ADS:

   ```
   https://my.ads.com/<path>?param1=value1:&param2=value2:
   ```

   In this way, the `param1` and `param2` key\-value pairs are included as first\-class query parameters in the ADS request\.

## Advanced Usage<a name="advanced-usage"></a>

You can customize the ADS request in many ways with player parameterization\. The only requirement is that the ADS hostname be included\.

Customization examples:

+ Concatenate player\_params and session parameters to create new parameters\. Example: 

  ```
  ?key1=[player_params.value1][session.id]
  ```

+ Use a player parameter as part of a path element\. Example:

  ```
  https://my.ads.com/[player_params.path]?key=value
  ```

+ Use player parameters to pass both path elements and keys themselves, rather than just values\. Example: 

  ```
  https://my.ads.com/[player_params.path]?[player_params.key1]=[player_params.value1]
  ```