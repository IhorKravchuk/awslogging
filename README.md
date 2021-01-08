# Native AWS logging capabilities

Unofficial documentation on the AWS services logging capabilities. Structured and enhanced.

official doc (missing a lot of services): https://aws.amazon.com/answers/logging/aws-native-security-logging-capabilities/

* [CloudTrail](#cloudtrail)
* [AWS ACM (Certificate Manager)](#aws_acm)
* [API Gateway](#apig)
* [AWS Amplify](#aws_amplify)
* [AWS App Mesh](#aws_mesh)
* [AWS AppSync](#appsync)
* [AWS Audit Manager](#audit_mngr)
* [Amazon Aurora](#aurora)
* [Amazon Auto Scaling (EC2)](#auto_ec2)
* [AWS Batch](#aws_batch)
* [Amazon Chime](#chime)
* [Amazon CloudFormation](#cf)
* [CloudFront](#cloudfront)
* [AWS CodeBuild](#code_build)
* [AWS CodeDeploy](#code_deploy)
* [AWS Config](#aws_config)
* [Amazon Connect](#connect)
* [AWS Data Pipeline](#data_pipe)
* [AWS DataSync](#data_sync)
* [AWS Directory Service](#directory)
* [AWS DMS (Database Migration Service)](#dms)
* [Amazon DocumentDB](#document_db)
* [Amazon DynamoDB, streams](#dynamodb_stream)
* [Amazon ECS - AWS Fargate](#fargate)
* [Amazon EKS (Elastic Kubernetes Service)](#eks)
* [Amazon ElastiCache for Redis](#cache)
* [AWS Elastic Beanstalk](#beanstalk)
* Load Balancers:
    * [Elastic Load Balancer(ELB) logs (classic)](#elblogs)
    * [Network Load Balancer(NLB) logs](#nlblogs)
    * [Aplication Load Balancer(ALB) logs](#alblogs)
* [Amazon EMR](#emr)
* [Amazon Kinesis Data Firehose](#firehose)
* [AWS Global Accelerator](#global_accelerator)
* [AWS IoT Greengrass](#green_grass)
* [Amazon Image Builder (EC2)](#image_builder)
* [AWS Import/Export](#aws_import)
* [Amazon Kafka (Managed Streaming for Apache Kafka)](#kafka)
* [Amazon Kendra](#kendra)
* [Amazon Kinesis Data Analytics](#kinesis_data)
* [AWS Lambda](#lambda)
* [Amazon Lex](#lex)
* [Amazon Lightsail](#lightsail)
* [Amazon Neptune](#neptune)
* [AWS Network Firewall](#net_fw)
* [OpsWorks](#opsworks)
* [Amazon (QLDB) Quantum Ledger Database](#ledger)
* [Amazon RDS (Relational Database Service)](#rds)
* [Amazon Redshift](#redshift)
* [Amazon Route53](#r53)


* [VPC Flow Logs](#vpcflowlogs)
* [S3 Server Access Logs](#s3accesslogs)







* [AWS WAF](#waf)

* [AWS Systems Manager](#sysman)





* [CloudWatch Logs](#cloudwatchlogs)

Services that logs only to the CLoudTrail (Control plane events only):

* SQS
* ECR
* AWS Glue
* [DynamoDB](#dynamodb)

## <a name="cloudtrail"></a> CloudTrail or AWS Account itself 
* Log coverage: 
    * all AWS API calls (covers web-ua, api or SDK actions)
    * List of the services covered by cloudtrail
     https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html
    * Options:
      1. A trail that applies to all regions
      2. A trail that applies to one region
      3. organization trail - global for all subaccount in organization
* Default status and how to enable:
    * Yes, but should be configured with S3 for long-term storage or CloudWatch to enable metrics.
    * To enable: https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html
* Exceptions and Limits:
    * Do not cover newest services/and api calls:
    * List of the uncovered services: 
    https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-unsupported-aws-services.html
* Log record/file format:
    * json, structure is service-specific
* Delivery latency:
    * Within 15 min of the activity
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudWatch Logs
    * S3 bucket
* Encryption at rest:
    * S3 - AES256, S3 SSE with amazon keys or KMS
* Data residency(AWS Region):
    * S3 bucket from any region.
* Retention capabilities:
    * AWS Cloudtrail Console (EventHistory) - 90 days
    * S3 -indefinite time/user defined
    * CloudWatch Logs:  indefinitely and never expire. User can define retention policy per log group (indefinite, or from 1 day to 10years)

## <a name="aws_acm"></a> AWS ACM (Certificate Manager)
* Log coverage:
    * API calls to CloudTrail
    * Certs --> certificate transparency logs
* Default status and how to enable:
    * Enabled by default
    * Could be disabled to prevent domain info discosure for internal or private domains: https://docs.aws.amazon.com/acm/latest/userguide/acm-bestpractices.html#best-practices-transparency
* Exceptions and Limits:
* Log record/file format:
    * Certificate transparency logs
* Delivery latency: 
* Transport/Encryption in transit:
* Supported log Destinations:
    * Certificate transparency logs
* Encryption at rest:
* Data residency(AWS Region):
* Retention capabilities:


## <a name="apig"></a>API Gateway
* Log coverage:
    * Logs API requests and responses
    * https://docs.aws.amazon.com/apigateway/latest/developerguide/view-cloudwatch-log-events-in-cloudwatch-console.html
* Default status and how to enable:
    * Disabled by deafult
    * To enable: https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-logging.html
* Exceptions and Limits:
    * Note: API Gateway creates log groups or log streams for an API stage at the time when it is deployed
* Log record/file format:
* Delivery latency:
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudWatch Logs
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined

## <a name="aws_amplify"></a> AWS Amplify
* Log coverage:
    * Access logs store information about all requests that are made to your Amplify hosted app.
    * Details: https://docs.aws.amazon.com/amplify/latest/userguide/access-logs.html
* Default status and how to enable:
    * Enabled by default
* Exceptions and Limits:
    * Need to be manually exported to S3 for longer storage
    * One way to analyze your access logs is to use Amazon Athena
* Log record/file format:
    * CSV
* Delivery latency: 
* Transport/Encryption in transit:
* Supported log Destinations:
    * Amplify Console
* Encryption at rest:
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * All sites hosted on Amplify Console have logs retrievable for any two week window

## <a name="aws_mesh"></a> AWS App Mesh
* Log coverage:
    * Access logs
    * Statistics
    * Proxy logs
    * Details: https://docs.aws.amazon.com/app-mesh/latest/userguide/observability.html
* Default status and how to enable:
    * Disabled by default
    * Proxy: Your specific logging level can be configured using the ENVOY_LOG_LEVEL environment variable
    * Access Logs: Advanced configuration section of the virtual node create or update workflows.
    * Enable access logs on Kubernetes: configure virtual nodes with access logging by adding the logging configuration to the virtual node spec
* Exceptions and Limits:
    * produced on the virtual nodes and use container-level primitives such as sending logs to stdout and stderr. 
    * logs can be proxied to CloudWatch Logs via an on-host agent
    * mixed in with the Envoy container logs
* Log record/file format:
* Delivery latency: 
* Transport/Encryption in transit:
    * produced on the virtual nodes
    * via https to the CloudWatch Logs
* Supported log Destinations:
    * /dev/stdout
    * /dev/stderr
    * CloudWatch Logs via standard Docker log drivers such as awslogs (https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_awslogs.html)
* Encryption at rest:
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * as per virtula node lifetime
    * CloudWatch Logs:  indefinitely and never expire. User can define retention policy per log group (indefinite, or from 1 day to 10years)

## <a name="appsync"></a> AWS AppSync
* Log coverage:
    * request-level logs
        * The request and response HTTP headers
        * The GraphQL query that is being executed in the request
        * The overall execution summary
        * New and existing GraphQL subscriptions that are registered
    * field-level logs
        * Generated Request Mapping with source and arguments for each field
        * The transformed Response Mapping for each field, which includes the data as a result of resolving that field
        * Tracing information for each field
* Default status and how to enable:
    * Disabled by default
    * To enable: https://docs.aws.amazon.com/appsync/latest/devguide/monitoring.html
* Exceptions and Limits:
* Log record/file format:
    * log group is named following the /aws/appsync/apis/{graphql_api_id} format. Within each log group, the logs are further divided into log streams.
    * Every log event is tagged with the x-amzn-RequestId of that request.
    * request-level logs:
        * The request and response HTTP headers
        * The GraphQL query that is being executed in the request
        * The overall execution summary
        * New and existing GraphQL subscriptions that are registered
    * field-level logs:
        * field-Level logging is configured with the following log levels:
            * NONE - No field-level logs are captured.
            * ERROR - Logs the following information only for the fields that are in error: 
                * The error section in the server response
                * Field-level errors
                * The generated request/response functions that got resolved for error fields 
            * ALL - The following information is logged for all fields in the query:
                * Field-level tracing information
                * The generated request/response functions that got resolved for each field
* Delivery latency:
    * as per CloudWatchLogs
* Transport/Encryption in transit:
    * as per CloudWatchLogs
* Supported log Destinations:
    * CloudWatchLogs
* Encryption at rest:
    * as per CloudWatchLogs
* Data residency(AWS Region):
    * any region
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined

## <a name="audit_mngr"></a> AWS Audit Manager
* Log coverage:
    * Change Logs - User activity related to selected controls .
    * Details: https://docs.aws.amazon.com/audit-manager/latest/userguide/review-controls.html#review-changelog
    * AWS Audit Manager tracks the following user activity in changelogs:
        * Creating an assessment
        * Editing an assessment
        * Completing an assessment
        * Deleting an assessment
        * Delegating a control set for review
        * Submitting a reviewed control set back to the audit owner
        * Uploading manual evidence
        * Updating a control status
        * Generating assessment reports
* Default status and how to enable:
    * Enabled by default
* Exceptions and Limits:
* Log record/file format:
    * Under Changelog, a table displays the following data columns:
        * Date – The date and time of the activity.
        * User – The IAM user or role that performed the activity.
        * Action – A description of the activity.
        * Type – The associated attribute that further describes the activity.
        * Resource – The related resource, if applicable
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * Internal database in the Audit Manager service. 
    * Change Logs can be accessed via the UI or API
* Encryption at rest:
* Data residency(AWS Region):
* Retention capabilities:

## <a name="aurora"></a> Amazon Aurora
* Log coverage:
    * To monitor database activity with Amazon Aurora, you can use the Database Activity Streams feature. Database activity streams provide a near real-time stream of the activity in your relational database. 
    * Details: https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/DBActivityStreams.html
* Default status and how to enable:
    * Disabled by default
    * To enable: https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/DBActivityStreams.html#DBActivityStreams.Enabling
* Exceptions and Limits:
    * For Aurora **PostgreSQL**, database activity streams are supported for version 2.3 or higher and versions 3.0 or higher. For PostgreSQL version compatibility, see Engine versions for Amazon Aurora PostgreSQL.
    * For Aurora **MySQL**, database activity streams are supported for version 2.08 or higher, which is compatible with MySQL version 5.7.
    * You can start a database activity stream on the primary or secondary cluster of an Aurora global database. For DB engine version requirements, see Limitations of Amazon Aurora global databases.
    * Database activity streams support the DB instance classes listed for Aurora in Supported DB engines for DB instance classes, with some exceptions:
    * For Aurora **PostgreSQL**, you can't use streams with the **db.t3.medium** instance class.
    * For **Aurora MySQL**, you can't use streams with any of the **db.t2 or db.t3** instance classes.
    * Database activity streams aren't supported in the following AWS Regions:
        * China (Beijing) Region, cn-north-1
        * China (Ningxia) Region, cn-northwest-1
        * Asia Pacific (Osaka-Local), ap-northeast-3
        * AWS GovCloud (US-East), us-gov-east-1
        * AWS GovCloud (US-West), us-gov-west-1
    * Database activity streams require use of AWS Key Management Service (AWS KMS). AWS KMS is required because the activity streams are always encrypted.
Database activity streams aren't supported in Aurora Serverless.
* Log record/file format:
    * JSON
    * Details of the JSON for,mat could be found here: https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/DBActivityStreams.html ,  Section **Audit log contents and examples**
    * Fields: (TBD)
* Delivery latency:
    * near real-time
* Transport/Encryption in transit:
* Supported log Destinations:
    * Kinesis
* Encryption at rest:
    * always encrypted via KMS
* Data residency(AWS Region):
    * regional
    * see list of unsupported region above (Exceptions and Limits)
* Retention capabilities:
    * as per Kinesis based destination:
        * Amazon Kinesis Data Firehose and from there to S3
        * AWS Lambda

## <a name="auto_ec2"></a> Amazon Auto Scaling (EC2)
* Log coverage:
    * autoscaling events
    * Details: https://docs.aws.amazon.com/autoscaling/ec2/userguide/ASGettingNotifications.html
* Default status and how to enable:
    * Enabled by default
    * SNS integration needs to be enabled
* Exceptions and Limits:
    * not exactly a log, but extremely useful event
* Log record/file format:
    * event
* Delivery latency:
    * near real time events
* Transport/Encryption in transit:
* Supported log Destinations:
    * An internal Auto Scaling database of event activity
    * SNS
* Encryption at rest:
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * as per SNS

## <a name="aws_batch"></a> AWS Batch
* Log coverage:
    * All jobs logs
    * With the new release of customer logging driver support, customers are now able to adjust how the job output is logged.
    * Logging details: https://docs.aws.amazon.com/batch/latest/userguide/using_cloudwatch_logs.html
    * Custom logging details: https://aws.amazon.com/blogs/compute/custom-logging-with-aws-batch/
* Default status and how to enable:
    * Enabled by default only to stdout
* Exceptions and Limits:
* Log record/file format:
    * default - stdout
    * with custom logging driver:
        * splunk
        * fluentd
        * json-files
        * syslog
        * gelf
        * journald
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * stdout
    * CloudWatch Logs via awslog driver (https://docs.aws.amazon.com/batch/latest/userguide/using_awslogs.html)
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined

## <a name="chime"></a> Amazon Chime
* Log coverage:
    * Voice Connector media quality metrics
    * SIP message logs.
    * Details: https://docs.aws.amazon.com/chime/latest/ag/monitoring-cloudwatch.html#cw-logs
* Default status and how to enable:
    * Disabled by default
* Exceptions and Limits:
* Log record/file format:
    * Media quality metric logs
        * JSON
        * log group name is /aws/ChimeVoiceConnectorLogs/${VoiceConnectorID}.
        *  Fields: (TBD)
     * SIP message logs
        * JSON
        * log group name is /aws/ChimeVoiceConnectorSipMessages/${VoiceConnectorID}.
        *  Fields: 
            * voice_connector_id : The Amazon Chime Voice Connector ID.
            * aws_region: The AWS Region associated with the event.
            * event_timestamp: The time when the message is captured, in number of milliseconds since the UNIX epoch (midnight on January 1, 1970) in UTC.
            * call_id: The Amazon Chime Voice Connector call ID.
            * sip_message: The full SIP message that is captured.    
* Delivery latency:
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudWatch Logs
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined


## <a name="cf"></a> Amazon CloudFormation
* Log coverage:
    * Stack events.
    * CloudFormation console UI under the “Stack Events” tab
    * Details: https://aws.amazon.com/premiumsupport/knowledge-center/cloudformation-rollback-email/
* Default status and how to enable:
    * Enabled by default
    * SNS integration needs to be enabled
* Exceptions and Limits:
    * not exactly a log, but extremely useful event
* Log record/file format:
    * event
* Delivery latency:
    * near real time events
* Transport/Encryption in transit:
* Supported log Destinations:
    * An internal CloudFormation database
    * SNS
* Encryption at rest:
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * as per SNS

## <a name="cloudfront"></a>CloudFront
* Log coverage:
    * Logs every user request that CloudFront receives. These access logs are available for both web and RTMP distributions
    * https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html
    * Process:
        * CloudFront routes each request to the appropriate edge location.
        * CloudFront writes data about each request to a log file specific to that distribution. For example, information about requests related to Distribution A goes into a log file just for Distribution A, and information about requests related to Distribution B goes into a log file just for Distribution B.
        * CloudFront periodically saves the log file for a distribution in the Amazon S3 bucket that you specified when you enabled logging. CloudFront then starts saving information about subsequent requests in a new log file for the distribution.
    * (NEW) Real-time logs:
        * The sampling rate for your real-time logs—that is, the percentage of requests for which you want to receive real-time log records.
        * The specific fields that you want to receive in the log records.
        * The specific cache behaviors (path patterns) that you want to receive real-time logs for.
        * Details: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/real-time-logs.html
* Default status and how to enable:
    * Disabled by default
* Exceptions and Limits:
    * Note, however, that some or all log file entries for a time period can sometimes be delayed by up to 24 hours (for non-real time logs)
* Log record/file format:
    * Web Distribution Log File Format: 
    * RTMP Distribution Log File Format
    * Each entry in a log file gives details about a single user request. The log files for web and for RTMP distributions are not identical, but they share the following characteristics:
        * Use the W3C extended log file format. (For more information, go to http://www.w3.org/TR/WD-logfile.html.)
        * Contain tab-separated values.
        * Contain records that are not necessarily in chronological order.
        * Contain two header lines: one with the file-format version, and another that lists the W3C fields included in each record.
        * Substitute URL-encoded equivalents for spaces and non-standard characters in field values.
    * More details: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html#LogFileFormat
    * Real-time logs details: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/real-time-logs.html
* Delivery latency:
    * up to several times an hour 
    * real-time via Kinesis 
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * S3 bucket
    * Amazon Kinesis Data Streams
* Encryption at rest:
    * S3 - AES256, S3 SSE with amazon keys
    * as per Amazon Kinesis
* Data residency(AWS Region):
    * As per S3 bucket location
    * As per Kinesis
* Retention capabilities:
    * S3 -indefinite time/user defined
    * As per Kinesis configuration and destination/consumer (S3, ES, Redshift, Lambda, Splunk, etc)


## <a name="code_build"></a> AWS CodeBuild
* Log coverage:
    * Job build logs
    * Details: https://docs.aws.amazon.com/codebuild/latest/APIReference/API_LogsLocation.html 
* Default status and how to enable:
    * Enabled by default for the CloudWatch Logs
    * Disabled by default for the S3
    * Configurable during job creation
* Exceptions and Limits:
* Log record/file format:
* Delivery latency:
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CodeBuild UI
    * CloudWatch logs
    * S3
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined


## <a name="code_deploy"></a> AWS CodeDeploy
* Log coverage:
    * Deployment logs for EC2 or on-premise deployments.
    * Details: https://docs.aws.amazon.com/codedeploy/latest/userguide/deployments-view-logs.html
        * Instance level logs: https://docs.aws.amazon.com/codedeploy/latest/userguide/deployments-view-logs.html#deployments-view-logs-instance
        * Cloudwatch Logs: https://aws.amazon.com/blogs/devops/view-aws-codedeploy-logs-in-amazon-cloudwatch-console/
* Default status and how to enable:
    * Instance level: Enabled by default
    * CloudWatch Logs: Disabled by default. 
        * To enable ClouWatch Agent must be installed
        * Details: https://aws.amazon.com/blogs/devops/view-aws-codedeploy-logs-in-amazon-cloudwatch-console/
* Exceptions and Limits:
    * Deployments to Lambda or ECS are not logged.
* Log record/file format:
    * Linux instance: opt/codedeploy-agent/deployment-root/deployment-logs/codedeploy-agent-deployments.log
    * Windows instance: C:\ProgramData\Amazon\CodeDeploy\log\codedeploy-agent-log.txt
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * text file of the EC2 instance 
    * CloudWatch Logs via CloudWatch Logs Agent
* Encryption at rest:
* Data residency(AWS Region):
* Retention capabilities:


## <a name="aws_config"></a> AWS Config
* Log coverage:
    * configuration changes of the AWS Infrastructure
    * config rule evaluation results and resource compliance status
* Default status and how to enable:
    * Config as a service is disabled by default
    * SQS integration needs to be enabled: https://docs.aws.amazon.com/config/latest/developerguide/monitor-resource-changes.html
* Exceptions and Limits:
    * not exactly a log, but extremely useful events
* Log record/file format:
    * event
* Delivery latency:
    * near real time events
* Transport/Encryption in transit:
* Supported log Destinations:
    * S3
    * SQS
    * EventBridge
* Encryption at rest:
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * as per S3


## <a name="connect"></a> Amazon Connect
* Log coverage:
    * Connect contact flows.
    * Details: https://docs.aws.amazon.com/connect/latest/adminguide/contact-flow-logs.html#enable-contact-flow-logs
* Default status and how to enable:
    * CloudWatch integration is enabled by default
    * Logging for each contact flow must be manually enabled
* Exceptions and Limits:
    * Logs are generated only for contact flows that include a **Set logging behavior** block with logging set to enabled
* Log record/file format:
    * CloudWatch Logs group associated with a Connect instances.
* Delivery latency:
    * as per CloudWatchLogs
* Transport/Encryption in transit:
    * as per CloudWatchLogs
* Supported log Destinations:
    * CloudWatchLogs
* Encryption at rest:
    * as per CloudWatchLogs
* Data residency(AWS Region):
    * any region
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined


## <a name="data_pipe"></a> AWS Data Pipeline
* Log coverage:
    * Pipeline task logs.
    * Details: https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-viewing-logs.html
* Default status and how to enable:
    * Console logs are enabled
    * Logs to S3 disabled by default and must be configured
* Exceptions and Limits:
* Log record/file format:
    * The directory structure for each pipeline within that URI is like the following:
        * pipelineId
            * componentName
                * instanceId
                   * attemptId
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * Console
    * S3
* Encryption at rest:
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * short time in console
    * as per S3

## <a name="data_sync"></a> AWS DataSync
* Log coverage:
    * Detailed logging for files and objects copied between your NFS servers, SMB servers, Amazon S3 buckets, Amazon Elastic File System (EFS) file systems, and Amazon FSx for Windows File Server file systems.
    * Details: https://docs.aws.amazon.com/datasync/latest/userguide/monitor-datasync.html#cloudwatchlogs
* Default status and how to enable:
    * Disabled by default, must be configured via Task settings
    * Details: https://docs.aws.amazon.com/datasync/latest/userguide/create-task.html
* Exceptions and Limits:
* Log record/file format:
* Delivery latency:
    * as per CloudWatchLogs
* Transport/Encryption in transit:
    * as per CloudWatchLogs
* Supported log Destinations:
    * CloudWatchLogs
* Encryption at rest:
    * as per CloudWatchLogs
* Data residency(AWS Region):
    * any region
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined

## <a name="directory"></a> AWS Directory Service
* Log coverage:
    * Domain controller logs for Managed Microsoft AD directory.
    * Details: https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_directory_logs.html
* Default status and how to enable:
    * Enabled by default.
    * CloudWatch Logs integration is supported and must be configured   
        * Details: https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_enable_log_forwarding.html
* Exceptions and Limits:
* Log record/file format:
    * AWS logs the following events for compliance:
        * Account Logon:
            * Audit Credential Validation:	Success, Failure
            * Audit Other Account Logon Events:	Success, Failure
        * Account Management:
            * Audit Computer Account Management:	Success, Failure
            * Audit Other Account Management Events:	Success, Failure
            * Audit Security Group Management:	Success, Failure
            * Audit User Account Management:	Success, Failure
        * Detailed Tracking:
            * Audit DPAPI Activity:	Success, Failure
            * Audit PNP Activity:	Success
            * Audit Process Creation:	Success, Failure
        * DS Access:
            * Audit Directory: Service Access	Success, Failure
            * Audit Directory Service Changes:	Success, Failure
        * Logon/Logoff:
            * Audit Account Lockout:	Success, Failure
            * Audit Logoff:	Success
            * Audit Logon:	Success, Failure
            * Audit Other Logon/Logoff Events:	Success, Failure
            * Audit Special Logon:	Success, Failure
        * Object Access:
            * Audit Other Object Access Events:	Success, Failure
            * Audit Removable Storage:	Success, Failure
            * Audit Central Access Policy Staging:	Success, Failure
        * Policy Change:
            * Audit Policy Change:	Success, Failure
            * Audit Authentication Policy Change:	Success, Failure
            * Audit Authorization Policy Change:	Success, Failure
            * Audit MPSSVC Rule-Level Policy Change:	Success
            * Audit Other Policy Change Events:	Failure
        * Privilege Use:	
            * Audit Sensitive Privilege Use:	Success, Failure
        * System:	
            * Audit IPsec Driver:	Success, Failure
            * Audit Other System Events:	Success, Failure
            * Audit Security State Change:	Success, Failure
            * Audit Security System Extension:	Success, Failure
            * Audit System Integrity:	Success, Failure
* Delivery latency:
    * as per CloudWatchLogs
* Transport/Encryption in transit:
    * as per CloudWatchLogs
* Supported log Destinations:
    * CloudWatchLogs
* Encryption at rest:
    * as per CloudWatchLogs
* Data residency(AWS Region):
    * any region
* Retention capabilities:
    * Default: Security logs from AWS Managed Microsoft AD domain controller instances are archived for a year.
    * CloudWatch logs: indefinite time/user defined


## <a name="dms"></a> AWS DMS (Database Migration Service)
* Log coverage:
    * Migration task logs.
    * Details: https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TaskSettings.Logging.html
* Default status and how to enable:
    * Disabled by default, 
    * Using logging task settings, you can specify which component activities are logged and what amount of information is written to the log. 
    * Details: https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TaskSettings.Logging.html
* Exceptions and Limits:
* Log record/file format:
    * You can specify logging for the following actions:
        * SOURCE_UNLOAD – Data is unloaded from the source database.
        * SOURCE_CAPTURE – Data is captured from the source database.
        * TARGET_LOAD – Data is loaded into the target database.
        * TARGET_APPLY – Data and data definition language (DDL) statements are applied to the target database.
        * TASK_MANAGER – The task manager triggers an event.
    * The levels of severity are in order from lowest to highest level of information. The higher levels always include information from the lower levels.
        * LOGGER_SEVERITY_ERROR – Error messages are written to the log.
        * LOGGER_SEVERITY_WARNING – Warnings and error messages are written to the log.
        * LOGGER_SEVERITY_INFO – Informational messages, warnings, and error messages are written to the log.
        * LOGGER_SEVERITY_DEFAULT – Informational messages, warnings, and error messages are written to the log.
        * LOGGER_SEVERITY_DEBUG – Debug messages, informational messages, warnings, and error messages are written to the log.
        * LOGGER_SEVERITY_DETAILED_DEBUG – All information is written to the log.
* Delivery latency:
    * as per CloudWatchLogs
* Transport/Encryption in transit:
    * as per CloudWatchLogs
* Supported log Destinations:
    * CloudWatchLogs
* Encryption at rest:
    * as per CloudWatchLogs
* Data residency(AWS Region):
    * any region
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined

## <a name="document_db"></a> Amazon DocumentDB
* Log coverage:
    * Profiler logs of the execution time and details of operations that were performed on the cluster.
    * Details: https://docs.aws.amazon.com/documentdb/latest/developerguide/logging-and-monitoring.html
* Default status and how to enable:
    * Disabled by default
* Exceptions and Limits:
* Log record/file format:
* Delivery latency:
    * as per CloudWatchLogs
* Transport/Encryption in transit:
    * as per CloudWatchLogs
* Supported log Destinations:
    * CloudWatchLogs
* Encryption at rest:
    * as per CloudWatchLogs
* Data residency(AWS Region):
    * any region
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined

## <a name="dynamodb_stream"></a> Amazon DynamoDB, streams
* Log coverage:
    * Changes to items stored in a DynamoDB table via streams
        Details: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/streamsmain.html
    * API action via CloudTrail
        * Details: [DynamoDB](#dynamodb)
* Default status and how to enable:
    * Streams are disabled by default
* Exceptions and Limits:
    * not exactly a log, but stream of the changes to items stored in a DynamoDB table
* Log record/file format:
* Delivery latency:
    * Pull model over HTTP using
* Transport/Encryption in transit:
    * Looks like None?: HTTP or HTTP/2
* Supported log Destinations:
    * DynamoDb
    * Kinesis
* Encryption at rest:
* Data residency(AWS Region):
* Retention capabilities:
    * Kinesis Data Streams for DynamoDB: up to 1 year
    * DynamoDB Streams: 24 hours

## <a name="fargate"></a> Amazon ECS (AWS Fargate)
* Log coverage:
    * You can configure the containers in your tasks to send log information to CloudWatch Logs. This allows you to view the logs from the containers in your Fargate tasks.
    * Details: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-logging-monitoring.html
* Default status and how to enable:
    * Enabled for the Fargate (but you need to specify log driver)
    * Needs to be configured for EC2
* Exceptions and Limits:
    * The type of information that is logged by your task's containers depends mostly on their ENTRYPOINT command. By default, the logs that are captured show the command output that you would normally see in an interactive terminal if you ran the container locally, which are the STDOUT and STDERR I/O streams.
* Log record/file format:
    * STDOUT and STDERR I/O streams
* Delivery latency:
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudWatch Logs
        * Details: https://docs.aws.amazon.com/AmazonECS/latest/userguide/using_awslogs.html
    * 3d party via supported log drivers
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined


## <a name="eks"></a> Amazon EKS (Elastic Kubernetes Service)
* Log coverage:
    * control plane logs:
        * Kubernetes control plane API
        * audit
        * authenticator
        * controller manager
        * scheduler
        * Details: https://docs.aws.amazon.com/eks/latest/userguide/control-plane-logs.html
    * worker node logs
    * task container logs
    * General EKS loging details: https://docs.aws.amazon.com/eks/latest/userguide/logging-monitoring.html
* Default status and how to enable:
    * Control Plane:
        * Disabled by default
        * To enable: https://docs.aws.amazon.com/eks/latest/userguide/control-plane-logs.html,  **Enabling and disabling control plane logs** section
* Exceptions and Limits:
* Log record/file format:
    * Cluster control plane:
        * /aws/eks/<cluster-name>/cluster
        * streams are:
            * Kubernetes API server component logs (api) – kube-apiserver-<nnn...>
            * Audit (audit) – kube-apiserver-audit-<nnn...>
            * Authenticator (authenticator) – authenticator-<nnn...>
            * Controller manager (controllerManager) – kube-controller-manager-<nnn...>
            * Scheduler (scheduler) – kube-scheduler-<nnn...>
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * Control Plane:
        * CloudWatch Logs
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * As per CloudWatchLogs configuration

## <a name="cache"></a> Amazon ElastiCache for Redis
* Log coverage:
    * events for different cluster-level activities (failure or success in adding new nodes, modifications to the security groups, etc.)
    * Details: https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ECEvents.Viewing.html
* Default status and how to enable:
    * Enabled by default as evens
    * SNS integration needs to be enabled: https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ECEvents.SNS.html
* Exceptions and Limits:
    * not exactly a log, but extremely useful event
* Log record/file format:
    * event (JSON)
* Delivery latency:
    * near real time events
* Transport/Encryption in transit:
* Supported log Destinations:
    * console
    * EventBridge
    * SNS
* Encryption at rest:
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * as per SNS
    * as per EventBridge

## <a name="beanstalk"></a> Elastic Beanstalk
* Log coverage:
    * The Amazon EC2 instances in your Elastic Beanstalk environment generate logs that you can view to troubleshoot issues with your application or configuration files. Logs created by the web server, application server, Elastic Beanstalk platform scripts, and AWS CloudFormation are stored locally on individual instances
    * https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.logging.html
* Default status and how to enable:
    * Enabled only for the local filesystem storage
    * CloudWatch forwarding needs to be enabled.
* Exceptions and Limits:
    * Elastic Beanstalk Windows Server platforms do not support bundle logs.
* Log record/file format:
    * Tail Logs - are the last 100 lines of the most commonly used log files—Elastic Beanstalk operational logs and logs from the web server or application server.
    * Bundle logs - are full logs for a wider range of log files, including logs from yum and cron and several logs from AWS CloudFormation
    * Rotated logs 
    * Log location on EC2 instance: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.logging.html#health-logs-instancelocation
        * Linux
            * /var/log/eb-activity.log
            * /var/log/eb-commandprocessor.log
            * /var/log/eb-version-deployment.log
        * Windows Server
            * C:\Program Files\Amazon\ElasticBeanstalk\logs\
            * C:\cfn\logs\cfn-init.log
        * Each application and web server stores logs in its own folder:
            * Apache – /var/log/httpd/
            * IIS – C:\inetpub\wwwroot\
            * Node.js – /var/log/nodejs/
            * nginx – /var/log/nginx/
            * Passenger – /var/app/support/logs/
            * Puma – /var/log/puma/
            * Python – /opt/python/log/
            * Tomcat – /var/log/tomcat8/
    * Log Location in Amazon S3: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.logging.html#health-logs-s3location
* Delivery latency:
    * as logs created - near real time(only in CloudWatch)
* Transport/Encryption in transit:
    * locally on the instance
    * logrotate to S3
* Supported log Destinations:
    * instance
    * CloudWatch Logs
    * S3
* Encryption at rest:
    * Instance encryption
    * As per CloudWatchLogs configuration (see below)
    * AES256 Encryption: Amazon S3 server-side encryption (SSE)
* Data residency(AWS Region):
    * As per instance location
    * any region
    * As per S3 bucket location
* Retention capabilities:
    * Instance lifetime
    * CloudWatch logs: indefinite time/user defined
    * Elastic Beanstalk deletes tail and bundle logs from Amazon S3 automatically 15 minutes after they are created. Rotated logs persist.

## <a name="elblogs"></a>Elastic Load Balancer(ELB) logs (classic)
* Log coverage:
    * access logs capture detailed information about requests sent to your load balancer. Each log contains information such as the time the request was received, the client's IP address, latencies, request paths, and server responses
    * https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/access-log-collection.html
* Default status and how to enable:
    * Disabled by default
* Exceptions and Limits:
    * Note: Elastic Load Balancing logs requests sent to the load balancer, including requests that never made it to the back-end instances
​
    * Limits: Elastic Load Balancing logs requests on a best-effort basis. We recommend that you use access logs to understand the nature of the requests, not as a complete accounting of all requests.
* Log record/file format:
    * Each log entry contains the details of a single request made to the load balancer. All fields in the log entry are delimited by spaces.
    * Format:
        1. timestamp
        2. elb client:port
        3. backend:port
        4. request_processing_time
        5.  backend_processing_time
        6. response_processing_time
        7. elb_status_code
        8. backend_status_code
        9. received_bytes
        10. sent_bytes
        11. "request"
        12. "user_agent"
        13. ssl_cipher
        14. ssl_protocol
* Delivery latency:
    * User defined publishing interval
    (5 min-60 min)
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * S3 bucket
* Encryption at rest:
    * * S3 - AES256, S3 SSE with amazon keys
* Data residency(AWS Region):
    * As per S3 bucket location
* Retention capabilities:
    * S3 -indefinite time/user defined

## <a name="nlblogs"></a> Network Load Balancer(NLB) Logs	
* Log coverage:
    * Access logs that capture detailed information about the TLS requests sent to your Network Load Balancer.
    * https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-access-logs.html
* Default status and how to enable:
    * Disabled by default
* Exceptions and Limits:
    * Access logs are created only if the load balancer has a TLS listener and they contain information only about TLS requests.
* Log record/file format:
    * All fields are delimited by spaces. When new fields are introduced, they are added to the end of the log entry.
    * Log file name format: *bucket[/prefix]/AWSLogs/aws-account-id/elasticloadbalancing/region/yyyy/mm/dd/aws-account-id_elasticloadbalancing_region_load-balancer-id_end-time_random-string.log.gz*
    * Access log fields:
        * type: The type of listener. The supported value is tls.
        * version: The version of the log entry. The supported version is 1.0.
        * timestamp: The timestamp recorded at the end of the TLS connection, in ISO 8601 format.
        * elb: The resource ID of the load balancer.
        * listener: The resource ID of the TLS listener for the connection.
        * client:port : The IP address and port of the client.
        * listener:port : The IP address and port of the listener.
        * connection_time: The total time for the connection to complete, from start to closure, in milliseconds.
        * tls_handshake_time: The total time for the TLS handshake to complete after the TCP connection is established, including client-side delays, in milliseconds. This time is included in the connection_time field.
        * received_bytes: The count of bytes received by the load balancer from the client, after decryption.
        * sent_bytes: The count of bytes sent by the load balancer to the client, before encryption.
        * incoming_tls_alert: The integer value of TLS alerts received by the load balancer from the client, if present. Otherwise, this value is set to -.
        * chosen_cert_arn: The ARN of the certificate served to the client. If no valid client hello message is sent, this value is set to -.
        * chosen_cert_serial: Reserved for future use. This value is always set to -.
        * tls_cipher: The cipher suite negotiated with the client, in OpenSSL format. If TLS negotiation does not complete, this value is set to -.
        * tls_protocol_version: The TLS protocol negotiated with the client, in string format. The possible values are tlsv10, tlsv11, and tlsv12. If TLS negotiation does not complete, this value is set to -.
        * tls_named_group: Reserved for future use. This value is always set to -.
        * domain_name: The value of the server_name extension in the client hello message. This value is URL-encoded. If no valid client hello message is sent or the extension is not present, this value is set to -.
* Delivery latency:
    * each 5 min
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * S3 bucket
* Encryption at rest:
    * * S3 - AES256, S3 SSE with amazon keys
* Data residency(AWS Region):
    * As per S3 bucket location
    * The bucket must be located in the same region as the load balancer.
* Retention capabilities:
    * S3 -indefinite time/user defined

## <a name="alblogs"></a>Application Load Balancer(ALB) logs	
* Log coverage:
    * Elastic Load Balancing logs requests sent to the load balancer, including requests that never made it to the targets. For example, if a client sends a malformed request, or there are no healthy targets to respond to the request, the request is still logged. 
    * https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html
* Default status and how to enable:
    * Disabled by default
* Exceptions and Limits:
    * Note: Elastic Load Balancing does not log health check requests
    * Limits: Elastic Load Balancing logs requests on a best-effort basis. We recommend that you use access logs to understand the nature of the requests, not as a complete accounting of all requests.
* Log record/file format:
    * Each log entry contains the details of a single request (or connection in the case of WebSockets) made to the load balancer. For WebSockets, an entry is written only after the connection is closed. If the upgraded connection can't be established, the entry is the same as for an HTTP or HTTPS request.
    All fields are delimited by spaces. When new fields are introduced, they are added to the end of the log entry.
    * Log file name format: *bucket[/prefix]/AWSLogs/aws-account-id/elasticloadbalancing/region/yyyy/mm/dd/aws-account-id_elasticloadbalancing_region_load-balancer-id_end-time_ip-address_random-string.log.gz*
    * Access log fields:
        * type: The type of request or connection. The possible values are as follows (ignore any other values):
            * http — HTTP
            * https — HTTP over SSL/TLS
            * h2 — HTTP/2 over SSL/TLS
            * ws — WebSockets
            * wss — WebSockets over SSL/TLS
        * timestamp: The time when the load balancer generated a response to the client, in ISO 8601 format. For WebSockets, this is the time when the connection is closed.
        * elb: The resource ID of the load balancer. If you are parsing access log entries, note that resources IDs can contain forward slashes (/).
        * client:port : The IP address and port of the requesting client.
        * target:port : The IP address and port of the target that processed this request. 
            * If the client didn't send a full request, the load balancer can't dispatch the request to a target, and this value is set to -.
            * If the target is a Lambda function, this value is set to -.
            * If the request is blocked by AWS WAF, this value is set to - and the value of elb_status_code is set to 403.
        * request_processing_time: The total time elapsed (in seconds, with millisecond precision) from the time the load balancer received the request until the time it sent it to a target.
            * This value is set to -1 if the load balancer can't dispatch the request to a target. This can happen if the target closes the connection before the idle timeout or if the client sends a malformed request.
            * This value can also be set to -1 if the registered target does not respond before the idle timeout.
        * target_processing_time: The total time elapsed (in seconds, with millisecond precision) from the time the load balancer sent the request to a target until the target started to send the response headers.
            * This value is set to -1 if the load balancer can't dispatch the request to a target. This can happen if the target closes the connection before the idle timeout or if the client sends a malformed request.
            * This value can also be set to -1 if the registered target does not respond before the idle timeout.
        * response_processing_time: The total time elapsed (in seconds, with millisecond precision) from the time the load balancer received the response header from the target until it started to send the response to the client. This includes both the queuing time at the load balancer and the connection acquisition time from the load balancer to the client.This value is set to -1 if the load balancer can't send the request to a target. This can happen if the target closes the connection before the idle timeout or if the client sends a malformed request.
        * elb_status_code: The status code of the response from the load balancer.
        * target_status_code: The status code of the response from the target. This value is recorded only if a connection was established to the target and the target sent a response. Otherwise, it is set to -.
        received_bytes: The size of the request, in bytes, received from the client (requester). For HTTP requests, this includes the headers. For WebSockets, this is the total number of bytes received from the client on the connection.
        * sent_bytes: The size of the response, in bytes, sent to the client (requester). For HTTP requests, this includes the headers. For WebSockets, this is the total number of bytes sent to the client on the connection.
        * "request" : The request line from the client, enclosed in double quotes and logged using the following format: HTTP method + protocol://host:port/uri + HTTP version. The load balancer preserves the URL sent by the client, as is, when recording the request URI. It does not set the content type for the access log file. When you process this field, consider how the client sent the URL.
        * "user_agent": A User-Agent string that identifies the client that originated the request, enclosed in double quotes. The string consists of one or more product identifiers, product[/version]. If the string is longer than 8 KB, it is truncated.
        * ssl_cipher: [HTTPS listener] The SSL cipher. This value is set to - if the listener is not an HTTPS listener.
        * ssl_protocol:[HTTPS listener] The SSL protocol. This value is set to - if the listener is not an HTTPS listener.
        * target_group_arn: The Amazon Resource Name (ARN) of the target group.
        * "trace_id": The contents of the X-Amzn-Trace-Id header, enclosed in double quotes.
        * "domain_name": [HTTPS listener] The SNI domain provided by the client during the TLS handshake, enclosed in double quotes. This value is set to - if the client doesn't support SNI or the domain doesn't match a certificate and the default certificate is presented to the client.
        * "chosen_cert_arn": [HTTPS listener] The ARN of the certificate presented to the client, enclosed in double quotes. This value is set to session-reused if the session is reused.
        * matched_rule_priority: The priority value of the rule that matched the request. If a rule matched, this is a value from 1 to 50,000. If no rule matched and the default action was taken, this value is set to 0. If an error occurs during rules evaluation, it is set to -1. For any other error, it is set to -.
        * request_creation_time: The time when the load balancer received the request from the client, in ISO 8601 format.
        * "actions_executed": The actions taken when processing the request, enclosed in double quotes. This value is a comma-separated list that can include the following possible values: waf, waf-failed, authenticate, redirect, fixed-response, and forward. If no action was taken, such as for a malformed request, this value is set to -.
        * "redirect_url": The URL of the redirect target for the location header of the HTTP response, enclosed in double quotes. If no redirect actions were taken, this value is set to -.
        * "error_reason": The error reason code, enclosed in double quotes. If the request failed, this is one of the error codes described in Error Reason Codes . If the actions taken do not include an authenticate action or the target is not a Lambda function, this value is set to -.
            * Error Reason Codes: https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html#error-reason-codes
* Delivery latency:
    * each 5 min
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * S3 bucket
* Encryption at rest:
    * * S3 - AES256, S3 SSE with amazon keys
* Data residency(AWS Region):
    * As per S3 bucket location
* Retention capabilities:
    * S3 -indefinite time/user defined


## <a name="emr"></a> Amazon EMR
* Log coverage:
    * Amazon EMR and Hadoop both produce log files that report status on the cluster.
    There are many types of logs written to the master node. Amazon EMR writes step, bootstrap action, and instance state logs. Apache Hadoop writes logs to report the processing of jobs, tasks, and task attempts. Hadoop also records logs of its daemons.
    * https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-manage-view-web-log-files.html
* Default status and how to enable:
    * Enabled by default
* Exceptions and Limits:
    * By default, Amazon EMR clusters launched using the console automatically archive log files to Amazon S3. You can specify your own log path, or you can allow the console to automatically generate a log path for you. For clusters launched using the CLI or API, you must configure Amazon S3 log archiving manually.
* Log record/file format:
    * Apache Hadoop specific logs
    * http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/ClusterSetup.html
    * Amazon EMR writes step, bootstrap action, and instance state logs
    * as per location:
        * Log files available on the Master node: https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-manage-view-web-log-files.html#emr-manage-view-web-log-files-master-node
        * Logs on the S3: https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-manage-view-web-log-files.html#emr-manage-view-web-log-files-s3
        * Debug tool: https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-manage-view-web-log-files.html#emr-manage-view-web-log-files-debug
* Delivery latency:
    * as logs created
* Transport/Encryption in transit:
    * local to host
* Supported log Destinations:
    * Master node
    * S3
* Encryption at rest:
    * EMR encryption:
    https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-data-encryption-options.html
    * AES256 Encryption: Amazon S3 server-side encryption (SSE)
* Data residency(AWS Region):
    * As per node location
    * As per S3 bucket location
* Retention capabilities:
    * Instance lifetime
    * S3: indefinite time/user defined

## <a name="firehose"></a>Amazon Kinesis Data Firehose
* Log coverage:
    * Kinesis Data Firehose integrates with Amazon CloudWatch Logs so that you can view the specific error logs when the Lambda invocation for data transformation or data delivery fails
    * https://docs.aws.amazon.com/firehose/latest/dev/monitoring-with-cloudwatch-logs.html
* Default status and how to enable:
    * Disabled by default
* Exceptions and Limits:
* Log record/file format:
    * log streams named S3Delivery, RedshiftDelivery, or ElasticsearchDelivery : S3Delivery log stream is used for logging errors related to delivery failure to the intermediate S3 bucket. The RedshiftDelivery log stream is used for logging errors related to Lambda invocation failure and delivery failure to your Amazon Redshift cluster.
    * Data Delivery Errors:
        1. Amazon S3 Data Delivery Errors
        2. Amazon Redshift Data Delivery Errors
        3. Splunk Data Delivery Errors
        4. Amazon Elasticsearch Service Data Delivery Errors
    * Lambda Invocation Errors
* Delivery latency:
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudWatch Logs
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined

## <a name="global_accelerator"></a> AWS Global Accelerator
* Log coverage:
    * Flow logs of network traffic across network interfaces in the accelerator.
    * Details: https://docs.aws.amazon.com/global-accelerator/latest/dg/monitoring-global-accelerator.flow-logs.html
* Default status and how to enable:
    * Disabled by default
* Exceptions and Limits:
* Log record/file format:
    * A flow log record is a space-separated string that has the following format:
        * version
        * aws_account_id
        * accelerator_id
        * client_ip
        * client_port
        * accelerator_ip 
        * accelerator_port
        * endpoint_ip
        * endpoint_port
        * protocol
        * ip_address_type
        * packets
        * bytes
        * start_time 
        * end_time 
        * action
        * log-status
        * globalaccelerator_source_ip
        * globalaccelerator_source_port
        * endpoint_region
        * globalaccelerator_region 
        * direction
        * vpc_id
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudWatch Logs
    * S3 bucket
* Encryption at rest:
    * S3 - AES256, S3 SSE with amazon keys or KMS
* Data residency(AWS Region):
    * S3 bucket from any region.
* Retention capabilities:
    * S3 -indefinite time/user defined
    * CloudWatch Logs:  indefinitely and never expire. User can define retention policy per log group (indefinite, or from 1 day to 10years)


## <a name="green_grass"></a> AWS IoT Greengrass
* Log coverage:
    * The AWS IoT Greengrass Core software can write logs to Amazon CloudWatch and to the local file system of your core device.
    * Lambda functions and connectors running on the core can also write logs to CloudWatch Logs and the local file system.
    * Details: https://docs.aws.amazon.com/greengrass/latest/developerguide/greengrass-logs-overview.html
* Default status and how to enable:
    * Enabled by default to the local filesystem
    * CloudWatch integration requires configuration: 
        * Logging is configured at the group level. 
        * Details: https://docs.aws.amazon.com/greengrass/latest/developerguide/greengrass-logs-overview.html#config-logs
* Exceptions and Limits:
    * Changes to logging settings take effect after you deploy the group.
    * uses batches to send logs to the Cloudwatch -> you can log at a rate higher than five requests per second per log stream.
    * if Lambda function logs more than 5 MB/second for a prolonged period of time, the internal processing pipeline eventually fills up. The theoretical worst case is 6 MB per Lambda function.
    * logging component signs requests to CloudWatch using the normal Signature Version 4 signing process. If the system time on the AWS IoT Greengrass core device is out of sync by more than 15 minutes, then the requests are rejected.
    * disk space must be allocate for logging.
    * if your AWS IoT Greengrass core device is configured to log only to CloudWatch and there's no internet connectivity, you have no way to retrieve the logs currently in the memory.
    * When Lambda functions are terminated (for example, during deployment), a few seconds' worth of logs are not written to CloudWatch.
* Log record/file format:
    * All AWS IoT Greengrass log entries include a timestamp, log level, and information about the event.
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * Local file system
    * CloudWatch Logs
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined

## <a name="image_builder"></a> Amazon Image Builder (EC2)
* Log coverage:
    * Build and debug logs.
    * Details: https://docs.aws.amazon.com/imagebuilder/latest/userguide/how-image-builder-works.html#image-builder-logs
* Default status and how to enable:
    * Enabled for the CloudWatch Logs
        * to disable: You can opt out of CloudWatch streaming by removing the following permissions associated with the instance profile.
    * Disabled for the S3
* Exceptions and Limits:
* Log record/file format:
    * Logs are retained on the instance and streamed to CloudWatch
    * The logs are streamed to the following LogStream:
        * LogGroup: /aws/imagebuilder/ImageName
        * LogStream: ImageVersion/ImageBuildVersion["x.x.x/x"]
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * Instance
    * CloudWatch Logs
    * s3    
* Encryption at rest:
    * As per CloudWatchLogs configuration
    * As per S3 configuration
* Data residency(AWS Region):
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined

## <a name="aws_import"></a> AWS Import/Export
* Log coverage:
    * Import job logs.
    * Details: http://s3.amazonaws.com/awsdocs/ImportExport/latest/AWSImportExport-dg.pdf
* Default status and how to enable:
    * Enabled by default
    * manual config for the S3 upload (logs bucket option in the job manifest)
* Exceptions and Limits:
* Log record/file format:
    * CSV
    * nformation about each file loaded to or from your storage device
    * The log file name of an export job always ends with the phrase export-log- followed by your JOBID.
    * For example, if the JOBID is 53TX4, the log file name would end in export-log-53TX4.
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * Device
    * s3    
* Encryption at rest:
    * As per S3 configuration
* Data residency(AWS Region):
* Retention capabilities:
    * As per S3 


## <a name="kafka"></a> Amazon Kafka (Managed Streaming for Apache Kafka)
* Log coverage:
    * Broker logs enable you to troubleshoot your Apache Kafka applications and to analyze their communications with your MSK cluster
    * Details: https://docs.aws.amazon.com/msk/latest/developerguide/msk-logging.html
* Default status and how to enable:
    * Disabled by default
    * to enable: Broker log delivery heading in the Monitoring section. You can specify the destinations to which you want Amazon MSK to deliver your broker logs
* Exceptions and Limits:
    * By default, when broker logging is enabled, Amazon MSK logs INFO level logs to the specified destinations. However, users of Apache Kafka 2.4.X and later can dynamically set the broker log level to any of the **log4j** log levels. 
    * If you dynamically set the log level to DEBUG or TRACE, use Amazon S3 or Kinesis Data Firehose as the log destination
    * If you use CloudWatch Logs as a log destination and you dynamically enable DEBUG or TRACE level logging, Amazon MSK may continuously deliver a sample of logs
    * using DEBUG or TRACE level logging with CloudWatch Logs as a log destination can significantly impact broker performance and should only be used when the INFO log level is not verbose enough to determine the root cause of an issue.
* Log record/file format:
    * INFO-level broker logs
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * CloudWatch Logs
    * S3
    * Kinesis Data Firehose
* Encryption at rest:
    * S3: KMS
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined
    * as per S3

## <a name="kendra"></a> Amazon Kendra
* Log coverage:
    * Insight into the operation of your data sources.
    * Data source log streams publish entries about your index synchronization jobs.
    * Amazon Kendra logs information about processing documents while the are being indexed.
    * Details: https://docs.aws.amazon.com/kendra/latest/dg/cloudwatch-logs.html
* Default status and how to enable:
    * Enabled by default
* Exceptions and Limits:
* Log record/file format:
    * Log groups – Amazon Kendra stores all of your log streams in a single log group for each index
        * log group created when the index is created. 
        * identifier always begins with "aws/kendra/".
    * Log streams – creates a new data source log stream in the log group for each index synchronization job that you run.
        * Data source log streams:
            * Each synchronization job creates a new log stream that it uses to publish entries
            * name is: **data source id/YYYY-MM-DD-HH/data source sync job ID**
            * new log stream is created for each synchronization job run.
            * Log messages:
                * document that failed to be sent for indexing.
                * document that failed to be sent for deletion
                * invalid metadata file for a document in an Amazon S3 bucket is found.
        * Document log streams
            * logs a set of messages for documents stored in an Amazon S3 data source. 
            * logs errors only for documents stored in a Microsoft SharePoint or a database data source
            * stream names: **YYYY-MM-DD-HH/UUID** or **dataSourceId/YYYY-MM-DD-HH/UUID**

    * Log entries – Amazon Kendra creates a log entry in the log stream as it indexes documents. Each entry provides information about processing the document or any errors that are encountered.
* Delivery latency:
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudWatch Logs
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
    * As per CloudWatchLogs
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined


## <a name="kinesis_data"></a> Amazon Kinesis Data Analytics
* Log coverage:
    * Task
    * Operator
    * Parallelism
    * application events or configuration problems.
    * Custom messages to Kinesis Data Analytics application's CloudWatch log. You do this by using the Apache log4j library or the Simple Logging Facade for Java (SLF4J)
        * Details: https://docs.aws.amazon.com/kinesisanalytics/latest/java/cloudwatch-logs-writing.html
    * Details: https://docs.aws.amazon.com/kinesisanalytics/latest/java/monitoring-overview.html
* Default status and how to enable:
    * Disabled by default
    * To enable: https://docs.aws.amazon.com/kinesisanalytics/latest/java/cloudwatch-logs.html
* Exceptions and Limits:
* Log record/file format:
    * application's monitoring log level:
        * Error: Potential catastrophic events of the application.
        * Warn: Potentially harmful situations of the application.
        * Info: Informational and transient failure events of the application. We recommend that you use this logging level.
        * Debug: Fine-grained informational events that are most useful to debug an application. Note: Only use this level for temporary debugging purposes.
    * Details: https://docs.aws.amazon.com/kinesisanalytics/latest/java/cloudwatch-logs-reading.html
* Delivery latency:
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudWatch Logs
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
    * As per CloudWatchLogs
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined

## <a name="lambda"></a> Lambda
* Log coverage:
    * Lambda logs all requests handled by your function and also automatically stores logs generated by your code through Amazon CloudWatch Logs
    * Details: https://docs.aws.amazon.com/lambda/latest/dg/monitoring-functions-logs.html
    * You can insert logging statements into your code to help you validate that your code is working as expected. Lambda automatically integrates with CloudWatch Logs and pushes all logs from your code to a CloudWatch Logs group associated with a Lambda function
* Default status and how to enable:
    * Enabled by default
* Exceptions and Limits:
* Log record/file format:
    * CloudWatch Logs group associated with a Lambda function, which is named /aws/lambda/<function name>. 
    * JSON
* Delivery latency:
    * as per CloudWatchLogs (near real time)
* Transport/Encryption in transit:
    * as per CloudWatchLogs
* Supported log Destinations:
    * CloudWatchLogs
* Encryption at rest:
    * as per CloudWatchLogs
* Data residency(AWS Region):
    * any region
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined


## <a name="lex"></a> Amazon Lex
* Log coverage:
    * detailed view of conversations that users have with your bot.
    * log text for the **PostText** operation
    * log both text and audio for the **PostContent** operation
    * Details: https://docs.aws.amazon.com/lex/latest/dg/conversation-logs.html
* Default status and how to enable:
    * Disabled by default
    * To enable: To configure logging, use the console or the **PutBotAlias** operation
* Exceptions and Limits:
    * You can't use conversation logs with a bot subject to the Children's Online Privacy Protection Act (COPPA).
    * You can't log conversations for the $LATEST alias of your bot or for the test bot available in the Amazon Lex console.
    * **Note:** Conversation logs might contain sensitive information and are subject to the corresponding privacy regulations.
* Log record/file format:
    * CloudWatch Logs:
        * JSON
        * Log entries for a user utterance is in multiple log streams. 
        * An utterance in the conversation has an entry in one of the log streams with the specified prefix. 
        * An entry in the log stream contains the following information: 
            * The intent, slots, and slotToElicit fields don't appear in an entry if the missedUtterance field is true.
            * The s3PathForAudio field doesn't appear if audio logs are disabled or if the inputDialogModefield is Text.
            * The responseCard field only appears when you have defined a response card for the bot.
            * The requestAttributes map only appears if you have specified request attributes in the request.
            * The kendraResponse field is only present when the AMAZON.KendraSearchIntent makes a request to search an Amazon Kendra index.
            * The developerOverride field is true when an alternative intent was specified in the bot's Lambda function.
            * The sessionAttributes map only appears if you have specified session attributes in the request.
            * The sentimentResponse map only appears if you configure the bot to return sentiment values.  
            * More details: https://docs.aws.amazon.com/lex/latest/dg/conversation-logs-cw.html     
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * CloudWatch Logs: Text logs store text input, transcripts of audio input, and associated metadata.\
    * S3: audio input of Audio logs 
* Encryption at rest:
    * KMS
    * Details on log encryption: https://docs.aws.amazon.com/lex/latest/dg/conversation-logs-encrypting.html
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * As per CloudWatch Logs
    * As per S3

## <a name="lightsail"></a> Amazon Lightsail
* Log coverage:
    * Database
    * application
    * container
    * error logs.
* Default status and how to enable:
    * Enabled by default
* Exceptions and Limits:
* Log record/file format:
    * container logs:
        * Details: https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-viewing-container-service-container-logs
    * database logs:
        * MySQL database logs:
            * Error log
            * General log 
            * Slow query log
        * PostgreSQL database logs
            * Postgres log
        * Details: https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-viewing-database-logs-and-history
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * Lightsail platform only
* Encryption at rest:
* Data residency(AWS Region):
* Retention capabilities:


## <a name="neptune"></a> Amazon Neptune
* Log coverage:
    * Neptune DB cluster activity
    * Details: https://docs.aws.amazon.com/neptune/latest/userguide/auditing.html
* Default status and how to enable:
    * Disabled by default
    * enable the collection of audit logs by setting a DB cluster parameter
* Exceptions and Limits:
    * Log entries are not in sequential order. You can use the timestamp value for ordering them.
    * Log files are rotated when they reach 100 MB in aggregate. This limit is not configurable
* Log record/file format:
    * UTF-8, CSV format. 
    * Logs are written in multiple files, the number of which varies based on the instance size. 
    * following comma-delimited information in rows:
        * Timestamp: The Unix timestamp for the logged event with microsecond precision.
        * ServerHost: The hostname or IP of the instance that the event is logged for.
        * ClientHost: The hostname or IP that the user connected from.
        * ConnectionType: The connection type. Can be Websocket, HTTP_POST, or HTTP_GET.
        * Caller's IAM ARN: The ARN of the IAM user or IAM role used to sign the request. Empty if IAM authentication is disabled. Its format is: **arn:partition:service:region:account:resource**
        * Auth Context: Contains a serialized JSON object that has authentication information. The field authenticationSucceeded is True if the user was authenticated. Empty if IAM authentication is disabled.
        * HttpHeader: The HTTP header information. Can contain a query. Empty for WebSocket connections.
        * Payload: The Gremlin or SPARQL query.       
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * Neptine console
    * Manual download option available
* Encryption at rest:
* Data residency(AWS Region):
* Retention capabilities:

## <a name="net_fw"></a> AWS Network Firewall
* Log coverage:
    * Network traffic logs
        * Log type: Alert
            * Sends logs for traffic that matches any stateful rule whose action is set to Alert or Drop.
        * Log type: Flow:
            * Sends logs for all network traffic that the stateless engine forwards to the stateful rules engine.
    * Details: https://docs.aws.amazon.com/network-firewall/latest/developerguide/firewall-update-logging-configuration.html
* Default status and how to enable:
    * Disabled by default
    * To enable: https://docs.aws.amazon.com/network-firewall/latest/developerguide/firewall-update-logging-configuration.html
* Exceptions and Limits:
    * Firewall logging is only available for traffic that you forward to the stateful rules engine.
    * You must forward traffic to the stateful engine through stateless rule actions and stateless default actions in the firewall policy. https://docs.aws.amazon.com/network-firewall/latest/developerguide/stateless-default-actions.html
* Log record/file format:
    * Flow logs are standard network traffic flow logs. Each flow log record captures the network flow for a specific 5-tuple.
    * Alert logs report traffic that matches your stateful rules that have an action that sends an alert. A stateful rule sends alerts for the rule actions DROP and ALERT.
    * S3
        * folder structure that's determined by the log's ID, Region, Network Firewall log type, and the date. 
            * s3-bucket-name/optional-s3-bucket-prefix/AWSLogs/aws-account-id/network-firewall/log-type/Region/firewall-name/timestamp/ 
        * log file name is determined by the flow log's ID, Region, and the date and time it was created
            * aws-account-id_network-firewall_log-type_Region_firewall-name_timestamp_hash.log.gz
    * CloudWatch Logs
        * log streams have the following naming format:
            * /aws/network-firewall/log-type/firewall-name_YYYY-MM-DD-HH
            * In the specification, the log type is either alert or flow.
    * Kinesis Data Firehose
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * S3
    * CloudWatch Logs
    * Kinesis Data Firehose
    * Details: https://docs.aws.amazon.com/network-firewall/latest/developerguide/firewall-logging-destinations.html
* Encryption at rest:
    * SSE-KMS or CMK-KMS
* Data residency(AWS Region):
    * regional
* Retention capabilities:
    * as per S3
    * as per CloudWatch Logs


## <a name="opsworks"></a> OpsWorks
* Log coverage:
    * Chef and application logs.
    * Details: https://docs.aws.amazon.com/opsworks/latest/userguide/monitoring-cloudwatch-logs.html
* Default status and how to enable:
    * Local filesystem:
        * Enabled by default
    * CloudWatch Logs:
        * Disabled by default
        * to enable: you enable CloudWatch Logs at the layer level in AWS OpsWorks Stacks.
* Exceptions and Limits:
    * CloudWatch Logs integration works with Chef 11.10 and Chef 12 Linux-based stacks. 
* Log record/file format:
    * OS/app specific
    * Log groups for AWS OpsWorks Stacks data have names that match the following pattern:
        * stack_name/layer_name/chef_log_name
    * Custom logs have names that match the following pattern:
        * /stack_name/layer_short_name/file_path_name.
* Delivery latency:
    * as per  AWS OpsWorks Stacks agent
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudWatch Logs
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined

## <a name="ledger"></a> Amazon (QLDB) Quantum Ledger Database
* Log coverage:
    * every document revision
    * Details: https://docs.aws.amazon.com/qldb/latest/developerguide/streams.html
* Default status and how to enable:
    * Disabled by default
* Exceptions and Limits:
    * Not a real log but rather stream that captures every document revision that is committed to your journal
    * QLDB streams provide an at-least-once delivery guarantee
* Log record/file format:
    * A QLDB stream writes your data to Kinesis Data Streams in three types of records: 
        * control
        * block summary
        * revision details.
    * Details: https://docs.aws.amazon.com/qldb/latest/developerguide/streams.records.html
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
    * Kinesis Data Streams
* Encryption at rest:
* Data residency(AWS Region):
* Retention capabilities:
    * as per stream consumer application
    * or per AWS or 3d part service such as:
        * Elasticsearch Service (Amazon ES)
        * Amazon Redshift
        * Amazon Simple Storage Service (Amazon S3)
        * Splunk


## <a name="rds"></a>  Amazon RDS (Relational Database Service)
* Log coverage:
    * Amazon RDS Database Logs are  very specific to the database engine:
    * You can view, download, and watch database logs using the Amazon RDS console, the AWS Command Line Interface (AWS CLI), or the Amazon RDS API
    * Details: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.html
* Default status and how to enable:
    * Enabled by default for the service
    * CloudWatch Logs integration needs to be configured
* Exceptions and Limits:
    * Viewing, downloading, or watching transaction logs is not supported.
* Log record/file format:
    * DataBase engine specific:
        1. MariaDB Database Log Files: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MariaDB.html
        2. Microsoft SQL Server Database Log Files : https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.SQLServer.html
        3. MySQL Database Log Files: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html
        4. Oracle Database Log Files: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.Oracle.html
        5. PostgreSQL Database Log Files: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.PostgreSQL.html
* Delivery latency:
    * DB engine specific
* Transport/Encryption in transit:
* Supported log Destinations:
    * RDS console
    * On DB servers itself
    * CloudWatchLogs
* Encryption at rest:
    * As per DB instance encryption - AES-256 encryption
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
* Retention capabilities:
    * DB-stored logs retention depends on the Db engine (3-7 days)
    * CloudWatch logs: indefinite time/user defined

## <a name="redshift"></a> Amazon Redshift
* Log coverage:
    * Amazon Redshift logs information about connections and user activities in your database.
    * Details: https://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html
* Default status and how to enable:
    * Disabled by default
    * Log files are not as current as the base system log tables, STL_USERLOG and STL_CONNECTION_LOG. Records that are older than, but not including, the latest record are copied to log files.
* Log record/file format:
    * Amazon Redshift logs information in the following log files:
        1. Connection log — logs authentication attempts, and connections and disconnections.
        2. User log — logs information about changes to database user definitions.
            * Create user
            * Drop user
            * Alter user (rename)
            * Alter user (alter properties)
        3. User activity log — logs each query before it is run on the database.
    * Logs format for each of the logs files can be found here: https://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html#db-auditing-logs
* Delivery latency:
    * depends on the Redshift cluster load. More load - more often you get logs
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * S3 bucket
* Encryption at rest:
    * * S3 - AES256, only S3 SSE with amazon keys
* Data residency(AWS Region):
    * As per S3 bucket location
* Retention capabilities:
    * S3 -indefinite time/user defined

## <a name="r53"></a> Amazon Route53
* Log coverage:
    * Public query logs:
        * The domain or subdomain that was requested
        * The date and time of the request
        * The DNS record type (such as A or AAAA)
        * The Route 53 edge location that responded to the DNS query
        * The DNS response code, such as NoError or ServFail
        * Details: https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/query-logs.html
    * Resolver query logs:
        * The AWS Region where the VPC was created
        * The ID of the VPC that the query originated from
        * The IP address of the instance that the query originated from
        * The instance ID of the resource that the query originated from
        * The date and time that the query was first made
        * The DNS name requested (such as prod.example.com)
        * The DNS record type (such as A or AAAA)
        * The DNS response code, such as NoError or ServFail
        * The DNS response data, such as the IP address that is returned in response to the DNS query
* Default status and how to enable:
    * Disabled by default
    * To enable:
        * Public query logs: https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/query-logs.html#query-logs-configuring
        * Resolver query logs: https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-query-logging-configurations-managing.html      
* Exceptions and Limits:
    * Query logging is available only for public hosted zones
    * cached response will not be logged
    * Resolver query logging logs only unique queries, not queries that Resolver is able to respond to from the cache.
* Log record/file format:
    * Public query logs:
        * newline-delimited log records, Each log record represents one request and consists of space-delimited fields
        * https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/query-logs.html#query-logs-format
        * Fields:
            1. Log format version
            2. Query timestamp
            3. Hosted zone ID
            4. Query name
            5. Query type
            6. Response code
            7. Layer 4 protocol
            8. Route 53 edge location
            9. Resolver IP address
            10. EDNS client subnet
    * Resolver query logs:
        * Details: https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-query-logs-format.html
* Delivery latency:
    * near real-time
* Transport/Encryption in transit:
* Supported log Destinations:
    * Public query logs:
        * CloudWatch Logs
    * Resolver query logs:
        * Amazon CloudWatch Logs (CloudWatch Logs) log group
        * Amazon S3 (S3) bucket
        * Kinesis Data Firehose delivery stream
* Encryption at rest:
    * As per CloudWatchLogs/S3 configuration
* Data residency(AWS Region):
    * Public query logs:
        * The log group must be in the US East (N. Virginia) Region.
    * Resolver query logs:
        * regional
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined
    * S3 -indefinite time/user defined


## <a name="vpcflowlogs"></a> VPC Flow logs
* Log coverage:
    * VPC
    * Subnet
    * Network interface
    * accepted traffic, rejected traffic, or all traffic
    https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html
* Exceptions and Limits:
    * https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html#flow-logs-limitations
    * Flow logs do not capture all IP traffic. The following types of traffic are not logged:
        *   Traffic generated by instances when they contact the Amazon DNS server. If you use your own DNS server, then all traffic to that DNS server is logged.
        * Traffic generated by a Windows instance for Amazon Windows license activation.
        * Traffic to and from 169.254.169.254 for instance metadata.
        * Traffic to and from 169.254.169.123 for the Amazon Time Sync Service.
        * DHCP traffic.
        * Traffic to the reserved IP address for the default VPC router. For more information, see VPC and Subnet Sizing.
        * Traffic between an endpoint network interface and a Network Load Balancer network interface. For more information, see VPC Endpoint Services (AWS PrivateLink).
* Log record/file format:
    * space-separated string
    * Format details:
        * A flow log record represents a network flow in your flow log. Each record captures the network flow for a specific 5-tuple, for a specific capture window. A 5-tuple is a set of five different values that specify the source, destination, and protocol for an internet protocol (IP) flow. The capture window is a duration of time during which the flow logs service aggregates data before publishing flow log records. 
        * https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html#flow-log-records
        * Fields of a flow log record:
            * version: The VPC Flow Logs version.
            * account-id: The AWS account ID for the flow log.
            * interface-id: The ID of the network interface for which the traffic is recorded.
            * srcaddr: The source IPv4 or IPv6 address. The IPv4 address of the network interface is always its private IPv4 address.
            * dstaddr: The destination IPv4 or IPv6 address. The IPv4 address of the network interface is always its private IPv4 address.
            * srcport: The source port of the traffic.
            * dstport: The destination port of the traffic.
            * protocol:	The IANA protocol number of the traffic. For more information, see Assigned Internet Protocol Numbers.
            * packets: The number of packets transferred during the capture window.
            * bytes:	The number of bytes transferred during the capture window.
            * start:	The time, in Unix seconds, of the start of the capture window.
            * end:	The time, in Unix seconds, of the end of the capture window.
            * action: The action associated with the traffic:
                * ACCEPT: The recorded traffic was permitted by the security groups or network ACLs.
                * REJECT: The recorded traffic was not permitted by the security groups or network ACLs.
            * log-status	The logging status of the flow log:
                * OK: Data is logging normally to the chosen destinations.
                * NODATA: There was no network traffic to or from the network interface during the capture window.
                * SKIPDATA: Some flow log records were skipped during the capture window. This may be because of an internal capacity constraint, or an internal error.
* Delivery latency:
    * each capture window: ~10 min
    * max 15 min
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudWatch Logs
    * S3 bucket
* Encryption at rest:
    * S3 - AES256, S3 SSE with amazon keys or KMS
* Data residency(AWS Region):
    * S3 bucket from any region.
* Retention capabilities:
    * S3 -indefinite time/user defined
    * CloudWatch Logs:  indefinitely and never expire. User can define retention policy per log group (indefinite, or from 1 day to 10years)


## <a name="s3accesslogs"></a> S3 bucket access logs (S3 Server Access Logs)

* Log coverage:
    * Logs all requests to the data in the bucket
    * https://docs.aws.amazon.com/AmazonS3/latest/dev/ServerLogs.html
* Exceptions and Limits:
    * Not guaranteed delivery
* Log record/file format:
    * Newline-delimited log records, Each log record represents one request and consists of space-delimited fields
    * Each access log record provides details about a single access request, such as the requester, bucket name, request time, request action, response status, and an error code, if relevant.
    * https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html
    * Fields:
        1. Bucket Owner
        2. Bucket
        3. Time
        4. Remote IP
        5. Requester
        6. Request ID
        7. Operation
        8. Key
        9. Request-URI
        10. HTTP status
        11. Error Code
        12. Bytes Sent
        13. Object Size
        14. Total Time
        15. Turn-Around Time
        16. Referrer
        17. User-Agent
        18. Version Id
* Delivery latency:
    * delivered on a best effort basis - normally hours.
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * S3 bucket, same as source bucket(with prefix) or different one
* Encryption at rest:
    * S3 - AES256, S3 SSE with amazon keys or KMS
* Data residency(AWS Region):
    * Same Region as the source bucket, bucket must be owned by the same AWS account
* Retention capabilities:
    * S3 -indefinite time/user defined



## <a name="waf"></a> AWS WAF	
* Log coverage:
    * You can enable logging to get detailed information about traffic that is analyzed by your web ACL. Information that is contained in the logs include the time that AWS WAF received the request from your AWS resource, detailed information about the request, and the action for the rule that each request matched.
    * https://docs.aws.amazon.com/waf/latest/developerguide/logging.html
* Exceptions and Limits:
* Log record/file format:
    * json
    * One AWS WAF log is equivalent to one Kinesis Data Firehose record.
* Delivery latency:
    * near real time
* Transport/Encryption in transit:
* Supported log Destinations:
    * Amazon Kinesis Data Firehose
* Encryption at rest:
    * as for Amazon Kinesis Data Firehose
* Data residency(AWS Region):
    * as for Amazon Kinesis Data Firehose
* Retention capabilities:
    * as for Amazon Kinesis Data Firehose


## <a name="sysman"></a> AWS Systems Manager
* Log coverage:
    * AWS Systems Manager Agent is Amazon software that runs on your Amazon EC2 instances and your hybrid instances that are configured for Systems Manager (hybrid instances).
    you can configure SSM Agent to send log data to Amazon CloudWatch Logs
    * https://docs.aws.amazon.com/systems-manager/latest/userguide/monitoring-ssm-agent.html
* Exceptions and Limits:
    * Note: The unified CloudWatch Agent has replaced SSM Agent as the tool for sending log data to Amazon CloudWatch Logs
* Log record/file format:
    * system specific logs
* Delivery latency:
    * as per agent settings
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudWatch Logs
* Encryption at rest:
    * As per CloudWatchLogs configuration
* Data residency(AWS Region):
    * As per CloudWatchLogs
* Retention capabilities:
    * CloudWatch logs: indefinite time/user defined





# AWS Built-in Centralized logging capabilities
## <a name="cloudwatchlogs"></a> Amazon CloudWatch Logs Service

Amazon CloudWatch Logs could be used to monitor, store, and access your log files from Amazon Elastic Compute Cloud (Amazon EC2) instances, AWS CloudTrail, Route 53, and other sources. You can then retrieve the associated log data from CloudWatch Logs.
https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html

**Service coverage:**

* Logs from Amazon EC2 Instances
    * *Logs Retrieval:* AWS provide agent (several different version are available)
    * *Delivery schedule:* as per agent settings
    * *Data residency:* any region
* Route 53 DNS Queries
    * *Logs Retrieval:* Service push
    * *Delivery schedule:*
    * *Data residency:* 	US East (N. Virginia) Region only
* VPC Flow Logs:
    * *Logs Retrieval:* Service push: https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-cwl.html
    * *Delivery schedule:* 
    * *Data residency:* 
* Lambda function Logs
    * *Logs Retrieval:* Service push
    * *Delivery schedule:* 
    * *Data residency:* any aws region
* Amazon RDS Database Log
    * *Logs Retrieval:* Service push
    * *Delivery schedule:* as per DB engine configuration
    * *Data residency:* any aws region
* Kinesis Data Firehose
    * *Logs Retrieval:* Service push
    * *Delivery schedule:* 
    * *Data residency:* any aws region
* Amazon ECS (AWS Fargate)
    * *Logs Retrieval:* Service push
    * *Delivery schedule:* 
    * *Data residency:* any aws region
* API Gateway
    * *Logs Retrieval:* Service push
    * *Delivery schedule:* 
    * *Data residency:* any aws region
* AWS Systems Manager
    * *Logs Retrieval:* Agent push
    * *Delivery schedule:* as per agent configuration
    * *Data residency:* any aws region
* Elastic Beanstalk
    * *Logs Retrieval:* Agent push
    * *Delivery schedule:* as per agent/service configuration
    * *Data residency:* any aws region
* OpsWorks
    * *Logs Retrieval:* OpsWorks Stacks agent push
    * *Delivery schedule:* as per agent configuration
    * *Data residency:* any aws region
* AWS Global Accelerator flow logs
    * *Logs Retrieval:* Service push through s3 : https://docs.aws.amazon.com/global-accelerator/latest/dg/monitoring-global-accelerator.flow-logs.html#monitoring-global-accelerator.flow-logs-publishing-S3
    * *Delivery schedule:* as per agent configuration
    * *Data residency:* any aws region

**Encryption at REST:**

AWS Key Management Service (AWS KMS) key: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/encrypt-log-data-kms.html

**Retention policy:**

CloudWatch logs: logs are kept indefinitely and never expire. User can create retention policy per log group with following option: indefinite, or from 1 day to 10years 
Data visualization and analyzes:
CloudWatch Logs Insights: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html
Metric filters: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringLogData.html

**Notification and alerting:**

CloudWatch Alarms: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html

**Real time processing options:**
Real-time Processing of Log Data with Subscriptions: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Subscriptions.html
Supported AWS Services for the real time data processing: AWS Kinesis, Lambda

**Data export and external tool integrations:**
    * Export data to S3: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/S3Export.html
    * Streaming data to the Amazon Elasticsearch Service: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_ES_Stream.html
**Known limits:**

Amazon CloudWatch Logs limits: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch_limits_cwl.html

**AWS recommended solutions/implementations**:

https://aws.amazon.com/answers/logging/centralized-logging/
​

## <a name="dynamodb"></a> DynamoDB
* Log coverage:
    * DynamoDB is integrated with AWS CloudTrail
    * https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/logging-using-cloudtrail.html
* Exceptions and Limits:
    Only following API calls are logged
    
    Amazon DynamoDB: 
    * CreateBackup
    * CreateGlobalTable
    * CreateTable
    * DeleteBackup
    * DeleteTable
    * DescribeBackup
    * DescribeContinuousBackups
    * DescribeGlobalTable
    * DescribeLimits
    * DescribeTable
    * DescribeTimeToLive
    * ListBackups
    * ListTables
    * ListTagsOfResource
    * ListGlobalTables
    * RestoreTableFromBackup
    * RestoreTableToPointInTime
    * TagResource
    * UntagResource
    * UpdateGlobalTable
    * UpdateTable
    * UpdateTimeToLive
    * DescribeReservedCapacity
    * DescribeReservedCapacityOfferings
    * DescribeScalableTargets
    * RegisterScalableTarget
    * PurchaseReservedCapacityOfferings
    
    DynamoDB Streams
    * DescribeStream
    * ListStreams
    
    DynamoDB Accelerator (DAX)
    * CreateCluster
    * CreateParameterGroup
    * CreateSubnetGroup
    * DecreaseReplicationFactor
    * DeleteCluster
    * DeleteParameterGroup
    * DeleteSubnetGroup
    * DescribeClusters
    * DescribeDefaultParameters
    * DescribeEvents
    * DescribeParameterGroups
    * DescribeParameters
    * DescribeSubnetGroup
    * IncreaseReplicationFactor
    * ListTags
    * RebootNode
    * TagResource
    * UntagResource
    * UpdateCluster
    * UpdateParameterGroup
    * UpdateSubnetGroup
* Log record/file format:
    * CloudTrail log format
* Delivery latency:
    * as per CloudTrail SLA
* Transport/Encryption in transit:
    * internal to AWS
* Supported log Destinations:
    * CloudTrail only
* Encryption at rest:
    * As per CLoudTrail configuration
* Data residency(AWS Region):
    * As per CLoudTrail configuration: region wehere event occur or in choosen region for the global cloudtrail
* Retention capabilities:
    * CloudTrail configuration based



# WishList:
-  add samples of the each logs


# Templates

## Serice Template:
* Log coverage:
* Default status and how to enable:
* Exceptions and Limits:
* Log record/file format:
* Delivery latency:
* Transport/Encryption in transit:
* Supported log Destinations:
* Encryption at rest:
* Data residency(AWS Region):
* Retention capabilities:

## Cloudwatchlogs service template
* *Logs Retrieval:*
* *Delivery schedule:*
* *Data residency:*