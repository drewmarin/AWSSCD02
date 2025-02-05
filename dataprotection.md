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
- AWS managed keys are rotated every year
- Grants allow you to programitxcallyu delegate use of the KMS keys to other principals, used to provide temporary permissions


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
- Rotated annually

#### Customer Managed

- Created explicitly by customers
- can be used by services which support customer managed keys
- Full control of the key policy including the account trust
- Do not support automatic rotation

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
 - Do not support automatic rotation


 #### Digital Signing using KMS

  - File or digest is submitted to KMS using the sign API
  - if less than 4kb it is handled by KMS, otherewise you create a digest(HAS) that you send to KMS telling it via the MessageType parameter
  - uses private key part to create a signature of the digest 
  - KMS returns the signature to node a
  - Node B then grabs the file and creates it's own HASH of the file and uses the verify API to verify the signature, hash algoright and key used
  - KMS then verifies the file if the digest matches
  - Can be done without KMS via the public key part which can be exported out of KMS


#### Encryption SDK

- Can use the Client Side encryption sdk - apache 2.0 license
- Supports C, .net, aws cli, java, javascript and python
- Handles data keys and wrapping keys
- by default uses a unique key for every encryption action, but KMS has rate limits 
- Rate limit is 50,000 across us-east-1, us-west-2 and eu-west-1, or 10k across (us-east-2, ap-southweast-1 ) 
- If you use Crypto Materials Cache(CMM) you can reuse keys used to reduce cost (Not best practice)
- CMM: can define max age, max messages encrypted, max bytes encrypted and more
- 

### KMS Grants

- Can provide permission on 1 KMS key 
- Designed for temporary access 
- Generally used by AWS services and created on behalf of a user
- Grant can only grant access , not deny.
- Can take 5 minutes of delay before it can be used

### Multi-region Keys

- Single or Multi-region key is defiend up front
- Can't use custom key stores
- can be symmetric or asymmetric
- can add or remove replica keys 
- replicas are full key, and can be become primary 
- when using rotation the new material is not used until the replicas have the new material also 
- aws services treat these as collections of single region keys sometimes, causing wonky performance
- Can aid in decreasing latency, disaster recovery 

### KMS Custom Key Stores

- Provides 140-2 Level 2 modules
- Keys can be interacted with via KMS and KMS APIs
- Encryption operations are done on CloudHSM via KMS
- Provides FIPS 140-2 Level 3 HSMs
- only uses symmetric keys
- no multi region keys
- each clouydhsm cluster only supports one custom key store
- account and region locked

## KMS Encryption Context

- implementation of Additional Authenticated Data (AAD)
- Use non secret key pairs generally an email /id or asometing else known
- when decrypting use the same non secret key pairs for authentication
- 

## AWS Secrets Manager

- shares functionality with parameter store
- designed for secrets (passwords, api keys)
- usable via console, cli, api or SDK
- supports automatic rotation.. this uses lambda
- directly integrates with some products such as RDS


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

## When to use KMS vs CloudHSM

- KMS : AWS integration, uses aws api, FIPS 140-2 lvl 2 validated , shared hardware
- HSM - Standard APIs (PKCD#11, JCE, CryptoNG), dedicated HSMs, FIPS 140-2 Lvl 3

## Elastic Load Balancers

- Gateway load balancer: Does not support SNI
- Support more than ec2 targets
- load balancers actually place a node in each subnet to work
- Created by using an A record that points to all the nodes used by the load balancer
- if you select internet facing the ENIs of the nodes are given public addresses
- only requirement is the nodes can talk to the backend instances
- in order to function they need 8 or more addesses in the subnets they are using so at least a /28 address, but /27 or larger is suggested after taking in account for aws reserved addresses.

### ALB 

- Only support http & https listeners
