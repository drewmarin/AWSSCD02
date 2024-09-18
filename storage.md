
# S3: Simple Storage Service 🪣

S3 is prviate by default. The only identity with initial access is the account root user. Anything else needs to be explicilty granted to that identity. 

## Bucket Policy

- Resource Policy: like identity policies, but attached to the bucket.
- Allow/Deny same or different accounts. Can reference any other identity.
- Can grant access to anonymous identities or other accounts.

## S3 PreSigned URL

A way to give access to an object inside a bucket using credentials in a safe way. The url has the creds/token embedded into it, and is set to expire at a specific date and time. Can be used for both gets and puts into S3 buckets

Example Process

1. User sends a request to s3 for a presigned url
2. Presigned url grants access to the s3 objects
3. CreateURL returns a url to the user
4. User can then use the presigned url to access the contents or upload

You can create a url for an object you have no access to: you can specific any object and a time and generate the url. but if you generate the url and have no access then whoever uses the link will also not have access
