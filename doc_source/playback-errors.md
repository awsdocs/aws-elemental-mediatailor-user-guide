# Troubleshooting playback errors returned by MediaTailor<a name="playback-errors"></a>

This section provides information about the HTTP error codes that you might receive while testing your player software and during the normal processing of player requests\. 

**Note**  
You might also receive errors from the AWS Elemental MediaTailor API, during configuration operations like `PutPlaybackConfiguration` and `GetPlaybackConfiguration`\. For information about those types of errors, see the [AWS Elemental MediaTailor API Reference](https://docs.aws.amazon.com/mediatailor/latest/apireference/)\. 

When your player sends a request to AWS Elemental MediaTailor, either directly or through a CDN, MediaTailor responds with a status code\. If MediaTailor successfully handles the request, it returns the HTTP status code `200 OK`, indicating success, along with the populated manifest\. If the request is unsuccessful, MediaTailor returns an HTTP status code, an exception name, and an error message\. 

AWS Elemental MediaTailor returns two classes of errors:
+ **Client errors** – errors that are usually caused by a problem in the request itself, like an improperly formatted request, an invalid parameter, or a bad URL\. These errors have an HTTP `4xx` response code\.
+ **Server errors** – errors that are usually caused by a problem with MediaTailor or one of its dependencies, like the ad decision server \(ADS\) or the origin server\. These errors have an HTTP `5xx` response code\.

**Topics**
+ [Client playback errors returned by AWS Elemental MediaTailor](#playback-errors-client)
+ [Server playback errors returned by AWS Elemental MediaTailor](#playback-errors-server)
+ [Playback error examples](#playback-errors-examples)

## Client playback errors returned by AWS Elemental MediaTailor<a name="playback-errors-client"></a>

General guidance: 
+ You can find detailed information for most errors in the headers and body of the response\.
+ For some errors, you need to check your configuration settings\. You can retrieve the settings for your playback configuration from AWS Elemental MediaTailor\. For the API, the resource is `GetPlaybackConfiguration/Name`\. For details, see the [AWS Elemental MediaTailor API Reference](https://docs.aws.amazon.com/mediatailor/latest/apireference/)\. 

The following table lists the client error codes that are returned by the manifest manipulation activities of AWS Elemental MediaTailor, probable causes, and actions you can take to resolve them\.


| Code  | Exception name  | Meaning | What to do | 
| --- | --- | --- | --- | 
| 400 | BadRequestException | MediaTailor is unable to service the request due to one or more errors in formatting or content\. A parameter might be improperly formatted, or the request might contain an invalid playback configuration or session ID\. | Check that your request is properly formatted and contains accurate information\. Make sure that the playback endpoint setting on the player matches the ManifestEndpointPrefix setting returned by GetPlaybackConfiguration\. Retry your request\.  | 
| 403 | AccessDeniedException | The host header provided in the request doesn't match the manifest endpoint prefix that is configured in the MediaTailor playback URL\. Your CDN might be misconfigured\.  | Check your CDN settings and make sure that you are using the correct manifest endpoint prefix for MediaTailor\. Retry your request\.  | 
| 404 | NotFoundException | MediaTailor is unable to find the information specified\. Possible reasons include a URL that doesn't map to anything in the service, a configuration that isn't defined, or a session that is unavailable\. | Check your configuration and the validity of your request, and then reinitialize the session\.  | 
| 409 | ConflictException | A player tried to load multiple playlists simultaneously for a single session\. As a result, MediaTailor detected a session consistency conflict\. This problem occurs for HLS players\. | Make sure that your player requests playlists one at a time\. This is in accordance with the HLS specification\.  | 
| 410 | Gone | An AWS Support operator has blocked a player session or customer configuration\. AWS Support does this in rare circumstances when we detect a very high volume of 4xx requests coming from errant traffic for a single session or configuration\.  | If you think that the request shouldn't be blocked, contact [AWS Support](https://aws.amazon.com/premiumsupport/)\. They can look into it and remove the blocking filter, if appropriate\.  | 

 If you need further assistance, contact [AWS Support](https://aws.amazon.com/premiumsupport/)\.

## Server playback errors returned by AWS Elemental MediaTailor<a name="playback-errors-server"></a>

General guidance: 
+ You can find detailed information for most errors in the headers and body of the response\.
+ For some errors, you need to check your configuration settings\. You can retrieve the settings for your playback configuration from AWS Elemental MediaTailor\. For the API, the resource is `GetPlaybackConfiguration/Name`\. For details, see the [AWS Elemental MediaTailor API Reference](https://docs.aws.amazon.com/mediatailor/latest/apireference/)\. 

The following table lists the server error codes returned by the manifest manipulation activities of AWS Elemental MediaTailor, probable causes, and actions you can take to resolve them\.


| Code | Exception name | Meaning | What to do | 
| --- | --- | --- | --- | 
| 500 | InternalServiceError | Unhandled exception\.  | Retry the request\. If the problem persists, check the reported health of MediaTailor for your AWS Region at [https://status.aws.amazon.com/](https://status.aws.amazon.com/)\. | 
| 502 | BadGatewayException | Either the origin server address or the ad decision server \(ADS\) address is invalid\. Examples of invalid addresses are a private IP address and localhost\.  | Make sure that your configuration has the correct settings for your ADS and origin server, and then retry the request\.  | 
| 502 | UnsupportedManifestException | Either the origin manifest has changed so that MediaTailor can't personalize it or MediaTailor doesn't support the origin's manifest format\.  | This might affect only the individual session\. Reinitialize the session\. You can usually accomplish this by refreshing the page in the viewer\. If the problem persists, verify that MediaTailor supports the origin's manifest format\. For information, see [Integrating a content source](integrating-origin.md)\.  | 
| 503 | LoadShed | MediaTailor experienced a resource constraint while servicing your request\. | Retry the request\. If the problem persists, check the reported health of MediaTailor for your AWS Region at [https://status.aws.amazon.com/](https://status.aws.amazon.com/)\. | 
| 503 | ThrottlingException | Your transactions per second have reached your quota, and MediaTailor is throttling your use\.  | Retry the request\. You can also check the reported health of MediaTailor for your AWS Region at [https://status.aws.amazon.com/](https://status.aws.amazon.com/)\. You might want to increase the quota on your transactions per second\. For more information, see [Quotas on ad insertion](quotas.md#ad-insertion-quotas)\.  | 
| 504 | GatewayTimeoutException | A timeout occurred while MediaTailor was contacting the origin server\.  | Retry the request\. If the problem persists, check the health of the origin server and make sure the origin server is responding within the content origin server timeout that is listed at [Quotas on ad insertion](quotas.md#ad-insertion-quotas)\. | 

 If you need further assistance, contact [AWS Support](https://aws.amazon.com/premiumsupport/)\.

## Playback error examples<a name="playback-errors-examples"></a>

This section lists some examples of the playback errors that you might see in command line interactions with AWS Elemental MediaTailor\. 

The following example shows the result when a timeout occurs between AWS Elemental MediaTailor and either the ad decision server \(ADS\) or the origin server\.

```
~[]> curl -vvv https://111122223333444455556666123456789012.mediatailor.us-west-2.amazonaws.com/v1/master/123456789012/Multiperiod_DASH_Demo/index.mpd
*   Trying 54.186.133.224...
* Connected to 111122223333444455556666123456789012.mediatailor.us-west-2.amazonaws.com (11.222.333.444) port 555 (#0)
* TLS 1.2 connection using TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
* Server certificate: mediatailor.us-west-2.amazonaws.com
* Server certificate: Amazon
* Server certificate: Amazon Root CA 1
* Server certificate: Starfield Services Root Certificate Authority - G2
> GET /v1/master/123456789012/Multiperiod_DASH_Demo/index.mpd HTTP/1.1
> Host: 111122223333444455556666123456789012.mediatailor.us-west-2.amazonaws.com
> User-Agent: curl/7.43.0
> Accept: */*
>
< HTTP/1.1 504 Gateway Timeout
< Date: Thu, 29 Nov 2018 18:43:14 GMT
< Content-Type: application/json
< Content-Length: 338
< Connection: keep-alive
< x-amzn-RequestId: 123456789012-123456789012
< x-amzn-ErrorType: GatewayTimeoutException:http://internal.amazon.com/coral/com.amazon.elemental.midas.mms.coral/
<
* Connection #0 to host 111122223333444455556666123456789012.mediatailor.us-west-2.amazonaws.com left intact
{"message":"failed to generate manifest: Unable to obtain template playlist. origin URL:[https://777788889999.mediapackage.us-west-2.amazonaws.com/out/v1/444455556666111122223333/index.mpd], asset path: [index.mpd], sessionId:[123456789012123456789012] customerId:[123456789012]"}%
```