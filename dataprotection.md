# Data Protection

## Hardware Security Modules

- seperate device or cluster and generally isolated.
- stores keys to be used for crypto functions 
- performs the crypto operation for the os, hardware, etc
- can only be accessed in a highly defined way 
- helps provide performance boost since the hsm takes on the crypto overhead

## KMey Management Service (KMS)

- Regional & Public service
- Can create, store and manage crypto keys
- Can handle symmetric and asymmetric keys
- Can perform operations (ex: decrypt, encrypt)
- Keys never leave KMS (FIPS 140-2, some features are level 3 but overall KMS is L2)
- CMK: Customer Master Key == KMS Keys
- KMS Keys are logical: contain ID, date, policy , description & state. Backed by phsyical key material
- Key Material can be generated or improted, can be used up to 4kb in data
- services like s3 will actually create a key for each individual object
- Keys made in a region never leave that region
- Key Policies are used to allow KMS to trust the account their, because ultimately KMS is a shared service
- Every Key has an associated policy 
- DEK created by KMS but not managed by KMS

### S3 Bucket Keys

- Can be used to reduce AWS KMS request costs
- Bucket keys are time limited and granted to the bucket and used for individiaul encryption options to reduce KMS API calls
- Not retroactive, only affects objects post enablement
- Howeveber cloudtrail kms events then show the bucket not the object
- if replicating plaintext items the bucket is encrypted at the destination side 

### AWS vs Customer Managed Keys

#### AWS Managed Keys

- Created by AWS when you use encryption in a service
- 1 per service per region
- Naming is standardized ie aws/s3....
- Only usable by that service in that region, however metadata is readable by the creator AWS account
- Key policy can be viewed but not changed

#### Customer Managed

- Created explicitly by customers
- can be used by services which support customer managed keys
- Full control of the key policy including the account trust

#### Key Material

 - Can only import symmetric key material
 - KMS Key = Logical Key Metadata + Material 
 - Regulatgion may require that you generated it
 - Can't change imported material after importation
 - Can't change key origin after it's been created
 - Deleting an imported the key only deletes the key metadata 


 #### Asymmetric Keys in KMS

 - Key Pair (Private & Publc ) - Private never leaves KMS
 - Public can be downloaded via console or cli 
 - RSA or SM2: can be uised for encrypt/decrypt OR signing and verification
 - Elliptci Curve (ECC) - Signing & verification 
 - KeySpec: can't be changed and sets what the key can be used for encrypt/decrypt or signing
 - AWS Servicves which use KMS use Symmetric NOT Asymmetric keys


## CloudHSM

- fully customer Managed
- phsycial tamper resistant hardware
- Fully FIPS 1420-2 Level 3 
- Access using industry standard Applications 
   - PKCS 11
   - Java Cryptography Extensions
   - Microsoft CryptoNG (CNG) libraries
- KMS can use CloudHSM as a custom key store, cloud HSM integrates with KMS
- HMS operate in AWS managed VPC
- NO native integration into other AWS products
- Can be used for Transparent Data Encreyptiopn for Oracle Databases
- Can be used to protect Private Keys for Certificate Authorities 


## Envelope Encreyptiopn

- Process of encrypting plaintext with a data key and then encrypting the data key with another key
- asymmetric keys are flexible, but slow 
