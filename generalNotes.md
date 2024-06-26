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
Allow Organizations to manage multiple AWS accounts in a cost-effective way. management account used to be called the master account. 

MGMT account is special 
Allow you to build complex nested structures if needed

### Service Control Policies
- Can be attached to root container OR to one of more OUs
- SCP inherit from upper level Policies 
- By design the root account in a OU will **not** be affected by SCPs
- Limit what the account can do including the root user, **BUT** are not granting permissions.
- Can use them as an allow list or a Deny List. Deny List behavior is default
   - To use as Allow List you must first get rid of the FullAWSAccess policy. And then you need to add any services you want to allow as a new policy.

## AWS Control Tower
- Easy way to setup multi account enviornments
- Orchastrates other AWS services to provide this functionality and leverages: Orgsm IAM Identity Center, Cloud Formation, COnfig and more. Provides Landing-Zone... SSO/ID federation, centralizewd logging
   - Account Factory: can create AWS accounts in automated fashion 
   - Landing Zone : Designed to allow you to have a home region and well architected enviornment. Uses AWS Orgs, Aws Config, Cloudformation and more. Creates a Security OU (Log Archivew & Audit Accounts), Sandbox OU, Uses IAM identity center for SSO, And uses SNS for Monitoring
   - Guard Rails - multi-account governance/rules. Uses Mandatory, Strongly Recommended or Elective type of rules.Works in two ways: Preventative, and Detective (compliance checks)

## AWS Config


## Resources
