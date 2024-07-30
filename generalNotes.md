# General Notes ☁️

## AWS Accounts
Accounts: High level an account is a container for identities and resources
- Can have many per tenant
- Has a name, needs a **unique email** and a payment method
- Every account has a root user
- Used as a logical segmentation for boundaries such as dev, prod or different teans
- **Every** AWS account has **own copy/db** of IAM 
- AWS rotates which facilities are used for AZs so my us-east-1a may not be the same as yours, but has introduced AZ IDS to allow you to have consistency for shared deployments 

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

## AWS Control Tower 🗼
- Easy way to setup multi account enviornments
- Orchastrates other AWS services to provide this functionality and leverages: Orgsm IAM Identity Center, Cloud Formation, COnfig and more. Provides Landing-Zone... SSO/ID federation, centralizewd logging
   - Account Factory: can create AWS accounts in automated fashion 
   - Landing Zone : Designed to allow you to have a home region and well architected enviornment. Uses AWS Orgs, Aws Config, Cloudformation and more. Creates a Security OU (Log Archivew & Audit Accounts), Sandbox OU, Uses IAM identity center for SSO, And uses SNS for Monitoring
   - Guard Rails - multi-account governance/rules. Uses Mandatory, Strongly Recommended or Elective type of rules.Works in two ways: Preventative, and Detective (compliance checks)

## AWS Config
Record config changes over time on resources. Does not prevent changes happening. Checks if resources are in complaince with standards. Regional service 

## AWS Service Catalog
Organized collection of products. Includes Key Product Info: owner, cost, requirements, support info, dependancies. Defines approval and provisionming from IT and cust side. Defined using cloud formation templates and service catalog configuration. Service Catalog launches the infrastructure using templates 

## AWS Resource Access Manager
Before RAM every AWS account was isolated, and notr visible to any other account. Only way to access was to use VPC peering. RAM allows you to share resources between AWS accounts. Products need to support RAM, not every product supports RAM. Products aree shared with principals(accounts, ous or orgs). The shared items can be accessed natively. RAM is a no cost feature. VPC owner is the only one who can provision resources 

## AWS Trusted Advisor
Provides Real time guidance using AWS best practices. Account Level product. Basic or Dev accounts get 7 core checks. Other accounts (biz and ent) prodvide other checks. Access via AWS support API 
   Checks:
      - S3 Bucket permissions: not on the objects
      - Security Groups: specific ports unrestricted
      - IAM use
      - MFA on Root Account
      - EBS Public Snapshots: checks permissions on EBS snapshots 
      - RDS Public Snapshots
      - 50 service limit checks:  

## CloudFormation
CF begins with a json or yaml document. Logical resources "the what". Templates are used to create "stacks". Stacks create the physical resources from the logical. Template parameters - accept input from console/cli/api and can be referenced from within logical resources allowing them to be part of the config. Psuedo Parameters allows aws to inject some options into stacks such as a region

### Mappings
object that contain logical resources allow you to map out settings to particular outcomes by using key pair values. They use a function called !FindInMap to retrieve data. 

### Outputs 
 Optional in template. Visible as outputs when using console ui and cli, and accessible from a parent stack when using nesting. and can be exposed. 

### Conditions
Created in the optional section of the template. Processed before resources are created. Uses other intrisitinc functions and, equals, if not or

### Depends on
Tries to determine a dependency order so it can figure out what can run in parallel or not. Uses references to create these orders. The DependsOn lets you explicitly define these lists.
You may get an error if depencies aren't defined and cloudformation tries to create a resource before it's dependent is created

### CFN Signal
Allows you to go more indepth about the status of resources provisioned with cloud formation. You can even have CF wait until a certain signal is sent. Can also use timeouts (12 our max). Utility runs on the instance itself. If timeout is reached it is inferred as a cfn failure. 

### Wait signal
Traditional logical resource, can depend on other resources and vice versa. 

### Wait Handle
Presigned URL that acts as a signal agrigator 

### Nested Stacks
Bundles all resources/ share lifecycle. can't easily reference other stacks. Has Root stack (created first). Parent Stack is the stack above a current stack. can use a stack as a logical resource if needed. 
- Use them to overcome resource limit (500 resources). 
- Use them when you want to make your stacks in a modular fashion
- When you want to make stack installations easier

### Cross Stack References 
Since stacks are desinged to be isolated and self-contained it is hard to share resources, and normally outputs of one stack is not visible from other places. Outputs can be exported which makes them visible to other stacks. 
- Export names have to be unique
- Use the import value function with the export name to use the resource from another stack if it is exported.
- Cross-region sharing is not supported for Cross-Stack References.

### Cloud Formation Deletion Policy
If you delete a logical resource from a template by default the resource is deleted. With a deletion policy you can define an action when it is deleted from a template such as retain, delete or snapshot(for supported resources such as ebs, neptune, rds and redshift). 
- Only apply to delete style resource. if you subtly change the logical resource in the template the resource will be replaced but a deletion policy will not be applied in this case.

### Stack Roles
when you create a stack cfn also creates a physical resource. CFN uses permissions of the logged in identity. CFN can assume a role to gain permissions allowing you to implement role separation. 

## Resources
