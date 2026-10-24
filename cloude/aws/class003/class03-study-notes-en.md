# AWS Object Storage, Serverless Compute & Managed NoSQL Database — Class 03 Study Notes

**Topic:** Amazon S3 Architecture & Storage Classes, Programmatic Automation with Python Boto3, Serverless Compute with AWS Lambda, Managed NoSQL with Amazon DynamoDB, CloudWatch Observability, and Disaster Recovery Backups  
**Covers:** Object Storage vs Block/File Storage, S3 Bucket Global Naming Rules, 11 9's Durability vs Availability, S3 Storage Class Economics (Standard, Intelligent-Tiering, Glacier Instant/Flexible/Deep Archive), S3 Multipart Upload Architecture, Python Boto3 SDK Integration (`client` vs `resource`), Serverless Computing Paradigm (FaaS/PaaS), Lambda Anatomy (`handler`, `event`, `context`, runtime, timeout, memory), Lambda Execution Roles (IAM), Amazon CloudWatch Logs & Retention Policies, Amazon DynamoDB Architecture (Tables, Items, Attributes, Partition Key vs Sort Key, On-Demand vs Provisioned Capacity), End-to-End Serverless Data Pipeline (External REST API $\rightarrow$ Lambda $\rightarrow$ IAM Inline Policy $\rightarrow$ DynamoDB $\rightarrow$ S3 Backup), and Production Disaster Recovery  
**Course & Class:** BongoDev DevOps & Cloud Engineering — Cloud Track: AWS Class 03 (Recorded 23 October 2026)  
**Instructor:** Nure Siddiqui  
**Builds on:** [Class 01 notes](../class001/class01-study-notes-en.md) (EC2, AMIs, EBS Volumes, Security Groups, Launch Templates) and [Class 02 notes](../class002/class02-study-notes-en.md) (Global Infrastructure, 3 AWS Access Paradigms, IAM Pillars, JSON Policies, AWS CLI Configuration)  
**Next Steps:** VPC Network Architecture (VPC, Public/Private Subnets, Internet Gateways, NAT Gateways, Route Tables), Containerization & ECS/EKS, and Infrastructure as Code with Terraform

> **About these study notes:** These notes compile every concept, architectural theory, live demonstration, debugging moment, and security rule taught in Class 03. In Class 01 and 02, we provisioned virtual servers (EC2) and secured our environment (IAM). In Class 03, we break free from the traditional "manage your own server" mindset. We explore **Object Storage (Amazon S3)** for infinite elastic data storage, write **Python automation scripts using Boto3**, transition to **Serverless Event-Driven Compute (AWS Lambda)** where servers disappear completely, store structured schemaless records in **Amazon DynamoDB (NoSQL)**, monitor executions with **CloudWatch Logs**, and architect automated disaster recovery pipelines to back up databases to S3. All diagrams are rendered using native Mermaid vector syntax for seamless visualization.

---

## Table of Contents

1. [Recap of Classes 01 & 02 and Class 03 Roadmap](#1-recap-of-classes-01--02-and-class-03-roadmap)
2. [Storage Paradigms: Block vs File vs Object Storage](#2-storage-paradigms-block-vs-file-vs-object-storage)
   - 2.1 [Why EBS Cannot Solve Every Storage Problem](#21-why-ebs-cannot-solve-every-storage-problem)
   - 2.2 [What is Amazon S3 (Simple Storage Service)?](#22-what-is-amazon-s3-simple-storage-service)
   - 2.3 [Core S3 Concepts: Buckets, Objects, Keys, and Metadata](#23-core-s3-concepts-buckets-objects-keys-and-metadata)
   - 2.4 [Bucket Naming Rules & Global DNS Uniqueness](#24-bucket-naming-rules--global-dns-uniqueness)
   - 2.5 [The 11 9's Durability ($99.999999999\%$) vs Availability ($99.99\%$)](#25-the-11-9s-durability-99999999999-vs-availability-9999)
   - 2.6 [Static Website Hosting on S3](#26-static-website-hosting-on-s3)
3. [Amazon S3 Storage Classes & Cost Optimization](#3-amazon-s3-storage-classes--cost-optimization)
   - 3.1 [The Cost Matrix: From \$23/TB to \$1/TB](#31-the-cost-matrix-from-23tb-to-1tb)
   - 3.2 [S3 Standard](#32-s3-standard)
   - 3.3 [S3 Intelligent-Tiering](#33-s3-intelligent-tiering)
   - 3.4 [S3 Standard-Infrequent Access (IA) & One Zone-IA](#34-s3-standard-infrequent-access-ia--one-zone-ia)
   - 3.5 [S3 Glacier Instant Retrieval vs Flexible Retrieval vs Deep Archive](#35-s3-glacier-instant-retrieval-vs-flexible-retrieval-vs-deep-archive)
   - 3.6 [S3 Lifecycle Management Transitions](#36-s3-lifecycle-management-transitions)
4. [S3 Advanced Operations & Security Architecture](#4-s3-advanced-operations--security-architecture)
   - 4.1 [Block Public Access (BPA): The Default Guardrail](#41-block-public-access-bpa-the-default-guardrail)
   - 4.2 [S3 Bucket Versioning: Preventing Accidental Overwrites & Deletions](#42-s3-bucket-versioning-preventing-accidental-overwrites--deletions)
   - 4.3 [S3 Multipart Upload: How AWS Ingests Large Files](#43-s3-multipart-upload-how-aws-ingests-large-files)
   - 4.4 [Server-Side Encryption: SSE-S3 vs SSE-KMS](#44-server-side-encryption-sse-s3-vs-sse-kms)
5. [Programmatic Cloud Automation with Python & Boto3](#5-programmatic-cloud-automation-with-python--boto3)
   - 5.1 [Why Real DevOps Engineers Automate with Python](#51-why-real-devops-engineers-automate-with-python)
   - 5.2 [Installing and Setting Up Boto3](#52-installing-and-setting-up-boto3)
   - 5.3 [Authentication Resolution: How Boto3 Finds `~/.aws/credentials`](#53-authentication-resolution-how-boto3-finds-awscredentials)
   - 5.4 [Boto3 Client vs Resource API Paradigms](#54-boto3-client-vs-resource-api-paradigms)
   - 5.5 [Automated Batch File Ingestion Script Walkthrough](#55-automated-batch-file-ingestion-script-walkthrough)
   - 5.6 [Handling Nested Directories with Recursive Traversal](#56-handling-nested-directories-with-recursive-traversal)
6. [Serverless Compute: AWS Lambda Deep Dive](#6-serverless-compute-aws-lambda-deep-dive)
   - 6.1 [The Serverless Mental Model: Function-as-a-Service (FaaS)](#61-the-serverless-mental-model-function-as-a-service-faas)
   - 6.2 [When to Use Lambda vs EC2 (The Cannon vs Flyswatter Principle)](#62-when-to-use-lambda-vs-ec2-the-cannon-vs-flyswatter-principle)
   - 6.3 [Anatomy of a Lambda Function: Handler, Event, and Context](#63-anatomy-of-a-lambda-function-handler-event-and-context)
   - 6.4 [Runtime Configuration: `<filename>.<handler_function>`](#64-runtime-configuration-filenamehandler_function)
   - 6.5 [Tuning Execution Timeouts and Memory Allocation](#65-tuning-execution-timeouts-and-memory-allocation)
   - 6.6 [Packaging Code: Zip File Uploads & Dependencies](#66-packaging-code-zip-file-uploads--dependencies)
7. [CloudWatch Observability & Logging for Serverless](#7-cloudwatch-observability--logging-for-serverless)
   - 7.1 [Observability Pillars: Metrics, Logs, and Alarms](#71-observability-pillars-metrics-logs-and-alarms)
   - 7.2 [The IAM Execution Role: `AWSLambdaBasicExecutionRole`](#72-the-iam-execution-role-awslambdabasicexecutionrole)
   - 7.3 [CloudWatch Log Groups, Log Streams, and Event Records](#73-cloudwatch-log-groups-log-streams-and-event-records)
   - 7.4 [Configuring Log Retention Periods to Avoid Hidden Cloud Costs](#74-configuring-log-retention-periods-to-avoid-hidden-cloud-costs)
8. [Managed NoSQL Database: Amazon DynamoDB](#8-managed-nosql-database-amazon-dynamodb)
   - 8.1 [Relational (SQL/RDS) vs Document / Key-Value NoSQL](#81-relational-sqlrds-vs-document--key-value-nosql)
   - 8.2 [Self-Hosted Databases (EC2) vs Managed Cloud Databases](#82-self-hosted-databases-ec2-vs-managed-cloud-databases)
   - 8.3 [DynamoDB Core Building Blocks: Tables, Items, and Attributes](#83-dynamodb-core-building-blocks-tables-items-and-attributes)
   - 8.4 [Primary Key Strategies: Partition Key (PK) vs Composite Key (PK + SK)](#84-primary-key-strategies-partition-key-pk-vs-composite-key-pk--sk)
   - 8.5 [Capacity Modes: On-Demand vs Provisioned (RCU / WCU)](#85-capacity-modes-on-demand-vs-provisioned-rcu--wcu)
9. [End-to-End Hands-on Project: The Serverless Data Pipeline](#9-end-to-end-hands-on-project-the-serverless-data-pipeline)
   - 9.1 [Project Architecture & Workflow Topology](#91-project-architecture--workflow-topology)
   - 9.2 [Step 1: DynamoDB Table Provisioning](#92-step-1-dynamodb-table-provisioning)
   - 9.3 [Step 2: Environment Variables for Infrastructure Abstraction](#93-step-2-environment-variables-for-infrastructure-abstraction)
   - 9.4 [Step 3: Developing the Lambda Python Handler](#94-step-3-developing-the-lambda-python-handler)
   - 9.5 [Step 4: Diagnosing the Live IAM Permission Deficit](#95-step-4-diagnosing-the-live-iam-permission-deficit)
   - 9.6 [Step 5: Writing and Attaching the Inline IAM DynamoDB Policy](#96-step-5-writing-and-attaching-the-inline-iam-dynamodb-policy)
   - 9.7 [Step 6: Executing the Test Event & Inspecting DynamoDB Items](#97-step-6-executing-the-test-event--inspecting-dynamodb-items)
10. [Disaster Recovery & Database Backups (Homework Assignment)](#10-disaster-recovery--database-backups-homework-assignment)
    - 10.1 [The 3-2-1 Backup Strategy in the Cloud](#101-the-3-2-1-backup-strategy-in-the-cloud)
    - 10.2 [DynamoDB Point-in-Time Recovery (PITR) & On-Demand Backups](#102-dynamodb-point-in-time-recovery-pitr--on-demand-backups)
    - 10.3 [Homework Assignment: Automated DynamoDB $\rightarrow$ S3 Archival Pipeline](#103-homework-assignment-automated-dynamodb--s3-archival-pipeline)
    - 10.4 [Multi-Service IAM Policy Architecture for the Backup Lambda](#104-multi-service-iam-policy-architecture-for-the-backup-lambda)
11. [Real-World Gotchas, Traps & Debugging](#11-real-world-gotchas-traps--debugging)
12. [Questions Asked in Class (Q&A)](#12-questions-asked-in-class-qa)
13. [Common Misconceptions, Corrected](#13-common-misconceptions-corrected)
14. [Hands-On Practice Exercises](#14-hands-on-practice-exercises)
15. [DevOps Interview Corner](#15-devops-interview-corner)
16. [Bilingual Technical Glossary](#16-bilingual-technical-glossary)
17. [One-Page S3, Lambda & DynamoDB Cheat Sheet](#17-one-page-s3-lambda--dynamodb-cheat-sheet)

---

## 1. Recap of Classes 01 & 02 and Class 03 Roadmap

In our journey through the BongoDev Cloud Track:

- **Class 01:** We demystified virtualized infrastructure. We provisioned Amazon EC2 instances, attached persistent Elastic Block Store (EBS) SSD volumes, configured Security Groups as stateful firewalls, established secure asymmetric SSH access, and automated horizontal server blueprints using Launch Templates.
- **Class 02:** We stepped back to analyze the global architectural footprint of AWS. We explored geographic Regions, fault-isolated Availability Zones (AZs), Local Zones, Outposts, and Edge Locations. We established root account security hygiene, broke down IAM Users, Groups, Roles, and JSON Policies, and mastered the AWS CLI.

```mermaid
flowchart LR
    subgraph C1["Class 01: Infrastructure Fundamentals"]
        EC2["Amazon EC2 (Compute)"] --> EBS["Amazon EBS (Block Disk)"]
        EBS --> SG["Security Groups"]
        SG --> LT["Launch Templates"]
    end

    subgraph C2["Class 02: Governance & Security"]
        GI["Global Infra (Regions & AZs)"] --> IAM["IAM Roles & JSON Policies"]
        IAM --> CLI["AWS CLI & Credentials"]
    end

    subgraph C3["Class 03: Serverless & Data Management"]
        S3["Amazon S3 (Object Storage)"] --> Boto3["Python Boto3 SDK"]
        Boto3 --> Lambda["AWS Lambda (Serverless Compute)"]
        Lambda --> DDB["Amazon DynamoDB (Managed NoSQL)"]
        DDB --> CW["CloudWatch Observability"]
        CW --> Backup["Disaster Recovery & S3 Backup"]
    end

    C1 --> C2 --> C3
```

In **Class 03**, we make a profound paradigm shift: **from server-centric operations to serverless, event-driven cloud native architectures**.

Running an EC2 instance 24/7 just to execute a 5-second database backup or an API sync is financially wasteful and operationally burdensome (requiring OS patches, kernel updates, security auditing, and disk management). We replace this overhead with **Amazon S3** for infinitely scalable storage, **AWS Lambda** for compute that executes on-demand without managing servers, **Amazon DynamoDB** for zero-maintenance NoSQL data persistence, and **Python Boto3** for programmatic glue.

---

## 2. Storage Paradigms: Block vs File vs Object Storage

To choose the right cloud storage architecture, a DevOps engineer must understand the fundamental differences between **Block Storage**, **File Storage**, and **Object Storage**.

```mermaid
flowchart TD
    subgraph BS["1. Block Storage (Amazon EBS)"]
        direction TB
        B1["Virtual Hard Disk attached to 1 EC2 instance"]
        B2["Data split into fixed raw blocks (512 bytes / 4KB)"]
        B3["High IOPS, ultra-low sub-millisecond latency"]
        B4["Use Case: OS Boot Volume, Relational Database Files"]
    end

    subgraph FS["2. File Storage (Amazon EFS)"]
        direction TB
        F1["Shared Network File System (NFSv4)"]
        F2["Hierarchical tree structure with folders & permissions"]
        F3["Concurrent read/write access from 100s of EC2 instances"]
        F4["Use Case: Shared CMS uploads, legacy enterprise shares"]
    end

    subgraph OS["3. Object Storage (Amazon S3)"]
        direction TB
        O1["Flat namespace: Bucket + Object Key"]
        O2["Accessed over HTTP/HTTPS REST APIs anywhere on the web"]
        O3["Practically unlimited scale, 11 9's durability"]
        O4["Use Case: Backups, media assets, logs, static web assets"]
    end
```

### 2.1 Why EBS Cannot Solve Every Storage Problem

In Class 01, we learned that **Amazon EBS** acts like an internal SSD drive physically attached to our EC2 instance through a virtual storage bus. However, EBS has severe architectural limitations for enterprise data:

1. **Single-AZ Binding:** An EBS volume lives inside exactly one Availability Zone. If that AZ experiences a hardware failure, the volume is inaccessible unless a snapshot was taken.
2. **Instance Coupling:** Standard EBS volumes can only attach to one EC2 instance at a time (excluding specialized EBS Multi-Attach for clustered file systems).
3. **Provisioned Capacity Waste:** If you create a 100 GB EBS volume and only store 2 GB of data, you pay AWS for the full 100 GB every single hour.
4. **Finite Scaling:** An individual EBS volume caps out at 16 TB or 64 TB depending on the volume type (`gp3`, `io2`).
5. **Cost:** General Purpose SSD (`gp3`) costs approximately $\$0.08$ per GB per month. Storing hundreds of terabytes of historical backups on EBS is prohibitively expensive.

### 2.2 What is Amazon S3 (Simple Storage Service)?

**Amazon Simple Storage Service (Amazon S3)** is AWS's flagship object storage service, launched in 2006. S3 operates not as a mounted block device or a hierarchical OS folder, but as a globally reachable, web-scale key-value store designed for $99.999999999\%$ (11 9's) durability.

You do not format S3 with `ext4` or `NTFS`. You do not mount it via an operating system driver (except with specialized tools like `s3fs`, which are antipatterns in production). Instead, you interact with S3 purely over **HTTP/HTTPS REST APIs** using standard methods:

- `GET` to read an object
- `PUT` to create or overwrite an object
- `DELETE` to remove an object
- `HEAD` to inspect object metadata without fetching bytes

### 2.3 Core S3 Concepts: Buckets, Objects, Keys, and Metadata

```mermaid
flowchart TD
    subgraph Bucket["S3 Bucket: bongodev-production-assets (Global Namespace)"]
        direction TB
        subgraph Obj1["Object: assets/images/logo.png"]
            K1["Key: 'assets/images/logo.png'"]
            V1["Value: Binary Image Data (Bytes)"]
            M1["Metadata: Content-Type: image/png, Size: 1.2MB"]
            ID1["Version ID: v1_a8f9c2d1"]
        end

        subgraph Obj2["Object: backups/2026-10-23/users.json"]
            K2["Key: 'backups/2026-10-23/users.json'"]
            V2["Value: JSON Text Stream"]
            M2["Metadata: Content-Type: application/json"]
            ID2["Version ID: null (Versioning Off)"]
        end
    end
```

- **Bucket:** The root container for data stored in Amazon S3. Think of a bucket as an isolated top-level tenant. Every AWS account can own multiple buckets, but the bucket name resides in a single, global AWS namespace.
- **Object:** The fundamental entity stored in S3. An object consists of object data (the file payload, from 0 bytes up to 5 TB) and metadata.
- **Key:** The unique identifier for an object within a bucket. Although the AWS Console renders slashes (`/`) as graphical folders, S3 is a flat key-value store. The key `images/users/avatar.png` is simply a string of characters.
- **Metadata:** A set of name-value pairs that describe the object. This includes system metadata (e.g., `Content-Length`, `Last-Modified`, `Content-Type`) and user-defined custom metadata.
- **S3 URI & URL Formats:**
  - **S3 Protocol URI:** `s3://bucket-name/path/to/file.txt` (used by AWS CLI, Boto3, and Lambda)
  - **Virtual-Hosted-Style Web URL:** `https://bucket-name.s3.region.amazonaws.com/path/to/file.txt`
  - **Path-Style Web URL (Legacy):** `https://s3.region.amazonaws.com/bucket-name/path/to/file.txt`

### 2.4 Bucket Naming Rules & Global DNS Uniqueness

Because S3 bucket names resolve directly to public DNS hostnames (e.g., `https://my-bucket.s3.us-west-2.amazonaws.com`), S3 bucket names must conform to strict DNS conventions:

| Rule                    | Requirement                                                | Invalid Example                        | Valid Example                 |
| :---------------------- | :--------------------------------------------------------- | :------------------------------------- | :---------------------------- |
| **Global Uniqueness**   | Must be unique across all AWS accounts worldwide           | `demo-bucket` (already taken)          | `bongodev-aws-class03-ns2026` |
| **Character Length**    | Between 3 and 63 characters long                           | `ab` (too short), `a...64chars`        | `bongodev-demo-bucket-01`     |
| **Allowed Characters**  | Lowercase letters, numbers, and hyphens (`-`)              | `MyBucket_Name` (no uppercase, no `_`) | `my-bucket-prod-01`           |
| **Start / End Rule**    | Must start and end with a lowercase letter or number       | `-bucket-name-`, `.bucket.`            | `bucket-name-01`              |
| **No IP Formatting**    | Must not be formatted as an IP address                     | `192.168.1.1`                          | `server-192-168-1-1`          |
| **No Adjacent Periods** | Cannot contain two adjacent periods or dashes next to dots | `my..bucket`, `my.-bucket`             | `my-bucket.prod`              |

> [!WARNING]
> If someone in Frankfurt, Tokyo, or New York has already created a bucket named `cloud-backup-2026`, you cannot create a bucket with that name in Oregon (`us-west-2`) or Ireland (`eu-west-1`). The AWS API will immediately reject the request with `BucketAlreadyExistsException`. Always prefix your bucket names with your company, project, or student ID.

### 2.5 The 11 9's Durability ($99.999999999\%$) vs Availability ($99.99\%$)

Learners frequently confuse **Durability** with **Availability**. A 10-year cloud architect knows the distinction by heart:

```mermaid
flowchart TD
    subgraph Durability["Durability: 99.999999999% (11 9's)"]
        D1["Will my data survive catastrophic hardware destruction?"]
        D2["Data is automatically copied across >= 3 physical AZs"]
        D3["If you store 10,000,000 objects in S3, on average you will lose 1 object every 10,000 years!"]
    end

    subgraph Availability["Availability: 99.99% (S3 Standard SLA)"]
        A1["Can I connect to S3 and retrieve my data right this second?"]
        A2["Governed by API uptime, network routing, and load balancing"]
        A3["99.99% uptime allows for approximately 52.6 minutes of downtime per year"]
    end
```

- **Durability ($99.999999999\%$):** Represents data retention integrity. AWS automatically writes every S3 object across multiple independent storage devices located in at least three physically separated Availability Zones before returning an `HTTP 200 OK`. Even if an entire data center is flooded or destroyed by a catastrophic event, your file remains intact.
- **Availability ($99.99\%$):** Represents system responsiveness. It defines the probability that the S3 HTTP API endpoint will successfully respond to incoming `GET` and `PUT` requests at any given millisecond.

### 2.6 Static Website Hosting on S3

S3 has a native web server built into every bucket. If your application consists purely of static assets—HTML, CSS, JavaScript (React, Vue, Angular SPA builds), images, and fonts—you **do not need an EC2 server or Nginx**.

```mermaid
sequenceDiagram
    autonumber
    actor User as Client Web Browser
    participant S3 as Amazon S3 Website Endpoint<br/>(bucket-name.s3-website-region.amazonaws.com)
    participant Bucket as S3 Storage Engine

    User->>S3: GET /index.html
    S3->>Bucket: Fetch object 'index.html'
    Bucket-->>S3: Stream bytes + Content-Type: text/html
    S3-->>User: HTTP 200 OK (Renders Webpage)

    User->>S3: GET /unknown-path
    S3->>Bucket: Object not found
    Bucket-->>S3: Trigger Error Document routing
    S3-->>User: HTTP 200/404 (Returns error.html or React index.html route)
```

**Configuration steps demonstrated in class:**

1. Enable **Static website hosting** in the Bucket Properties.
2. Specify the **Index document** (e.g., `index.html`) and optional **Error document** (e.g., `error.html`).
3. S3 generates a dedicated website endpoint: `http://<bucket-name>.s3-website-<region>.amazonaws.com`.
4. (Optional) Disable Block Public Access and attach a Bucket Policy granting `s3:GetObject` to public traffic, or restrict access via Amazon CloudFront using Origin Access Control (OAC).

---

## 3. Amazon S3 Storage Classes & Cost Optimization

Storage cost is one of the largest line items in an enterprise cloud budget. As DevOps engineers, our job is not just to make systems work, but to make them **financially sustainable**. S3 offers specialized storage tiers designed for different data access patterns.

### 3.1 The Cost Matrix: From \$23/TB to \$1/TB

In class, the instructor highlighted standard pricing benchmarks that every cloud engineer must internalize:

```mermaid
flowchart LR
    Standard["S3 Standard<br/>~$23 - $25 / TB / month<br/>Retrieval: Instant (ms)"] --> IT["S3 Intelligent-Tiering<br/>Auto-moves data based on access<br/>Retrieval: Instant (ms)"]
    IT --> IA["S3 Standard-IA<br/>~$12.50 / TB / month<br/>Retrieval: Instant (ms) + Retrieval Fee"]
    IA --> G_Instant["Glacier Instant Retrieval<br/>~$4.00 / TB / month<br/>Retrieval: Instant (ms)"]
    G_Instant --> G_Flexible["Glacier Flexible Retrieval<br/>~$3.50 / TB / month<br/>Retrieval: 1 min - 5 hrs"]
    G_Flexible --> G_Deep["Glacier Deep Archive<br/>~$0.99 - $1.00 / TB / month<br/>Retrieval: 12 - 48 hrs"]
```

| Storage Class               | Durability    | Availability | Retrieval Latency | Storage Cost (per TB/mo)          | Retrieval Fee | Typical Use Case                                           |
| :-------------------------- | :------------ | :----------- | :---------------- | :-------------------------------- | :------------ | :--------------------------------------------------------- |
| **S3 Standard**             | 11 9's        | 99.99%       | Milliseconds      | ~\$23.00                          | None          | Active web assets, daily operations, streaming media       |
| **S3 Intelligent-Tiering**  | 11 9's        | 99.9%        | Milliseconds      | Varies (auto) + small monitor fee | None          | Data with unknown or unpredictable access patterns         |
| **S3 Standard-IA**          | 11 9's        | 99.9%        | Milliseconds      | ~\$12.50                          | Per GB fee    | Disaster recovery backups, monthly audit reports           |
| **S3 One Zone-IA**          | 11 9's (1 AZ) | 99.5%        | Milliseconds      | ~\$10.00                          | Per GB fee    | Secondary reproducible copies, non-critical caches         |
| **S3 Glacier Instant**      | 11 9's        | 99.9%        | Milliseconds      | ~\$4.00                           | Per GB fee    | Archives accessed once a quarter that need instant viewing |
| **S3 Glacier Flexible**     | 11 9's        | 99.9%        | 1 min – 5 hours   | ~\$3.50                           | Per GB fee    | Historical archives, tape replacement, annual audits       |
| **S3 Glacier Deep Archive** | 11 9's        | 99.9%        | 12 – 48 hours     | ~\$0.99 – \$1.00                  | Per GB fee    | 7-10 year compliance records, legal discovery records      |

### 3.2 S3 Standard

- **Default tier:** Any file uploaded without an explicit storage class flag lands in S3 Standard.
- **Attributes:** Low latency, high throughput, zero retrieval fees, highest storage cost per gigabyte.
- **When to use:** Files accessed daily or weekly by end users, application servers, or real-time pipelines.

### 3.3 S3 Intelligent-Tiering

- **How it works:** AWS monitors object access patterns. If an object is not accessed for 30 consecutive days, AWS automatically shifts it to the _Infrequent Access tier_ (saving ~40%). After 90 days of zero access, it shifts to the _Archive Instant Access tier_ (saving ~68%). If a user suddenly requests the object, it immediately transitions back to the Frequent Access tier with **zero retrieval fees and zero performance impact**.
- **When to use:** Ideal when data access patterns are unknown or erratic, eliminating manual lifecycle management.

### 3.4 S3 Standard-Infrequent Access (IA) & One Zone-IA

- **Standard-IA:** Multi-AZ durability with half the storage cost of S3 Standard. However, AWS charges an egress/retrieval fee per gigabyte retrieved. If you read from Standard-IA daily, retrieval charges will exceed the storage savings.
- **One Zone-IA:** Data is stored in only **one** Availability Zone. If that physical AZ is destroyed, data is permanently lost. Used strictly for reproducible data, such as thumbnail caches generated from master images.

### 3.5 S3 Glacier Instant Retrieval vs Flexible Retrieval vs Deep Archive

As the instructor vividly illustrated in class with the metaphor of a hibernating polar bear:

> _"In cold countries, bears go into hibernation for 6 to 9 months. They sleep silently. S3 Glacier is the digital hibernation chamber of AWS. You store historical archives—such as national records, bank transactions from 1995, or CCTV footage—that you rarely touch. If you keep them in S3 Standard, you are burning \$25 per TB every month. In Glacier Deep Archive, you pay only \$1 per TB!"_

1. **Glacier Instant Retrieval:** Designed for medical imaging or news archives where files are accessed once or twice a year, but when requested, a doctor or editor cannot wait hours. Data arrives in milliseconds.
2. **Glacier Flexible Retrieval (formerly S3 Glacier):** Offers three retrieval tiers:
   - _Expedited:_ 1 to 5 minutes (higher fee)
   - _Standard:_ 3 to 5 hours
   - _Bulk:_ 5 to 12 hours (free/cheapest)
3. **Glacier Deep Archive:** The cheapest cloud storage on earth. Files are written to high-density magnetic tape robotics inside AWS facilities. Retrieving an object takes between **12 and 48 hours**.

### 3.6 S3 Lifecycle Management Transitions

Instead of manually running scripts to move old files to Glacier, AWS S3 allows you to define declarative JSON lifecycle rules that automate data aging.

```mermaid
flowchart LR
    Day0["Day 0: File Created<br/>S3 Standard ($23/TB)"] -->|After 30 Days| Day30["Day 30: Transition to<br/>S3 Standard-IA ($12.50/TB)"]
    Day30 -->|After 90 Days| Day90["Day 90: Transition to<br/>Glacier Flexible ($3.50/TB)"]
    Day90 -->|After 365 Days| Day365["Day 365: Transition to<br/>Glacier Deep Archive ($1/TB)"]
    Day365 -->|After 2555 Days (7 Years)| Expire["Day 2555: Permanent Deletion<br/>(Compliance lifecycle complete)"]
```

---

## 4. S3 Advanced Operations & Security Architecture

### 4.1 Block Public Access (BPA): The Default Guardrail

Historically, misconfigured S3 buckets caused some of the highest-profile corporate data leaks in the tech industry. In response, AWS introduced **S3 Block Public Access (BPA)**, which is enabled by default on all newly created buckets.

BPA acts as a centralized "circuit breaker" that overrides all bucket policies and Object ACLs:

1. `BlockPublicAcls`: Blocks new public ACLs from being applied.
2. `IgnorePublicAcls`: Ignores all existing public ACLs on objects.
3. `BlockPublicPolicy`: Rejects any bucket policy that grants open public access.
4. `RestrictPublicBuckets`: Restricts access to AWS service principals and authorized users only.

```mermaid
flowchart TD
    Req["Incoming Web Request (Internet Client)"] --> BPA{"Is Block Public Access Enabled?"}
    BPA -- Yes --> Drop["HTTP 403 Forbidden (Blocked by Global Guardrail)"]
    BPA -- No --> BP{"Does Bucket Policy Allow Public Access?"}
    BP -- Yes --> Allow["HTTP 200 OK (Object Delivered)"]
    BP -- No --> Drop
```

### 4.2 S3 Bucket Versioning: Preventing Accidental Overwrites & Deletions

By default, S3 bucket versioning is **Disabled**. If you upload `logo.png` and then upload a new file also named `logo.png`, the original object is permanently overwritten.

When **Versioning** is enabled:

- S3 assigns a unique alphanumeric `Version ID` (e.g., `3/L4bqtJlKG5VTBEwayMoGN`) to every upload.
- If you upload `logo.png` twice, both versions are stored. The newest version becomes the **Current Version**, and the older becomes the **Noncurrent Version**.
- If a user deletes an object, S3 does not erase the bytes; it inserts a **Delete Marker**. If you delete the delete marker, the file instantly reappears.
- To permanently delete an object, you must explicitly specify the `Version ID` in your `DELETE` API call.

```mermaid
flowchart TD
    subgraph V_Off["Versioning Disabled"]
        U1["Upload logo.png (v1)"] --> O1["logo.png on S3"]
        U2["Upload logo.png (v2)"] --> Overwrite["logo.png Overwritten! (v1 is permanently gone)"]
    end

    subgraph V_On["Versioning Enabled"]
        U3["Upload logo.png (v1)"] --> S3_V1["logo.png (Version ID: a111)"]
        U4["Upload logo.png (v2)"] --> S3_V2["logo.png (Version ID: b222 - Current)"]
        S3_V2 -.-> S3_V1
        Del["Delete logo.png"] --> DM["Delete Marker Placed on Top"]
        DM -.-> S3_V2
        DM -.-> S3_V1
    end
```

### 4.3 S3 Multipart Upload: How AWS Ingests Large Files

In class, a student asked: _"What happens if I need to upload a 50 GB database dump or a 2 TB video asset? Does S3 upload it in one single connection?"_

The instructor explained **S3 Multipart Upload**:

1. Single `PUT` uploads can only handle files up to **5 GB**.
2. For any file larger than **100 MB** (and mandatory for files $> 5\text{ GB}$ up to $5\text{ TB}$), AWS best practice mandates **Multipart Upload**.
3. **The Workflow:**
   - The client program breaks the large file into smaller chunks (parts), typically between 5 MB and 500 MB each.
   - The client uploads multiple parts in parallel across separate HTTP connections.
   - If part 3 fails due to a network timeout, only part 3 is retried—the other 99 parts do not need to restart.
   - Once all parts reach S3, the client sends a `CompleteMultipartUpload` API call. S3 verifies cryptographic checksums (`ETag` / MD5), stitches the chunks together, and presents the object as a single unified file.

```mermaid
sequenceDiagram
    autonumber
    actor Client as Python Boto3 Script
    participant S3 as Amazon S3 Service Engine

    Client->>S3: InitiateMultipartUpload (Filename: bigdata.tar.gz)
    S3-->>Client: Return UploadId: 'XYZ-987-UPLOAD'

    par Parallel Uploads
        Client->>S3: Upload Part 1 (Bytes 0-100MB) [UploadId]
        S3-->>Client: Part 1 ETag: 'c4ca4238a0b9'
    and
        Client->>S3: Upload Part 2 (Bytes 100-200MB) [UploadId]
        S3-->>Client: Part 2 ETag: 'c81e728d9d4c'
    and
        Client->>S3: Upload Part 3 (Bytes 200-300MB) [UploadId]
        S3-->>Client: Part 3 ETag: 'eccbc87e4b5c'
    end

    Client->>S3: CompleteMultipartUpload (UploadId, List of Part ETags)
    Note over S3: Checksum reassembly & stitching into single 300MB file
    S3-->>Client: HTTP 200 OK (Object Ready)
```

### 4.4 Server-Side Encryption: SSE-S3 vs SSE-KMS

S3 automatically encrypts all new objects at rest using AES-256 bit encryption:

- **SSE-S3 (`AES256`):** Encryption keys are fully managed by Amazon S3. Zero cost, zero configuration.
- **SSE-KMS (`aws:kms`):** Uses keys managed in AWS Key Management Service (KMS). Allows audit logging of every single key decrypt call in AWS CloudTrail and enables role-based key separation.
- **SSE-C:** Customer-provided keys where the client manages the keys and sends them along with each HTTP header.

---

## 5. Programmatic Cloud Automation with Python & Boto3

### 5.1 Why Real DevOps Engineers Automate with Python

In class, the instructor addressed a very common student query:

> _"Why do I need to learn Python if I already know Bash scripting? Why can't I just run `aws s3 cp` in the terminal?"_

The instructor's response:

- **Bash & AWS CLI** are fantastic for ad-hoc one-liners, initial server setup, or simple terminal tasks.
- **Python with Boto3** is indispensable when you need complex business logic: error handling with `try/except`, reading JSON payloads from third-party APIs, querying database rows, running mathematical checks, parsing nested dictionaries, and integrating across multiple cloud services in an automated pipeline.
- If you have to ingest 50,000 files across deeply nested directories, filter by extension, validate checksums, and update a database record upon success, a Bash script becomes brittle and unmaintainable. A Python script handles it with structured elegance.

### 5.2 Installing and Setting Up Boto3

**Boto3** is the official AWS Software Development Kit (SDK) for Python. It allows Python developers to write software that creates, configures, and manages AWS services.

```bash
# Upgrade pip to ensure wheel compatibility
python -m pip install --upgrade pip

# Install boto3 inside your virtual environment or global system
pip install boto3
```

### 5.3 Authentication Resolution: How Boto3 Finds `~/.aws/credentials`

When you write `import boto3` and instantiate a client, you never hardcode your AWS Access Key ID or Secret Access Key in your source code. Boto3 automatically searches for credentials across standard locations in a strict order of precedence:

```mermaid
flowchart TD
    Start["boto3.client('s3') executed"] --> C1{"Explicit parameters passed?<br/>(aws_access_key_id='...')"}
    C1 -- Yes --> Auth1["Use Explicit Credentials (Antipattern!)"]
    C1 -- No --> C2{"Environment Variables set?<br/>(AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY)"}
    C2 -- Yes --> Auth2["Use Environment Variables"]
    C2 -- No --> C3{"Shared Credentials File exists?<br/>(~/.aws/credentials generated by 'aws configure')"}
    C3 -- Yes --> Auth3["Use ~/.aws/credentials (Local Dev Machine)"]
    C3 -- No --> C4{"Running on AWS EC2 or Lambda?<br/>(IAM Instance Profile / Execution Role)"}
    C4 -- Yes --> Auth4["Use Temporary STS Metadata Tokens (Production Best Practice)"]
    C4 -- No --> Fail["botocore.exceptions.NoCredentialsError: Unable to locate credentials"]
```

In Class 02, we executed `aws configure`. That command created two local files on our workstation:

1. `~/.aws/credentials`:
   ```ini
   [default]
   aws_access_key_id = AKIAIOSFODNN7EXAMPLE
   aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   ```
2. `~/.aws/config`:
   ```ini
   [default]
   region = us-west-2
   output = json
   ```

Because these files exist, Boto3 automatically discovers them without any manual configuration.

### 5.4 Boto3 Client vs Resource API Paradigms

Boto3 provides two distinct abstraction layers for interacting with AWS services:

```mermaid
classDiagram
    class Boto3Client {
        +Low-level 1:1 mapping to AWS HTTP REST APIs
        +Always returns raw Python dictionaries
        +Supports 100% of AWS services and features
        +Syntax: client.upload_file(Filename, Bucket, Key)
    }

    class Boto3Resource {
        +High-level object-oriented abstraction
        +Returns Python objects with identifier attributes and sub-resources
        +Does NOT support all services (legacy / partial coverage)
        +Syntax: s3.Bucket('my-bucket').upload_file(...)
    }
```

1. **Client (`boto3.client('s3')`):** Low-level service connection. Provides complete, direct access to the underlying AWS REST API. All responses are returned as raw Python dictionaries containing HTTP status codes and headers.
2. **Resource (`boto3.resource('dynamodb')`):** High-level, object-oriented abstraction. It wraps API calls into Python objects (e.g., `Table.put_item()`).

### 5.5 Automated Batch File Ingestion Script Walkthrough

Here is the exact Python script demonstrated in class to upload local digital assets (`assets/` directory containing `hello.txt` and `my_image.png`) directly into an Amazon S3 bucket.

```python
"""
Script: s3_upload.py
Purpose: Upload all files from a local directory to an Amazon S3 bucket using Boto3.
Instructor Demo: Cloud & AWS Class 03
"""

from pathlib import Path
import sys
import boto3
from botocore.exceptions import ClientError, NoCredentialsError

# 1. Define configuration constants
BUCKET_NAME = "bongodev-demo-bucket-ns2026"
LOCAL_FOLDER_PATH = Path("assets")


def upload_assets_to_s3(folder_path: Path, bucket_name: str) -> None:
    """Iterates through a local folder and uploads each file to the target S3 bucket."""
    # Ensure local directory exists
    if not folder_path.exists() or not folder_path.is_dir():
        print(
            f"Error: Target directory '{folder_path}' does not exist.",
            file=sys.stderr,
        )
        return

    # 2. Instantiate the Boto3 S3 low-level client
    # Boto3 automatically reads credentials from ~/.aws/credentials
    s3_client = boto3.client("s3")

    print(f"Connecting to AWS S3... Target Bucket: [{bucket_name}]")

    # 3. Iterate through directory items
    upload_count = 0
    for file_path in folder_path.iterdir():
        # Exclude sub-directories for single-level upload
        if file_path.is_file():
            object_key = file_path.name  # The name of the file in S3

            try:
                print(f"Uploading '{file_path}' as key '{object_key}'...")

                # 4. Execute the upload_file API call
                # Signature: upload_file(Filename, Bucket, Key)
                s3_client.upload_file(
                    Filename=str(file_path),
                    Bucket=bucket_name,
                    Key=object_key,
                )
                print(f"--> Successfully uploaded: {object_key}")
                upload_count += 1

            except ClientError as e:
                error_code = e.response["Error"]["Code"]
                if error_code == "NoSuchBucket":
                    print(
                        f"FATAL: Bucket '{bucket_name}' does not exist! Please create it first.",
                        file=sys.stderr,
                    )
                else:
                    print(
                        f"AWS ClientError during '{file_path.name}': {e}",
                        file=sys.stderr,
                    )
            except NoCredentialsError:
                print(
                    "FATAL: No AWS credentials found. Please run 'aws configure'.",
                    file=sys.stderr,
                )
                return

    print(
        f"\nAll operations completed. Successfully transferred {upload_count} assets."
    )


if __name__ == "__main__":
    upload_assets_to_s3(LOCAL_FOLDER_PATH, BUCKET_NAME)
```

### 5.6 Handling Nested Directories with Recursive Traversal

In class, a student asked: _"What if my `assets` folder contains subfolders, and those subfolders have more subfolders? How do we preserve the folder structure in S3?"_

The instructor explained the concept of **recursion**. In Python, you use `os.walk()` or `Path.rglob('*')` to traverse arbitrarily nested folder hierarchies and construct the S3 Object Key using relative paths:

```python
"""
Recursive S3 Upload Snippet
Preserves folder hierarchy by constructing S3 keys from relative paths.
"""

from pathlib import Path
import boto3

s3_client = boto3.client("s3")
root_dir = Path("my_assets")
bucket_name = "bongodev-demo-bucket-ns2026"

for item in root_dir.rglob("*"):
    if item.is_file():
        # Construct relative key: e.g. "subfolder/nested/image.png"
        relative_key = str(item.relative_to(root_dir)).replace("\\", "/")
        print(f"Uploading: {item} -> s3://{bucket_name}/{relative_key}")
        s3_client.upload_file(
            Filename=str(item), Bucket=bucket_name, Key=relative_key
        )
```

---

## 6. Serverless Compute: AWS Lambda Deep Dive

### 6.1 The Serverless Mental Model: Function-as-a-Service (FaaS)

Up to this point in the course, executing code required a server:

1. Provision an EC2 virtual machine (Class 01).
2. Install the operating system and configure packages (`apt update && apt install python3`).
3. Maintain security patches and configure firewall rules.
4. Keep the server powered on 24/7, paying for idle CPU cycles even at 3:00 AM when zero traffic arrives.

```mermaid
flowchart TD
    subgraph Traditional["Traditional EC2 Server Model"]
        VM["Virtual Machine (Ubuntu EC2)"]
        OS["OS Kernel & Patching"]
        Runtime["Python Runtime Maintenance"]
        Cost1["Billing: 24/7 per second, even when idle"]
        Scaling1["Scaling: Slow (minutes to boot new EC2 AMI)"]
        VM --- OS --- Runtime --- Cost1 --- Scaling1
    end

    subgraph Serverless["Serverless FaaS Model (AWS Lambda)"]
        FaaS["Function as a Service"]
        NoOS["Zero OS Management (AWS handles host & patches)"]
        AutoR["Managed Runtime (Python 3.12 / Node / Java)"]
        Cost2["Billing: Exact duration in milliseconds of execution"]
        Scaling2["Scaling: Instant (0 to 1,000s of executions in seconds)"]
        FaaS --- NoOS --- AutoR --- Cost2 --- Scaling2
    end
```

**AWS Lambda** represents the **Serverless Function-as-a-Service (FaaS)** paradigm:

- **No server provisioning:** You never see or manage an EC2 instance.
- **Pay-per-use billing:** If your function runs for 61 milliseconds, you pay only for 61 milliseconds of compute. If nobody calls your function for a week, your bill is **\$0.00**.
- **Automatic elastic scaling:** If 1 request arrives, 1 container runs. If 10,000 requests arrive simultaneously, AWS instantly provisions thousands of isolated execution sandboxes in parallel.

### 6.2 When to Use Lambda vs EC2 (The Cannon vs Flyswatter Principle)

In class, the instructor used a memorable analogy:

> _"If a mosquito flies into your room, do you bring out an artillery cannon to shoot it? No! An EC2 instance is a cannon. If you have a small script that fetches user data from an API once every 10 minutes and saves it to a database, spinning up an EC2 machine is total overkill. You use AWS Lambda—it executes for 100 milliseconds and instantly vanishes."_

| Feature / Dimension        | Amazon EC2                                                   | AWS Lambda                                                       |
| :------------------------- | :----------------------------------------------------------- | :--------------------------------------------------------------- |
| **Architectural Model**    | Infrastructure as a Service (IaaS)                           | Function as a Service (FaaS) / Serverless                        |
| **Server Management**      | Customer manages OS, security patches, systemd, web server   | Fully managed by AWS; zero OS access                             |
| **Max Execution Duration** | Unlimited (runs continuously for years)                      | **15 minutes (900 seconds)** hard maximum                        |
| **Pricing Model**          | Per-second while instance state is `running`                 | Per-millisecond while code is actively executing                 |
| **Scaling Mechanism**      | Auto Scaling Groups (takes 1–5 minutes to launch AMIs)       | Automatic event-driven concurrency (sub-second scaling)          |
| **Statefulness**           | Stateful (local disk EBS persists)                           | Stateless (ephemeral `/tmp` storage discarded after run)         |
| **Best Used For**          | Monolithic backends, long-running batch jobs, Docker daemons | Microservices, API backends, event processors, automated backups |

### 6.3 Anatomy of a Lambda Function: Handler, Event, and Context

Every Python AWS Lambda function revolves around a primary entry-point function called the **Handler**:

```python
def lambda_handler(event, context):
    # Your business logic executes here
    return {"statusCode": 200, "body": "Success"}
```

```mermaid
flowchart LR
    Trigger["Event Source<br/>(API Gateway, S3 Upload, EventBridge Cron, Manual Test)"] -->|JSON Payload| Event["'event' Parameter<br/>(Dictionary containing request headers, query params, body)"]
    AWS["AWS Lambda Runtime Engine"] -->|Runtime Metadata| Context["'context' Parameter<br/>(Function name, memory limit, remaining execution time)"]
    Event --> Handler["lambda_handler(event, context)"]
    Context --> Handler
    Handler --> Result["Return Value<br/>(JSON response / Status code)"]
```

1. **`event` (Python Dictionary):** Contains the invocation payload. If triggered by an S3 upload, `event` contains the bucket name and uploaded key. If triggered by an API Gateway, it contains HTTP headers and JSON bodies. If triggered by a manual test event, it contains your custom JSON test mock.
2. **`context` (Python Object):** Provides runtime metadata about the execution environment. Methods include:
   - `context.get_remaining_time_in_millis()`: Returns how many milliseconds remain before Lambda enforces a timeout shutdown.
   - `context.function_name`: The name of the running Lambda function.
   - `context.memory_limit_in_mb`: The allocated memory size.

### 6.4 Runtime Configuration: `<filename>.<handler_function>`

A common pitfall demonstrated in class is the **Handler Configuration string** in the Lambda console:

```
Runtime Settings:
Handler: lambda_function.lambda_handler
```

- `lambda_function`: The name of the Python file on disk (**without** the `.py` extension, e.g., `lambda_function.py`).
- `lambda_handler`: The exact name of the Python function inside that file that AWS will invoke.
- If your script is named `app.py` and your function is named `main(event, context)`, your handler string **must** be configured as `app.main`. If these names do not match, Lambda will fail with `Runtime.HandlerNotFound`.

### 6.5 Tuning Execution Timeouts and Memory Allocation

- **Default Timeout:** By default, new Lambda functions have a timeout of **3 seconds**.
  - _The Trap:_ If your function calls an external REST API (like `jsonplaceholder.typicode.com`) or runs a heavy database scan, a slight network hiccup can push execution past 3.01 seconds. Lambda will forcefully terminate the execution with `Task timed out after 3.00 seconds`.
  - _The Fix:_ In class, the instructor demonstrated navigating to **Configuration $\rightarrow$ General Configuration** and increasing the timeout to **10 or 15 seconds**.
- **Memory Allocation:** Can be set between **128 MB and 10,240 MB (10 GB)**.
  - CPU power scales proportionally with memory. When you allocate more memory, AWS automatically gives your function a larger share of a vCPU.
  - In our demo, allocating 128 MB was sufficient; the function consumed only ~54 MB of RAM.

### 6.6 Packaging Code: Zip File Uploads & Dependencies

When a function requires only standard Python libraries (`json`, `urllib.request`, `os`, `sys`), you can edit the code directly inside the Lambda Web Code Editor.

However, when your code requires external third-party dependencies (`requests`, `pydantic`, `pandas`) or multi-file modules:

1. Develop and test your code locally inside VS Code.
2. Package the code and its installed libraries into a `.zip` archive.
3. In the Lambda Console, select **Upload from $\rightarrow$ .zip file**. AWS extracts the archive into the function's root directory.

---

## 7. CloudWatch Observability & Logging for Serverless

### 7.1 Observability Pillars: Metrics, Logs, and Alarms

When you run code on your local laptop, you see standard output directly inside your terminal. If an exception occurs, the traceback prints to your screen.

In a serverless environment like AWS Lambda, there is **no physical terminal** attached to the execution container. If an error occurs, how do you debug it? AWS solves this with **Amazon CloudWatch**.

```mermaid
flowchart TD
    Lambda["AWS Lambda Function Execution"] -->|Standard Output: print() & Tracebacks| CWL["Amazon CloudWatch Logs<br/>(Centralized Log Storage)"]
    Lambda -->|Telemetry: Duration, Invocations, Errors| CWM["Amazon CloudWatch Metrics<br/>(Time-Series Performance Graphs)"]
    CWM -->|Threshold Exceeded: Errors > 5| CWA["CloudWatch Alarm"]
    CWA -->|Trigger Alert| SNS["Amazon SNS (Email / Slack Alert)"]
```

### 7.2 The IAM Execution Role: `AWSLambdaBasicExecutionRole`

By default, an AWS Lambda function has **zero permissions**. It cannot write logs to CloudWatch, it cannot read from S3, and it cannot write to DynamoDB.

To allow Lambda to publish logs, AWS automatically creates an **IAM Execution Role** when you create the function. This role has an AWS-managed policy attached: **`AWSLambdaBasicExecutionRole`**.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:*:*:*"
    }
  ]
}
```

Without this policy, any `print()` statement executed in your Python code is discarded into the void.

### 7.3 CloudWatch Log Groups, Log Streams, and Event Records

CloudWatch organizes log data hierarchically:

```mermaid
flowchart TD
    LG["Log Group: /aws/lambda/fetch-json-placeholder-users"] --> LS1["Log Stream: 2026/10/23/[$LATEST]a8f9c2d104... (Container 1)"]
    LG --> LS2["Log Stream: 2026/10/23/[$LATEST]b3e1d4c209... (Container 2)"]
    LS1 --> E1["Event: START RequestId: 7b8e1a2c-..."]
    LS1 --> E2["Event: User 1: Leanne Graham"]
    LS1 --> E3["Event: Total users fetched: 10"]
    LS1 --> E4["Event: END RequestId: 7b8e1a2c-..."]
    LS1 --> E5["Event: REPORT Duration: 61.24 ms Billed: 62 ms Memory: 54 MB"]
```

- **Log Group:** Represents the collection of logs sharing the same retention and access control. AWS naming convention: `/aws/lambda/<function-name>`.
- **Log Stream:** Represents the sequence of log events emitted by a specific instance of the Lambda container. Every concurrent container gets its own log stream.
- **Log Events:** The individual timestamped log lines generated by your Python `print()` statements and runtime reports.
- **The REPORT Line:** At the end of every execution, CloudWatch outputs an invaluable performance summary:
  ```
  REPORT RequestId: 4d82b7c1-84e1-4569-8bc3-d98c251d1e4e
  Duration: 61.42 ms    Billed Duration: 62 ms    Memory Size: 128 MB    Max Memory Used: 54 MB
  ```

### 7.4 Configuring Log Retention Periods to Avoid Hidden Cloud Costs

> [!CAUTION]
> By default, CloudWatch Log Groups are set to **"Never Expire"**. In a production system processing millions of events per month, your log files will accumulate indefinitely, leading to thousands of dollars in surprise storage charges.

In class, the instructor demonstrated how to navigate to **CloudWatch $\rightarrow$ Log groups**, select the Lambda log group, click **Actions $\rightarrow$ Edit retention setting**, and change the retention period from _Never Expire_ to **1 Day, 7 Days, or 30 Days**. This ensures older logs are automatically purged, keeping storage costs lean.

---

## 8. Managed NoSQL Database: Amazon DynamoDB

### 8.1 Relational (SQL/RDS) vs Document / Key-Value NoSQL

In traditional software architecture, developers rely on **Relational Database Management Systems (RDBMS)** such as MySQL or PostgreSQL. In modern distributed cloud systems, **NoSQL databases** like DynamoDB play a crucial role.

```mermaid
flowchart TD
    subgraph RDBMS["Relational Database (MySQL / PostgreSQL / Amazon RDS)"]
        R1["Strict schema: Tables, Rows, Columns defined upfront"]
        R2["Complex SQL queries, multi-table JOINs, foreign keys"]
        R3["Vertical scaling (bigger EC2 instances); horizontal scaling is complex"]
        R4["Use Case: Banking ledgers, complex enterprise relational models"]
    end

    subgraph NoSQL["NoSQL Document / Key-Value (Amazon DynamoDB)"]
        N1["Flexible schema: Items have attributes in JSON/BSON format"]
        N2["Zero multi-table JOINs; query via Partition Keys & Sort Keys"]
        N3["Horizontal scaling across partitioned storage nodes"]
        N4["Use Case: User sessions, shopping carts, gaming leaderboards, IoT events"]
    end
```

### 8.2 Self-Hosted Databases (EC2) vs Managed Cloud Databases

In class, the instructor highlighted a critical career milestone for DevOps engineers:

- You _can_ install MongoDB or MySQL on an EC2 instance. But that means **you** must configure RAID arrays, manage storage expansion, perform kernel patching, configure replication nodes, and write custom backup cron jobs.
- **Amazon DynamoDB is a 100% Fully Managed Database-as-a-Service (DBaaS):**
  - No servers to provision or patch.
  - High availability built-in (data is automatically replicated synchronously across 3 Availability Zones).
  - Consistent single-digit millisecond latency at any scale.

### 8.3 DynamoDB Core Building Blocks: Tables, Items, and Attributes

```mermaid
flowchart TD
    subgraph Table["DynamoDB Table: jsonplaceholder_users"]
        direction TB

        subgraph Item1["Item 1 (Primary Key: id = 1 [Number])"]
            I1_1["id: 1 (Number) - Partition Key"]
            I1_2["name: 'Leanne Graham' (String)"]
            I1_3["username: 'Bret' (String)"]
            I1_4["email: 'Sincere@april.biz' (String)"]
            I1_5["address: {'city': 'Gwenborough', 'zip': '92998'} (Map)"]
        end

        subgraph Item2["Item 2 (Primary Key: id = 2 [Number])"]
            I2_1["id: 2 (Number) - Partition Key"]
            I2_2["name: 'Ervin Howell' (String)"]
            I2_3["username: 'Antonette' (String)"]
            I2_4["phone: '010-692-6593' (String)"]
            I2_5["custom_tag: 'VIP_CUSTOMER' (String) <- Schemaless flexibility!"]
        end
    end
```

- **Table:** A collection of data items (equivalent to a table in SQL or a collection in MongoDB).
- **Item:** A group of attributes that is uniquely identifiable among all other items (equivalent to a row in SQL or a document in MongoDB).
- **Attributes:** A fundamental data element, something that does not need to be broken down any further (equivalent to a column in SQL).
- **Schemaless Nature:** Other than the Primary Key, attributes do not need to be defined when the table is created. Item 1 can have 5 attributes, while Item 2 can have 20 attributes!

### 8.4 Primary Key Strategies: Partition Key (PK) vs Composite Key (PK + SK)

When creating a DynamoDB table, you must define its Primary Key. DynamoDB uses the Primary Key to physically distribute items across underlying storage partitions:

```mermaid
flowchart LR
    subgraph SimplePK["1. Simple Primary Key (Partition Key Only)"]
        PK1["Partition Key: user_id (String)"]
        Desc1["Items distributed using internal hash function.<br/>No two items can share the same user_id."]
    end

    subgraph CompositePK["2. Composite Primary Key (Partition Key + Sort Key)"]
        PK2["Partition Key: user_id (String)"]
        SK2["Sort Key: timestamp (Number)"]
        Desc2["All items with the same user_id stored together,<br/>sorted in order by timestamp.<br/>Enables range queries: GET orders where date > yesterday."]
    end
```

1. **Partition Key (PK / Hash Key):** A single attribute. DynamoDB passes the PK value through an internal cryptographic hash function to determine the physical partition where the item will be stored.
   - _Example:_ In our class lab, we used `id` (Number) as the simple Partition Key.
2. **Composite Primary Key (Partition Key + Sort Key / Range Key):** Consists of two attributes.
   - All items with the same Partition Key are stored together in physical proximity.
   - Within that partition, items are ordered by the Sort Key.
   - _Example:_ `tenant_id` (PK) and `created_at` (SK).

### 8.5 Capacity Modes: On-Demand vs Provisioned (RCU / WCU)

DynamoDB provides two billing and capacity options:

1. **On-Demand Capacity Mode:**
   - **How it works:** You do not specify how much read and write throughput you expect. DynamoDB instantly accommodates workloads as they ramp up or down.
   - **Billing:** You pay purely per million read/write request units (e.g., ~\$1.25 per million write request units).
   - **Best for:** Unpredictable traffic, new product launches, development and testing environments.
2. **Provisioned Capacity Mode:**
   - **How it works:** You specify the exact number of **Read Capacity Units (RCUs)** and **Write Capacity Units (WCUs)** your application requires.
   - **Billing:** You pay an hourly rate for the provisioned capacity, regardless of whether you consume it.
   - **Best for:** Predictable, steady-state production workloads where provisioned capacity with Auto Scaling provides substantial cost savings.

---

## 9. End-to-End Hands-on Project: The Serverless Data Pipeline

In Class 03, the instructor guided students through an end-to-end, production-grade cloud integration: **fetching live JSON data from an external REST API, parsing it inside AWS Lambda, and persisting structured records into Amazon DynamoDB, while monitoring execution through CloudWatch Logs**.

### 9.1 Project Architecture & Workflow Topology

```mermaid
sequenceDiagram
    autonumber
    actor Dev as DevOps Engineer
    participant Console as Lambda Console (Test Event)
    participant Lambda as AWS Lambda (Python Handler)
    participant API as External REST API<br/>(jsonplaceholder.typicode.com/users)
    participant IAM as AWS IAM Role (Permissions)
    participant DDB as Amazon DynamoDB (Table: jsonplaceholder_users)
    participant CW as Amazon CloudWatch Logs

    Dev->>Console: Click "Test" button (Invokes Event)
    Console->>Lambda: Dispatches execution trigger
    Lambda->>API: HTTP GET /users (urllib.request)
    API-->>Lambda: Returns 10 JSON User Objects
    Lambda->>Lambda: Parse JSON & extract attributes (id, name, email)

    rect rgb(240, 248, 255)
    Note over Lambda,IAM: Security Evaluation
    Lambda->>IAM: Verify permission for action 'dynamodb:PutItem'
    alt IAM Policy Missing (Live Class Bug)
        IAM-->>Lambda: 403 AccessDeniedException
        Lambda->>CW: Log error: AccessDeniedException
    else IAM Policy Attached (Fixed)
        IAM-->>Lambda: Permission Allowed
        Lambda->>DDB: PutItem(Item={id, name, username, email, ...})
        DDB-->>Lambda: 200 OK (Item Stored)
    end
    end

    Lambda->>CW: Stream stdout (print statements & reports)
    Lambda-->>Console: Execution Result: HTTP 200 Success
```

### 9.2 Step 1: DynamoDB Table Provisioning

1. Navigate to **Amazon DynamoDB Console $\rightarrow$ Tables $\rightarrow$ Create table**.
2. **Table name:** `jsonplaceholder_users`
3. **Partition key:** `id` (Select type **Number**).
4. **Table settings:** Select **Default settings** (which defaults to On-Demand capacity in modern regions).
5. Click **Create table** and wait until the status displays **Active**.
6. Copy the **Amazon Resource Name (ARN)** from the General Information card.
   - Format: `arn:aws:dynamodb:us-west-2:123456789012:table/jsonplaceholder_users`

### 9.3 Step 2: Environment Variables for Infrastructure Abstraction

A critical DevOps best practice emphasized by the instructor: **Never hardcode table names, database endpoints, or bucket names directly into your application code.**

If you hardcode `"jsonplaceholder_users"` into your script, you cannot reuse that script across `staging` and `production` environments without modifying the source code.

1. Navigate to **Lambda $\rightarrow$ Configuration $\rightarrow$ Environment variables**.
2. Click **Edit $\rightarrow$ Add environment variable**:
   - **Key:** `TABLE_NAME`
   - **Value:** `jsonplaceholder_users`
3. Click **Save**.
4. In Python, retrieve this variable dynamically:

   ```python
   import os

   TABLE_NAME = os.environ.get("TABLE_NAME")
   ```

### 9.4 Step 3: Developing the Lambda Python Handler

Below is the complete, production-grade Python Lambda script executed in class:

```python
"""
AWS Lambda Function: fetch_and_store_users.py
Purpose: Fetch user data from JSONPlaceholder API and persist records into DynamoDB.
Runtime: Python 3.12 / 3.13
Instructor: Nure Siddiqui (Cloud & AWS Class 03)
"""

import json
import os
import sys
import urllib.error
import urllib.request
import boto3
from botocore.exceptions import ClientError

# 1. Fetch infrastructure configuration from environment variables
TABLE_NAME = os.environ.get("TABLE_NAME")
API_ENDPOINT = "https://jsonplaceholder.typicode.com/users"

# 2. Initialize Boto3 DynamoDB resource outside handler for container reuse
# When Lambda warms up, keeping client initialization in the outer scope
# saves execution milliseconds on subsequent invocations.
dynamodb = boto3.resource("dynamodb")


def lambda_handler(event, context):
    """Main Lambda entry-point function invoked by AWS."""
    print(f"--> Initiating execution. Target Table: [{TABLE_NAME}]")

    # Guard clause: ensure environment configuration is present
    if not TABLE_NAME:
        error_msg = "FATAL: 'TABLE_NAME' environment variable is not defined!"
        print(error_msg, file=sys.stderr)
        return {"statusCode": 500, "body": json.dumps({"error": error_msg})}

    table = dynamodb.Table(TABLE_NAME)

    # 3. Fetch external data over HTTPS using Python standard library
    # Using urllib.request eliminates the need for bundling external 'requests' wheel
    req = urllib.request.Request(
        API_ENDPOINT, headers={"User-Agent": "BongoDev-Lambda-Agent/1.0"}
    )

    try:
        print(f"Fetching user records from: {API_ENDPOINT}")
        with urllib.request.urlopen(req, timeout=10) as response:
            if response.status != 200:
                print(
                    f"API responded with non-200 code: {response.status}",
                    file=sys.stderr,
                )
                return {
                    "statusCode": response.status,
                    "body": json.dumps(
                        {"error": "Failed to fetch upstream API"}
                    ),
                }

            raw_payload = response.read().decode("utf-8")
            users_list = json.loads(raw_payload)

    except urllib.error.URLError as e:
        print(f"Network error while contacting upstream API: {e}")
        return {"statusCode": 502, "body": json.dumps({"error": str(e)})}

    print(
        f"Successfully fetched {len(users_list)} users. Beginning DynamoDB batch insert..."
    )

    # 4. Iterate and insert items into DynamoDB
    inserted_count = 0
    for idx, user in enumerate(users_list):
        # Extract and sanitize user attributes
        user_id = int(user.get("id"))
        user_name = str(user.get("name"))
        user_username = str(user.get("username"))
        user_email = str(user.get("email"))
        user_phone = str(user.get("phone", "N/A"))
        user_website = str(user.get("website", "N/A"))

        # Flatten nested company details
        company_data = user.get("company", {})
        company_name = (
            str(company_data.get("name", "N/A"))
            if isinstance(company_data, dict)
            else "N/A"
        )

        # Construct the DynamoDB Item dictionary
        db_item = {
            "id": user_id,  # Partition Key (Number)
            "name": user_name,
            "username": user_username,
            "email": user_email,
            "phone": user_phone,
            "website": user_website,
            "company_name": company_name,
        }

        try:
            # 5. Execute DynamoDB PutItem API call
            table.put_item(Item=db_item)
            print(f"[{idx+1}/{len(users_list)}] Saved user ID {user_id}: {user_name}")
            inserted_count += 1

        except ClientError as e:
            error_code = e.response["Error"]["Code"]
            print(
                f"ClientError writing user ID {user_id} to DynamoDB [{error_code}]: {e}",
                file=sys.stderr,
            )
            # If access denied, terminate early to avoid spamming 10 permission errors
            if error_code == "AccessDeniedException":
                return {
                    "statusCode": 403,
                    "body": json.dumps(
                        {"error": "IAM Permission Denied for dynamodb:PutItem"}
                    ),
                }

    # 6. Return successful API summary
    success_summary = {
        "message": f"Successfully processed and stored {inserted_count} users in DynamoDB.",
        "table": TABLE_NAME,
        "total_records": inserted_count,
    }
    print(f"--> Execution finished. {json.dumps(success_summary)}")

    return {"statusCode": 200, "body": json.dumps(success_summary)}
```

### 9.5 Step 4: Diagnosing the Live IAM Permission Deficit

When the instructor first deployed and tested the Lambda function, it failed immediately with the following CloudWatch error:

```
ClientError: An error occurred (AccessDeniedException) when calling the PutItem operation:
User: arn:aws:sts::123456789012:assumed-role/fetch_json_placeholder_role/fetch_json_placeholder
is not authorized to perform: dynamodb:PutItem on resource:
arn:aws:dynamodb:us-west-2:123456789012:table/jsonplaceholder_users
```

**Why did this happen?**
As established in Section 7.2, Lambda's auto-generated execution role _only_ contained permissions for CloudWatch Logs (`AWSLambdaBasicExecutionRole`). It possessed zero permissions to touch Amazon DynamoDB.

### 9.6 Step 5: Writing and Attaching the Inline IAM DynamoDB Policy

To resolve the error following the **Principle of Least Privilege (PoLP)**:

1. In the Lambda Console, navigate to **Configuration $\rightarrow$ Permissions**.
2. Click on the **Role name** hyperlink to jump directly to the IAM Console.
3. Click **Add permissions $\rightarrow$ Create inline policy**.
4. Select the **JSON** editor and paste the following least-privilege policy, binding the `PutItem` action strictly to our specific table ARN:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowLambdaPutItemToUsersTable",
      "Effect": "Allow",
      "Action": ["dynamodb:PutItem"],
      "Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/jsonplaceholder_users"
    }
  ]
}
```

5. Click **Next**, name the policy `LambdaWriteToDynamoDBPolicy`, and click **Create policy**.

### 9.7 Step 6: Executing the Test Event & Inspecting DynamoDB Items

1. Return to the Lambda Console and open the **Test** tab.
2. Event name: `TriggerPipelineTest` (the event JSON body can be `{}` because our function fetches from an external HTTP API).
3. Click **Test**.
4. **Execution Result:**
   ```json
   {
     "statusCode": 200,
     "body": "{\"message\": \"Successfully processed and stored 10 users in DynamoDB.\", \"table\": \"jsonplaceholder_users\", \"total_records\": 10}"
   }
   ```
5. Navigate to **Amazon DynamoDB Console $\rightarrow$ Tables $\rightarrow$ jsonplaceholder_users**.
6. Click **Explore table items**.
7. **Verification:** All 10 user items are visible, correctly populated with their Partition Key `id` (1 through 10), email, name, website, and company details!

---

## 10. Disaster Recovery & Database Backups (Homework Assignment)

### 10.1 The 3-2-1 Backup Strategy in the Cloud

In enterprise DevOps, storing data in only one database—even a highly available one like DynamoDB—is insufficient for complete disaster recovery.

- A rogue script, application bug, or malicious actor could accidentally issue batch delete calls.
- The golden rule of enterprise engineering is the **3-2-1 Backup Rule**:
  - Keep **3** copies of your data.
  - On **2** different storage media / services (e.g., DynamoDB database + S3 object storage).
  - With **1** copy stored offsite or in a separate cold archive (e.g., S3 Glacier in another AWS region).

### 10.2 DynamoDB Point-in-Time Recovery (PITR) & On-Demand Backups

DynamoDB provides two native backup features:

1. **On-Demand Backups:** Creates a full snapshot of your table with zero impact on live performance. Retained until you manually delete it.
2. **Point-in-Time Recovery (PITR):** Continuous automated backups. Allows you to restore your table to any single second within the past 35 days.

### 10.3 Homework Assignment: Automated DynamoDB $\rightarrow$ S3 Archival Pipeline

At the conclusion of Class 03, the instructor issued a hands-on architectural assignment:

> _"Now that you have written data from Lambda into DynamoDB, you must complete the circle of disaster recovery. Create a second Lambda function that scans the DynamoDB table, dumps all records into a JSON file, and uploads that backup to an Amazon S3 bucket named `bongodev-db-backups-<your-id>`. This is how real cloud engineers protect enterprise assets."_

```mermaid
flowchart LR
    EventBridge["Amazon EventBridge<br/>(Cron: Every Night at Midnight)"] -->|Trigger| BackupLambda["AWS Lambda<br/>(Backup Handler)"]
    BackupLambda -->|1. dynamodb:Scan| DDB["DynamoDB Table<br/>(jsonplaceholder_users)"]
    DDB -->>|2. Returns all items| BackupLambda
    BackupLambda -->|3. Serializes to JSON| Ephemeral["/tmp/backup-2026-10-23.json"]
    BackupLambda -->|4. s3:PutObject| S3["Amazon S3 Bucket<br/>(bongodev-db-backups)"]
    S3 -->|5. Lifecycle Rule| Glacier["S3 Glacier Deep Archive<br/>($1/TB/month)"]
```

#### Complete Python Implementation for the Backup Lambda:

```python
"""
AWS Lambda Function: export_dynamodb_to_s3.py
Purpose: Homework Assignment - Backup all DynamoDB table items to S3 as a timestamped JSON file.
Runtime: Python 3.12 / 3.13
"""

from datetime import datetime, timezone
from decimal import Decimal
import json
import os
import sys
import boto3
from botocore.exceptions import ClientError

# Read configuration from environment variables
SOURCE_TABLE = os.environ.get("SOURCE_TABLE", "jsonplaceholder_users")
BACKUP_BUCKET = os.environ.get(
    "BACKUP_BUCKET", "bongodev-db-backups-ns2026"
)

dynamodb = boto3.resource("dynamodb")
s3_client = boto3.client("s3")


class DecimalEncoder(json.JSONEncoder):
    """DynamoDB returns numbers as Decimal objects; this serializer converts them to int/float."""

    def default(self, o):
        if isinstance(o, Decimal):
            return int(o) if o % 1 == 0 else float(o)
        return super().default(o)


def lambda_handler(event, context):
    print(f"Beginning backup of Table [{SOURCE_TABLE}] to S3 [{BACKUP_BUCKET}]")

    table = dynamodb.Table(SOURCE_TABLE)

    # 1. Scan the DynamoDB table to retrieve all records
    try:
        response = table.scan()
        items = response.get("Items", [])

        # Handle pagination if table exceeds 1MB scan limit
        while "LastEvaluatedKey" in response:
            response = table.scan(
                ExclusiveStartKey=response["LastEvaluatedKey"]
            )
            items.extend(response.get("Items", []))

        print(f"Scanned total of {len(items)} items from DynamoDB.")

    except ClientError as e:
        print(f"Error scanning DynamoDB table: {e}", file=sys.stderr)
        return {"statusCode": 500, "body": str(e)}

    # 2. Serialize items into formatted JSON
    timestamp = datetime.now(timezone.utc).strftime("%Y-%m-%d_%H-%M-%S")
    backup_filename = f"dynamodb_backups/{SOURCE_TABLE}/backup_{timestamp}.json"
    json_data = json.dumps(items, cls=DecimalEncoder, indent=2)

    # 3. Stream JSON data directly to Amazon S3
    try:
        s3_client.put_object(
            Bucket=BACKUP_BUCKET,
            Key=backup_filename,
            Body=json_data.encode("utf-8"),
            ContentType="application/json",
        )
        print(f"Successfully uploaded backup to s3://{BACKUP_BUCKET}/{backup_filename}")

    except ClientError as e:
        print(f"Error uploading backup to S3: {e}", file=sys.stderr)
        return {"statusCode": 500, "body": str(e)}

    return {
        "statusCode": 200,
        "body": json.dumps(
            {
                "status": "SUCCESS",
                "table": SOURCE_TABLE,
                "s3_path": f"s3://{BACKUP_BUCKET}/{backup_filename}",
                "records_backed_up": len(items),
            }
        ),
    }
```

### 10.4 Multi-Service IAM Policy Architecture for the Backup Lambda

To execute this backup pipeline, the Lambda execution role requires a unified policy granting read permissions on DynamoDB and write permissions on S3:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowDynamoDBScan",
      "Effect": "Allow",
      "Action": ["dynamodb:Scan"],
      "Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/jsonplaceholder_users"
    },
    {
      "Sid": "AllowS3PutBackupObject",
      "Effect": "Allow",
      "Action": ["s3:PutObject"],
      "Resource": "arn:aws:s3:::bongodev-db-backups-ns2026/dynamodb_backups/*"
    }
  ]
}
```

---

## 11. Real-World Gotchas, Traps & Debugging

During the live session, several real-world debugging moments occurred. These are invaluable for building practical DevOps problem-solving intuition:

### Gotcha 1: The S3 "NoSuchBucket" Crash

- **Symptom:** Running the Python script resulted in `ClientError: (NoSuchBucket) The specified bucket does not exist`.
- **Cause:** In the Python script, the bucket name was set to `nure-s3-bucket-2`, but the instructor had only created `nure-s3-bucket-1` in the AWS console.
- **Fix:** Either create the bucket in the console first, or add programmatic bucket existence checks using `s3_client.head_bucket(Bucket=bucket_name)`.

### Gotcha 2: The 3-Second Lambda Timeout Wall

- **Symptom:** Lambda test executions fail with `Task timed out after 3.00 seconds`.
- **Cause:** Public internet API calls or multiple database round-trips can take 2–4 seconds depending on DNS resolution and network latency.
- **Fix:** Never leave production Lambda functions at the default 3-second timeout. Increase the timeout to at least 15 or 30 seconds.

### Gotcha 3: DynamoDB Number vs String Data Type Mismatches

- **Symptom:** DynamoDB PutItem rejects records or items sort unexpectedly.
- **Cause:** If the Primary Key `id` is defined as a **Number** in DynamoDB, passing `"1"` (a Python string) will throw a `SerializationException`.
- **Fix:** Explicitly cast values using `int(user["id"])` before constructing the item dictionary.

### Gotcha 4: Boto3 DynamoDB Decimal Serialization Failure

- **Symptom:** Attempting to run `json.dumps()` on items scanned from DynamoDB throws `TypeError: Object of type Decimal is not JSON serializable`.
- **Cause:** The Python Boto3 library deserializes DynamoDB numbers into Python `Decimal` objects (from the standard `decimal` module) to prevent floating-point precision loss. Standard `json.dumps` does not know how to serialize `Decimal`.
- **Fix:** Use a custom JSON encoder (e.g., `DecimalEncoder` shown in Section 10.3) that checks `isinstance(o, Decimal)`.

### Gotcha 5: Regional Mismatch Silently Breaking Connections

- **Symptom:** Lambda function in `us-west-2` (Oregon) cannot communicate with a DynamoDB table created in `us-east-1` (N. Virginia).
- **Cause:** By default, Boto3 connects to services in the region specified by the Lambda function's execution environment. If the table is in another region, Boto3 throws `ResourceNotFoundException`.
- **Fix:** Keep your Lambda functions, S3 buckets, and DynamoDB tables in the **same region** (`us-west-2`), or explicitly specify the region in Boto3: `boto3.resource('dynamodb', region_name='us-west-2')`.

---

## 12. Questions Asked in Class (Q&A)

### Q1: Can we connect to S3 using SFTP?

**Answer:** Yes. AWS offers a dedicated managed service called **AWS Transfer Family**. It provides fully managed SFTP, FTPS, and FTP endpoints directly in front of Amazon S3 buckets. External clients connect via standard SFTP clients (e.g., FileZilla, Cyberduck) using SSH keys, and files land directly as S3 objects.

### Q2: Is Drag-and-Drop recommended for uploading files in the S3 Console?

**Answer:** Drag-and-drop in the AWS Management Console is fine for one-off manual testing with a handful of files. However, it is **strongly discouraged for production workflows**. The web browser can crash, connections cannot be reliably resumed, and batch scale is limited. Production ingestions must always use the AWS CLI (`aws s3 sync`), Python Boto3 scripts, or S3 Multipart Upload APIs.

### Q3: How difficult is it to migrate data from MongoDB to DynamoDB?

**Answer:** While both MongoDB and DynamoDB are NoSQL document databases that store JSON/BSON records, **they are not drop-in replacements for each other**.

- In MongoDB, you can query across any arbitrary field using rich query operators (`$lookup`, `$regex`, complex aggregations).
- In DynamoDB, you must structure your queries around the **Partition Key and Sort Key**. Querying non-key attributes requires expensive table scans or Secondary Indexes (GSI/LSI).
- Moving data from MongoDB to DynamoDB requires rethinking your access patterns, designing proper partition keys, flattening nested relationships where appropriate, and using tools like **AWS Database Migration Service (AWS DMS)** or custom Python migration scripts.

### Q4: Can we run our entire full-stack application inside AWS Lambda?

**Answer:** While frameworks like Serverless Framework, AWS SAM, and tools like Mangum (for FastAPI) allow running full web apps on Lambda, **it is not always the best architectural choice**. Monolithic apps with long-lived WebSocket connections, stateful memory caches, or requests running longer than 15 minutes cannot run on Lambda. A hybrid microservices approach—running the containerized frontend/backend on EC2/ECS and offloading event processing, image resizing, and asynchronous jobs to Lambda—is the industry standard.

---

## 13. Common Misconceptions, Corrected

### Misconception 1: "S3 is just a cloud folder on a virtual hard drive."

**Correction:** S3 is not a POSIX-compliant file system. There are no actual physical folders, no inodes, and no block allocations. S3 is a globally distributed, key-value object store accessed over HTTP REST APIs. Slashes (`/`) in keys are merely string prefixes that the AWS web console graphically renders as folder icons for human convenience.

### Misconception 2: "Serverless means there are no servers involved."

**Correction:** Serverless does not mean servers do not exist; it means **you do not manage, provision, patch, or pay for idle servers**. Behind the scenes, AWS manages massive fleets of hardened physical host servers, firecracker microVMs, and container runtimes that boot your code in milliseconds.

### Misconception 3: "DynamoDB On-Demand is always cheaper than Provisioned mode."

**Correction:** On-Demand is cheaper for intermittent, low-volume, or unpredictable traffic. However, for steady-state workloads with high, predictable traffic (e.g., millions of requests per hour 24/7), On-Demand billing per request can be **3x to 5x more expensive** than properly tuned Provisioned Capacity with Auto Scaling and Reserved Capacity commitments.

### Misconception 4: "Lambda functions can run indefinitely if you keep sending traffic."

**Correction:** Every single individual Lambda invocation has a strict, unchangeable **15-minute (900-second) hard execution ceiling**. If your script takes 15 minutes and 1 second to complete a video transcoding or database processing job, AWS will terminate the process immediately. Long-running workloads must be orchestrated using **AWS Step Functions**, run as containers on **AWS Fargate**, or executed on **Amazon EC2**.

---

## 14. Hands-On Practice Exercises

### Lab 1: Automated S3 Clean-up Script

Write a Python Boto3 script that connects to your test S3 bucket, inspects all objects, and deletes any object that ends with the `.tmp` extension or is older than 7 days.

### Lab 2: S3-Triggered Lambda Thumbnail Generator

Configure an S3 bucket with an **Event Notification** that triggers an AWS Lambda function whenever a new `.png` or `.jpg` file is uploaded to the `raw-images/` prefix. The Lambda function prints the file name and size to CloudWatch Logs.

### Lab 3: DynamoDB CRUD Operations via Boto3

Extend the class Lambda function to implement a full RESTful CRUD interface:

- `POST /users`: Inserts a new user item into DynamoDB.
- `GET /users/{id}`: Retrieves a single user item by its Partition Key `id` using `table.get_item(Key={'id': user_id})`.
- `DELETE /users/{id}`: Removes the user item using `table.delete_item(Key={'id': user_id})`.

---

## 15. DevOps Interview Corner

### Question 1: How does Amazon S3 achieve 11 9's ($99.999999999\%$) durability, and what is the difference between S3 Standard and S3 Glacier Deep Archive?

**Model Answer:**

> "Amazon S3 achieves 11 9's of durability by automatically creating redundant copies of every uploaded object across a minimum of three physically separated Availability Zones within an AWS Region. Each AZ is an isolated data center facility with independent power, cooling, and networking. This multi-site replication ensures that even the total physical destruction of an entire AZ will not result in data loss.
>
> The core difference between S3 Standard and S3 Glacier Deep Archive lies in access latency and cost economics:
>
> - **S3 Standard** is engineered for active, frequently accessed data. It delivers sub-second millisecond retrieval latencies with zero retrieval fees, but costs approximately \$0.023 per GB per month (~$23/TB).
> - **S3 Glacier Deep Archive** is designed for cold, compliance-driven archival records accessed perhaps once every few years. It costs roughly \$0.00099 per GB per month (~$1/TB)—a 96% cost reduction. However, data retrieval is asynchronous, requiring between 12 and 48 hours to restore objects from magnetic tape libraries to an active S3 tier."

### Question 2: What is the AWS Lambda execution lifecycle, and what is the difference between a 'Cold Start' and a 'Warm Start'?

**Model Answer:**

> "The AWS Lambda execution lifecycle consists of three distinct phases: **Init**, **Invoke**, and **Shutdown**.
>
> 1. **Cold Start (Full Initialization):** When a function is invoked for the first time, or after scaling up to handle concurrent requests, Lambda must provision a microVM (Firecracker container), download the function code package, initialize the language runtime (e.g., Python 3.12), and execute the code outside the handler (global imports and client initializations). This initial setup adds latency (often 100ms to 2 seconds).
> 2. **Warm Start (Container Reuse):** After an execution finishes, AWS freezes the execution container and keeps it alive for a brief period (typically 5 to 15 minutes). If another invocation arrives while the container is warm, Lambda skips the Init phase entirely and jumps straight to the Invoke phase.
>
> As DevOps engineers, we optimize for warm starts by instantiating database connections and Boto3 SDK clients **outside** the `lambda_handler()` function so they are reused across invocations."

### Question 3: Explain the role of the Partition Key in Amazon DynamoDB and how it impacts horizontal scaling.

**Model Answer:**

> "In Amazon DynamoDB, the Partition Key (or Hash Key) serves as the primary mechanism for data distribution and partition routing. DynamoDB internally maps data across a cluster of storage nodes called physical partitions.
>
> When an application issues a `PutItem` or `GetItem` request, DynamoDB takes the Partition Key value, passes it through an internal MD5-based cryptographic hash function, and uses the resulting 128-bit hash value to determine the exact physical partition responsible for storing that record.
>
> A well-architected partition key must possess **high cardinality** (many distinct values, such as UUIDs, user IDs, or device serial numbers) and uniform access patterns. If an application uses a low-cardinality key (such as status `active`/`inactive` or a static date), it creates a 'Hot Partition'—saturating a single storage node's 1,000 WCU or 3,000 RCU limit and triggering `ProvisionedThroughputExceededException`, while other partition nodes sit idle."

### Question 4: How do IAM Execution Roles differ from IAM User Access Keys when granting permissions to AWS Lambda?

**Model Answer:**

> "IAM Users rely on long-lived, static access keys (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`). Storing static credentials inside code, configuration files, or environment variables creates severe security vulnerabilities, as credentials can leak via git commits or application logs.
>
> An **IAM Execution Role** relies on AWS Security Token Service (STS) to grant temporary, rotating credentials directly to the Lambda execution environment. When Lambda boots, AWS assumes the execution role on behalf of the function and injects temporary credentials into the container's metadata service. These credentials automatically rotate every few hours without operational overhead. Furthermore, permissions are governed by an **IAM Trust Policy** (`Service: lambda.amazonaws.com`), ensuring that only the authorized Lambda service principal can assume the role."

### Question 5: How would you architect a disaster recovery solution to back up a production DynamoDB table with minimal cost and zero performance impact?

**Model Answer:**

> "To achieve enterprise-grade disaster recovery for DynamoDB without impacting production throughput:
>
> 1. **Continuous Backup:** Enable **Point-in-Time Recovery (PITR)** on the table. This provides continuous, zero-impact transaction log backups, enabling restoration to any second within the past 35 days with zero performance degradation on live read/write capacity units.
> 2. **Long-Term Offsite Archival:** Configure a scheduled **Amazon EventBridge** rule that triggers an automated export to **Amazon S3** via DynamoDB's native S3 Export feature (or an orchestrated AWS Lambda/Glue job). The export converts table items into compressed JSON/Parquet files in S3.
> 3. **Cost Optimization:** Attach an **S3 Lifecycle Policy** to the backup bucket. Transition backup files to **S3 Glacier Instant Retrieval** after 30 days, and to **S3 Glacier Deep Archive** after 90 days. This brings long-term storage costs down to \$1 per TB per month while maintaining complete offsite disaster recovery compliance."

---

## 16. Bilingual Technical Glossary

| English Technical Term           | Bengali Translation & Contextual Meaning                         | Scholarly DevOps Engineering Context                                                                        |
| :------------------------------- | :--------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------- |
| **Object Storage**               | **অবজেক্ট স্টোরেজ** (ফ্ল্যাট নেমস্পেস ডাটা স্টোরেজ)              | ফাইল সিস্টেমের ডিরেক্টরি ট্রি ছাড়া কি-ভ্যালু (Key-Value) ভিত্তিক অসীম স্কেলের ডাটা স্টোরেজ (Amazon S3)।     |
| **Block Storage**                | **ব্লক স্টোরেজ** (ভার্চুয়াল ডিস্ক ড্রাইভ)                        | কাঁচা বাইট ব্লকে বিভক্ত উচ্চগতির স্টোরেজ যা সরাসরি ভার্চুয়াল মেশিনের সাথে যুক্ত থাকে (Amazon EBS)।          |
| **Bucket**                       | **বাকেট** (এস৩ কনটেইনার)                                         | এস৩-তে অবজেক্ট সংরক্ষণের গ্লোবাল ইউনিক নামধারী শীর্ষস্থানীয় স্টোরেজ কনটেইনার।                               |
| **Durability**                   | **ডিউরেবিলিটি** (ডাটার স্থায়িত্ব বা অবিনশ্বরতা)                 | দুর্যোগ বা হার্ডওয়্যার ধ্বংসের মুখেও ডাটা অক্ষত থাকার সম্ভাব্যতা ($99.999999999\%$).                       |
| **Availability**                 | **অ্যাভেইলেবিলিটি** (সেবার প্রাপ্যতা বা আপটাইম)                  | যেকোনো মুহূর্তে সিস্টেম বা সার্ভিসের রিকোয়েস্টে সাড়া দেওয়ার সক্ষমতা ($99.99\%$).                         |
| **Storage Class**                | **স্টোরেজ ক্লাস** (ডাটা ব্যবহারের ধরনভিত্তিক স্তর)               | ডাটা অ্যাক্সেসের ফ্রিকোয়েন্সি ও খরচের ওপর ভিত্তি করে নির্ধারিত টিয়ার (Standard, Glacier Deep Archive)।    |
| **Lifecycle Transition**         | **লাইফসাইকেল ট্রানজিশন** (স্বয়ংক্রিয় ডাটা স্থানান্তর)            | নিয়মের ভিত্তিতে পুরনো ডাটা স্বয়ংক্রিয়ভাবে দামি স্টোরেজ থেকে সস্তা আর্কাইভে পাঠানোর প্রক্রিয়া।               |
| **Multipart Upload**             | **মাল্টিপার্ট আপলোড** (টুকরো করে সমান্তরাল আপলোড)                | বড় ফাইলকে ছোট ছোট চাঙ্কে (Chunks) ভাগ করে সমান্তরালে দ্রুত ও নির্ভরযোগ্যভাবে আপলোড করার আর্কিটেকচার।        |
| **Serverless Compute**           | **সার্ভারলেস কম্পিউট** (সার্ভারবিহীন অন-ডিমান্ড কম্পিউটেশন)      | কোনো সার্ভার প্রভিশন বা ওএস ম্যানেজমেন্ট ছাড়া অন-ডিমান্ড কোড চালানোর ক্লাউড মডেল (AWS Lambda)।              |
| **FaaS (Function as a Service)** | **ফাংশন-অ্যাজ-আ-সার্ভিস**                                        | ইভেন্টভিত্তিক ক্ষুদ্র কোড ব্লক বা ফাংশন এক্সিকিউট করার ক্লাউড কম্পিউট প্ল্যাটফর্ম।                          |
| **Handler**                      | **হ্যান্ডলার** (ফাংশনের প্রধান এন্ট্রি-পয়েন্ট)                  | ল্যামডা ফাংশন চালু হওয়ার সময় এডব্লিউএস রানটাইম যে নির্দিষ্ট পাইথন মেথডকে প্রথম কল করে।                      |
| **Execution Role**               | **এক্সিকিউশন রোল** (অস্থায়ী পারমিশন আইডেন্টিটি)                 | ল্যামডা বা কোনো সার্ভিসকে অন্যান্য এডব্লিউএস সার্ভিস ব্যবহারের সাময়িক অনুমতি প্রদানকারী আইএএম রোল।          |
| **Observability**                | **অবজার্ভেবিলিটি** (সিস্টেমের ভেতরের অবস্থা পর্যবেক্ষণের ক্ষমতা) | মেট্রিক্স, লগ ও ট্রেসের মাধ্যমে অ্যাপ্লিকেশনের অভ্যন্তরীণ স্বাস্থ্য ও ত্রুটি নির্ণয়ের আধুনিক আর্কিটেকচার।   |
| **NoSQL Database**               | **নো-এসকিউএল ডাটাবেস** (নন-রিলেশনাল ডকুমেন্ট স্টোর)              | রিলেশনাল টেবিল ও জয়েন মুক্ত ফ্লেক্সিবল স্কিমার অতিদ্রুত অনুভূমিকভাবে স্কেলযোগ্য ডাটাবেস (DynamoDB)।         |
| **Partition Key**                | **পার্টিশন কী** (হ্যাশ কী)                                       | ডাইনামোডিবিতে ডাটা কোন ফিজিক্যাল স্টোরেজ নোডে জমা হবে তা নির্ধারণকারী প্রাইমারি কী।                         |
| **On-Demand Capacity**           | **অন-ডিমান্ড ক্যাপাসিটি** (ব্যবহারভিত্তিক পে-অ্যাজ-ইউ-গো মোড)    | আগে থেকে ক্যাপাসিটি বরাদ্দ না করে হুবহু রিকোয়েস্ট সংখ্যার ওপর ভিত্তি করে বিল দেওয়ার পদ্ধতি।                |
| **Least Privilege (PoLP)**       | **লিস্ট প্রিভিলেজ নীতি** (সর্বনিম্ন প্রয়োজনীয় অনুমতি)          | কোনো সিস্টেম বা ইউজারকে তার কাজ সম্পাদনের জন্য ঠিক যতটুকু অনুমতি প্রয়োজন তার বেশি না দেওয়ার নিরাপত্তা নীতি। |

---

## 17. One-Page S3, Lambda & DynamoDB Cheat Sheet

### Essential AWS CLI & Boto3 Snippets

```bash
# -------------------------------------------------------------
# Amazon S3 Management Commands
# -------------------------------------------------------------
# 1. Create a globally unique bucket
aws s3 mb s3://bongodev-demo-bucket-ns2026 --region us-west-2

# 2. Upload a single file
aws s3 cp document.pdf s3://bongodev-demo-bucket-ns2026/docs/

# 3. Synchronize an entire local directory to S3 (delta sync)
aws s3 sync ./assets s3://bongodev-demo-bucket-ns2026/assets/

# 4. List all objects in bucket
aws s3 ls s3://bongodev-demo-bucket-ns2026/ --human-readable

# 5. Enable bucket versioning
aws s3api put-bucket-versioning \
    --bucket bongodev-demo-bucket-ns2026 \
    --versioning-configuration Status=Enabled
```

```python
# -------------------------------------------------------------
# Python Boto3 Quick Reference
# -------------------------------------------------------------
import boto3

# Initialize clients
s3 = boto3.client("s3")
dynamodb = boto3.resource("dynamodb")

# S3: Upload a file
s3.upload_file(
    Filename="local.txt", Bucket="my-bucket", Key="remote/path.txt"
)

# S3: Download a file
s3.download_file(
    Bucket="my-bucket", Key="remote/path.txt", Filename="downloaded.txt"
)

# DynamoDB: Write an Item
table = dynamodb.Table("my_table")
table.put_item(
    Item={"id": 101, "name": "Nure Siddiqui", "role": "DevOps Lead"}
)

# DynamoDB: Read an Item by Primary Key
response = table.get_item(Key={"id": 101})
item = response.get("Item")
```

### Key Architectural Thresholds & Limits

```mermaid
flowchart TD
    subgraph S3Limits["Amazon S3 Limits"]
        S1["Max Single Object Size: 5 Terabytes"]
        S2["Single PUT Upload Limit: 5 Gigabytes"]
        S3["Multipart Upload Required: > 100 MB up to 5 TB"]
        S4["Bucket Limit per Account: 100 (soft), 1,000 (hard)"]
    end

    subgraph LambdaLimits["AWS Lambda Limits"]
        L1["Max Execution Timeout: 15 minutes (900s)"]
        L2["Memory Allocation: 128 MB to 10,240 MB (10 GB)"]
        L3["Ephemeral /tmp Storage: 512 MB to 10,240 MB"]
        L4["Max Deployment Package: 50 MB (zipped), 250 MB (unzipped)"]
    end

    subgraph DDBLimits["Amazon DynamoDB Limits"]
        D1["Max Single Item Size: 400 Kilobytes (attributes + names)"]
        D2["Partition Throughput Limit: 1,000 WCU / 3,000 RCU per node"]
        D3["Query / Scan Maximum Batch Size: 1 Megabyte"]
    end
```
