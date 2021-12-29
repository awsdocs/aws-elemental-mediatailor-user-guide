# Configuring authentication for your source location<a name="channel-assembly-source-locations-access-configuration"></a>

Use **access configuration** to configure authentication for your source location\. When access configuration is on, MediaTailor only retrieves source manifests from your origin if the request is authorized between MediaTailor and your origin\. Access configuration is turned off by default\.

MediaTailor supports the following authentication types:
+ SigV4 for Amazon S3 authentication
+ AWS Secrets Manager access token

 This chapter explains how to use SigV4 for Amazon S3 and AWS Secrets Manager access tokens for source location authentication\.

 For more information, select the applicable topic\.

**Topics**
+ [Working with SigV4 for Amazon S3](channel-assembly-access-configuration-sigv4.md)
+ [Working with AWS Secrets Manager access token authentication](channel-assembly-access-configuration-access-token.md)