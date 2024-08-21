# IAM 
IAM: acts an identity provider and can auth those identities. Nost cost service. Global service. No direct control on external accounts
- Users: represent humans or apps that need access
- Group: collection or related users
- Role:  can be used by aws services or external entites to access your account 
- Has one 1 username and password per user, but can have two access keys.

Keys can be created, deleted, made inactive or made active. Keys made up of access key id and secret access key

## IAM Policies
Get attached to identities such as users, roles and groups. It is really just a set of security statements in AWS. Policy documents are created in JSON made up of statement blocks. Best practice to use all fields. 
    Variables: allow you to use a placeholder when you do not know the exact value of a resouce or key when writing the policies. Allow policies to be smarter/ modular 

### Types:
   - Inline Policies: Applying JSON individual to resources. Management is harder 
   - Managed Policies: Created as own object and then you can apply to a resource. Allows ease of updating/modifying if necessary
        - AWS Managed: Managed by AWS
        - Customer Defined: Owned by you

### Components: 
   - SID: optional
   - Effect: Allow or Deny  
   - Action: Matches one of more action , can list specific actions as part of service such as S3 create object 
   - Resource: List the Amazon Resource Names 

### Priorities: 
   - Explicity Deny
   - Explicity Allow - if explicity deny is specified that will take precedence
   - Default DENY - Outside of root user the typical aws user will not be granted access to any service.

### Example:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "FullAccess",
            "Effect": "Allow",
            "Action": ["s3:*"],
            "Resource": ["*"]
        }
    ]
}
```
## Principals 
Can be individual people, computers, services or group of those things, Needs to authenticate or be authorized 

## IAM Users
Identity used for anything requiring long term acess such as humans or service accounts. Most of the time IAM user is what you want to provision.
    - Can only have 5k IAM users per account
    - IAM member can only be a member of 10 groups

## ARN
Used to uniquely identify resources within any AWS accounts.

## IAM Groups
Containers for IAM users. They exist to make organizing users easier. You can't login as a group and they have no credentials on their own. No built in all users group . 
    - no nesting
    - only 300 groups per account but can be raised with ticket
    - not a true identity cant be referenced as a principal in a policy 

## IAM Roles
One type of identity inside of AWS account. Best suited by an unknown or multiple users. Represents a level of access and to be used/borrow for a short period(quick token expirary). sts:assumerole is used when someone is assuming a role for access. 

## Service-Linked Roles
Is an IAM role linked to a specific AWS service. Predefined permissions by the service. Service might create/delete the role or allow you to during the setup of the role. The action in the IAM json will say CreateServiceLinkedRole 
 
 ## Security Token Service
 Underpins many processes in AWS. Generates temporary credentials when the operation is used. The credentials expire and do not belong to the identity that assumes the role. The authorization by default is based on the policy for the role. But can be augented. Can be used to access resources, and are always requested by another identity (AWS or External/IdP)

 ## EC2 Instance Roles 
 Roles an instance can assume.Can use permissions defined in the policy. The instance profile is a wrapper around the role that allows the permissions to get inside of the instance. Creds delivered via instance metadata. should be used rather than adding keys into the instance. 

 ## Policy Evaluation
 First AWS gathers all policies that apply to that resource. 

    - Explicity Deny: 
    - Organization SCPs applied on the current account 
    - Resource Policies
    - IAM identity boundaries
    - Session Policies
    - Identity Policies

### Permissions Boundry - 
don't grant access but define max permissions an identity can recieve . Alllows others to delegate permissions that they inheriting user can't get around.

## ExternalID
Hub and spoke models allow cross account role switching because of the confused deputy paradigm so use external ID for better segmentation and get around security issues associated with aws account trusts. But when you create unique external IDs for the truted account trusts you can get around the confused deputy issues. 

## Directory Service - MSFT AD
   - Built using AD 2012 r2
   - Uses AD schema, and standard AD tools
   - Supports group policy and sso 
   - Used for AD Auth and authorization, uses 2az by default for HA
   - Supports one way and two way external and forest trusts with on prem ad 
   - Can operate through network link failure
   - SUpports RADIUS based MFA thats already in place
   - best choice for more than 5k users and need trust relationships
   