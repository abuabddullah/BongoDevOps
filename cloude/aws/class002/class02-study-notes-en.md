# AWS Cloud Architecture & IAM Security — Class 02 Study Notes

**Topic:** Cloud Service Models, AWS Global Infrastructure Deep Dive, The Core DevOps Service Catalog, and Enterprise Identity & Access Management (IAM)  
**Covers:** Cloud Service Models (IaaS, PaaS, SaaS), AWS Shared Responsibility Model, Global Infrastructure (Regions, Availability Zones, Local Zones, Wavelength, Outposts, Edge Locations), Taxonomy of AWS Services (Compute, Containers, Storage, Database, ML, Security), The 3 AWS Access Paradigms (Console, CLI, SDK/IaC), IAM Root Account Security, The Four Pillars of IAM (Users, Groups, Roles, Policies), IAM JSON Policy Anatomy (`Version`, `Statement`, `Effect`, `Action`, `Resource`), Enterprise Identity Federation (IdP & SSO), and Hands-on AWS CLI Configuration (`aws configure`, credential storage, CLI queries)  
**Course & Class:** BongoDev DevOps & Cloud Engineering — Cloud Track: AWS Class 02 (Recorded 10 October 2026)  
**Instructor:** Nure Siddiqui  
**Builds on:** [Class 01 notes](../class001/class01-study-notes-en.md) (EC2 provisioning, AMI blueprints, EBS storage, Security Groups, SSH key pairs, User Data, Launch Templates)  
**Next Steps:** VPC Network Architecture (Subnets, Route Tables, Internet Gateways, NAT Gateways), S3 Object Storage Deep Dive, Full-Stack App Deployment without Docker, and Infrastructure as Code with Terraform

> **About these study notes:** These notes compile every concept, architectural theory, live demonstration, and security rule taught in Class 02. In Class 01, we provisioned live EC2 instances and automated fleets with Launch Templates. In Class 02, we step back to build the foundational mental models that govern enterprise cloud systems: how AWS organizes physical infrastructure globally, how the 240+ service catalog maps to practical DevOps engineering, and how AWS Identity and Access Management (IAM) controls the security perimeter of multi-million dollar cloud environments. All diagrams are rendered using native Mermaid vector diagrams for seamless display.

---

## Table of Contents

1. [Recap of Class 01 & Class 02 Roadmap](#1-recap-of-class-01--class-02-roadmap)
2. [Cloud Computing Service Models: IaaS, PaaS, and SaaS](#2-cloud-computing-service-models-iaas-paas-and-saas)
   - 2.1 [The Pizza-as-a-Service & Transportation Model](#21-the-pizza-as-a-service--transportation-model)
   - 2.2 [Infrastructure as a Service (IaaS)](#22-infrastructure-as-a-service-iaas)
   - 2.3 [Platform as a Service (PaaS)](#23-platform-as-a-service-paas)
   - 2.4 [Software as a Service (SaaS)](#24-software-as-a-service-saas)
   - 2.5 [The AWS Shared Responsibility Model](#25-the-aws-shared-responsibility-model)
3. [AWS Global Infrastructure: Beyond Single Regions](#3-aws-global-infrastructure-beyond-single-regions)
   - 3.1 [AWS Regions: Geographic Isolation](#31-aws-regions-geographic-isolation)
   - 3.2 [Availability Zones (AZs): The Rule of Minimum 3](#32-availability-zones-azs-the-rule-of-minimum-3)
   - 3.3 [AWS Local Zones: Metropolitan Low Latency](#33-aws-local-zones-metropolitan-low-latency)
   - 3.4 [AWS Wavelength: 5G Edge Computing](#34-aws-wavelength-5g-edge-computing)
   - 3.5 [AWS Outposts: True Hybrid Cloud](#35-aws-outposts-true-hybrid-cloud)
   - 3.6 [Edge Locations & Points of Presence (CloudFront & Route 53)](#36-edge-locations--points-of-presence-cloudfront--route-53)
4. [Demystifying the 240+ AWS Service Catalog: The Core DevOps Toolkit](#4-demystifying-the-240-aws-service-catalog-the-core-devops-toolkit)
   - 4.1 [The 80/20 Rule of Cloud Engineering](#41-the-8020-rule-of-cloud-engineering)
   - 4.2 [Compute & Serverless](#42-compute--serverless)
   - 4.3 [Containers & Orchestration](#43-containers--orchestration)
   - 4.4 [Storage Solutions: Block, Object, and File](#44-storage-solutions-block-object-and-file)
   - 4.5 [Databases: SQL vs NoSQL Managed Engines](#45-databases-sql-vs-nosql-managed-engines)
   - 4.6 [Artificial Intelligence & Machine Learning](#46-artificial-intelligence--machine-learning)
   - 4.7 [Security, Identity, and Governance](#47-security-identity-and-governance)
5. [The Three Ways to Access & Manage AWS](#5-the-three-ways-to-access--manage-aws)
   - 5.1 [AWS Management Console (Web GUI)](#51-aws-management-console-web-gui)
   - 5.2 [AWS Command Line Interface (AWS CLI)](#52-aws-command-line-interface-aws-cli)
   - 5.3 [Software Development Kits (SDKs) & Infrastructure as Code (IaC)](#53-software-development-kits-sdks--infrastructure-as-code-iac)
6. [AWS Identity & Access Management (IAM): The Security Bedrock](#6-aws-identity--access-management-iam-the-security-bedrock)
   - 6.1 [The Cardinal Sin: Never Use the Root Account](#61-the-cardinal-sin-never-use-the-root-account)
   - 6.2 [The Four Pillars of IAM: Users, Groups, Roles, Policies](#62-the-four-pillars-of-iam-users-groups-roles-policies)
   - 6.3 [IAM Users: Identity for Humans and Machines](#63-iam-users-identity-for-humans-and-machines)
   - 6.4 [IAM User Groups: Role-Based Access Control (RBAC)](#64-iam-user-groups-role-based-access-control-rbac)
   - 6.5 [IAM Roles: Temporary STS Credentials for Services](#65-iam-roles-temporary-sts-credentials-for-services)
   - 6.6 [IAM Policies: The JSON Rule Book](#66-iam-policies-the-json-rule-book)
7. [Anatomy of an IAM Policy (JSON Deep Dive)](#7-anatomy-of-an-iam-policy-json-deep-dive)
   - 7.1 [JSON Structure Breakdown: Version, Statement, Effect, Action, Resource](#71-json-structure-breakdown-version-statement-effect-action-resource)
   - 7.2 [The 2012-10-17 Version String Explained](#72-the-2012-10-17-version-string-explained)
   - 7.3 [Explicit Deny vs Implicit Deny: The Evaluation Logic](#73-explicit-deny-vs-implicit-deny-the-evaluation-logic)
   - 7.4 [Sample Policies Walkthrough](#74-sample-policies-walkthrough)
8. [Enterprise Identity Federation & Single Sign-On (SSO)](#8-enterprise-identity-federation--single-sign-on-sso)
   - 8.1 [What is an Identity Provider (IdP)?](#81-what-is-an-identity-provider-idp)
   - 8.2 [SAML 2.0 & OIDC Federation](#82-saml-20--oidc-federation)
   - 8.3 [Why Modern Enterprises Avoid Standalone IAM Users](#83-why-modern-enterprises-avoid-standalone-iam-users)
9. [Hands-On Lab: Installing & Configuring the AWS CLI](#9-hands-on-lab-installing--configuring-the-aws-cli)
   - 9.1 [Installation Steps (Linux & Windows)](#91-installation-steps-linux--windows)
   - 9.2 [Creating CLI Access Keys in IAM](#92-creating-cli-access-keys-in-iam)
   - 9.3 [Executing `aws configure`](#93-executing-aws-configure)
   - 9.4 [Where Credentials Live: `~/.aws/credentials` and `~/.aws/config`](#94-where-credentials-live-awscredentials-and-awsconfig)
   - 9.5 [Verifying Access: Querying EC2 and S3 via CLI](#95-verifying-access-querying-ec2-and-s3-via-cli)
10. [Real-World Gotchas, Security Traps & Troubleshooting](#10-real-world-gotchas-security-traps--troubleshooting)
11. [Questions Asked in Class (Q&A)](#11-questions-asked-in-class-qa)
12. [Common Misconceptions, Corrected](#12-common-misconceptions-corrected)
13. [Hands-On Practice Exercises](#13-hands-on-practice-exercises)
14. [DevOps Interview Corner](#14-devops-interview-corner)
15. [Bilingual Technical Glossary](#15-bilingual-technical-glossary)
16. [One-Page AWS IAM & CLI Cheat Sheet](#16-one-page-aws-iam--cli-cheat-sheet)

---

## 1. Recap of Class 01 & Class 02 Roadmap

In **Class 01**, we jumped straight into the trenches of server provisioning:

- Transitioned from on-premises hardware bottlenecks (CapEx, supply chain delays) to the elastic pay-as-you-go cloud model (OpEx).
- Deconstructed Amazon EC2: AMIs, instance sizing (vCPU vs RAM), EBS persistent volumes, and Security Groups.
- Connected via SSH using asymmetric RSA key pairs (`chmod 400`).
- Automated software installation via User Data (`cloud-init`) and scaled from 1 to 50 servers using AWS Launch Templates (v1 $\rightarrow$ v2 $\rightarrow$ v3).

In **Class 02**, the instructor established the overarching theoretical, architectural, and security foundation that connects these individual components into an enterprise cloud platform.

### Class 02 Architectural Roadmap

```mermaid
flowchart TD
    A["1. Cloud Service Models<br/>(IaaS vs PaaS vs SaaS & Shared Responsibility)"] --> B["2. AWS Global Infrastructure Deep Dive<br/>(Regions, Multi-AZ HA, Local Zones, Wavelength, Outposts)"]
    B --> C["3. The 240+ Service Catalog Demystified<br/>(Compute, Storage, Database, Containers, ML, Security)"]
    C --> D["4. The 3 Access Paradigms<br/>(Management Console, AWS CLI, SDKs/IaC)"]
    D --> E["5. IAM: Security Bedrock & Root Account Hardening<br/>(Locking Root, MFA, Principle of Least Privilege)"]
    E --> F["6. The 4 Pillars of IAM<br/>(Users, User Groups, Roles, Policies)"]
    F --> G["7. IAM Policy JSON Mechanics<br/>(Version, Statements, Effect, Action, Resource ARN)"]
    G --> H["8. Enterprise Identity Federation<br/>(IdP, SAML 2.0, OIDC, Corporate SSO)"]
    H --> I["9. Hands-on AWS CLI & Credentials<br/>(aws configure, ~/.aws/credentials, describe-instances)"]
```

---

## 2. Cloud Computing Service Models: IaaS, PaaS, and SaaS

### 2.1 The Pizza-as-a-Service & Transportation Model

In cloud computing, services are categorized by how much of the underlying infrastructure stack is managed by the cloud vendor versus how much is managed by your organization.

```mermaid
flowchart TD
    subgraph OnPrem["Traditional On-Premises (You Manage Everything)"]
        direction TB
        OP1["Applications"] --- OP2["Data & Databases"] --- OP3["Runtime & OS"] --- OP4["Virtualization & Servers"] --- OP5["Physical Storage & Networking"]
    end

    subgraph IaaS["IaaS: Infrastructure as a Service (e.g. AWS EC2, EBS, VPC)"]
        direction TB
        IA1["You Manage: Applications & Logic"] --- IA2["You Manage: Data & Databases"] --- IA3["You Manage: OS & Patching"] --- IA4["AWS Manages: Hypervisor & Virtualization"] --- IA5["AWS Manages: Physical Data Centers & Silicon"]
    end

    subgraph PaaS["PaaS: Platform as a Service (e.g. AWS Elastic Beanstalk, Lambda)"]
        direction TB
        PA1["You Manage: Application Code"] --- PA2["You Manage: Business Data"] --- PA3["AWS Manages: OS, Runtimes, Auto-Scaling"] --- PA4["AWS Manages: Servers & Capacity"] --- PA5["AWS Manages: Physical Infrastructure"]
    end

    subgraph SaaS["SaaS: Software as a Service (e.g. Gmail, Jira, AWS QuickSight)"]
        direction TB
        SA1["Vendor Manages 100%: End-User Application, Code, Data, Servers, Infrastructure"]
    end
```

### 2.2 Infrastructure as a Service (IaaS)

- **What it is:** The vendor provides raw virtualized computing resources—virtual machines, raw block storage, and software-defined network routing.
- **Customer Responsibility:** You choose, install, configure, patch, and secure the operating system (e.g., Ubuntu Linux), runtime runtimes (Python, Node.js), middleware, database engines, and your application code.
- **AWS Examples:** **Amazon EC2**, **Amazon EBS**, **Amazon VPC**.
- **Analogy:** Renting an unfurnished apartment. The landlord provides the concrete walls, water pipes, and electricity lines, but you must bring your own furniture, paint, locks, and appliances.

### 2.3 Platform as a Service (PaaS)

- **What it is:** The vendor abstracts away the underlying operating system, server patching, hypervisors, and capacity provisioning. You simply upload your application code or container, and the platform automatically provisions servers, balances load, scales capacity, and monitors health.
- **Customer Responsibility:** You only care about writing clean application code and database schemas.
- **AWS Examples:** **AWS Elastic Beanstalk**, **AWS Lambda** (Serverless Compute), **AWS App Runner**.
- **Analogy:** Staying at a fully serviced hotel. You do not worry about plumbing, vacuuming, or changing bedsheets; you simply stay and work.

### 2.4 Software as a Service (SaaS)

- **What it is:** A complete, fully functional software application delivered over the web browser. The customer manages zero infrastructure or code; they simply use the software via credentials.
- **Customer Responsibility:** User configuration, data entry, and user account management.
- **AWS & Industry Examples:** **AWS QuickSight** (Business Intelligence), **Amazon WorkSpaces**, Google Workspace, Jira, Salesforce.
- **Analogy:** Taking a taxi or Uber. You do not own the car, do not change the motor oil, and do not drive; you simply ride to your destination.

### 2.5 The AWS Shared Responsibility Model

Security and compliance in AWS is a shared partnership:

```mermaid
flowchart TD
    subgraph Customer["CUSTOMER RESPONSIBILITY: Security IN the Cloud"]
        direction TB
        C1["Customer Data & Encryption (Client & Server Side)"]
        C2["Identity & Access Management (IAM Users, Groups, Roles, MFA)"]
        C3["Operating System Configuration, Security Patches & Firewalls"]
        C4["Network & Firewall Rules (Security Groups, Network ACLs, Route Tables)"]
    end

    subgraph AWS["AWS RESPONSIBILITY: Security OF the Cloud"]
        direction TB
        A1["Global Physical Infrastructure (Regions, AZs, Edge Locations)"]
        A2["Hardware Compute, Silicon, Storage Disks & Network Cables"]
        A3["Hypervisors (AWS Nitro System) & Virtualization Software"]
        A4["Data Center Physical Security, Power Grids, HVAC & Biometric Doors"]
    end

    Customer --- AWS
```

> **The Golden Formula:**
>
> - **AWS is responsible for the security OF the cloud** (the physical facilities, fiber optic cables, hardware racks, hypervisor virtualization).
> - **The Customer is responsible for security IN the cloud** (encrypting data, creating strong passwords, enabling MFA, configuring Security Group port rules, patching guest OS vulnerabilities).

---

## 3. AWS Global Infrastructure: Beyond Single Regions

In Class 01, we discussed how AWS groups data centers into geographic Regions. In Class 02, the instructor analyzed the complete tier of AWS's global edge infrastructure:

```mermaid
flowchart TD
    subgraph GlobalTier["AWS Global Presence"]
        direction TB
        R["AWS Regions<br/>(33+ Geographic Locations Worldwide)"]
        AZ["Availability Zones (AZs)<br/>(Minimum 3 Isolated Data Centers per Region)"]
        LZ["AWS Local Zones<br/>(Metro Extensions for Ultra-Low Single-Digit ms Latency)"]
        WL["AWS Wavelength<br/>(Embedded at 5G Telecom Carrier Edge)"]
        OP["AWS Outposts<br/>(Physical AWS Hardware Racks in Private Data Centers)"]
        EDGE["Edge Locations / PoPs<br/>(600+ CloudFront CDN & Route 53 Caching Caches)"]
    end

    R --> AZ
    R -.-> LZ
    R -.-> WL
    R -.-> OP
    R -.-> EDGE
```

### 3.1 AWS Regions: Geographic Isolation

- An **AWS Region** is a completely separate geographic area (e.g., `ap-south-1` Mumbai, `ap-southeast-1` Singapore, `eu-central-1` Frankfurt).
- Regions are completely autonomous; an outage in one region (e.g., `us-east-1`) does not cascade into another region (e.g., `ap-south-1`).
- Used to optimize user latency, comply with data sovereignty regulations, and minimize pricing.

### 3.2 Availability Zones (AZs): The Rule of Minimum 3

Every single AWS Region worldwide contains a **minimum of three Availability Zones (AZs)** (e.g., `ap-south-1a`, `ap-south-1b`, `ap-south-1c`).

- **What is an AZ?** One or more discrete, physical data centers located miles apart from each other within the same metropolitan region.
- **Why minimum 3? High Availability (HA) & Fault Tolerance (FT)!**
  - If a flood, electrical blackout, or fire physically destroys Data Center A (`ap-south-1a`), your production workload seamlessly continues serving traffic from Data Center B (`ap-south-1b`) and Data Center C (`ap-south-1c`).
  - Inter-AZ connections run over dedicated, private, ultra-low latency fiber-optic rings, enabling sub-millisecond synchronous database replication.

```mermaid
flowchart LR
    subgraph Region["AWS Region: Singapore (ap-southeast-1)"]
        direction TB
        AZ1["AZ: ap-southeast-1a<br/>[Primary DB Master]"]
        AZ2["AZ: ap-southeast-1b<br/>[Standby Sync Replica]"]
        AZ3["AZ: ap-southeast-1c<br/>[Read Replica / App Node]"]
        AZ1 <-->|"Sub-millisecond Fiber Ring"| AZ2
        AZ2 <-->|"Sub-millisecond Fiber Ring"| AZ3
        AZ1 <-->|"Sub-millisecond Fiber Ring"| AZ3
    end
```

### 3.3 AWS Local Zones: Metropolitan Low Latency

- **The Problem:** Some major metropolitan cities (e.g., Los Angeles, Chicago, Delhi) do not have a full AWS Region, but local media companies, high-frequency financial traders, and online gaming platforms require **single-digit millisecond latency** ($< 5\text{ ms}$).
- **The Solution:** **AWS Local Zones** are geographic extensions of a parent AWS Region that place compute, storage, and database services right in the heart of major population centers.
- **Instructor's Analogy:** If the parent region is Singapore or Mumbai, a Local Zone is like placing an AWS-managed mini-hub directly in Savar or Gazipur to serve the Dhaka industrial belt with near-zero latency.

### 3.4 AWS Wavelength: 5G Edge Computing

- **What it is:** AWS infrastructure deployed directly inside the physical data centers of major telecommunications 5G network providers (e.g., Verizon, Vodafone, KDDI, SK Telecom).
- **Why it matters:** Eliminates internet transit hops. Mobile devices connecting over 5G reach the application server without ever routing out to the public internet, achieving ultra-low latency for autonomous vehicles, smart factories, AR/VR, and real-time medical robotics.

### 3.5 AWS Outposts: True Hybrid Cloud

- **What it is:** Physical 42U server racks built and owned by AWS, shipped directly to your corporate office or private data center, and plugged into your local power and network.
- **The Power:** You run native AWS APIs, EC2 instances, EBS volumes, and EKS clusters locally on-premises, managed seamlessly through the same AWS Management Console. Perfect for banks and defense agencies with absolute legal prohibitions against moving data off-site.

### 3.6 Edge Locations & Points of Presence (CloudFront & Route 53)

- Over **600+ Points of Presence (PoPs)** globally.
- Used by **Amazon CloudFront** (Content Delivery Network - CDN) to cache static images, videos, and HTML files at the edge close to end-users, and **Amazon Route 53** for fast, authoritative DNS lookups.

---

## 4. Demystifying the 240+ AWS Service Catalog: The Core DevOps Toolkit

### 4.1 The 80/20 Rule of Cloud Engineering

When a beginner opens the AWS Management Console, they are confronted with a bewildering array of over **240 distinct cloud services**.

- **The Reality Check:** Nobody memorizes or uses all 240 services.
- In professional DevOps and SRE practice, **10 to 15 core foundational services** comprise over 90% of all real-world architectures.

```mermaid
flowchart TD
    subgraph Catalog["The Core AWS DevOps Services Taxonomy"]
        direction TB
        subgraph Compute["1. Compute & Serverless"]
            EC2["Amazon EC2 (Virtual Servers)"]
            LAMBDA["AWS Lambda (Serverless Code Execution)"]
            EB["Elastic Beanstalk (PaaS App Deployment)"]
        end
        subgraph Containers["2. Containers & K8s"]
            ECS["Amazon ECS (Docker Engine Orchestration)"]
            EKS["Amazon EKS (Managed Kubernetes Engine)"]
            ECR["Amazon ECR (Container Image Registry)"]
        end
        subgraph Storage["3. Storage Engines"]
            EBS["Amazon EBS (Virtual Block SSD Disks for EC2)"]
            S3["Amazon S3 (Infinite Object Storage)"]
            EFS["Amazon EFS (Managed Shared NFS File System)"]
        end
        subgraph Databases["4. Managed Databases"]
            RDS["Amazon RDS (Relational: PostgreSQL, MySQL)"]
            DDB["Amazon DynamoDB (NoSQL Key-Value Store)"]
            AURORA["Amazon Aurora (Cloud-Native Enterprise SQL)"]
        end
        subgraph Security["5. Security & Governance"]
            IAM["AWS IAM (Identity & Access Management)"]
            CW["Amazon CloudWatch (Logs, Metrics, Alarms)"]
        end
    end
```

### 4.2 Compute & Serverless

- **Amazon EC2:** Virtual servers where you control the OS, runtime, and network.
- **AWS Lambda:** Serverless compute. You write a function in Python, Node.js, or Go; AWS runs it only when triggered by an event (e.g., an S3 file upload or an API call), billing you per millisecond of execution. Zero servers to manage or patch.
- **AWS Elastic Beanstalk:** PaaS platform that takes your zip file or Git repo and automatically provisions EC2, Load Balancers, and Auto Scaling groups.

### 4.3 Containers & Orchestration

- **Amazon ECR (Elastic Container Registry):** Highly available private Docker image registry.
- **Amazon ECS (Elastic Container Service):** AWS-native, lightweight container orchestrator.
- **Amazon EKS (Elastic Kubernetes Service):** Fully managed, upstream-certified Kubernetes control planes.

### 4.4 Storage Solutions: Block, Object, and File

1. **Block Storage (Amazon EBS):** Virtual hard drive directly attached to one EC2 instance via a dedicated virtual bus. Stores the boot OS, binaries, and local data files.
2. **Object Storage (Amazon S3 - Simple Storage Service):** Unlimited, internet-accessible bucket storage. Files (objects) are stored with unique URLs and metadata. Designed for 99.999999999% (11 9's) durability. Ideal for user uploads, media files, logs, and database backups.
3. **File Storage (Amazon EFS):** Fully managed NFS (Network File System) that can be mounted concurrently by hundreds of EC2 instances and containers simultaneously.

### 4.5 Databases: SQL vs NoSQL Managed Engines

- **Amazon RDS (Relational Database Service):** Automated provisioning, patching, multi-AZ replication, and automated backups for standard SQL engines (PostgreSQL, MySQL, MariaDB, Oracle, SQL Server).
- **Amazon DynamoDB:** Fully managed, serverless NoSQL document/key-value database delivering consistent single-digit millisecond latency at any scale.

### 4.6 Artificial Intelligence & Machine Learning

- **Amazon SageMaker:** Fully managed platform for developers and data scientists to build, train, tune, and deploy machine learning models.
- **Amazon Bedrock:** Serverless access to state-of-the-art Foundation Models (Claude, Llama, Titan) via a single unified API.

### 4.7 Security, Identity, and Governance

- **AWS IAM (Identity and Access Management):** Central authentication and authorization service controlling who can access which AWS resources.
- **Amazon CloudWatch:** Observability suite for logs, operational metrics, and threshold-based alarm automation.

---

## 5. The Three Ways to Access & Manage AWS

AWS exposes every single cloud capability through standardized REST APIs. Engineers interact with these underlying APIs through three primary interfaces:

```mermaid
flowchart TD
    CLIENT1["1. AWS Management Console<br/>(Interactive Web GUI Browser)"] --> API
    CLIENT2["2. AWS Command Line Interface<br/>(Terminal: aws ec2, aws s3)"] --> API
    CLIENT3["3. SDKs & IaC Tools<br/>(Boto3, Terraform, CloudFormation)"] --> API

    subgraph AWSAPI["AWS Core REST API Fabric"]
        API["HTTPS API Endpoints (https://ec2.amazonaws.com)"]
    end

    API --> R1["EC2 Instances"]
    API --> R2["S3 Buckets"]
    API --> R3["RDS Databases"]
    API --> R4["IAM Identities"]
```

### 5.1 AWS Management Console (Web GUI)

- **Best for:** Visual exploration, learning, initial resource inspection, monitoring graphs, and architectural experimentation.
- **Limitations:** Slow, manual, unrepeatable, and prone to human clicking errors in multi-server production environments.

### 5.2 AWS Command Line Interface (AWS CLI)

- **Best for:** Rapid terminal queries, administrative automation, and bash shell scripting.
- Unified open-source tool installed on your local computer (`aws`).
- Authenticates using **Access Key ID** and **Secret Access Key**.

### 5.3 Software Development Kits (SDKs) & Infrastructure as Code (IaC)

- **SDKs (e.g., Boto3 for Python):** Used by software applications to programmatically interact with AWS (e.g., a Python Flask app uploading images directly to S3).
- **Infrastructure as Code (Terraform / OpenTofu / AWS CloudFormation):** The industry standard for production DevOps. Entire cloud architectures are declared in repeatable code files, committed to Git, and automated via CI/CD pipelines.

---

## 6. AWS Identity & Access Management (IAM): The Security Bedrock

### 6.1 The Cardinal Sin: Never Use the Root Account

When you first sign up for an AWS account with an email address and password, you create the **AWS Root User**:

- **Absolute Power:** The Root account has omnipotent, unrestricted administrative privileges over every resource, billing record, and account configuration. It cannot be restricted by IAM policies!
- **The Catastrophe of Compromise:** If an attacker obtains your Root credentials, they can delete production databases, lock you out of your account, and spin up thousands of high-end GPU instances to mine cryptocurrency, incurring tens of thousands of dollars in debt within hours.
- **Enterprise Best Practice:**
  1. Set an exceptionally complex, randomized 32+ character password.
  2. Enable hardware or virtual **Multi-Factor Authentication (MFA)** immediately.
  3. Delete any Root Access Keys if they exist.
  4. Lock the Root credentials in a secure company vault.
  5. **NEVER use the Root account for daily DevOps engineering tasks!**

```mermaid
flowchart TD
    ROOT["AWS Root User<br/>(Account Email + Password)<br/>🚨 Unrestricted God-Mode Power!"]

    subgraph Protection["Root Account Security Protocol"]
        direction TB
        MFA["1. Enable Multi-Factor Authentication (MFA)"]
        VAULT["2. Store Password in Company Password Vault"]
        LOCK["3. Do NOT Create Root Access Keys"]
        DAILY["4. Cease Daily Use Completely!"]
    end

    ROOT --> Protection
    Protection --> DELEGATE["Delegate Operations to IAM Users & Groups"]
```

### 6.2 The Four Pillars of IAM: Users, Groups, Roles, Policies

IAM is built upon four interconnected architectural concepts:

```mermaid
flowchart TD
    subgraph Core["The 4 Pillars of AWS IAM"]
        direction TB
        U["1. IAM Users<br/>(Identities for People or Dedicated CLI Tools)"]
        G["2. IAM User Groups<br/>(Collections of Users for RBAC Governance)"]
        R["3. IAM Roles<br/>(Temporary STS AssumeRole Credentials for Services)"]
        P["4. IAM Policies<br/>(JSON Rule Books Defining What is Allowed / Denied)"]
    end

    P -->|Attached to| U
    P -->|Attached to| G
    P -->|Attached to| R
    G -->|Contains| U
```

### 6.3 IAM Users: Identity for Humans and Machines

- An **IAM User** represents a specific person (e.g., `nure.siddiqui`, `rafi.devops`) or a machine process requiring long-term access.
- Each user can be assigned:
  1. **Console Password:** For logging into the AWS Web Console.
  2. **Access Keys (Access Key ID & Secret Access Key):** For authenticating via the AWS CLI or API.

### 6.4 IAM User Groups: Role-Based Access Control (RBAC)

- An **IAM Group** is a collection of IAM Users (e.g., `Admins`, `Developers`, `Finance-Billing`, `Security-Auditors`).
- **Best Practice:** You do not attach individual permissions directly to individual users! Instead, attach permissions to the Group. When a new engineer joins the team, simply add them to the `Developers` group, and they automatically inherit all necessary permissions. When an engineer leaves the company, remove them from the group, and their access terminates immediately.

```mermaid
flowchart LR
    subgraph Group["IAM User Group: DevOps-Engineers"]
        direction TB
        POLICY["Attached Policy:<br/>AdministratorAccess"]
        U1["User: nure"]
        U2["User: rafi"]
        U3["User: intern"]
    end
    POLICY -.-> U1
    POLICY -.-> U2
    POLICY -.-> U3
```

### 6.5 IAM Roles: Temporary STS Credentials for Services

- **What is an IAM Role?** An identity that is not tied to a specific human user and has **no permanent password or access key**.
- An IAM Role is meant to be **assumed** by anyone or anything that needs temporary access:
  - An **EC2 virtual machine** that needs to read files from an S3 bucket.
  - A **Lambda function** that needs to write records into a DynamoDB table.
  - A developer from an external AWS account performing cross-account maintenance.
- When an entity assumes a role, AWS **Security Token Service (STS)** issues temporary security credentials that expire automatically after a short duration (e.g., 1 hour).
- **Massive Security Advantage:** You never hardcode API secret keys inside server config files or application code!

### 6.6 IAM Policies: The JSON Rule Book

An **IAM Policy** is a formal JSON document that defines authorization permissions. By default, an IAM User or Role has **zero permissions** (Implicit Deny). They cannot do anything in AWS until an IAM Policy granting specific actions is attached to them.

---

## 7. Anatomy of an IAM Policy (JSON Deep Dive)

### 7.1 JSON Structure Breakdown: Version, Statement, Effect, Action, Resource

Every IAM Policy is a structured JSON document:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3BucketListing",
      "Effect": "Allow",
      "Action": ["s3:ListAllMyBuckets", "s3:GetBucketLocation"],
      "Resource": "*"
    }
  ]
}
```

```mermaid
flowchart TD
    subgraph JSON["IAM Policy JSON Document"]
        direction TB
        V["Version: 2012-10-17 (Language Syntax Release)"]
        subgraph Statement["Statement Block (Array of Permission Rules)"]
            SID["Sid (Optional): Human-Readable Rule Identifier"]
            EFF["Effect: 'Allow' or 'Deny'"]
            ACT["Action: List of Specific API Calls (e.g. s3:ListBucket, ec2:StartInstances)"]
            RES["Resource: Target ARN (e.g. arn:aws:s3:::my-bucket/* or *)"]
            COND["Condition (Optional): Context Constraints (e.g. Source IP, MFA Required)"]
        end
    end
```

| Element         | Required? | Meaning & Technical Role                                                                             |
| --------------- | --------- | ---------------------------------------------------------------------------------------------------- |
| **`Version`**   | **Yes**   | Declares the policy language syntax version. **Always use `"2012-10-17"`!**                          |
| **`Statement`** | **Yes**   | The core array holding one or more individual permission declarations.                               |
| **`Sid`**       | No        | Statement ID: a custom string used to document the statement's purpose.                              |
| **`Effect`**    | **Yes**   | Must be either `"Allow"` or `"Deny"`.                                                                |
| **`Action`**    | **Yes**   | The specific API operations allowed or denied (e.g., `ec2:DescribeInstances`, `s3:GetObject`).       |
| **`Resource`**  | **Yes**   | The Amazon Resource Name (ARN) identifying the exact AWS assets the actions apply to.                |
| **`Condition`** | No        | Advanced constraints (e.g., only allow action if client IP matches company office or MFA is active). |

### 7.2 The 2012-10-17 Version String Explained

In class, students often wonder: _"Why does every modern policy written in 2026 specify `"Version": "2012-10-17"`? Is this policy outdated?"_

- **The Explanation:** `"2012-10-17"` is **not** the creation date of your policy! It is the release date of the AWS IAM policy language specification engine itself.
- Prior to this version, AWS used the `"2008-10-17"` specification, which lacked support for advanced policy variables and complex condition evaluation.
- **Rule of Thumb:** Always write `"Version": "2012-10-17"`. If omitted or typed incorrectly, policy features will fail validation.

### 7.3 Explicit Deny vs Implicit Deny: The Evaluation Logic

AWS applies a rigorous, mathematically defined authorization logic for every single incoming API request:

```mermaid
flowchart TD
    REQ["Incoming API Request"] --> D1{"Is there an Explicit DENY?"}
    D1 -- "YES" --> DENIED["ACCESS DENIED (Overrides Everything!)"]
    D1 -- "NO" --> D2{"Is there an explicit ALLOW?"}
    D2 -- "YES" --> ALLOWED["ACCESS GRANTED"]
    D2 -- "NO" --> DEFAULT_DENY["ACCESS DENIED (Default Implicit Deny)"]
```

1. **Default State = Implicit Deny:** By default, every request is denied until an applicable policy explicitly allows it.
2. **Explicit Allow Grants Access:** If an attached policy has `"Effect": "Allow"` matching the action and resource, access is granted.
3. **Explicit Deny Trumps Everything:** If _any_ policy attached to the user, group, or resource contains an explicit `"Effect": "Deny"`, **the request is immediately blocked**, even if 10 other policies grant `"Allow"`.

### 7.4 Sample Policies Walkthrough

#### Example 1: Full Administrator Access (The Dangerous Powerhouse)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

_Allows every API call (`_`) across every AWS resource (`_`). Only senior DevOps/Cloud leads should ever hold this policy._

#### Example 2: Read-Only S3 Bucket & Object Access (Least Privilege)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowBucketListing",
      "Effect": "Allow",
      "Action": ["s3:ListAllMyBuckets"],
      "Resource": "*"
    },
    {
      "Sid": "AllowSpecificBucketObjectRead",
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::bongodev-assets",
        "arn:aws:s3:::bongodev-assets/*"
      ]
    }
  ]
}
```

_Allows the user to see the list of buckets, but strictly limits object reading to the `bongodev-assets` bucket._

---

## 8. Enterprise Identity Federation & Single Sign-On (SSO)

### 8.1 What is an Identity Provider (IdP)?

In small startups, administrators manually create individual IAM users inside the AWS Console. But in enterprise organizations with 500, 5,000, or 50,000 employees, manually creating separate IAM users for every employee across dozens of AWS accounts is an administrative and security nightmare.

An **Identity Provider (IdP)** is a centralized corporate directory service (e.g., Google Workspace, Microsoft Azure Active Directory / Entra ID, Okta, Ping Identity) where employee identities are centrally managed.

```mermaid
flowchart LR
    subgraph Enterprise["Corporate Enterprise Domain"]
        USER["Employee: rafi@company.com"]
        IDP["Corporate IdP (Google / Okta / Azure AD)"]
        USER -->|Single Corporate Login| IDP
    end

    subgraph AWSFederation["AWS Cloud Federation"]
        STS["AWS STS (Security Token Service)"]
        ROLE["IAM Federated Role: DeveloperRole"]
        RESOURCES["AWS Resources (EC2, S3, RDS)"]
        IDP -->|"SAML 2.0 / OIDC Assertion"| STS
        STS -->|"Issues Temporary Session Token"| ROLE
        ROLE -->|Authorized Access| RESOURCES
    end
```

### 8.2 SAML 2.0 & OIDC Federation

- **SAML 2.0 (Security Assertion Markup Language):** The enterprise standard XML-based protocol used to federate corporate directories with AWS.
- **OpenID Connect (OIDC):** Modern JSON/REST-based authentication used by platforms like GitHub Actions, GitLab CI, and Google to authenticate CI/CD pipelines directly with AWS without storing static access keys!

### 8.3 Why Modern Enterprises Avoid Standalone IAM Users

1. **Zero Long-Lived Static Credentials:** IAM Users have permanent passwords and access keys that can be leaked or stolen. Federated SSO relies exclusively on short-lived temporary STS tokens that expire automatically.
2. **Instant Deprovisioning:** When an employee leaves the company, HR deactivates their single corporate email account in Okta/Google. Instantly, all access to AWS, Jira, Slack, and GitHub is cut off in one click.

---

## 9. Hands-On Lab: Installing & Configuring the AWS CLI

### 9.1 Installation Steps (Linux & Windows)

#### On Ubuntu / Debian Linux:

```bash
# Download the official AWS CLI v2 installation package
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

# Unzip the installer package (install unzip if missing: sudo apt install unzip -y)
unzip awscliv2.zip

# Execute the installation script
sudo ./aws/install

# Verify installation and view version
aws --version
# Expected output: aws-cli/2.x.x Python/3.x.x Linux/x86_64...
```

#### On Windows (PowerShell):

```powershell
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi /qn
```

### 9.2 Creating CLI Access Keys in IAM

1. In the AWS Console, navigate to **IAM $\rightarrow$ Users $\rightarrow$ Select your User**.
2. Go to the **Security credentials** tab.
3. Scroll down to **Access keys** $\rightarrow$ Click **Create access key**.
4. Select use case: **Command Line Interface (CLI)**.
5. Acknowledge recommendations $\rightarrow$ Click **Next** $\rightarrow$ Click **Create access key**.
6. **CRITICAL STEP:** Immediately download the `.csv` file containing:
   - **Access Key ID:** A public identifier (e.g., `AKIAIOSFODNN7EXAMPLE`).
   - **Secret Access Key:** A private secret string (e.g., `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`).
   - _This is the only time AWS will ever display the Secret Access Key!_

### 9.3 Executing `aws configure`

Open your local terminal and execute:

```bash
aws configure
```

The interactive wizard prompts for four parameters:

```text
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: ap-south-1
Default output format [None]: json
```

### 9.4 Where Credentials Live: `~/.aws/credentials` and `~/.aws/config`

When `aws configure` completes, it creates two hidden files in your user home directory:

#### 1. `~/.aws/credentials` (Stores Secrets):

```ini
[default]
aws_access_key_id = AKIAIOSFODNN7EXAMPLE
aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

#### 2. `~/.aws/config` (Stores Region & Format):

```ini
[default]
region = ap-south-1
output = json
```

> **Security Warning:** The `~/.aws/credentials` file is unencrypted plaintext! Never commit this file to Git repositories, never share screenshots of it, and restrict local file permissions with `chmod 600 ~/.aws/credentials`.

### 9.5 Verifying Access: Querying EC2 and S3 via CLI

```bash
# 1. List all S3 buckets in your AWS account
aws s3 ls

# 2. Query running EC2 instances in your default region
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"

# 3. Retrieve your current authenticated caller identity
aws sts get-caller-identity
```

Output of `aws sts get-caller-identity`:

```json
{
  "UserId": "AIDAEXAMPLEUSERID",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/nure.siddiqui"
}
```

---

## 10. Real-World Gotchas, Security Traps & Troubleshooting

### Gotcha 1: Hardcoding AWS Access Keys in Git Repositories

- **The Disaster:** A junior engineer commits code containing hardcoded `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` strings to a public GitHub repo. Automated scanner bots scrape the keys within **3 to 10 seconds** and deploy massive EC2 cryptocurrency mining fleets.
- **The Remediation:**
  1. Immediately delete or deactivate the compromised access key in the AWS IAM Console.
  2. Audit AWS CloudTrail logs to inspect unauthorized resources created globally.
  3. Use **IAM Roles** instead of long-term access keys whenever possible.
  4. Install pre-commit secret scanners (e.g., `git-secrets`, `trufflehog`) to prevent accidental commits.

### Gotcha 2: The "Version": "2012-10-17" Policy Validation Error

- **The Issue:** An engineer attempts to save a custom IAM policy and writes today's date in the version string (`"Version": "2026-10-10"`). AWS rejects the policy with a syntax error.
- **The Cause:** AWS only recognizes `"2012-10-17"` and `"2008-10-17"` as valid language specification versions.
- **The Fix:** Always specify `"2012-10-17"`.

### Gotcha 3: The CLI Missing Default Region Trap

- **The Issue:** Running `aws ec2 describe-instances` returns an error: `You must specify a region. You can also configure your region by running "aws configure"`.
- **The Fix:** Run `aws configure` and specify a default region (e.g., `ap-south-1` or `us-east-1`), or append `--region ap-south-1` to the CLI command.

### Gotcha 4: Explicit Deny Overriding Group Allows

- **The Issue:** A user is added to the `Admins` group (which has `AdministratorAccess`), but they are still unable to delete S3 buckets.
- **The Cause:** A secondary policy attached to the user or an AWS Organizations Service Control Policy (SCP) contains an explicit `"Effect": "Deny", "Action": "s3:DeleteBucket"`.
- **The Fix:** Remember that an **Explicit Deny always overrules every Allow**. Inspect all attached inline and managed policies.

---

## 11. Questions Asked in Class (Q&A)

### Q1: Should a junior DevOps engineer or new intern be given full AdministratorAccess to the AWS Console?

**Answer:**
**Absolutely not.** Granting unrestricted administrator access to new team members is extremely risky. An accidental click or misconfigured script can terminate production databases or delete critical networking routes.

- **Best Practice:** Apply the **Principle of Least Privilege**. Start the engineer with a restricted group containing only the specific services they are currently working on (e.g., EC2 read/write, S3 read-only, CloudWatch logs). Elevate permissions only as their responsibilities expand.

---

### Q2: Why does an IAM Role not have an Access Key ID and Secret Access Key?

**Answer:**
Because **IAM Roles are designed for temporary, automated credential assumption**, not permanent storage.

- When an EC2 instance or Lambda function assumes a role, the **AWS Security Token Service (STS)** automatically generates a temporary session token with a short lifespan (15 minutes to 12 hours).
- The instance retrieves these temporary keys automatically from its internal Instance Metadata Service (`http://169.254.169.254/latest/meta-data/iam/security-credentials/`). If a hacker compromises the server hours later, the temporary credentials will have already expired!

---

### Q3: What is the practical difference between AWS Regions and AWS Local Zones?

**Answer:**

- An **AWS Region** is a full-scale geographic infrastructure hub with at least 3 distinct Availability Zones, massive server capacity, and the entire catalog of 240+ AWS services.
- An **AWS Local Zone** is a targeted extension of a parent region located inside a major metropolitan city that lacks a full region. It hosts a curated subset of core compute, storage, and database services to deliver ultra-low single-digit millisecond latency to local end-users.

---

### Q4: If an engineer already has a company Google Workspace email (`user@company.com`), why should we use Single Sign-On (SSO) instead of creating an IAM User?

**Answer:**
Security, governance, and centralized lifecycle management:

1. With SSO, the engineer logs in once using their corporate Google account (protected by corporate MFA).
2. The company does not manage hundreds of separate AWS passwords.
3. If the engineer resigns, the IT administrator suspends their Google Workspace account, and their access to all AWS environments vanishes instantaneously.

---

## 12. Common Misconceptions, Corrected

### Misconception 1: "AWS is responsible if my S3 bucket is publicly exposed or my database is deleted."

> **Correction:** Under the **AWS Shared Responsibility Model**, AWS is responsible for the security _of_ the cloud (physical infrastructure, hypervisors). You are 100% responsible for security _in_ the cloud (configuring S3 bucket policies, IAM access controls, database backup schedules). If you misconfigure your policy to allow public access, AWS is not liable.

### Misconception 2: "An IAM Role is just a group of IAM Users."

> **Correction:** An **IAM Group** is a collection of users. An **IAM Role** is an independent identity with temporary credentials that can be assumed by applications, services (like EC2 or Lambda), or cross-account federated users.

### Misconception 3: "The Version string in an IAM policy represents the date the policy was written."

> **Correction:** The `"Version": "2012-10-17"` string declares the grammar version of the AWS IAM policy language engine. It does not reflect when the policy author drafted the document.

### Misconception 4: "Local Zones and Edge Locations are the same thing."

> **Correction:** **Edge Locations** are caching points used strictly by CloudFront CDN and Route 53 DNS to cache static files and route traffic. **Local Zones** are compute and storage outposts where you can run live EC2 virtual instances and EBS volumes close to end-users.

---

## 13. Hands-On Practice Exercises

### Exercise 1: Hardening the AWS Root Account

1. Log into your AWS account using your Root email and password.
2. Navigate to the **IAM Console**.
3. Under **Security recommendations**, inspect the Root user status.
4. Set up an authenticator app (Google Authenticator, Authy, or 1Password) and enable **MFA for the Root user**.
5. Log out and verify you cannot log back in without supplying the 6-digit MFA time-based token.

### Exercise 2: Building an RBAC Group Structure

1. In IAM, create a User Group named `DevOps-Engineers`.
2. Attach the AWS-managed policy `AmazonEC2FullAccess` and `AmazonS3ReadOnlyAccess`.
3. Create a new IAM User named `junior.devops` with console password access enabled.
4. Add `junior.devops` to the `DevOps-Engineers` group.
5. Log into the AWS Console as `junior.devops` in an incognito window.
6. Verify you can view EC2 instances and read S3 buckets, but observe the permission error if you attempt to create an RDS database or delete an S3 bucket!

### Exercise 3: AWS CLI Configuration & Profile Query

1. Generate an Access Key pair for your IAM User.
2. Run `aws configure` in your local terminal and populate the keys.
3. Execute:
   ```bash
   aws sts get-caller-identity
   aws ec2 describe-regions --output table
   ```
4. Verify the output displays your user ARN and the global AWS regions table.

---

## 14. DevOps Interview Corner

### Q1: Explain the AWS IAM Policy Evaluation Logic when multiple policies apply.

**Model Answer:**
AWS starts with a default **Implicit Deny**. When an API request is received, AWS evaluates all applicable policies (Identity-based policies, Resource-based policies, Permission Boundaries, and SCPs):

1. If there is an **Explicit Deny** anywhere, the request is immediately denied, regardless of any allows.
2. If no explicit deny exists, AWS checks for an **Explicit Allow**. If found, the request is granted.
3. If neither an explicit deny nor an explicit allow is found, the default **Implicit Deny** takes effect, and access is refused.

---

### Q2: Why should an EC2 instance use an IAM Role instead of storing static IAM Access Keys?

**Model Answer:**
Storing static `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` credentials on an EC2 instance (in environment variables or configuration files) presents severe security vulnerabilities: keys can be accidentally committed to Git, leaked via directory traversals, or stolen by attackers.
By attaching an **IAM Role** to the instance profile, the instance leverages **AWS Security Token Service (STS)** to retrieve short-lived, automatically rotated credentials via the Instance Metadata Service (IMDS). This satisfies the principle of least privilege and eliminates credential lifecycle management.

---

### Q3: What is the architectural difference between AWS Availability Zones and AWS Local Zones?

**Model Answer:**
An **Availability Zone (AZ)** consists of one or more isolated physical data centers located within an AWS Region, interconnected via ultra-low latency fiber to provide multi-AZ high availability and fault tolerance.
An **AWS Local Zone** extends AWS infrastructure into major metropolitan centers where no AWS Region currently exists. It provides single-digit millisecond latency for compute, storage, and database workloads for latency-sensitive local applications (e.g., real-time streaming, gaming, financial trading).

---

### Q4: Explain the difference between IaaS, PaaS, and SaaS in terms of operational overhead.

**Model Answer:**

- **IaaS (e.g., EC2):** Maximum operational control, highest maintenance overhead. The customer must configure, patch, and secure the OS, runtime, and application stack.
- **PaaS (e.g., Elastic Beanstalk, Lambda):** Balanced overhead. The vendor manages the OS, runtime patching, server provisioning, and auto-scaling. The customer only manages the application code and data.
- **SaaS (e.g., QuickSight, Google Workspace):** Zero infrastructure overhead. The vendor completely manages the entire software stack, hosting, security, and updates; the customer operates strictly as an end-user.

---

### Q5: What is the purpose of OpenID Connect (OIDC) in modern CI/CD pipelines deploying to AWS?

**Model Answer:**
Historically, CI/CD tools (like GitHub Actions) required storing long-lived AWS IAM access keys as repository secrets. If compromised, attackers could exploit those keys indefinitely.
With **OIDC Federation**, GitHub Actions authenticates directly with AWS STS using a trusted cryptographic JWT token. AWS validates the token against the configured OIDC Identity Provider and issues temporary, short-lived STS credentials for the specific deployment workflow, entirely eliminating static secret storage in CI/CD pipelines.

---

## 15. Bilingual Technical Glossary

| English Term                           | Bengali Translation                       | Technical Definition & Practical Context                                                                                                                 |
| -------------------------------------- | ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Identity & Access Management (IAM)** | আইডেন্টিটি অ্যান্ড অ্যাক্সেস ম্যানেজমেন্ট | ক্লাউড রিসোর্সে কে প্রবেশ করতে পারবে (Authentication) এবং কী কী কাজ করতে পারবে (Authorization) তা নিয়ন্ত্রণকারী কেন্দ্রীয় সিকিউরিটি সার্ভিস।           |
| **Root User**                          | রুট ইউজার                                 | অ্যাকাউন্ট তৈরির মূল মালিক অ্যাকাউন্ট যার সবকিছুর ওপর সর্বময় ক্ষমতা থাকে; এটি নিত্যদিনের ইঞ্জিনিয়ারিং কাজে ব্যবহার সম্পূর্ণ নিষিদ্ধ।                   |
| **IAM User**                           | আইএএম ইউজার                               | কোনো ব্যক্তি বা টুলের জন্য তৈরি করা দীর্ঘমেয়াদী ক্লাউড আইডেন্টিটি, যা নিজস্ব পাসওয়ার্ড বা এপিআই অ্যাক্সেস কী দিয়ে কাজ করে।                            |
| **IAM User Group**                     | আইএএম ইউজার গ্রুপ                         | একই ধরনের দায়িত্ব পালনকারী একাধিক ইউজারের দল (যেমন: Developers, Admins); এখানে পারমিশন পলিসি একবার যুক্ত করলে সব ইউজার তা পায়।                         |
| **IAM Role**                           | আইএএম রোল                                 | কোনো স্থায়ী পাসওয়ার্ড বা চাবি ছাড়া এমন একটি আইডেন্টিটি যা কোনো সার্ভিস (যেমন EC2, Lambda) বা বাইরের ব্যবহারকারী সাময়িকভাবে গ্রহণ (Assume) করতে পারে। |
| **IAM Policy**                         | আইএএম পলিসি                               | জেসন (JSON) ফরম্যাটে লেখা আনুষ্ঠানিক নিয়মাবলী (Rule Book), যা স্পষ্ট করে দেয় কোন অ্যাকশন অনুমোদিত (Allow) এবং কোনটি নিষিদ্ধ (Deny)।                    |
| **Security Token Service (STS)**       | সিকিউরিটি টোকেন সার্ভিস                   | স্বল্পমেয়াদী, সাময়িক এবং মেয়াদোত্তীর্ণ হয়ে যাওয়ার মতো নিরাপদ ক্লাউড ক্রেডেনশিয়াল প্রদানকারী এডব্লিউএস সার্ভিস।                                     |
| **Single Sign-On (SSO)**               | সিঙ্গেল সাইন-অন                           | কোম্পানির একটিমাত্র কর্পোরেট ইমেইল ও পাসওয়ার্ড দিয়ে সমস্ত ক্লাউড অ্যাকাউন্ট ও সফটওয়্যারে কেন্দ্রীয়ভাবে লগইন করার আধুনিক পদ্ধতি।                      |
| **Identity Provider (IdP)**            | আইডেন্টিটি প্রোভাইডার                     | কেন্দ্রীয় ইউজার ডিরেক্টরি সিস্টেম (যেমন: Google Workspace, Okta, Azure AD) যা ব্যবহারকারীর পরিচয় সংরক্ষণ ও প্রমাণীকরণ করে।                             |
| **AWS CLI**                            | এডব্লিউএস কমান্ড লাইন ইন্টারফেস           | টার্মিনাল বা কমান্ড প্রম্পটে লিখে এডব্লিউএস ক্লাউড পরিচালনা ও অটোমেশন করার অফিশিয়াল টুল।                                                                |
| **Access Key ID & Secret Access Key**  | অ্যাক্সেস কী আইডি ও সিক্রেট অ্যাক্সেস কী  | টার্মিনাল এবং এপিআই-এর মাধ্যমে ইউজার ভেরিফিকেশনের জন্য ব্যবহৃত ইউজারনেম ও পাসওয়ার্ডের ক্রিপ্টোগ্রাফিক সমতুল্য গোপন চাবি।                                |
| **Availability Zone (AZ)**             | অ্যাভেইলেবিলিটি জোন                       | একটি রিজিয়নের ভেতরের ভৌগোলিকভাবে আলাদা ও ফাইবার অপটিক দিয়ে যুক্ত এক বা একাধিক স্বাধীন ডাটা সেন্টার।                                                     |
| **Local Zone**                         | লোকাল জোন                                 | মূল রিজিয়নের বাইরে বড় বড় মেট্রোপলিটন শহরের কাছে স্থাপিত ক্লাউড অবকাঠামো, যা আল্ট্রা-লো লেটেন্সি নিশ্চিত করে।                                           |
| **AWS Outposts**                       | এডব্লিউএস আউটপোস্টস                       | কোম্পানির নিজস্ব প্রাইভেট ডাটা সেন্টারে স্থাপন করার জন্য এডব্লিউএস-এর তৈরি ফিজিক্যাল সার্ভার র‍্যাক (হাইব্রিড ক্লাউড)।                                   |
| **AWS Wavelength**                     | এডব্লিউএস ওয়েভলেংথ                       | ৫জি (5G) মোবাইল নেটওয়ার্ক ক্যারিয়ারের ডাটা সেন্টারের ভেতরে বসানো এডব্লিউএস কম্পিউট অবকাঠামো।                                                           |

---

## 16. One-Page AWS IAM & CLI Cheat Sheet

### Essential AWS CLI Commands

```bash
# 1. Configure or update local AWS CLI credentials
aws configure

# 2. Verify current authenticated caller identity
aws sts get-caller-identity

# 3. List all IAM users in the AWS account
aws iam list-users

# 4. List all S3 buckets
aws s3 ls

# 5. List running EC2 instances with concise output
aws ec2 describe-instances \
    --filters "Name=instance-state-name,Values=running" \
    --query "Reservations[*].Instances[*].[InstanceId,InstanceType,PublicIpAddress]" \
    --output table

# 6. List all available AWS Regions
aws ec2 describe-regions --output table
```

### IAM JSON Policy Boilerplate Template

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DescriptiveStatementName",
      "Effect": "Allow",
      "Action": ["service:Action1", "service:Action2"],
      "Resource": "arn:aws:service:region:account-id:resource-type/resource-id"
    }
  ]
}
```

### Local AWS Credentials File Paths

| Operating System  | Credentials File Path            | Configuration File Path     |
| ----------------- | -------------------------------- | --------------------------- |
| **Linux & macOS** | `~/.aws/credentials`             | `~/.aws/config`             |
| **Windows**       | `%USERPROFILE%\.aws\credentials` | `%USERPROFILE%\.aws\config` |

---

_End of Study Notes — AWS Class 02_
