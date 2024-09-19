
# S3: Simple Storage Service 🪣

S3 is prviate by default. The only identity with initial access is the account root user. Anything else needs to be explicilty granted to that identity. 

## Bucket Policy

- Resource Policy: like identity policies, but attached to the bucket.
- Allow/Deny same or different accounts. Can reference any other identity.
- Can grant access to anonymous identities or other accounts.
- Can use Condition Policies with items such as Source and Not IP addresses.

## S3 PreSigned URL

A way to give access to an object inside a bucket using credentials in a safe way. The url has the creds/token embedded into it, and is set to expire at a specific date and time. Can be used for both gets and puts into S3 buckets

Example Process

1. User sends a request to s3 for a presigned url
2. Presigned url grants access to the s3 objects
3. CreateURL returns a url to the user
4. User can then use the presigned url to access the contents or upload

You can create a url for an object you have no access to: you can specific any object and a time and generate the url. but if you generate the url and have no access then whoever uses the link will also not have access

## Object Lock

* Enable on new buckets (contact support for existing)
* Object lock also enables versioning and that cannot be disabled
* Write-Once-Read-Many - No delete, No overwrite
* Can set a retention period a legal hold or none of these conditions
    * Retention Period - Specified in Days & Years
        * Compliance: Can't be adjusted, deleted or overwritten even by Root user
        * Governance: Special permissions can be granted allowing lock settings to be adjusted.

## Versioning & MFA Delete

- Controlled at Bucket level
- Once enabled you can't disable it, but you can suspend it
- without versioning it is referenced by id
- when an item is deleted with versioning the id is set to null
