# Getting Started with AWS Elemental MediaTailor<a name="getting-started"></a>

This Getting Started tutorial shows you how to integrate AWS Elemental MediaTailor into your workflow, including how to create an MediaTailor configuration that holds information about the origin server and ad decision server \(ADS\)\.

**Topics**
+ [Prerequisites](#prerequisites)
+ [Step 1: Access AWS Elemental MediaTailor](#access-emt)
+ [Step 2: Prepare a Stream](#getting-started-prep-stream)
+ [Step 3: Configure ADS Request URL and Query Parameters](#getting-started-configure-request)
+ [Step 4: Create a Configuration](#getting-started-add-mapping)
+ [Step 5: Test the Configuration](#getting-started-test-config)
+ [Step 6: Send the Playback Request to MediaTailor](#send-request-to-mediatailor)
+ [\(Optional\) Step 7: Monitor MediaTailor Activity](#monitor-step)
+ [Step 8: Clean Up](#clean-up)

## Prerequisites<a name="prerequisites"></a>

Before you can use AWS Elemental MediaTailor, you need an AWS account and the appropriate permissions to access, view, and edit MediaTailor configurations\. Complete the steps in [Setting Up AWS Elemental MediaTailor](setting-up.md), and then return to this tutorial\.

## Step 1: Access AWS Elemental MediaTailor<a name="access-emt"></a>

Using your IAM credentials, sign in to the MediaTailor console at **https://console\.aws\.amazon\.com/mediatailor/home**\.

## Step 2: Prepare a Stream<a name="getting-started-prep-stream"></a>

Configure your origin server to produce appropriately formatted HLS manifests\.

Manifests must satisfy the following requirements:
+ Manifests must be live or video\-on\-demand \(VOD\)\.
+ Manifests must be accessible on the public internet\.
+ For live content, manifests must contain markers to delineate ad breaks\. This is optional for VOD content, which can use VMAP timeoffsets instead\. 

  The manifest file must have ad slots marked with one of the following:
  + **\#EXT\-X\-CUE\-OUT / \#EXT\-X\-CUE\-IN** \(more common\) with durations as shown in the following example:

    ```
    #EXT-X-CUE-OUT:60.00
    #EXT-X-CUE-IN
    ```
  + **\#EXT\-X\-DATERANGE** \(less common\) with durations as shown in the following example:

    ```
    #EXT-X-DATERANGE:ID="",START-DATE="",DURATION=30.000,SCTE35-OUT=0xF
    #EXT-X-DATERANGE:ID="",START-DATE="",DURATION=30.000,SCTE35-OUT=0xF
    ```

    All fields shown for `#EXT-X-DATERANGE` are required\.

  The way that you configure the ad markers in the manifest influences whether ads are inserted in a stream or replace other fragments in the stream\. For more information, see [Ad Behavior in AWS Elemental MediaTailor](ad-behavior.md)\.
+ The master playlists in the manifests must follow the HLS specification documented at [HTTP Live Streaming: Master Playlist Tags](https://tools.ietf.org/html/draft-pantos-http-live-streaming-21#section-4.3.4)\. In particular, `#EXT-X-STREAM-INF` must include the fields `RESOLUTION`, `BANDWIDTH`, and `CODEC`\.

When the stream is configured, note the URL to its master playlist\. You need the URL when you create the configuration in MediaTailor, in a later step of this procedure\.

## Step 3: Configure ADS Request URL and Query Parameters<a name="getting-started-configure-request"></a>

To determine the query parameters that the ad decision server \(ADS\) requires, generate an ad tag URL from the ADS\. This URL acts as a template for requests to the ADS, and consists of the following:
+ Static values
+ MediaTailor\-generated values \(denoted by `session` or `avail` query parameters\)
+ Player\-generated values, obtained from the client application \(denoted by `player_params.` query parameters\)

**Example**  

```
https://my.ads.com/ad?output=vast&content_id=12345678&playerSession=[session.id]&cust_params=[player_params.cust_params]
```
Where:  
+ **output** and **content\_id** are static values
+ **playerSession=\[session\.id\]** is a dynamic value provided by MediaTailor\. The value of **\[session\.id\]** changes for each player session and results in a different URL for the VAST request for each session\. 
+ **cust\_params** are player\-supplied dynamic values

The master manifest request from the player must have corresponding key\-value pairs for all `player_params.` query parameters in the ADS request URL\. For more information about configuring key\-value pairs in the request to MediaTailor, see [Dynamic Ad Variables in AWS Elemental MediaTailor](variables.md)\.

Enter the configured "template" URL when you create the origin server/ADS mapping in MediaTailor, in [Step 4: Create a Configuration](#getting-started-add-mapping)\.

### Testing Purposes<a name="testing-purposes"></a>

You can use a static VAST response from your ADS for testing purposes\. Ideally, the VAST response returns a mezzanine quality \*\.mp4 rendition that MediaTailor can transcode upon receipt\. If the response from the ADS contains multiple playback renditions, MediaTailor picks the highest quality and resolution \*\.mp4 rendition and sends it to the transcoder\.

## Step 4: Create a Configuration<a name="getting-started-add-mapping"></a>

The MediaTailor configuration holds mapping information for the origin server and ad decision server \(ADS\)\.

**To create a configuration \(console\)**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. On the **Configurations** page, choose **Create configuration**\.

1.  For **Configuration name**, type a unique name that describes the configuration\. The name is the primary identifier for the configuration\. The maximum length allowed is 512 characters\.

1.  For **Video content source**, type the URL prefix for the master playlist for this stream, minus the asset ID\. For example, if the master playlist URL is `http://origin-server.com/a/master.m3u8`, you would type `http://origin-server.com/a/`\. Alternatively, you can type a shorter prefix such as `http://origin-server.com` but the `/a/` must be included in the asset ID in the player request for content\. The maximum length is 512 characters\.
**Note**  
If your content origin uses HTTPS, its certificate must be from a well\-known certificate authority \(it cannot be a self\-signed certificate\)\. Otherwise, MediaTailor fails to connect to the content origin and can't serve manifests in response to player requests\.

1.  For **Ad decision server**, type the URL for your ad decision server \(ADS\)\. This is either the URL with variables as described in [Step 3: Configure ADS Request URL and Query Parameters](#getting-started-configure-request), or the static VAST URL that you are using for testing purposes\. The maximum length is 25000 characters\.
**Note**  
If your ADS uses HTTPS, its certificate must be from a well\-known certificate authority \(it cannot be a self\-signed certificate\)\. The same also applies to mezzanine ad URLs returned by the ADS\. Otherwise, MediaTailor can't retrieve and stitch ads into the manifests from the content origin\.

1. Choose **Create configuration**\.

   MediaTailor displays the new configuration on the **Configurations** page\.

## Step 5: Test the Configuration<a name="getting-started-test-config"></a>

After you save the configuration, test the stream using a URL in the following format:

```
playback-endpoint/v1/master/hashed-account-id/origin-id/assetID.m3u8
```

Where:
+ *playback\-endpoint* is the unique playback endpoint that MediaTailor generated when the configuration was created:

  ```
  https://bdaaeb4bd9114c088964e4063f849065.mediatailor.us-east-1.amazonaws.com
  ```
+ *hashed\-account\-id* is your AWS account ID:

  ```
  111122223333
  ```
+ *origin\-id* is the name that you gave when creating the configuration:

  ```
  myOrigin
  ```
+ *assetID\.m3u8* is the name of the master playlist from the test stream, excluding the URL path elements that you gave when adding the origin server to the MediaTailor configuration\.

Using the values from the preceding examples, the full URL is the following:

```
https://bdaaeb4bd9114c088964e4063f849065.mediatailor.us-east-1.amazonaws.com/v1/master/111122223333/myOrigin/assetID.m3u8
```

You can test the stream using one of the following methods:
+ As shown in the preceding example, type the URL in a standalone player\.
+ Test the stream in your own player environment\.

## Step 6: Send the Playback Request to MediaTailor<a name="send-request-to-mediatailor"></a>

Configure the downstream player or CDN to send playback requests to the configuration's playback endpoint provided from MediaTailor\. Any player\-defined dynamic variables that you used in the ADS request URL in [Step 3: Configure ADS Request URL and Query Parameters](#getting-started-configure-request) must be defined in the manifest request from the player\.

**Example**  
If your template ADS URL is the following:  

```
https://my.ads.com/ad?output=vast&content_id=12345678&playerSession=[session.id]&cust_params=[player_params.cust_params]
```
Then define `[player_params.cust_params]` in the player request by prefacing the key\-value pair with `ads.`\. MediaTailor passes any parameters that aren't preceded with `ads.` to the origin server instead of the ADS\.  
The player request URL is some variation of this:   

```
https://bdaaeb4bd9114c088964e4063f849065.mediatailor.us-east-1.amazonaws.com/v1/master/111122223333/myOrigin/assetId.m3u8?ads.cust_params=viewerinfo
```
When MediaTailor receives the player request, it defines the player variables based on the information in the request\. The resulting ADS request URL is some variation of this:   

```
https://my.ads.com/ad?output=vast&content_id=12345678&playerSession=<filled_in_session_id>&cust_params=viewerinfo
```

For more information about configuring key\-value pairs to pass to the ADS, see [Dynamic Ad Variables in AWS Elemental MediaTailor](variables.md)\.

## \(Optional\) Step 7: Monitor MediaTailor Activity<a name="monitor-step"></a>

Use Amazon CloudWatch and Amazon CloudWatch Logs to track MediaTailor activity, such as the counts of requests, errors, and ad breaks filled\. 

If this is your first time using CloudWatch with MediaTailor, create an AWS Identity and Access Management \(IAM\) role to allow communication between the services\.

**To allow MediaTailor access to CloudWatch \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the IAM console, choose **Roles**, and then choose **Create role**\.

1. Choose the **Another AWS account** role type\.

1. For **Account ID**, type your AWS account ID\.

1. Select **Require external ID** and enter **midas**\. This option automatically adds a condition to the trust policy that allows the service to assume the role only if the request includes the correct `sts:ExternalID`\.

1. Choose **Next: Permissions**\.

1. Add a permissions policy that specifies what actions this role can complete\. Select from one of the following options and choose **Next: Review**:
   + **CloudWatchLogsFullAccess** to provide full access to Amazon CloudWatch Logs\.
   + **CloudWatchFullAccess** to provide full access to Amazon CloudWatch\.

1. For **Role name**, type **MediaTailorLogger**, and then choose **Create role**\.

1. On the **Roles** page, select the role that you just created\. 

1. Edit the trust relationship to update the principal:

   1. On the role's **Summary** page, choose the **Trust relationship** tab\.

   1. Choose **Edit trust relationship**\.

   1. In the policy document, change the principal to the MediaTailor service\. It should look like this:

      ```
      "Principal": {
         "Service": "mediatailor.amazonaws.com"
      },
      ```

      The entire policy should read as follows:

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Principal": {
              "Service": "mediatailor.amazonaws.com"
            },
            "Action": "sts:AssumeRole",
            "Condition": {
              "StringEquals": {
                "sts:ExternalId": "Midas"
              }
            }
          }
        ]
      }
      ```

   1. Choose **Update Trust Policy**\.

## Step 8: Clean Up<a name="clean-up"></a>

To avoid extraneous charges, delete all unnecessary configurations\.

**To delete a configuration \(console\)**

1. On the MediaTailor **Configurations** page, do one of the following:
   + Choose the **Configuration name** for the configuration that you want to delete\.
   + In the **Configuration name** column, choose the radio button, and then choose **Delete**\.

1.  In the **Delete configuration** confirmation box, type **Delete**, and then choose **Delete** again\.

   MediaTailor removes the configuration\.