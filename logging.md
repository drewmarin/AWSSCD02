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
