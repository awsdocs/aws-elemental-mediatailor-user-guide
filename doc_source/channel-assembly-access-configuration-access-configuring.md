# Configuring AWS Secrets Manager access token authentication<a name="channel-assembly-access-configuration-access-configuring"></a>

When you want to use AWS Secrets Manager access token authentication, you perform the following steps:

1. You [create an AWS Key Management Service customer managed key](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html)\. 

1. You [create a AWS Secrets Manager secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html)\. The secret contains your access token, which is stored in Secrets Manager as an encrypted secret value\. MediaTailor uses the AWS KMS customer managed key to decrypt the secret value\.

1. You configure an AWS Elemental MediaTailor source location to use Secrets Manager access token authentication\.

The following section provides step\-by\-step guidance on how to configure AWS Secrets Manager access token authentication\.

**Topics**
+ [Step 1: Create an AWS KMS symmetric customer managed key](channel-assembly-access-configuration-access-token-how-to-create-kms.md)
+ [Step 2: Create an AWS Secrets Manager secret](channel-assembly-access-configuration-access-token-how-to-create-secret.md)
+ [Step 3: Configure a MediaTailor source location with access token authentication](channel-assembly-access-configuration-access-token-how-to-enable-access-token-auth.md)