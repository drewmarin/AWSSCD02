# Infrastructure

## Instance Metadata

- Service that EC2 provides to the insatnce that can be used to configure or manage the machine. 
- Accesible at <http://169.254.169.254/latest/metadata>
- Accessible inside all instances

## Private vs Public Services
Refers to networking only. Services still respect permissions that control access. 
- Public : Accessed using public endpoints
- Private : Accessed within a VPC 

VPCs are isolated unless configured otherwise 

## VPCs

In AWS diagrams Green means Public, and blue means private subnets. 

### Subnets

- AZ resilient
- a sub network of a vpc 
- cannot be in multiple AZs
- CIDRs are subsetof the CIDR in the VPC
- cannot overlap with other subnets
- optional ipv6 cidr if the vpc has ipv6 ranges
- always has at least 5 reserved addresses
    - network address
    - network +1 address, used by vpc router 
    - network +2 address, used for dns/route 53 
    - network +3 future use
    - broadcast address (last ip in subnet)
- dhcp options set: 
