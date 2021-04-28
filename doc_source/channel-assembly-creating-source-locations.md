# Creating a source location<a name="channel-assembly-creating-source-locations"></a>

The following procedure explains how to create source locations using the MediaTailor console\. For information about how to create source locations using the MediaTailor API, see [CreateSourceLocation](https://docs.aws.amazon.com/mediatailor/latest/apireference/sourcelocation-sourcelocationname.html) in the *AWS Elemental MediaTailor API Reference*\.<a name="create-source-location-procedure"></a>

**To create a source location**

1. Open the MediaTailor console at [https://console\.aws\.amazon\.com/mediatailor/](https://console.aws.amazon.com/mediatailor/)\.

1. In the navigation pane, choose **Channel assembly** > **Source locations**\.

1. On the navigation bar, choose **Create source location**\.

1. Under **Source location configuration**, enter an name and the base URL of your origin server:
   + **Name**: An identifier for your source location, such as **my\-origin**\.
   + **Base URL**: The protocol and base URL of the origin server your content is stored, such as **https://111111111111\.cloudfront\.net**\. The URL must be in a standard HTTP URL format, prefixed with **http://** or **https://**\.

     Optionally select **Use SigV4 for Amazon S3 authentication** if your source location is an Amazon S3 bucket, and if you'd like to use AWS Signature Version 4 for Amazon S3 access authentication\. For advanced information, see [Using access authentication](channel-assembly-source-locations-access-configuration.md)\.

1. Under **Segment delivery server configuration**, optionally configure a server to deliver your content segments:
   + **Use a default segment delivery server**: Enter the base URL of the server that is used to deliver your content segments, such as a CDN\. Configure **Default segment host name** if you'd like to use a different server than the source location server to serve the content segments\. For example, you can restrict access to the origin manifests from players by using a different CDN configuration for the **Base HTTP URL** \(what MediaTailor uses to access the manifests\) and the **Default Segment Base URL** \(what players uses to access the content segments\)\. If you don't enter a value, MediaTailor defaults to the source location server for segment delivery\.

1. Choose **Create source location**\.

1. To add more source locations, repeat steps 2\-6\.