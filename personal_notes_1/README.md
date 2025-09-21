# AWS Notes

## Amazon FSx for NetApp ONTAP
Amazon FSx for NetApp ONTAP is a fully managed service that provides shared storage with support for both **SMB** and **NFS** protocols.  
FSx for ONTAP instances are deployed in both the **primary region** and the **disaster recovery (DR) region**.

---

## VMWare to EC2 Export
The **VM Import/Export** enables you to easily import virtual machine images from your existing environment to Amazon EC2 instances.  
If you use the VMware vSphere virtualization platform, you can also use the AWS Management Portal for vCenter to import your VM.

---

## Lambda Deployments via CodeDeploy
- All at once  
- Canary  
- Linear  

---

## Autoscaling Group Metrics
You can configure a deployment group to stop a deployment whenever any CloudWatch alarm you associate with the deployment group is activated.

---

## CloudFormation Custom Resources
- If a **custom resource** is used to invoke a **Lambda function** in AWS CloudFormation, the request will include a **pre-signed URL**.  
- The Lambda function is responsible for returning a response to the pre-signed URL to indicate if the resource creation was successful or not.  
- **Use cases**: dynamically updating AMIs, populating S3 buckets with static HTML, etc.

---

## CloudWatch to Kinesis to S3
- Create a CloudWatch Logs subscription filter integrated with Amazon Kinesis to analyze the logs.  
- Configure the CloudWatch Logs to export the logs to an S3 bucket.

---

## CodePipeline Input Parameters
In a CodePipeline stage, you can specify parameter overrides for AWS CloudFormation actions.  
Parameter overrides let you specify template parameter values that override values in a template configuration file.

---

## AWS Session Manager Run Commands
Administrators use Run Command to perform tasks on their managed instances, such as:
- Install or bootstrap applications  
- Build a deployment pipeline  
- Capture log files when an instance is terminated from an Auto Scaling group  
- Join instances to a Windows domain  

---

## AWS Trusted Advisor
- Inspects your AWS environment and makes recommendations to save money, improve availability/performance, or close security gaps.  
- Integrated with **Amazon EventBridge** and **Amazon CloudWatch**.

---

## CodeDeploy Lifecycle Hooks
The **BeforeAllowTraffic** and **AfterAllowTraffic** lifecycle hooks in `AppSpec.yaml` allow you to use Lambda functions to validate the new version task set using test traffic.  

Validation scripts can be created in AWS Lambda and defined in the **AfterAllowTestTraffic** hook. These can validate the deployment and rollback if tests fail.

**Available Lifecycle Hooks:**
- **BeforeInstall** – Run tasks before replacement task set creation. No rollback possible.  
- **AfterInstall** – Run tasks after replacement task set creation. Rollback possible.  
- **AfterAllowTestTraffic** – Run tasks after test listener traffic. Rollback possible.  
- **BeforeAllowTraffic** – Run tasks before shifting traffic to replacement task set. Rollback possible.  
- **AfterAllowTraffic** – Run tasks after traffic shift. Rollback possible.  

**Available environment variables:**
- `APPLICATION_NAME` – Application name in CodeDeploy  
- `DEPLOYMENT_ID` – Deployment ID  
- `DEPLOYMENT_GROUP_NAME` – Deployment group name  
- `DEPLOYMENT_GROUP_ID` – Deployment group ID  
- `LIFECYCLE_EVENT` – Current lifecycle event  

---

## Canary Deployment of ASG
- Prepare another stack with an **ALB + Auto Scaling group** containing the new app version (blue/green).  
- Create weighted **Alias A records in Route 53** to adjust traffic between the ALBs.

---

## AWS CodePipeline
- Use `runOrder` for parallelization.  

**Custom Actions:**
- Custom build action – builds/transforms items  
- Custom deploy action – deploys to servers, sites, or repos  
- Custom test action – configures/runs automated tests  
- Custom invoke action – runs functions  

Custom actions require a **job worker** to poll CodePipeline, execute jobs, and return status. This worker can be anywhere with access to the public endpoint.  

You can add **cross-region actions** in CodePipeline.

---

## CloudFormation Cross Stack References
- Use the **Export** output field to flag a resource for export.  
- Use the **Fn::ImportValue** intrinsic function to import the value.

---

## Aurora Global Database
- Lets you create **read replicas** in multiple regions with <1s replication latency.  
- Improves global performance and resilience.  
- Replicates all data, which may conflict with **data sovereignty rules**.  
- Common pattern: Use Aurora Global Database for shared/catalog data + local Aurora clusters for sensitive data.

---

## AWS Config
- Provides a **visual dashboard** to detect non-compliant resources.  
- Continuously evaluates resource configurations.  
- Records configuration changes and stores reports in S3 (analyze with QuickSight).  
- Supports **custom rules** with Lambda for compliance checks.  
- Can automatically **revert configuration changes** via SSM automation.  

**Triggers:**
- Configuration changes  
- Periodic  

**Automation use cases:** create golden AMIs, recover unreachable EC2s, etc.

---

## CloudWatch Log Subscriptions
Use subscriptions to get a real-time feed of log events from CloudWatch Logs to:  
- Amazon Kinesis stream  
- Amazon Data Firehose stream  
- AWS Lambda  

---

## ASG Lifecycle Hooks
**Scale out event:**
- Pending → Pending:Wait (lifecycle hook) → Pending:Proceed → InService  

**Scale in event:**
- Terminating → Terminating:Wait (lifecycle hook) → Terminating:Proceed → Terminated  

---

## Route53 Failover
- Set up **health checks** for non-alias records to endpoints.  
- Use **Failover routing policy**.  
- Configure **alias records** with `Evaluate Target Health = Yes`.

---

## Serverless Canary Deployments
- Set up a Lambda function alias with weighted routing (e.g., 20% new version).  
- Canary deployment in API Gateway with weighted traffic (e.g., 20%).  

---

## AWS Personal Health Dashboard
- Integrated with **Amazon EventBridge**.  
- Provides visibility into AWS resource health, outages, and remediation guidance.  

---

## AWS Config Aggregators
- Collect data across multiple accounts and regions.  
- Export to S3, notify via SNS.  

---

## RDS Read Replicas
- Availability mechanism complementing Multi-AZ.  
- Can be promoted if source DB fails.  
- Supports cross-region replication for DR.

---

## AWS Storage Gateway
- Hybrid storage between on-premises and AWS.  

**Types:**
- **File Gateway** – Access S3 via NFS/SMB.  
- **Volume Gateway** – Cloud-backed iSCSI volumes.  
- **Tape Gateway** – Backup/archive to Glacier.  

**Operation:** `RefreshCache` updates cached S3 inventory.

---

## Amazon Inspector
- Tests EC2 instance network accessibility and app security.  
- Assesses vulnerabilities and best-practice deviations.

---

## CodeDeploy Monitoring
Monitor deployments using:
- Amazon EventBridge  
- CloudWatch alarms  
- CloudWatch Logs  

**Targets via EventBridge:**
- Lambda  
- Kinesis streams  
- SQS queues  
- CloudWatch alarm actions  
- SNS topics  

---

## Transit Gateway
- Use when fixing overlapping IPs.  
- Create a TGW in a new account and share with AWS RAM.  
- Attach VPCs, add routes, use NLBs for communication.  

---

## AWS Application Discovery Service
Helps plan cloud migration by collecting on-premises usage/config data.  

**Discovery methods:**
- Agentless – via vCenter OVA connector  
- Agent-based – via Discovery Agent on servers  

---

## Egress-only Internet Gateway
- For IPv6 outbound traffic only.  
- Redundant and highly available.  

---

## DynamoDB Indexes
- **Global Secondary Index (GSI):** Partition + Sort key (different from base table).  
- **Local Secondary Index (LSI):** Same Partition key, different Sort key.  

Example: Query `LastPostDateTime` with an LSI instead of scanning entire table.

---

## ASG UpdatePolicy
**AutoScalingRollingUpdate options:**
- MaxBatchSize  
- MinInstancesInService  
- MinSuccessfulInstancesPercent (avoid rollback)  
- PauseTime (avoid success signal issues)  
- SuspendProcesses  
- WaitOnResourceSignal (avoid success signal issues)  

---

## ELB Deployment Types
- All at once  
- Rolling  
- Rolling with additional batch  
- Immutable  
- Blue/Green  

---

## AWS X-Ray
- Analyzes distributed app performance.  
- **Insights** detects outliers.  
- Supports Lambda tracing and EventBridge/CloudWatch integration.  

---

## Build AMI from SSM Automation
- Use SSM Automation documents.  
- Trigger via CodePipeline + EventBridge.  
- Store AMI IDs in Parameter Store.  

---

## SSM Patch Manager
- Install SSM agent on EC2 + on-prem.  
- Use patch baselines and maintenance windows.  

---

## Cross Region RDS Snapshot
- First copy = full snapshot.  
- Subsequent copies = incremental.  

---

## Amazon Data Firehose
- Capture/load streaming data into destinations (e.g., S3).  

---

## Amazon Athena
- Query S3 data directly with SQL.  

---

## CloudTrail Log File Integrity Validation
- Uses **SHA-256 + RSA** for hash/signing.  
- Detects modification, deletion, or forgery.  

---

## Audit Account Logging
- Centralize logs in an **Audit account**.  
- Stream VPC Flow Logs + CloudWatch Logs via Kinesis.  
- Store and search in **OpenSearch**.  

---

## Amazon Kinesis Adapter
- Recommended way to consume DynamoDB Streams.  
- Streams API intentionally mirrors Kinesis API.