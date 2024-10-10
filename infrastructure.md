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

### dhcp

- starts with layer 2 broadcast message
- reserved dhcp address responds with ip address, subnet mask and default gateway as well as DNS servers which are typically r53, also sends ntp netbios
- can use either amazon provided dns or custom dns domain 
- dhcp option sets can be used but once set they cannot be changed and can be associated with 0 or more VPCs
- changing the associated dhcp option set is immediate but existing clients won't get new settings until they need a new lease

### VPC router
