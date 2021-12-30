# Step 2: Create an AWS Secrets Manager secret<a name="channel-assembly-access-configuration-access-token-how-to-create-secret"></a>

Use Secrets Manager to store your access token in the form of a `SecretString` that's encrypted by an AWS KMS customer managed key\. MediaTailor uses the key to decrypt the `SecretString`\. For information about how Secrets Manager uses AWS KMS to protect secrets, see the topic [How AWS Secrets Manager uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-secrets-manager.html) in the *AWS Key Management Service Developer Guide*\.

If you use AWS Elemental MediaPackage as your source location origin, and would like to use MediaTailor Secrets Manager access token authentication follow the procedure [Integrating with MediaPackage endpoints that use CDN authorization](channel-assembly-access-configuration-access-token-integrating-emp-cdn-auth.md)\.

You can create a Secrets Manager secret using the AWS Management Console or programmatically with the Secrets Manager APIs\.

## To create a secret<a name="channel-assembly-access-configuration-access-token-create-secret"></a>

Follow the steps for [https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) in the *AWS Secrets Manager User Guide*\.

Keep in mind the following considerations when creating your secret:
+ The [https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_ReplicaRegionType.html#SecretsManager-Type-ReplicaRegionType-KmsKeyId](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_ReplicaRegionType.html#SecretsManager-Type-ReplicaRegionType-KmsKeyId) must be the [key ARN](https://docs.aws.amazon.com/kms/latest/developerguide/find-cmk-id-arn.html) of the customer managed key you created in Step 1\.
+ You must supply a `[SecretString](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_CreateSecret.html#SecretsManager-CreateSecret-request-SecretString)`\. The `SecretString` should be a valid JSON object that includes a key and value containing the access token\. For example, \{"MyAccessTokenIdentifier":"112233445566"\}\. The value must between 8\-128 characters long\.

  When you configure your source location with access token authentication, you specify the `SecretString` key\. MediaTailor uses the key to look up and retrieve the access token stored in the `SecretString`\.

  Make a note of the secret ARN and the `SecretString` key\. You'll use them when you configure your source location to use access token authentication\.

## Attaching a resource\-based secret policy<a name="channel-assembly-access-configuration-access-token-secret-policy"></a>

To let MediaTailor access the secret value, you must attach a resource\-based policy to the secret\. For more information, see [Managing a resource\-based policy for a secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_secret-policy.html) in the *AWS Secrets Manager User Guide*\.

The following is a policy statement example that you can add for MediaTailor:

```
{
  "Version" : "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Principal" : {
        "Service" : "mediatailor.amazonaws.com"
      },
      "Action" : "secretsmanager:GetSecretValue",
      "Resource" : "<secret ARN>"
    }
  ]
}
```