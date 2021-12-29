# Working with SigV4 for Amazon S3<a name="channel-assembly-access-configuration-sigv4"></a>

Signature Version 4 \(SigV4\) for Amazon S3 is a signing protocol used to authenticate requests to Amazon S3 over HTTP\. When you use SigV4 for Amazon S3, MediaTailor includes a signed authorization header in the HTTP request to the Amazon S3 bucket used as your origin\. If the signed authorization header is valid, your origin fulfills the request\. If it isn't valid, the request fails\.

 For general information about SigV4 for Amazon S3, see the [Authenticating Requests \(AWS Signature Version 4\)](Amazon Simple Storage Service User Guide/latest/API/sig-v4-authenticating-requests) topic in the *Amazon S3 API reference*\. 

## Requirements<a name="channel-assembly-access-configuration-sigv4-how-to"></a>

 If you activate SigV4 for Amazon S3 authentication for your source location, you must meet these requirements: 
+ You must allow MediaTailor to access your S3 bucket by granting **mediatailor\.amazonaws\.com** principal access in IAM\. For information about configuring access in IAM, see [Access management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.
+ The **mediatailor\.amazonaws\.com** service principal must have permissions to read all top\-level manifests referenced by the VOD source package configurations\.
+ The caller of the API must have **s3:GetObject** IAM permissions to read all top\-level manifests referenced by your MediaTailor VOD source package configurations\.
+ Your MediaTailor source location base URL must follow the Amazon S3 virtual hosted\-style request URL format\. For example, https://*bucket\-name*\.s3\.*Region*\.amazonaws\.com/*key\-name*\. For information about Amazon S3 hosted virtual\-style access, see [Virtual Hosted\-Style Requests](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/VirtualHosting.html#virtual-hosted-style-access)\.