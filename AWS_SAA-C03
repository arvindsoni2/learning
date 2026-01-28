# AWS Solutions Architect Associate (SAA-C03) Cheat Sheet

## 📋 Exam Overview
- **Total Questions**: 65 (50 scored, 15 unscored)
- **Duration**: 130 minutes
- **Passing Score**: 720/1000
- **Question Types**: Multiple choice, Multiple response
- **Cost**: $150 USD (50% discount often available)

## 🎯 Exam Domains & Weightings

1. **Design Secure Architectures** - 30%
2. **Design Resilient Architectures** - 26%
3. **Design High-Performing Architectures** - 24%
4. **Design Cost-Optimized Architectures** - 20%

---

## 🔐 Domain 1: Design Secure Architectures (30%)

### IAM (Identity & Access Management)
**Core Concepts:**
- **Users**: Individual identities with permanent credentials
- **Groups**: Collections of users with shared permissions
- **Roles**: Temporary credentials for services/cross-account access
- **Policies**: JSON documents defining permissions
  - Identity-based (attached to users/groups/roles)
  - Resource-based (attached to resources like S3 buckets)
- **Service Control Policies (SCPs)**: Organizational-level permission boundaries

**Key Principles:**
- Principle of Least Privilege
- Root user MFA is mandatory
- Use roles instead of sharing credentials
- Never embed credentials in code

**IAM Best Practices:**
```
Root Account → Enable MFA
           → Create admin user
           → Lock away root credentials

Admin User → Create specific IAM users
          → Assign to groups with policies
          → Use roles for AWS services
```

### VPC (Virtual Private Cloud)
**Architecture:**
```
VPC (10.0.0.0/16)
├── Public Subnet (10.0.1.0/24) → Internet Gateway
│   ├── Web Servers
│   └── NAT Gateway
├── Private Subnet (10.0.2.0/24)
│   ├── App Servers
│   └── Route to NAT Gateway
└── Private Subnet (10.0.3.0/24)
    └── Database Servers
```

**Key Components:**
- **Subnets**: Public (has route to IGW) vs Private
- **Route Tables**: Direct traffic flow
- **Internet Gateway (IGW)**: Allows internet access for public subnets
- **NAT Gateway**: Allows private subnets to access internet (outbound only)
- **Security Groups**: Stateful firewall at instance level
- **NACLs**: Stateless firewall at subnet level
- **VPC Peering**: Connect VPCs (not transitive)
- **Transit Gateway**: Hub to connect multiple VPCs

**Security Group vs NACL:**
| Security Group | NACL |
|---------------|------|
| Stateful | Stateless |
| Instance-level | Subnet-level |
| Only allow rules | Allow + Deny rules |
| All rules evaluated | Rules in order |

### Encryption & Key Management
**KMS (Key Management Service):**
- **AWS Managed Keys**: Automatic rotation, managed by AWS
- **Customer Managed Keys**: You control rotation, policies
- **CloudHSM**: Dedicated hardware security module

**Encryption Types:**
- **At Rest**: S3-SSE, EBS encryption, RDS encryption
- **In Transit**: TLS/SSL, VPN, AWS Certificate Manager

### Security Services
- **AWS WAF**: Web Application Firewall (Layer 7)
- **AWS Shield**: DDoS protection (Standard free, Advanced paid)
- **GuardDuty**: Threat detection using ML
- **Inspector**: Automated security assessment
- **Macie**: Discover and protect sensitive data in S3
- **Secrets Manager**: Rotate, manage secrets
- **Parameter Store**: Hierarchical storage for config data

---

## 🏗️ Domain 2: Design Resilient Architectures (26%)

### EC2 (Elastic Compute Cloud)
**Instance Types (Mnemonic: FIGHT DR MC PX):**
- **F**: FPGA
- **I**: High IOPS
- **G**: Graphics
- **H**: High disk throughput
- **T**: Burstable (T2/T3)
- **D**: Dense storage
- **R**: RAM optimized
- **M**: General purpose
- **C**: Compute optimized
- **P**: GPU
- **X**: Extreme memory

**Purchasing Options:**
- **On-Demand**: Pay by hour/second, no commitment
- **Reserved**: 1-3 year commitment (up to 72% savings)
  - Standard: Most discount
  - Convertible: Can change instance type
  - Scheduled: Specific time windows
- **Spot**: Up to 90% discount, can be interrupted
- **Dedicated Host**: Physical server, compliance needs
- **Savings Plans**: Flexible pricing (Compute or EC2)

**Connection Diagram:**
```
User → [Route 53] → [CloudFront] → [ALB/NLB]
                                      ↓
                              [Auto Scaling Group]
                                      ↓
                              [EC2 Instances]
                                      ↓
                              [EBS Volumes]
```

### Load Balancing
**Application Load Balancer (ALB):**
- Layer 7 (HTTP/HTTPS)
- Path-based routing (/api → backend, /images → S3)
- Host-based routing (api.example.com, www.example.com)
- Target: EC2, Lambda, IP addresses
- Best for: Web applications, microservices

**Network Load Balancer (NLB):**
- Layer 4 (TCP/UDP)
- Ultra-high performance, millions of requests/sec
- Static IP support
- Best for: TCP/UDP traffic, extreme performance

**Gateway Load Balancer:**
- Layer 3 (IP packets)
- Third-party virtual appliances
- Best for: Firewalls, intrusion detection

**Classic Load Balancer:**
- Legacy (avoid in exam unless specified)

### Auto Scaling
**Components:**
1. **Launch Template/Configuration**: What to launch
2. **ASG**: Where and when to launch
3. **Scaling Policies**: How to scale
   - Target Tracking: Maintain metric (e.g., 70% CPU)
   - Step Scaling: Add/remove based on CloudWatch alarms
   - Scheduled: Time-based scaling
   - Predictive: ML-based forecasting

**ASG + ALB Pattern:**
```
CloudWatch Alarm (CPU > 70%)
    ↓
Auto Scaling Policy
    ↓
Launch new EC2 instances
    ↓
Register with ALB
```

### Storage Solutions

**EBS (Elastic Block Store):**
| Type | Use Case | IOPS | Size |
|------|----------|------|------|
| gp3/gp2 | General purpose SSD | 16,000 | 1GB-16TB |
| io2/io1 | High performance SSD | 64,000 | 4GB-16TB |
| st1 | Throughput HDD | 500 | 125GB-16TB |
| sc1 | Cold HDD (lowest cost) | 250 | 125GB-16TB |

**Key Points:**
- EBS volumes are AZ-specific
- Snapshots are stored in S3, incremental
- Can create AMI from snapshot
- Encryption must be enabled at creation

**EFS (Elastic File System):**
- Managed NFS for Linux
- Multi-AZ by default
- Performance modes: General Purpose vs Max I/O
- Throughput modes: Bursting vs Provisioned
- Storage classes: Standard vs Infrequent Access (EFS-IA)
- Use case: Shared file system across multiple EC2 instances

**Instance Store:**
- Ephemeral storage (data lost on stop/terminate)
- Very high IOPS
- Use case: Temporary data, caches, buffers

### S3 (Simple Storage Service)
**Storage Classes:**
```
Frequent Access:
├── S3 Standard (99.99% availability)

Infrequent Access:
├── S3 Standard-IA (99.9% availability)
├── S3 One Zone-IA (99.5% availability)

Archive:
├── S3 Glacier Instant Retrieval (ms)
├── S3 Glacier Flexible Retrieval (minutes-hours)
└── S3 Glacier Deep Archive (12-48 hours)

Smart Tiering: Auto-moves between tiers
```

**S3 Features:**
- **Versioning**: Keep multiple versions of objects
- **Replication**: Cross-Region (CRR) or Same-Region (SRR)
- **Lifecycle Policies**: Auto-transition between storage classes
- **Transfer Acceleration**: Use CloudFront edge locations for uploads
- **MFA Delete**: Require MFA to delete objects
- **Object Lock**: WORM (Write Once Read Many) compliance
- **Event Notifications**: Trigger Lambda, SQS, SNS on object changes

**S3 Consistency:**
- Strong read-after-write consistency for all operations

### Database Services

**RDS (Relational Database Service):**
- **Engines**: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Aurora
- **Multi-AZ**: Synchronous replication for HA (automatic failover)
- **Read Replicas**: Asynchronous replication for read scaling
  - Can be cross-region
  - Up to 5 read replicas
  - Can be promoted to standalone DB
- **Automated Backups**: Point-in-time recovery (up to 35 days)
- **Storage Auto Scaling**: Automatically increase storage

**Aurora:**
- MySQL/PostgreSQL compatible, 5x/3x faster
- 6 copies across 3 AZs
- Self-healing storage
- Aurora Serverless: Auto-scaling, pay per second
- Aurora Global Database: Cross-region replication (< 1 sec)
- Aurora Multi-Master: Multiple write nodes

**DynamoDB:**
- NoSQL, fully managed
- Single-digit millisecond latency
- Auto-scaling
- **Capacity Modes:**
  - On-Demand: Pay per request
  - Provisioned: Specify RCU/WCU
- **Features:**
  - DynamoDB Streams: Change data capture
  - Global Tables: Multi-region, multi-active
  - DAX: In-memory cache (microsecond latency)
  - Point-in-time Recovery (PITR)

**ElastiCache:**
- **Redis**: Advanced data structures, persistence, Multi-AZ
- **Memcached**: Simple, multi-threaded, no persistence
- Use case: Session storage, caching, real-time analytics

**Database Selection Guide:**
```
Relational + ACID → RDS/Aurora
NoSQL + Scale → DynamoDB
In-memory + Cache → ElastiCache
Data Warehouse → Redshift
Graph DB → Neptune
Time-series → Timestream
```

### High Availability & Disaster Recovery

**DR Strategies (increasing cost & decreasing RTO/RPO):**
1. **Backup & Restore**: Lowest cost, hours RTO
2. **Pilot Light**: Core components running, 10s of minutes
3. **Warm Standby**: Scaled-down version, minutes RTO
4. **Multi-Site/Hot Site**: Full production, seconds RTO

**RTO**: Recovery Time Objective (how long to recover)
**RPO**: Recovery Point Objective (how much data loss acceptable)

---

## ⚡ Domain 3: Design High-Performing Architectures (24%)

### Compute Services

**Lambda:**
- Serverless compute, pay per invocation
- Max execution: 15 minutes
- Max memory: 10GB
- Use cases: Event-driven, microservices, ETL
- **Triggers**: S3, DynamoDB, Kinesis, API Gateway, EventBridge, etc.
- **Concurrency**: Default 1000, can increase
- **Cold starts**: Initialize execution environment

**ECS (Elastic Container Service):**
- **Launch Types:**
  - EC2: You manage the instances
  - Fargate: Serverless, AWS manages infrastructure
- **Components:**
  - Task Definition: Docker container settings
  - Service: Maintain desired task count
  - Cluster: Logical grouping

**EKS (Elastic Kubernetes Service):**
- Managed Kubernetes
- More complex than ECS, more portable
- Use when: Already using K8s, multi-cloud

**Compute Selection:**
```
Need control over OS → EC2
Event-driven, short tasks → Lambda
Containers + simple → ECS Fargate
Containers + complex orchestration → EKS
Batch jobs → AWS Batch
```

### Content Delivery & Caching

**CloudFront:**
- Global CDN, 400+ edge locations
- **Origin**: S3, ALB, EC2, custom HTTP
- **Features:**
  - Signed URLs/Cookies for private content
  - Field-level encryption
  - Lambda@Edge: Run code at edge locations
  - Origin Failover: Automatic failover to secondary origin
- **Use cases**: Video streaming, static assets, API acceleration

**Architecture Pattern:**
```
User Request → CloudFront (Edge Location)
                    ↓ (Cache Miss)
              Regional Edge Cache
                    ↓ (Cache Miss)
              Origin (S3/ALB/Custom)
```

**Global Accelerator:**
- Uses AWS global network, not caching
- 2 static anycast IPs
- Best for: TCP/UDP traffic, gaming, IoT
- vs CloudFront: CloudFront caches, GA doesn't

### Database Performance

**Read Replicas vs Multi-AZ:**
| Read Replicas | Multi-AZ |
|--------------|----------|
| Scale read traffic | High availability |
| Async replication | Sync replication |
| Multiple replicas | 1 standby |
| Can be cross-region | Same region only |

**Caching Strategies:**
```
Application → ElastiCache → Database
            ↓ (Cache Miss)
          Read from DB → Update Cache
```

**DynamoDB Accelerator (DAX):**
- Microsecond latency for DynamoDB
- In-memory cache
- 10x performance improvement

### Networking Performance

**Enhanced Networking:**
- SR-IOV (Single Root I/O Virtualization)
- Higher bandwidth, higher PPS
- Lower latency
- Enabled on most modern instance types

**Placement Groups:**
- **Cluster**: Low latency, high throughput (same AZ)
  - Use: HPC, big data
- **Spread**: Across hardware (max 7 instances per AZ)
  - Use: Critical applications
- **Partition**: Groups of instances on different hardware
  - Use: Distributed systems (Hadoop, Cassandra)

---

## 💰 Domain 4: Design Cost-Optimized Architectures (20%)

### Storage Cost Optimization

**S3 Intelligent-Tiering:**
- Automatic cost optimization
- No retrieval fees
- Small monitoring fee
- Moves between access tiers automatically

**S3 Lifecycle Rules:**
```
Day 0: S3 Standard
Day 30: S3 Standard-IA
Day 90: S3 Glacier
Day 365: S3 Deep Archive
```

**EBS Cost Optimization:**
- Delete unattached volumes
- Snapshot old volumes, delete volume
- Use gp3 instead of gp2 (same performance, 20% cheaper)
- Right-size volumes

### Compute Cost Optimization

**Savings Plans vs Reserved Instances:**
- Savings Plans: More flexible, cover Lambda, Fargate
- Reserved: Instance-specific, higher discount

**Spot Instance Best Practices:**
- Use for fault-tolerant workloads
- Spot Fleet: Mix of Spot and On-Demand
- Spot Block: Uninterrupted for 1-6 hours (deprecated)

**Lambda Cost:**
- Free tier: 1M requests, 400,000 GB-seconds/month
- Optimize memory allocation (affects CPU)
- Use provisioned concurrency only when needed

### Data Transfer Costs

**Cost Tips:**
- Same AZ: Free
- Different AZ: $0.01/GB
- Inter-region: $0.02/GB
- Use VPC endpoints to avoid NAT Gateway costs
- Use CloudFront to reduce origin requests
- Use S3 Transfer Acceleration for large files

**NAT Gateway vs NAT Instance:**
- NAT Gateway: Managed, expensive, highly available
- NAT Instance: Self-managed, cheaper, single point of failure

---

## 🔗 Service Connections & Common Patterns

### Serverless Architecture
```
API Gateway → Lambda → DynamoDB
     ↓           ↓
CloudWatch  S3 (logs/data)
```

### Web Application (3-Tier)
```
Route 53 (DNS)
    ↓
CloudFront (CDN)
    ↓
ALB (Load Balancer)
    ↓
Auto Scaling Group
    ↓
EC2 Instances (App Tier)
    ↓
RDS Multi-AZ (Database)
```

### Event-Driven Processing
```
S3 Upload → S3 Event → Lambda → Process Data → Store in DynamoDB
                                  ↓
                              SNS Notification
```

### Data Processing Pipeline
```
Data Source → Kinesis Data Stream → Lambda/Kinesis Analytics
                                          ↓
                                    S3 (Data Lake)
                                          ↓
                                    Athena (Query)
                                          ↓
                                    QuickSight (Visualize)
```

### Hybrid Architecture
```
On-Premises ←→ VPN/Direct Connect ←→ VPC
                                      ↓
                               AWS Services
```

---

## 📊 Analytics & Integration Services

### Data Streaming
**Kinesis:**
- **Data Streams**: Real-time data streaming (custom processing)
- **Data Firehose**: Load streaming data to S3, Redshift, ElasticSearch
- **Data Analytics**: SQL queries on streaming data
- **Video Streams**: Capture, process video streams

### Message Queuing
**SQS (Simple Queue Service):**
- **Standard**: Best-effort ordering, at-least-once delivery
- **FIFO**: Guaranteed ordering, exactly-once delivery
- Visibility timeout: Message invisible after read
- Dead Letter Queue: Failed messages
- Long polling: Reduce empty responses

**SNS (Simple Notification Service):**
- Pub/Sub messaging
- Push notifications to subscribers
- Fan-out pattern: SNS → Multiple SQS queues

**EventBridge:**
- Serverless event bus
- Schedule events (cron)
- Event patterns from AWS services
- Custom applications

**SQS vs SNS:**
| SQS | SNS |
|-----|-----|
| Pull-based | Push-based |
| Queue | Topic |
| Decoupling | Fan-out |
| Message retention | No retention |

### ETL & Data Processing
**Glue:**
- Serverless ETL
- Data Catalog: Metadata repository
- Crawlers: Auto-discover schema
- Use case: Prepare data for analytics

**EMR (Elastic MapReduce):**
- Managed Hadoop framework
- Big data processing (Spark, HBase, Presto)
- Cluster-based, can use Spot instances

**Data Pipeline:**
- Orchestrate data movement
- Schedule and dependency management
- Use case: Regular ETL jobs

### Data Warehousing
**Redshift:**
- Petabyte-scale data warehouse
- Columnar storage
- Massively parallel processing (MPP)
- **Redshift Spectrum**: Query S3 without loading
- Use case: Analytics, BI

**Athena:**
- Serverless SQL queries on S3
- Pay per query
- Use with Glue Catalog
- Use case: Ad-hoc analysis

---

## 🛠️ Management & Monitoring

### CloudWatch
**Metrics:**
- Default: CPU, Network, Disk (not memory)
- Custom: Application metrics
- Resolution: Standard (5 min), Detailed (1 min)

**Alarms:**
- Trigger actions (SNS, ASG, EC2)
- Based on metric thresholds

**Logs:**
- Centralized log storage
- Log groups and streams
- Retention policies
- Insights: Query logs

**Events → EventBridge:**
- Schedule events
- React to state changes

### CloudTrail
- Governance, compliance, audit
- Records API calls
- Stores logs in S3
- **Use cases:**
  - Security analysis
  - Compliance auditing
  - Track changes

### Config
- Resource inventory and configuration history
- Config Rules: Compliance checking
- Remediation actions
- Use case: Compliance, governance

### CloudFormation
- Infrastructure as Code (IaC)
- JSON/YAML templates
- **Components:**
  - Resources: AWS resources to create
  - Parameters: Input values
  - Outputs: Return values
  - Mappings: Key-value pairs
- **Features:**
  - Stack: Group of resources
  - StackSets: Deploy across accounts/regions
  - Change Sets: Preview changes

**CloudFormation vs Terraform:**
- CloudFormation: AWS-native, free
- Terraform: Multi-cloud, more popular

### Systems Manager
- **Parameter Store**: Secure strings, config
- **Session Manager**: Shell access without SSH
- **Patch Manager**: Automate patching
- **Run Command**: Execute commands on EC2

---

## 🚀 Migration & Transfer Services

**Snow Family:**
- **Snowcone**: 8TB, portable
- **Snowball**: 80TB, compute optimized available
- **Snowmobile**: 100PB, truck
- Use case: Migrate large amounts of data (TB/PB)

**DataSync:**
- Online data transfer
- AWS to AWS or on-premises to AWS
- Schedule transfers
- Bandwidth throttling

**Transfer Family:**
- SFTP, FTPS, FTP to S3/EFS
- Managed service
- Use case: Third-party file transfers

**Database Migration Service (DMS):**
- Migrate databases to AWS
- Homogeneous (Oracle → Oracle) or Heterogeneous (Oracle → PostgreSQL)
- Schema Conversion Tool (SCT) for heterogeneous
- Minimal downtime

**Application Migration Service:**
- Lift-and-shift migrations
- Automated replication
- Use case: Move servers to AWS

---

## 🔍 Key Exam Scenarios & Solutions

### Scenario 1: High Availability Web Application
**Requirements**: 99.99% availability, scale automatically, global users

**Solution:**
```
Route 53 (Latency-based routing)
    ↓
CloudFront (Global CDN)
    ↓
Multi-Region ALB
    ↓
Auto Scaling Groups (Multiple AZs)
    ↓
Aurora Global Database (Multi-Region)
```

### Scenario 2: Cost Optimization
**Requirements**: Reduce costs, maintain performance

**Solution Checklist:**
- Use Reserved Instances/Savings Plans for steady workloads
- Use Spot Instances for fault-tolerant workloads
- S3 Intelligent-Tiering or Lifecycle policies
- Right-size EC2 instances (CloudWatch metrics)
- Use ElastiCache to reduce database load
- Delete unattached EBS volumes
- Use S3 Transfer Acceleration only when needed

### Scenario 3: Serverless Application
**Requirements**: No server management, event-driven

**Solution:**
```
API Gateway → Lambda → DynamoDB
S3 Events → Lambda → Process → Store in S3
CloudWatch Events → Lambda → Automated tasks
```

### Scenario 4: Hybrid Cloud
**Requirements**: Connect on-premises to AWS securely

**Solutions:**
- **VPN**: Quick setup, internet-based, lower cost
- **Direct Connect**: Dedicated connection, consistent, higher cost
- **Storage Gateway**: Hybrid storage (File, Volume, Tape)
- **DataSync**: Scheduled data transfers

### Scenario 5: Disaster Recovery
**Requirements**: RPO 1 hour, RTO 4 hours

**Solution**: Pilot Light
- Keep critical data replicated (S3, RDS snapshots)
- Minimal resources running (Route 53, small RDS)
- CloudFormation templates ready
- AMIs prepared
- On disaster: Launch full infrastructure

---

## 🎓 Exam Tips

### Question Keywords
- **"Most cost-effective"** → Serverless (Lambda, S3, DynamoDB), Spot Instances
- **"Least operational overhead"** → Managed services, serverless
- **"Highly available"** → Multi-AZ, Multi-Region, ALB
- **"Scalable"** → Auto Scaling, serverless, DynamoDB
- **"Secure"** → Encryption, IAM policies, VPC, Security Groups
- **"Real-time"** → Kinesis, Lambda, DynamoDB Streams
- **"Lowest latency"** → CloudFront, ElastiCache, DAX

### Service Comparison Quick Reference

**Storage:**
- **EBS**: Block storage, single EC2
- **EFS**: File storage, multiple EC2
- **S3**: Object storage, internet-accessible
- **Instance Store**: Ephemeral, highest IOPS

**Database:**
- **RDS**: Relational, managed
- **Aurora**: Enhanced RDS, global
- **DynamoDB**: NoSQL, serverless
- **Redshift**: Data warehouse

**Compute:**
- **EC2**: Virtual machines
- **Lambda**: Serverless functions
- **ECS**: Containers (simple)
- **EKS**: Kubernetes (complex)

**Networking:**
- **ALB**: Layer 7 (HTTP/HTTPS)
- **NLB**: Layer 4 (TCP/UDP)
- **CloudFront**: CDN
- **Route 53**: DNS

### Common Mistakes to Avoid
1. Confusing Multi-AZ with Read Replicas
2. Not reading question carefully (cost vs performance)
3. Choosing managed NAT Gateway when cost is priority
4. Not considering data transfer costs
5. Choosing CloudFront when CloudWatch is needed
6. Forgetting S3 is object storage (can't mount as volume)
7. Using FIFO SQS when standard would work (FIFO limited throughput)

### Study Strategy
1. **Understand, don't memorize**: Know WHY a service is used
2. **Hands-on practice**: Free tier, labs
3. **Practice exams**: Tutorials Dojo, Whizlabs
4. **Focus on relationships**: How services connect
5. **Know service limits**: Lambda timeout, S3 object size
6. **Read ALL answers**: Eliminate wrong answers first
7. **Time management**: Flag difficult questions, come back

---

## 📚 Additional Resources

**Official AWS:**
- AWS Well-Architected Framework
- AWS Whitepapers (Disaster Recovery, Cost Optimization)
- AWS Architecture Center
- AWS FAQs (for each service)

**Practice:**
- Tutorials Dojo Practice Exams
- Stephane Maarek's Udemy Course
- Adrian Cantrill's Course
- AWS SkillBuilder

**Cheat Sheets:**
- Tutorials Dojo Cheat Sheets
- AWS Service comparison guides
- This document!

---

## ✅ Pre-Exam Checklist

**Week Before:**
- [ ] Complete 3+ practice exams (score 80%+)
- [ ] Review all service FAQs
- [ ] Re-read this cheat sheet
- [ ] Review wrong answers from practice exams
- [ ] Understand Well-Architected Framework pillars

**Day Before:**
- [ ] Light review, don't cram
- [ ] Get good sleep
- [ ] Prepare ID documents
- [ ] Test your exam environment (if online)

**Exam Day:**
- [ ] Arrive 15 minutes early
- [ ] Read questions carefully
- [ ] Flag difficult questions
- [ ] Eliminate wrong answers
- [ ] Review flagged questions
- [ ] Check all answers before submitting

---

**Good luck with your exam! You've got this! 🚀**

*Remember: The exam tests your ability to design solutions, not just memorize services. Think like an architect!*
