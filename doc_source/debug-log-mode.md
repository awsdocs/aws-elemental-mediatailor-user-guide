# Debug log mode<a name="debug-log-mode"></a>

Use debug logs to troubleshoot playback session issues\. To generate debug logs, set the log mode to debug in the player's request to MediaTailor\. For server\-side reporting, set the log mode in the *playback request*\. For client\-side reporting, set the log mode in the *session initialization request*\. 

When the log mode is set to debug, MediaTailor writes the debug log data to CloudWatch Logs\. The debug logs provide information about the following events\. For a complete list of the data produced in the debug logs, see [Debug log fields](https://docs.aws.amazon.com/mediatailor/latest/ug/debug-log-mode.html#debug-log-mode-fields)\.
+ **Origin interaction** – Details about MediaTailor's interaction with the origin server\. For example, the origin manifest response, manifest type, and origin URL\.
+ **Generated manifest** – Details about the playback session response from MediaTailor\. For example, the manifest that MediaTailor generates\.
+ **Session initialized** – Session initialization details, such as the session ID\.

## Prerequisites<a name="prerequisites"></a>

To set the log mode to debug, first you need to grant MediaTailor permission to send logs to CloudWatch, if you haven't already\. Once you've granted permission for MediaTailor to access CloudWatch, then you're ready to enable the debug log mode\. For information about how to grant MediaTailor permission to access CloudWatch see [ Setting Up Permissions for Amazon CloudWatch](https://docs.aws.amazon.com/mediatailor/latest/ug/monitoring-permissions.html)\.

## How to set the log mode to debug<a name="how-to-turn-on-debug-log-mode"></a>

This section explains how to set the log mode to debug for server\-side reporting and client\-side reporting\.

### Server\-side reporting<a name="server-side-reporting"></a>

For server\-side reporting, include the `?aws.logMode=DEBUG` query parameter and value in your player's `GET HTTP` playback request to the HLS or DASH MediaTailor endpoint\. For general information about server\-side reporting see [ Server\-side Reporting](https://docs.aws.amazon.com/mediatailor/latest/ug/ad-reporting-server-side.html)\.

**Important**  
The `DEBUG` value is case\-sensitive\.

 A playback request that includes `?aws.logMode=DEBUG` looks like the following:

**Example Playback request to an HLS endpoint**  

```
GET <mediatailorURL>/v1/master/<hashed-account-id>/<origin-id>/<asset-id>?aws.logMode=DEBUG
```
After you set the log mode to debug, we recommend that you verify that the debug logging session is active\. To verify that the debug session is active, check to see if there are any CloudWatch logs for the session ID\. The session ID is included in the playback endpoint that MediaTailor provides\. For more information, see [Verify that the debug log mode is active for your playback session](#debug-active)\. 

### Client\-side reporting<a name="client-side-reporting"></a>

For client\-side reporting, include the `logMode` key and `DEBUG` value in your client's `POST HTTP` session initialization request body to the MediaTailor /v1/session endpoint\. For general information about client\-side reporting see [ Client\-Side Reporting](https://docs.aws.amazon.com/mediatailor/latest/ug/ad-reporting-client-side.html)\.

**Important**  
The `DEBUG` value is case\-sensitive\.

 After you set the log mode to debug, we recommend that you verify that the debug session is active\. To verify that the debug session is active, confirm that there's a `SESSION_INITIALIZED` event associated with the session ID in the CloudWatch logs\. The session ID is included in the playback endpoint that MediaTailor provides\. For more information, see [Verify that the debug log mode is active for your playback session](#debug-active)\. 

## Maximum active debug sessions<a name="maximum-active-debug-log-sessions"></a>

You can have a maximum of 10 active debug log sessions\. When your player sends its session initialization or playback request to MediaTailor, MediaTailor checks to see if the limit has been reached\. If it has, MediaTailor checks to see if there are any stale sessions\. A session is stale if it hasn't been accessed within a certain period of time\. For live streams this time period is 10 minutes, for VOD streams it's 30 minutes\.

If the maximum active debug log sessions limit has been reached, debug logs aren't written to CloudWatch Logs for your session\. If you don't see debug logs in CloudWatch Logs for your session, you could have reached this limit\. To confirm if the limit has been reached, see [Verify that the debug log mode is active for your playback session](#debug-active)\. 

## Debug log fields<a name="debug-log-mode-fields"></a>

 The following table lists the debug log fields that MediaTailor writes to CloudWatch\. 


| Field | Description | 
| --- | --- | 
| awsAccountId | Your AWS account ID\. | 
| customerId | Your MediaTailor customer ID\. | 
| eventTimestamp | The ISO 8601 timestamp associated with the debug log event\. | 
| eventType | The type of debug log event\. Values:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/mediatailor/latest/ug/debug-log-mode.html) | 
| originRequestUrl | The URL of your origin server that is retrieved for this request\. | 
| mediaTailorPath | The MediaTailor endpoint that was called, including any parameters passed to MediaTailor in the initial manifest request\. | 
| requestId | The ID of a specific HTTP request to MediaTailor\. | 
| responseBody | The manifest in the response body from MediaTailor\. This is either the raw origin manifest or the manifest generated by MediaTailor\. | 
| sessionId | The playback session ID\. | 
| sessionType | The type of playback session\. Values: `HLS`, `DASH` | 

## Read the debug logs<a name="read-the-debug-logs"></a>

MediaTailor writes the debug logs to Amazon CloudWatch Logs\. Typical CloudWatch Logs charges apply\. Use CloudWatch Insights to read the debug logs\. For information on how to use CloudWatch Logs Insights, see [Analyzing Log Data with CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html) in the *AWS CloudWatch Logs User Guide*\.

**Note**  
The debug logs can take a few minutes to appear in CloudWatch\. If you don't see the logs, wait a few minutes and try again\. If you still don't see the logs, it could be that you've reached the maximum number of active debug log sessions\. To verify if this is the case, run a CloudWatch query to see if there was a debug session initialized for your playback session\. For more information, see [Verify that the debug log mode is active for your playback session](#debug-active)\.

### Examples<a name="query-examples"></a>

 This section includes example queries that you can use to read MediaTailor debug log data\. 

**Example 1: Verify that the debug log mode is active for your playback session**  

```
fields @timestamp, @message
 | filter sessionId = "32002de2-837c-4e3e-9660-f3075e8dfd90"
 | filter eventType = "SESSION_INITIALIZED" # client-side reporting
 or mediaTailorPath like “/v1/master" # server-side reporting HLS
 or mediaTailorPath like “/v1/dash" # server-side reporting DASH
```

**Example 2: View the responses from your origin**  

```
fields @timestamp, responseBody, @message, mediaTailorPath
 | filter eventType = "ORIGIN_MANIFEST" and sessionId = "32002de2-837c-4e3e-9660-f3075e8dfd90"
```

**Example 3: View the manifest generated by MediaTailor for a given session**  

```
fields @timestamp, responseBody, @message
 | filter mediaTailorPath like "/v1/master/" and eventType = "GENERATED_MANIFEST" and sessionId = "32002de2-837c-4e3e-9660-f3075e8dfd90"
```

**Example 4: View all events for a given `requestId`**  
 Use this query to view the origin manifest and the manifest generated by MediaTailor\.   

```
fields @timestamp, responseBody, @message, mediaTailorPath
 | filter requestId = "e5ba82a5-f8ac-4efb-88a0-55bed21c45b4"
```