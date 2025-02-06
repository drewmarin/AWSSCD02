# Logging

## Cloudwatch

- Ingests stores and management of metrics
- Uses Public space endpoints 
- Can use built in service or leverage the agent to provide more metrics
- Can setup alarms based on metrics
- Namespace = contaier for metrics
- Datapoint = timestamp, value , (optional) unit of measure. SMallest part of CLoudwatch units
- Dimension = name/value pair. can be used to aggregate data
- Resolution = standard (60s granulatiry) .. high (1s). As data ages it's resolution becomes less and less
- Statistic = aggregation over a period (Min, Max, Sum, Average)
- Alarm = watches a metric over a time period

### Cloud Watch Logs

- Ingestion side: getting logs into service. Supports logs from anywehre if they can send to public endpoint
- Sub side: what other services can use these logs
- Cloudtrail : api calls, can use cloud watches
- can bring in beanstalk, ecs api gateway and lambda logs
- r53 logs also supported
- log events: events logged by cloud watch. COnsist of time stamp and message
- log stream: sequence of events that share the same source ie a collection
- log group: group of log streams. generally represents the thing being monitored. set things like retention, permissions and encryption settings on the log group.
- metric filter: defined on log group, looks for patterns ie app crashes or failed ssh attempts
- s3 export can take up to 12 hour of delays and data is sent to a bucket. can only be encrypted with sse-s3. Not real time
- subscriptions filter: can allow you to send something outside in real time. 
- data firehose allows almost real time delivery
- can also use custom lambda functions to send data to anywhere
- on instances it always includes the eb-activity.log and access logs from nginx or apache proxy
- duplicates in the [logstream] section of the agent config file will cause issues

### CloudWatch Events & Event Bridge

- EventBridge can also handle events from third parties and custom applications
- EventBridghe uses same api as cloudwatch and is the replacement 
- Uses an EventBus: stream of events that occur from any supported service
- Uses tules to matych incoming events and then can route the events to the target


## S3 Events

- Notifications are generated when events occur in a bucket
- can be delivered to sns, sqs and lambda functions
- types supported: objects created(put, post, copy), objects deleted, restores, replication


## Simple Notification Service

- PUb SUB notification system 
- Public service 
- Messages have a limit of 256kb in size
- Topics are the base of SNS for permissions and config
- Publishers send messages to a TOPIC 
- TOPICS can have subsribers which recieve messages
- HTTP, JSON, Mobile Puish and SMS messages as well as Lambda can be used as subscribers
- Delivery Status from supported subscribers
- Offers delivery retries 

## SES 

- Uses port 587, 465

## Amazon Inspector

- Designed to check ec2 instances & the OS or containers
- Runs an assessment/benchmark check against best practices 
- Rules packages determine what is checked
- Agent required checks for stuff like host assessments , includes CVEs , CIS benchmarks, best practices

## Trusted Advisor 

- account level, so no agents
- Focuses on cost, performance, security, fault tolerance and service limits
- 7 core checks with basic and dev support plans
    - S3 bucket permissions - not objkects
    - Security Groups: specific ports unrestricted
    - IAM usage
    - MFA on Root account
    - EBS public snapshots
    - RDS publick snapshots
    - 50 service limit checks ( looks for 80%+ utlization)
 - Enterprise support has 115 further checks (14 cost, 17 security, 24 fault tolerance, 10 performance and 50 service limit). Also provides the AWS support API 

## VPC Flow Logs

- Capture packet metadata, not the contents
- Can be attached to a VPC / so all interfaces
- Can be attached to all ENIS in a subnet
- Can be attached to ENIs directly
- FLow logs are not realtime
- Can go to s3 or cloudwatch logs
- Can use Athena for querying
- icmp =1 , tcp = 6 and udp = 17
- some things are excluded, time server, dhcp, amazon windows license server and to the metadata service


## CloudTrail

- Logs API actions and account events as CloudTrail Events
- Stores last 90 days of history of Event History by default (no cost)
- Events can be Managedment, Data and Insight Events
- Trails can be one region or all regions
- global services can sometimes only send their events to us-east-1
- Data events not enabled by default
- Can put data into s3 or cloudwatch logs, the s3 bucket can be cross account if needed however requester pays must be off
- s3 must be enabled on trails

## AWS Macie

- Data Security & Privacy service
- Can be used to automate discovery of data, PII, PHI and Finance data
- Discover Monitor and Protect data stored in S3
- Data identifies: Managed or Custom rules to identify this data
- Custom Data identifies: Propiertary - Regex based
- Integrates with Security Hub & finding events to Event Bridge
- Supports Multi account strucutres either via Org or one Macie Account inviting other accounts
- Discovery Jobs: Analyzing objects in s3 buckets using data idenitifers

## AWS Open Search

- s

## Kinesis

- Data Streams: You cna use to collect and process large streams of data records in real time
- firehose: Does not automatically encrypt data in transit

## IoT Core

### IoT Device Defender

- can audit the config of your devices, member connected devices to detect abnormal behavior and mitigate security risks.


## Layer 7 firewalls

- Adds additional capabilities compared to other firewalls
   - Example for HTTP: they understand the headers, hosts, data etc
      - can identity protocol specific attacks
      - looks for abnormailities in the protocols
      - 
## Athena

- Serverless interactive querying service
- adhoc queires, pay only data consunmed
- Uses Schema on read: table like translation, never alters data in s3
- Supports sources such as xml, json  as well as from aws sources such as cloud trail, elb and flow logs

## Amazon Security Lake

- Centralized security data lake , supports ocsf 