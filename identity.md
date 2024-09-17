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

### Types

- Inline Policies: Applying JSON individual to resources. Management is harder
- Managed Policies: Created as own object and then you can apply to a resource. Allows ease of updating/modifying if necessary
  - AWS Managed: Managed by AWS
  - Customer Defined: Owned by you

### Components

- SID: optional
- Effect: Allow or Deny  
- Action: Matches one of more action , can list specific actions as part of service such as S3 create object
- Resource: List the Amazon Resource Names

### Priorities

- Explicity Deny
- Explicity Allow - if explicity deny is specified that will take precedence
- Default DENY - Outside of root user the typical aws user will not be granted access to any service.

### Example

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

### Permissions Boundry

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

   If use AD connector you need connection between aws and on prme , only use if compelling reason to do so such as legal and complaince reason

## Identity Federation

Process where external identityes are used instead of services maintaining their own identities. Better experience for customers and less admin overhead. allows you go beyond aws lmits (5k iam). In AWS exchange is not direct so a hand off needs to occur
    Components:
        Service Provider : App like IG
        Identity Provider: Systems that use standards like Oauth, saml
    Flow:
        1. opens an app/service
        2. Service reaches out to iDP for auth
        3. customers login with tokens provided by idp

## Cognito

Provides two pieces of functuality. Provides Authentication, Authorization and user managerment for web/mobile apps
    Components:
        1. User Pool - signins and get a json web token (jwt) (most services can't use jwt in AWS). User directory mgmt and profiles, sign up and in, mfa and other security features
        2. Identity pool - allow you to offer access to temp AWS credentials. Allow users of apps to post into s3 or dbs, etc..
            Unauthenticated identities - Guest Users
            Federated identities - saml2.0 & user pool for short term aws creds to access aws resources

## SAML

Secureity Assertion Markup Language. Open standard used by many IdP's(MS ADFS). Indirectly uses on premises IDs with AWS (console & cli ). Look into it if you have an existing identity management team and if you have more than 5k users. Uses Iam roles & temp credentials typically on a 12 hour validity
    if mentions probably not the right selection
        microsoft
        google
        facebook

### Example Workflow

1. App requests access via IdP
2. IdP authenticates the request which roles are available for the app
3. SAMl assertion token is then handed and trusted by AWS / Other platform 
4. App then communicates with AWS STS using sts assume role operation leveraging the saml assertion
5. if accepted by STS then temp credentials are returned and used by the app hence the exchange has occured

## IAM Identity Center

Formally AWS SSO. somewhat replaces saml 2.0 solutions in aws.

1. Centrally Manage AWS accounts and external apps
2. Flexible identity source
3. AWS Built in Identity store
4. Can use AWS Managed AD or on premmised AD (trust or ad connector) or SDAML2.0 provider
5. preferred by aws vs traditional workforce federation
