# Step 1: Create an AWS KMS symmetric customer managed key<a name="channel-assembly-access-configuration-access-token-how-to-create-kms"></a>

You use AWS Secrets Manager to store your access token in the form of a `SecretString` stored in a secret\. The `SecretString` is encrypted through the use of an *AWS KMS symmetric customer managed key *that you create, own, and manage\. MediaTailor uses the symmetric customer managed key to facilitate access to the secret via a grant, and to encrypt and decrypt the secret value\. 

Customer managed keys let you perform tasks such as the following:
+ Establishing and maintaining key policies
+ Establishing and maintaining IAM policies and grants
+ Enabling and disabling key policies
+ Rotating cryptographic key material
+ Adding tags

  For information about how Secrets Manager uses AWS KMS to protect secrets, see the topic [How AWS Secrets Manager uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-secrets-manager.html) in the *AWS Key Management Service Developer Guide*\.

  For more information about customer managed keys, see [Customer managed keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk) in the *AWS Key Management Service Developer Guide*\.

**Note**  
AWS KMS charges apply for using a customer managed key For more information about pricing, [see the AWS Key Management Service pricing page](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#encrypt_context)\.

You can create an AWS KMS symmetric customer managed key using the AWS Management Console or programmatically with the AWS KMS APIs\.

## To create a symmetric customer managed key<a name="channel-assembly-access-configuration-access-token-create-symmetric-key"></a>

Follow the steps for [Creating a symmetric customer managed key](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html#create-symmetric-cmk) in the *AWS Key Management Service Developer Guide\.*

Make a note of the key Amazon Resource Name \(ARN\); you'll need it in [Step 2: Create an AWS Secrets Manager secret](channel-assembly-access-configuration-access-token-how-to-create-secret.md)\.

## Encryption context<a name="channel-assembly-access-configuration-access-token-encryption-context"></a>

An [encryption context](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#encrypt_context) is an optional set of key\-value pairs that contain additional contextual information about the data\.

Secrets Manager includes an [encryption context](https://docs.aws.amazon.com/kms/latest/developerguide/services-secrets-manager.html#asm-encryption-context) when encrypting and decrypting the `SecretString`\. The encryption context includes the secret ARN, which limits the encryption to that specific secret\. As an added measure of security, MediaTailor creates an AWS KMS grant on your behalf\. MediaTailor applies a [https://docs.aws.amazon.com/kms/latest/APIReference/API_GrantConstraints.html](https://docs.aws.amazon.com/kms/latest/APIReference/API_GrantConstraints.html) operation that only allows us to *decrypt* the `SecretString` associated with the secret ARN contained in the Secrets Manager encryption context\.

For information about how Secrets Manager uses encryption context, see the [Secrets Manager encryption context ](https://docs.aws.amazon.com/kms/latest/developerguide/services-secrets-manager.html#asm-encryption-context)topic in the *AWS Key Management Service Developer Guide*\. 

## Setting the key policy<a name="channel-assembly-access-configuration-access-token-key-policy"></a>

Key policies control access to your customer managed key\. Every customer managed key must have exactly one key policy, which contains statements that determine who can use the key and how they can use it\. When you create your customer managed key you can use the default key policy\. For more information, see [Managing access to customer managed keys](https://docs.aws.amazon.com/kms/latest/developerguide/control-access-overview.html#managing-access) in the *AWS Key Management Service Developer Guide*\.

To use your customer managed key with your MediaTailor source location resources, you must give permission to the IAM principal that calls [https://docs.aws.amazon.com/mediatailor/latest/apireference/sourcelocation-sourcelocationname.html](https://docs.aws.amazon.com/mediatailor/latest/apireference/sourcelocation-sourcelocationname.html) or [`UpdateSourceLocation` ](https://docs.aws.amazon.com/mediatailor/latest/apireference/sourcelocation-sourcelocationname.html)to use the following API operations:
+ `kms:CreateGrant` – Adds a grant to a customer managed key\. MediaTailor creates a grant on your customer managed key that lets it use the key to create or update a source location configured with access token authentication\. For more information about [Using grants](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html), see the *AWS Key Management Service Developer Guide\.*

  This allows MediaTailor to do the following:
  + Call `Decrypt` so that it can successfully retrieve your Secrets Manager secret when calling [https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_GetSecretValue.html](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_GetSecretValue.html)\.
  + Call `RetireGrant` to retire the grant when the source location is deleted, or when access to the secret has been revoked\.

The following is an example policy statement that you can add for MediaTailor:

```
{
    "Sid": "Enable MediaTailor Channel Assembly access token usage for the MediaTailorManagement IAM role",
    "Effect": "Allow",
    "Principal": {
        "AWS": "arn:aws:iam::account number:role/MediaTailorManagement"
    },
    "Action": "kms:CreateGrant",
    "Resource": "*",
    "Condition": {
        "StringEquals": {
            "kms:ViaService": "mediatailor.region.amazonaws.com"
        }
    }
}
```

For more information about[ specifying permissions in a policy](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html), see the *AWS Key Management Service Developer Guide*\.

For information about [troubleshooting key access](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html), see the *AWS Key Management Service Developer Guide*\.