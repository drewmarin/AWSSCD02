# IAM 
IAM: acts an identity provider and can auth those identities. Nost cost service. Global service. No direct control on external accounts
- Users: represent humans or apps that need access
- Group: collection or related users
- Role:  can be used by aws services or external entites to access your account 
- Has one 1 username and password per user, but can have two access keys.

Keys can be created, deleted, made inactive or made active. Keys made up of access key id and secret access key

## IAM Policies
Get attached to identities such as users, roles and groups. It is really just a set of security statements in AWS. Policy documents are created in JSON made up of statement blocks. Best practice to use all fields. 

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

## IAM Users
Identity used for anything requiring long term acess such as humans or service accounts. Most of the time IAM user is what you want to provision