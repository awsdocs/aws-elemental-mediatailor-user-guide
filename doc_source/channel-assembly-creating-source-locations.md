# Creating a source location<a name="channel-assembly-creating-source-locations"></a>

The following procedure explains how to create a source location using the MediaTailor console\. For information about how to create source locations using the MediaTailor API, see [CreateSourceLocation](https://docs.aws.amazon.com/mediatailor/latest/apireference/sourcelocation-sourcelocationname.html) in the *AWS Elemental MediaTailor API Reference*\.<a name="create-source-location-procedure"></a>

**To create a source location**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Source locations**\.

1. On the navigation bar, choose **Create source location**\.

1. Under **Source location configuration**, enter a name and the base URL of your origin server:
   + **Name**: An identifier for your source location, such as **my\-origin**\.
   + **Base URL**: The protocol and base URL of the origin server your content is stored, such as **https://111111111111\.cloudfront\.net**\. The URL must be in a standard HTTP URL format, prefixed with **http://** or **https://**\.

     Optionally select **Use SigV4 for Amazon S3 authentication** if your source location is an Amazon S3 bucket, and if you'd like to use AWS Signature Version 4 for Amazon S3 access authentication\. For advanced information, see [Configuring authentication for your source location](channel-assembly-source-locations-access-configuration.md)\.

1. <a name="access-configuration-console"></a>Under **Access configuration**, optionally configure authentication for your source location:
   + **Access type**: Select the authentication type that MediaTailor uses to access the content stored on the source location's origin\. 
     + **SigV4 for Amazon S3** \- MediaTailor uses Amazon Signature Version 4 \(SigV4\) to authorize request to your origin\. For more information, see [Working with SigV4 for Amazon S3](channel-assembly-access-configuration-sigv4.md)\.
     + **Secrets Manager access token authentication** \- MediaTailor uses Secrets Manager and a AWS KMS customer managed key created, owned, and managed by you to facilitate access token authentication between MediaTailor and your origin\. For information about how to configure **Secrets Manager access token authentication**, see [Working with AWS Secrets Manager access token authentication](channel-assembly-access-configuration-access-token.md)\.
       + **Header name** \- Specify a HTTP header name\. MediaTailor uses the HTTP header to send the access token to your origin in content manifest requests\. You can can use any header name as long as it doesn’t start with `x-amz-` or `x-amzn-`\. If you're integrating with [MediaPackage CDN authorization](https://docs.aws.amazon.com/mediapackage/latest/ug/cdn-auth.html), the header value should be `X-MediaPackage-CDNIdentifier`\.
       + **Secret string key** \- The `SecretString` key that you specified in your Secrets Manager secret\. For example, if your `SecretString` contains a key and value pair such as: `{"MyHeaderName": "11111111-2222-3333-4444-111122223333"}`, then `MyHeaderName` is the `SecretString` key you enter in this field\.
       + **Secret ARN** \- The ARN of the secret that holds your access token\. For a step\-by\-step guide, see [Step 2: Create an AWS Secrets Manager secret](channel-assembly-access-configuration-access-token-how-to-create-secret.md)\.

1. Under **Segment delivery server configuration**, optionally configure a server to deliver your content segments:
   + **Use a default segment delivery server**: Enter the base URL of the server that is used to deliver your content segments, such as a CDN\. Configure **Default segment host name** if you'd like to use a different server than the source location server to serve the content segments\. For example, you can restrict access to the origin manifests from players by using a different CDN configuration for the **Base HTTP URL** \(what MediaTailor uses to access the manifests\) and the **Default Segment Base URL** \(what players uses to access the content segments\)\. If you don't enter a value, MediaTailor defaults to the source location server for segment delivery\.
   + **Use named segment delivery servers**: If you have configured a default segment delivery server, you can also configure additional segment delivery servers\. Each one must have a unique name and a base URL\. The base URL can be a full HTTP URL, or it can be a relative path like `/some/path/` \. The names are used to identify which server should be used when MediaTailor receives a request for content segments\. If the request contains the header `X-MediaTailor-SegmentDeliveryConfigurationName` and the value of the header matches a name, the corresponding base URL will be used to serve the content\. If the header is not included in the request, or if it does not match any names, then the default segment delivery server will be used\.

1. Choose **Create source location**\.

1. To add more source locations, repeat steps 2\-6\.