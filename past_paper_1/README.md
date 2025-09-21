# 🧾 AWS Certified DevOps Engineer – Professional Study Guide  

---

## Domain 1: SDLC Automation (22%)  

### CodePipeline  
- **Actions & Stages**  
  - Run actions in parallel using `runOrder`.  
  - Supports **custom actions** (build, deploy, test, invoke). Requires **job worker** to poll for jobs and return results.  
  - **Cross-region actions** supported.  
- **CloudFormation Integration**  
  - Parameter overrides for templates at pipeline stage.  
- **AMI Builds**  
  - Use **CodePipeline + EventBridge + SSM Automation** for automated AMI builds. Store IDs in **SSM Parameter Store**.  

### CodeDeploy  
- **Deployment Types**  
  - All-at-once, Canary, Linear.  
  - For ELB: All-at-once, Rolling, Rolling + extra batch, Immutable, Blue/Green.  
- **Lifecycle Hooks** (AppSpec.yaml)  
  - BeforeInstall → AfterInstall → AfterAllowTestTraffic → BeforeAllowTraffic → AfterAllowTraffic.  
  - Rollbacks can be triggered after install/test/traffic hooks.  
  - Hooks can call **Lambda functions** for validation.  
- **Environment Variables**:  
  - `APPLICATION_NAME`, `DEPLOYMENT_ID`, `DEPLOYMENT_GROUP_NAME`, `DEPLOYMENT_GROUP_ID`, `LIFECYCLE_EVENT`.  
- **Monitoring**: EventBridge, CloudWatch alarms, CloudWatch Logs.  
- **Targets**: Lambda, Kinesis, SQS, SNS.  

### Lambda Deployment Strategies  
- **All-at-once, Canary, Linear** (via CodeDeploy).  
- **Serverless Canary**  
  - Lambda Aliases → split % traffic between versions.  
  - API Gateway canary → % traffic to canary stage.  

### Canary Deployment of ASG  
- Deploy new stack (ASG + ALB).  
- Weighted Route53 Alias records → gradually shift traffic.  

---

## Domain 2: Configuration Management & Infrastructure as Code (17%)  

### CloudFormation  
- **Custom Resources**  
  - Invoke Lambda via pre-signed URL.  
  - Lambda must respond success/failure → used for AMI updates, populating S3, etc.  
- **Cross-Stack References**  
  - `Export` outputs in one stack, `Fn::ImportValue` in another.  
- **ASG UpdatePolicy (AutoScalingRollingUpdate)**  
  - Params: MaxBatchSize, MinInstancesInService, MinSuccessfulInstancesPercent, PauseTime, SuspendProcesses, WaitOnResourceSignal.  

### AWS Config  
- **Capabilities**  
  - Tracks config changes, compliance dashboards, integrates with QuickSight.  
  - Supports **Config Rules** (managed or custom with Lambda).  
  - Triggers: config changes OR periodic.  
  - Automations: revert drifted configs, create golden AMIs, recover instances.  
- **Aggregators**  
  - Aggregate compliance data across accounts/regions.  
- **Integration**: store history in S3, analyze with QuickSight, notify with SNS.  

### SSM (Systems Manager)  
- **Run Command**: bootstrap apps, join domains, capture logs.  
- **Patch Manager**: preconfigured baselines, hybrid EC2/on-premises, schedule via maintenance windows.  
- **Automation**: build AMIs, remediate drift, rollback configs.  

### AWS Storage Gateway  
- **File Gateway**: NFS/SMB access → S3.  
- **Volume Gateway**: iSCSI block storage.  
- **Tape Gateway**: VTL → Glacier.  
- `RefreshCache` refreshes S3 bucket inventory.  

### AWS Application Discovery Service  
- **Agentless**: OVA for VMware vCenter.  
- **Agent-based**: install agent on each server.  

---

## Domain 3: Monitoring, Logging, and Performance Tuning (15%)  

### CloudWatch  
- **ASG Metrics**: deploy can stop if CloudWatch alarm triggered.  
- **Log Subscriptions**: send logs in real-time → Lambda, Kinesis Stream, Kinesis Firehose.  
- **CloudWatch → Kinesis → S3**: for analytics/archiving.  

### AWS X-Ray  
- Traces distributed applications.  
- **Insights**: detect anomalies and outliers.  
- Integrates with **Lambda** (enable tracing, IAM role for X-Ray).  
- Configure **groups + EventBridge rules + alarms**.  

### AWS Trusted Advisor  
- Cost, security, performance, fault tolerance, service quotas.  
- Integrated with **EventBridge + CloudWatch**.  

### AWS Health Dashboard  
- Personalized view of AWS events impacting your resources.  
- Integrated with **EventBridge** for proactive remediation.  

### CloudTrail Log Integrity Validation  
- Uses SHA-256 hash + RSA signature → detect tampering of logs.  

---

## Domain 4: Policies and Standards Automation (10%)  

### AWS Config (Compliance Automation)  
- Detect & auto-remediate drift with **SSM Automation**.  
- Example: EC2 must run on dedicated host → custom Config rule + Lambda.  

### Audit Account Logging  
- Centralized logging with **Kinesis Data Streams + Lambda + OpenSearch**.  
- Subscription filters forward logs from sub-accounts → Audit account.  

### Amazon Inspector  
- Automated vulnerability assessment.  
- Scans EC2 network accessibility and app security state.  

---

## Domain 5: Incident and Event Response (18%)  

### Route53  
- **Failover Routing Policy**  
  - Health checks → non-alias records.  
  - Alias records must set *Evaluate Target Health = Yes*.  

### ASG Lifecycle Hooks  
- **Launch flow**: Pending → Pending:Wait → Pending:Proceed → InService.  
- **Terminate flow**: Terminating → Terminating:Wait → Terminating:Proceed → Terminated.  
- Hooks allow customization before instance is InService/terminated.  

### Transit Gateway  
- Multi-VPC networking across accounts.  
- Fix overlapping CIDRs by renumbering.  
- Share TGW across org with RAM.  
- Attach VPCs + update routes.  

### Egress-Only Internet Gateway  
- IPv6 outbound-only internet access (no inbound).  

---

## Domain 6: High Availability, Fault Tolerance, and Disaster Recovery (18%)  

### Databases  
- **Aurora Global Database**  
  - Replication <1s across regions.  
  - Great for read scale + DR.  
  - Caveat: data sovereignty (solution: global for shared data, local clusters for sensitive data).  
- **RDS Read Replicas**  
  - Promote for HA/DR.  
  - Cross-region replication supported.  
- **Cross-Region Snapshots**  
  - First copy is full, subsequent copies are incremental.  

### Amazon FSx for NetApp ONTAP  
- Managed ONTAP file system.  
- Supports SMB + NFS.  
- Can be deployed in **primary + DR regions**.  

### VMware to EC2 Migration  
- **VM Import/Export** for VM images.  
- Use **AWS Management Portal for vCenter** for direct import.  

### Hybrid Storage  
- **Storage Gateway** (File, Volume, Tape).  

---

## Domain 7: Data & Analytics for Operations  

### Amazon Kinesis  
- **Data Firehose**: auto-ingest → S3, Redshift, OpenSearch.  
- **Kinesis Adapter**: recommended for DynamoDB Streams consumption.  

### DynamoDB  
- **Global Secondary Index (GSI)**: partition + sort key, spans table.  
- **Local Secondary Index (LSI)**: same partition key, diff sort key.  
- Use LSIs for time-based queries.  

### Amazon Athena  
- Query S3 data directly with SQL.  

---

# 📌 Key Takeaways (Exam-Cram)  

- **Deployments**: CodeDeploy supports EC2, Lambda, ECS with multiple strategies (blue/green, canary, linear). Use lifecycle hooks for validations/rollbacks.  
- **Pipelines**: CodePipeline integrates with CloudFormation, supports custom actions, and cross-region actions.  
- **IaC**: CloudFormation custom resources use Lambda + pre-signed URL. Cross-stack references with Export/ImportValue.  
- **Config & Compliance**: AWS Config monitors drift, enforces compliance, integrates with SSM for remediation. Aggregators collect data org-wide.  
- **Monitoring**: CloudWatch + X-Ray for observability. Trusted Advisor + Health Dashboard integrate with EventBridge.  
- **Event Handling**: Route53 failover + ASG lifecycle hooks critical for incident response.  
- **Hybrid & Migration**: FSx for ONTAP, Storage Gateway, Application Discovery Service, VM Import/Export.  
- **Databases**: Aurora Global DB <1s replication, RDS read replicas for HA/DR, cross-region snapshots.  
- **Networking**: Transit Gateway for multi-VPC, egress-only IGW for IPv6.  
- **Streaming & Analytics**: Kinesis + Firehose for logs/metrics, Athena for S3 queries, DynamoDB GSIs/LSIs for scaling queries.  