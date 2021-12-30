# Integrating with MediaPackage endpoints that use CDN authorization<a name="channel-assembly-access-configuration-access-token-integrating-emp-cdn-auth"></a>

If you use AWS Elemental MediaPackage as your source location origin, MediaTailor can integrate with MediaPackage endpoints that use CDN authorization\.

To integrate with a MediaPackage endpoint that uses CDN authorization, use the following procedure\.<a name="channel-assembly-access-configuration-access-token-integrating-emp-cdn-auth-procedure"></a>

**To integrate with MediaPackage**

1. Completed the steps in [Setting up CDN authorization](https://docs.aws.amazon.com/mediapackage/latest/ug/cdn-auth-setup.html) in the *AWS Elemental MediaPackage User Guide*, if you haven't already\.

1. Complete the procedure in [Step 1: Create an AWS KMS symmetric customer managed key](channel-assembly-access-configuration-access-token-how-to-create-kms.md)\.

1. Modify the secret that you created when you set up MediaPackage CDN authorization\. Modify the secret with the following values:
   + Update the `KmsKeyId` with the customer managed key ARN that you created in [Step 1: Create an AWS KMS symmetric customer managed key](channel-assembly-access-configuration-access-token-how-to-create-kms.md)\. 
   + \(Optional\) For the `SecretString`, you can either rotate the UUID to a new value, or you can use the existing encrypted secret as long as it's a key and value pair in a standard JSON format, such as `{"MediaPackageCDNIdentifier": "112233445566778899"}`\.

1. Complete the steps in [Attaching a resource\-based secret policy](channel-assembly-access-configuration-access-token-how-to-create-secret.md#channel-assembly-access-configuration-access-token-secret-policy)\.

1. Complete the steps in [Step 3: Configure a MediaTailor source location with access token authentication](channel-assembly-access-configuration-access-token-how-to-enable-access-token-auth.md)\.