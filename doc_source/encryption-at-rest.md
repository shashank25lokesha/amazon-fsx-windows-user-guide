# Encryption at Rest<a name="encryption-at-rest"></a>

All Amazon FSx file systems are encrypted at rest with keys managed using AWS Key Management Service \(AWS KMS\)\. Data is automatically encrypted before being written to the file system, and automatically decrypted as it is read\. These processes are handled transparently by Amazon FSx, so you don't have to modify your applications\.

Amazon FSx uses an industry\-standard AES\-256 encryption algorithm to encrypt Amazon FSx data and metadata at rest\. For more information, see [Cryptography Basics](https://docs.aws.amazon.com/kms/latest/developerguide/crypto-intro.html) in the *AWS Key Management Service Developer Guide*\.

**Note**  
The AWS key management infrastructure uses Federal Information Processing Standards \(FIPS\) 140\-2 approved cryptographic algorithms\. The infrastructure is consistent with National Institute of Standards and Technology \(NIST\) 800\-57 recommendations\.

## How Amazon FSx uses AWS KMS<a name="EFSKMS"></a>

Amazon FSx integrates with AWS KMS for key management\. Amazon FSx uses an AWS KMS key to encrypt your file system\. You choose the KMS key used to encrypt and decrypt file systems \(both data and metadata\)\. You can enable, disable, or revoke grants on this KMS key\. This KMS key can be one of the two following types:
+ **AWS managed key** – This is the default KMS key, and it's free to use\.
+ **Customer managed key** – This is the most flexible KMS key to use, because you can configure its key policies and grants for multiple users or services\. For more information on creating customer managed keys, see [Creating keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the* AWS Key Management Service Developer Guide*\.

If you use a customer managed key as your KMS key for file data encryption and decryption, you can enable key rotation\. When you enable key rotation, AWS KMS automatically rotates your key once per year\. Additionally, with a customer managed key, you can choose when to disable, re\-enable, delete, or revoke access to your KMS key at any time\. For more information, see [Rotating AWS KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html) in the* AWS Key Management Service Developer Guide\.*

File system encryption and decryption at rest are handled transparently\. However, AWS account IDs specific to Amazon FSx appear in your AWS CloudTrail logs related to AWS KMS actions\. 

## Amazon FSx Key Policies for AWS KMS<a name="FSxKMSPolicy"></a>

Key policies are the primary way to control access to KMS keys\. For more information on key policies, see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide\. *The following list describes all the AWS KMS\-related permissions supported by Amazon FSx for encrypted at rest file systems:
+ **kms:Encrypt** – \(Optional\) Encrypts plaintext into ciphertext\. This permission is included in the default key policy\.
+ **kms:Decrypt** – \(Required\) Decrypts ciphertext\. Ciphertext is plaintext that has been previously encrypted\. This permission is included in the default key policy\.
+ **kms:ReEncrypt** – \(Optional\) Encrypts data on the server side with a new KMS key, without exposing the plaintext of the data on the client side\. The data is first decrypted and then re\-encrypted\. This permission is included in the default key policy\.
+ **kms:GenerateDataKeyWithoutPlaintext** – \(Required\) Returns a data encryption key encrypted under a KMS key\. This permission is included in the default key policy under **kms:GenerateDataKey\***\.
+ **kms:CreateGrant** – \(Required\) Adds a grant to a key to specify who can use the key and under what conditions\. Grants are alternate permission mechanisms to key policies\. For more information on grants, see [Using grants](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) in the *AWS Key Management Service Developer Guide\.* This permission is included in the default key policy\.
+ **kms:DescribeKey** – \(Required\) Provides detailed information about the specified KMS key\. This permission is included in the default key policy\.
+ **kms:ListAliases** – \(Optional\) Lists all of the key aliases in the account\. When you use the console to create an encrypted file system, this permission populates the list of KMS keys\. We recommend using this permission to provide the best user experience\. This permission is included in the default key policy\.