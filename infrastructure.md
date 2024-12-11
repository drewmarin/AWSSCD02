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

- In AWS diagrams Green means Public, and blue means private subnets. 
- VPCs are regionally isolated services
- Nothing in or out without explicity config
- private cidr block is default 
- min /28 or max /16 
- max of 5 additionally secondary ipv4 blocks
- enableDNSHostnames is used to give hosts with public ipv4 addresses are registered in dns
- enableDNSSUpport - use to enable dns resolution in vpc

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

- Virtual router within a vpc. HA and in all AZs.
- Routes traffic from external into VPC and from VPC to outside
- uses the +1 address in the subnet and every subnet has this access provisioned
- every vpc router has a main route table, custom route tables can be associated to remove the basic main
- can only have one route table associated at a time
- edge association : route table with an edge gateway 
- just a list of routes 
- if multiple routes match a prefix is used as the priority and the higher the value the higher priority 
- local routes always take priority even with a lower priority 

## Firewalls
Stateless: sess request connect and response as two individual parts , outbound rules sometimes require full access to emphimeral port range

stateful: smart enough to relate the request and response connectiosn together. this reduces need to create wide open outbound rules

### Network Access Control Lists
Similar to a psuedo firewall defined within a VPC 

- Contains rules grouped into inbound and outbound.
- Stateless 
- Offers both explicity denies and allows 
- Rules are processed in 
- each subnet can only have one nacl
- can be used in conjunction with security groups
- can be applied to many subnets

### Security groups
Similar to a psuedo firewall

- stateful
- no explicit deny 
- supports ip/cidr and logical resources including itself
- attached to ENI's and not instances

## Internet Gateways

- Optional but needed if you want internet access to not just public internet but also AWS public services
- 1:1 IGW to VPC relationship
- H/A and scalable by default
- Works for both IPv4 and IPv6
- 1:1 static nat 
- to allow your vpc services to have public spaces
   - create igw
   - create igw to vpc 
   - create custom route table
   - associate route table
   - default routes -> gateway
   - ENIs need to be launched with a public ip
   
### Egress-Only Gateways
 - IPv4 NAT allows private ip to access public networks without allowing external traffic
 - All ipv6 addesses in AWS are public
 - Egress Only is outboud only 
 - HA by default across all AZs in in the region 
 - Stateful so traffic back will work , but inbound denied


### Bastion Host
- Runs at edge
- Hardened to withstand attacks
- Ingress control

## NAT Instance
- Used before nat gateways this was used 
- EOL because used Amazon Linux 
- Just running special software
- can be used as bastion hosts

## NAT Gateway

- Has a private ip that is mapped by the internet gateway to allow outbount
- Allows you to point subnets route table to gatewya vs igw
- records ip and port numbers to remember which traffic goes to each place
- allows multuple IPs to masquerade behind the gateways ip
- runs from a public subnet and uses an elastic (static ip)
- AZ resillient service, but need to deploy one in each az for full region ha
- scales to 45gbps 
- need to use route tables to point to the nat gateway 
- nat gateways can't change IPs
- capable of handling 55k simulatenous to each unique destination
- or 900/s to single destination, if you go above this you may see an errorport allocation errorport
- for s3 or dynamodb you can use a gateway endpoint (0 cost)
- Use NACL on subnet to secure them , you can't use SG on the gateway itself

## IPSEC

- Group of protocols used to create tunnels accross insecure networks between "peers"
- Provides authentication and encryption
- "interesting traffic" just means it matches certain rules
- IKE (Internet key ecchange)
   - uses asymmetrical encryption
- IKE Phase 2 (faster)
   - uses keys agreed upon in p1
   - agree on encryption method used 
   - create ipsec sa p2 tunnels

## Virtual Private Gateway

- Gateway between aws vpcs and non aws networks (on prem, other clouds, etc)
- can be attached and detached 
- 1:1 vpc relationship
- Given a private asn 
- uses bgp to learn new routes 

## AWS Site-To-Site VPN

- logical connection between vpc and on prem network running over public internet
- FULL HA if you design it correctly , not by default
- provision in less than an hour vs direct connect
- Leverages VPC, VGW technologies , Customer Gateway (CGW) can refer to both logical and physical object/router
- Needs IP range of VPC , Need on prem network ip range including the router/vpn appliance 
- Customer's on prem router is single point of failure
- dynamic vpn uses bgp 
- 1.25gbps limitation on aws side
- hourly cost, gb out cost
- can be used with DX 


## Client VPN

- Managed implementation of OpenVPN
- connect to client VPN endpoint associated with one vpc 
- charges are based on flat rate on subnet networks , and hourly fee per user

## VPC Endpoints

- provide private access to s3 and dynamodb (other services maybe in the future)
- prefix list added to oute table -> gateway endpoint
- HA across all AZs in a region by default
- Endpoint policy is used to control what it can access 
- can;t access cross region services with this

### VPC Endpoint Policies

- Allow access to an entire service in a region via the endpoint, really just controlling the endpoint now the rest of the iam policies/needs
- Needs to be a supported Service <https://docs.aws.amazon.com/vpc/latest/privatelink/aws-services-privatelink-support.html>
- Can contain conditions and a principal
- Used to limit what a Private VPC can access


## Interface Endpoints

- provide private access to AWS public services
- historically not s2 or ddb but now s3 is supported
- not ha because it is specific to subnets - an eni 
- controlled via sgs 
- tcp and ipv4 only
- uses privatelink 
- essentially overwrites dns to reroute stuff

## VPC DNS 

- .2 addess reserved now called route53 resolver
- only accessible within a vpc, hard to use in hybrid enviornment 
- historically could not forward dns request outside of r53 
- r53 resolver will forward outbound requests to public DNS 
- could use dhcp ooptions to forward selected requests to on-premises
- ec2 forwarder could act as an intermediary for AWS -> onprem , but is a legacy architecture

## R53 endpoints

- presented as ENIs in the VPC and accessible over vpn or direct connect
- can be inbound or outbound endpoints
   - inbound is for on premises requests going to r53
   - outbound is to forward quieries back to on prem 
- rules are used to control what is forwarded and where to 

## VPC Peering

- Let's you create a encrypted tunnel between two VPCs and only TWO
- Can be in the same account or different account 
- Can optionally allow public hostnames to resolve to private IPS
- If in the same regions than you can reference SGs in the peer accounts
- Does not allow transitive peering , meaning if two VPCs and A&C are paired to B this does not allow A&C to talk to each other, they can still only talk to B 
- still needs routing components to be put in place 
- uses global aws transit network 

## Cloud Front

- CDN service
- Origin- original location of the object either s3 or custom origin
- distribution - cfg unit of Cloud Front
- edge location: local cache of your data
- regional local cache - larger edhge cache, provide another layer of caching
- integrates with acm for https/ssl requests
- no upload support, download only 
- SSL supported by default
- can use dns providers to add alternative domain names to cloud front cnames
- both ssl connections viewer -> cloud front and cf -> origin need to have valid public certs

### Zones

- Origin Fetch : transfer of data from origin locations to edge networks
- Origin Access Identity (OAI): type of identity , can be associated with CF Distributions
- Custom Origins: Can use custom headers for access over https (can add this header via CF)

### Georestriction

- Can restrict content to specific locations
- Can use a Whitelist or Blacklist by country only 
- Applies to entire distribution
- Can use third party geolocation and is customisable which allows you to do this via browsers and more
- third party geo location requires some sort of compute to act as a "decider"
- private access requires signed url or cookie


### Access

- public: open access
- private: requests require signed cookie or url
- 1 behavior for whole distribituion
- trusted signer is used for private behaviors
- trusted key groups: determine which key can be used to sign urls and cookies. Used because you don't need to use the root user to sign.
- signed urls provide access to one object
- signed cookies 
- field-level encryption: happens at the edge and separately from the https tunne. privat ekey is needed to decrypted

## Resources 
