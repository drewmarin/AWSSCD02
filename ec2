# EC2

## EBS
- Block storage devices, presented over network
- By default no encryption is applied
- can only be decrypted by KMS , and only if they have the corresponding kms key and permissions
- if snapshots are made they are also encrypted by the same kms key 
- accounts can be set to encrypt by default 
- each volumes uses 1 unique DEK 
- once a volume is encrypted you cannot encrypt it
- uses aes256

## Nitro Envlave

- EC2 feature that allows you to create isolated execution enviornments
- seperate, hardened and constrained
- only secure local socket connectivity to parent ec2
- Used to secure sensitve data 
- Supports attestation

### Data Sanitization

- Ebs volumes are presented as raw unformatted devices 
- However when unallocated they are not completely zeroed out unless they completely leave your account

### S3 access points

- Simplying managing access to s3 buckets/objects
- rather than 1 bucket w/ policy 
   - create many access points, with different policies
   

~TBD~