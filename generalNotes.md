# General Notes ☁️

## AWS Accounts
Accounts: High level an account is a container for identities and resources
- Can have many per tenant
- Has a name, needs a **unique email** and a payment method
- Every account has a root user
- Used as a logical segmentation for boundaries such as dev, prod or different teans
- **Every** AWS account has **own copy/db** of IAM 

## IAM 
IAM: acts an identity provider and can auth those identities. Nost cost service. Global service. No direct control on external accounts
- Users: represent humans or apps that need access
- Group: collection or related users
- Role:  can be used by aws services or external entites to access your account 
- Has one 1 username and password per user, but can have two access keys.

Keys can be created, deleted, made inactive or made active. Keys made up of access key id and secret access key

## AWS Organizations
Allow Organizations to manage multiple AWS accounts in a cost-effective way.


## Resources
