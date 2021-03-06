
# Menu

* [Data-Pipeline](#data-pipeline)
* [AWS-IOT](#aws-iot)
* [Elasticsearch](#elasticsearch)
* [Kinesis](#kinesis)
* [RStudio](#rstudio)

## Data Pipeline

[Templates](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/welcome.html) available are -

* Process Data Using Amazon EMR with Hadoop Streaming
* Import and Export DynamoDB Data Using AWS Data Pipeline
* Copy CSV Data Between Amazon S3 Buckets Using AWS Data Pipeline
* Export MySQL Data to Amazon S3 Using AWS Data Pipeline
* Copy Data to Amazon Redshift Using AWS Data Pipeline

## AWS-IOT

| Component   |      Reference      |
|----------|:-------------:|
| Python IoT SDK |  https://github.com/aws/aws-iot-device-sdk-python |
| Device Gateway | Responsible for maintaining the sessions and subscriptions for all connected devices in an IoT solution |
| Rule Engine | It utilizes a SQL-based syntax that selects data from message payloads and triggers actions based on the characteristics of the IoT data|
| Device Shadow | It acts as a message channel to send commands reliably to a device, and store the lastknown state of a device in the AWS platform. |
| Things | The device and fleet of devices |
| Control Layer | The control point for access to the Speed Layer and the nexus for fleet management |
| Speed Layer | The inbound, high-bandwidth device telemetry data bus and the outbound device command bus |
| Serving Layer | The access point for systems and humans to interact with the devices in a fleet, to perform analysis, archive, and correlate data, and to use real-time views of the fleet.|
| Authorization | https://docs.aws.amazon.com/iot/latest/developerguide/authorization.html |

Ref - https://d0.awsstatic.com/whitepapers/core-tenets-of-iot1.pdf

### AWS IOT Rule Actions

Used to specify what to do when a rule is triggered.

* cloudwatchAlarm to change a CloudWatch alarm.
* cloudwatchMetric to capture a CloudWatch metric.
* dynamoDB to write data to a DynamoDB database.
* dynamoDBv2 to write data to a DynamoDB database.
* elasticsearch to write data to an Amazon Elasticsearch Service domain.
* firehose to write data to an Amazon Kinesis Data Firehose stream.
* iotAnalytics to send data to an AWS IoT Analytics channel.
* kinesis to write data to a Kinesis stream.
* lambda to invoke a Lambda function.
* republish to republish the message on another MQTT topic.
* s3 to write data to an Amazon S3 bucket.
* salesforce to write a message to a Salesforce IoT input stream.
* sns to write data as a push notification.
* sqs to write data to an SQS queue.
* stepFunctions to start execution of a Step Functions state machine.

Ref - https://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html

## Elasticsearch

| Component   |      Reference      |
|----------|:-------------:|
| Elasticsearch Domain |  https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-managedomains.html |
| Sample Demo Setup with VPC Logs | https://aws.amazon.com/blogs/security/how-to-visualize-and-refine-your-networks-security-by-adding-security-group-ids-to-your-vpc-flow-logs/#more-3559 | 

## Kinesis
General Kinesis Information and Libraries

### General Information

| Component   |      Reference      |
|----------|:-------------:|
| Kinesis Producer Library |  https://github.com/awslabs/amazon-kinesis-producer |
| Kinesis Connector Library | https://github.com/awslabs/amazon-kinesis-connectors | 
| Kinesis Data Transformation | https://aws.amazon.com/blogs/compute/amazon-kinesis-firehose-data-transformation-with-aws-lambda/ |


### Shards

| Name   |      Provisioning      |
|----------|:-------------:|
| 1 Stream |  2 Shards |
| Data Input | 2 Mb/sec |
| Data Output | 4 Mb/sec |
| Reads | 10 transactions/sec |
| Writes | 2000 records/sec |

Resharding

* Shard Split
* Shard Merge

### Record

It is composed of 

* Partition Key
* Sequence Number
* Data Blob

### Kinesis Connector Library Pipeline Workflow

* (1) Kinesis Stream 
* (2) iTransformer
* (3) iFilter
* (4) iBuffer
* (5) iEmitter
* (6) S3, Dynamodb, RedShift etc.


### Kinesis Firehose

| Component Name   | Max Retry Time Period  |   Failed Folder Name |
|------------------:|:-------------------------:|----------------------|
| S3 | 24hrs | failed objects are stored in (s3_failed folder) |
| RedShift | 0-120 min (0-7200 sec) | Skips S3 objects and creates a manifest file in S3 for manual backup under (errors folder) |
| Elasticsearch | 0-120 min (0-7200 sec) | Skips index requests and stores in (elasticsearch_failed folder)|

* The PutRecordBatch() operation can take up to 500 records per call or 4 MB per call, whichever is smaller. 
* Buffer size ranges from 1 MB to 128 MB.

## Lambda Blueprints for Kinesis Fireshose for Data Transformation

* Apache Log to JSON
* Apache Log to CSV
* Syslog to JSON
* Syslog to CSV
* General Firehose Processing

## RStudio

Can be integrated with -

* RedShift
* S3
* EMR
* RDS
* Athena

No Connectors. But can be implemented using drivers and packages on EC2 hosted RStudio.

RStudio Installation - https://www.rstudio.com/products/rstudio/download/#download
R Installation - https://cran.rstudio.com/

Installation on EC2 Amazon Linux - 
```
-- install R
sudo yum install -y R

-- download and install RStudio-Server
wget https://download2.rstudio.org/rstudio-server-rhel-1.0.153-x86_64.rpm
sudo yum install -y --nogpgcheck rstudio-server-rhel-1.0.153-x86_64.rpm

-- add user(s)
sudo useradd rstudio

-- install libcurl-devel package
sudo yum install libcurl-devel.x86_64 -y
```

EC2 Login Page - https://<YOUR-EC2-DNS-NAME>:8787
Login ID - rstudio (created above)
Password - As Set above in the the shell script

Run the below commands on RStudio console - 
```
-- install RCurl package in RStudio
install.packages(â€œRCurl")

-- load RCurl
library("RCurl")

-- Code to read an AWS public data set from S3 -
data <- read.table(textConnection(getURL("https://cgiardata.s3-us-west-2.amazonaws.com/ccafs/amzn.csv")), sep=",", header=FALSE) 

head(data)
```

References -

* https://aws.amazon.com/blogs/big-data/connecting-r-with-amazon-redshift/
* https://aws.amazon.com/blogs/big-data/statistical-analysis-with-open-source-r-and-rstudio-on-amazon-emr/
* https://aws.amazon.com/blogs/big-data/running-r-on-aws/
* https://aws.amazon.com/blogs/big-data/statistical-analysis-with-open-source-r-and-rstudio-on-amazon-emr/
