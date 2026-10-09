# AWS Cloud & Infrastructure Automation — Class 01 Study Notes

**Topic:** Foundation of Cloud Computing, AWS Infrastructure, EC2 Deep Dive, User Data Automation, and Fleet Provisioning with Launch Templates  
**Covers:** On-Premises vs Cloud, CapEx vs OpEx, AWS Global Infrastructure (Regions & Availability Zones), Amazon EC2 Architecture, AMI Blueprints, Instance Types & Sizing, EBS Block Storage vs RAM, Security Groups & Network Firewalls, SSH Asymmetric Cryptography (`.pem` keys & `chmod 400`), Automated Bootstrapping via User Data (cloud-init), and Scalable Fleet Provisioning via Launch Templates (Versioning v1, v2, v3)  
**Course & Class:** BongoDev DevOps & Cloud Engineering — Cloud Track: AWS Class 01 (Recorded 9 October 2026)  
**Instructor:** Nure Siddiqui  
**Builds on:** Linux Fundamentals (permissions, package managers, shell scripts), Computer Networking (IP addressing, TCP ports, SSH handshake), and Python/CI-CD workflows  
**Next Steps:** VPC Deep Dive (Subnets, Route Tables, Internet Gateways, NAT Gateways), IAM (Users, Roles, Policies), Infrastructure as Code with Terraform, and Ansible Fleet Management

> **About these study notes:** These notes are compiled directly from the live class recording and curriculum materials. Every architectural concept, console configuration, bash bootstrap script, troubleshooting scenario, and student inquiry discussed in class is documented with meticulous precision. Where the live session encountered real-world console quirks (e.g., the Launch Template EBS volume validation bug or SSH permission errors), dedicated **Bug Alerts** and **Precision Notes** provide the underlying root causes, workarounds, and production-grade best practices.

---

## Table of Contents

1. [Recap and Today's Roadmap](#1-recap-and-todays-roadmap)
2. [Why Cloud? The Industry Landscape & Market Realities](#2-why-cloud-the-industry-landscape--market-realities)
3. [On-Premises vs Cloud Computing: An Architectural Breakdown](#3-on-premises-vs-cloud-computing-an-architectural-breakdown)
   - 3.1 [What is On-Premises (On-Prem)?](#31-what-is-on-premises-on-prem)
   - 3.2 [The Four Fatal Bottlenecks of On-Premises](#32-the-four-fatal-bottlenecks-of-on-premises)
   - 3.3 [CapEx vs OpEx & The Pay-As-You-Go Economic Model](#33-capex-vs-opex--the-pay-as-you-go-economic-model)
4. [AWS Global Infrastructure: Regions & The "Kolatoli Beach" Analogy](#4-aws-global-infrastructure-regions--the-kolatoli-beach-analogy)
   - 4.1 [What is an AWS Region?](#41-what-is-an-aws-region)
   - 4.2 [The Default Trap: Why us-east-1 is the "Kolatoli Beach" of AWS](#42-the-default-trap-why-us-east-1-is-the-kolatoli-beach-of-aws)
   - 4.3 [Strategic Region Selection: Latency, Compliance, and Cost](#43-strategic-region-selection-latency-compliance-and-cost)
5. [Amazon EC2 (Elastic Compute Cloud): Anatomy of a Virtual Server](#5-amazon-ec2-elastic-compute-cloud-anatomy-of-a-virtual-server)
   - 5.1 [Why is it called EC2 (Elastic Compute Cloud)?](#51-why-is-it-called-ec2-elastic-compute-cloud)
   - 5.2 [AMI (Amazon Machine Image): The Immutable OS Blueprint](#52-ami-amazon-machine-image-the-immutable-os-blueprint)
   - 5.3 [Instance Types & Hardware Families: vCPU vs RAM](#53-instance-types--hardware-families-vcpu-vs-ram)
   - 5.4 [Storage Architecture: EBS (Elastic Block Store) vs System RAM](#54-storage-architecture-ebs-elastic-block-store-vs-system-ram)
6. [Network Security & Access Control: Security Groups Deep Dive](#6-network-security--access-control-security-groups-deep-dive)
   - 6.1 [What is an AWS Security Group?](#61-what-is-an-aws-security-group)
   - 6.2 [Inbound Rules vs Outbound Rules](#62-inbound-rules-vs-outbound-rules)
   - 6.3 [Standard Ports Demystified (22, 80, 443, 25, 27017)](#63-standard-ports-demystified-22-80-443-25-27017)
   - 6.4 [Least Privilege Access: "My IP" vs 0.0.0.0/0](#64-least-privilege-access-my-ip-vs-000000)
7. [SSH Cryptography & Key Management: Public vs Private Keys](#7-ssh-cryptography--key-management-public-vs-private-keys)
   - 7.1 [How Asymmetric Key Pairs Work](#71-how-asymmetric-key-pairs-work)
   - 7.2 [The Linux File Permission Requirement: `chmod 400`](#72-the-linux-file-permission-requirement-chmod-400)
   - 7.3 [The Handshake: Connecting from the Terminal](#73-the-handshake-connecting-from-the-terminal)
8. [Automated Provisioning: Bootstrap Scripts with EC2 User Data](#8-automated-provisioning-bootstrap-scripts-with-ec2-user-data)
   - 8.1 [What is User Data (cloud-init)?](#81-what-is-user-data-cloud-init)
   - 8.2 [Writing the Bootstrap Bash Script](#82-writing-the-bootstrap-bash-script)
   - 8.3 [The Non-Interactive Flag: Why `-y` is Life or Death in DevOps](#83-the-non-interactive-flag-why--y-is-life-or-death-in-devops)
   - 8.4 [Verifying Nginx on the Public Internet](#84-verifying-nginx-on-the-public-internet)
9. [Scaling Out: Launch Templates & Fleet Automation](#9-scaling-out-launch-templates--fleet-automation)
   - 9.1 [The Problem: Provisioning 50 Servers by Hand](#91-the-problem-provisioning-50-servers-by-hand)
   - 9.2 [The Analogy: Cupcake Molds & Brick Factory Dies](#92-the-analogy-cupcake-molds--brick-factory-dies)
   - 9.3 [Building a Launch Template (Version 1)](#93-building-a-launch-template-version-1)
   - 9.4 [Modifying & Versioning (Version 2 & 3)](#94-modifying--versioning-version-2--3)
   - 9.5 [Launching Multiple Instances Simultaneously from a Template](#95-launching-multiple-instances-simultaneously-from-a-template)
10. [Real-World Gotchas, Bugs, & Troubleshooting in Class](#10-real-world-gotchas-bugs--troubleshooting-in-class)
11. [Questions Asked in Class (Q&A)](#11-questions-asked-in-class-qa)
12. [Common Misconceptions, Corrected](#12-common-misconceptions-corrected)
13. [Hands-On Practice Exercises](#13-hands-on-practice-exercises)
14. [DevOps Interview Corner](#14-devops-interview-corner)
15. [Bilingual Technical Glossary](#15-bilingual-technical-glossary)
16. [One-Page AWS EC2 Cheat Sheet](#16-one-page-aws-ec2-cheat-sheet)

---

## 1. Recap and Today's Roadmap

Over the preceding weeks, the course laid down foundational engineering pillars:

1. **Git & GitHub:** Version control, distributed branching strategies, PR workflows, and CI/CD basics.
2. **Linux Operating System:** File hierarchies, process management, users, groups, POSIX permissions, package managers (`apt`), and bash scripting.
3. **Computer Networking:** OSI layers, IP addressing (IPv4/CIDR), routing, TCP/UDP sockets, DNS resolution, and standard port protocols.
4. **Python Automation:** Data structures, modular programming, REST APIs, JSON data pipelines, database CRUD operations, and debugging.

Now, we enter **Infrastructure & Cloud Engineering**. We transition from running code locally or in containers to building, provisioning, securing, and automating enterprise-grade cloud infrastructure on **Amazon Web Services (AWS)**.

### Today's Learning Journey

```
On-Premises Limitations (Hardware, Lead Time, CapEx)
      ↓
The Cloud Paradigm (OpEx, Pay-As-You-Go, Elasticity)
      ↓
AWS Global Architecture (Regions, Default us-east-1, Latency)
      ↓
Amazon EC2 Core Anatomy (AMI Blueprint, vCPU/RAM, EBS Volumes)
      ↓
Network Security (Security Groups, Inbound/Outbound Rules, Open Ports)
      ↓
Asymmetric Cryptography (SSH Key Pairs, chmod 400, Private vs Public Keys)
      ↓
Automated Bootstrapping (User Data, cloud-init, Non-interactive -y)
      ↓
Fleet Automation (Launch Templates, Versioning v1/v2/v3, Mass Deployment)
```

---

## 2. Why Cloud? The Industry Landscape & Market Realities

### The Exponential Rise of Cloud Infrastructure

Why has cloud computing become non-negotiable for modern software engineering?
Three converging technological shifts have accelerated cloud adoption along a steep trajectory:

1. **Microservices Architecture:** Instead of deploying a single monolithic codebase onto one server, applications are decomposed into dozens or hundreds of independent microservices. Provisioning and maintaining dedicated physical servers for every microservice is financially and operationally impossible.
2. **Container Orchestration (Kubernetes):** Running production Kubernetes clusters requires dynamic node auto-scaling, automated control plane maintenance, elastic load balancers, and persistent network storage volumes—capabilities inherently native to cloud platforms.
3. **AI & Machine Learning Integration:** Modern software integrates Large Language Models (LLMs), GPU clusters, and vector databases. Procuring enterprise-grade GPUs (e.g., NVIDIA H100s/A100s) on-premise requires astronomical capital investments and months of procurement lead times. In the cloud, specialized AI/compute instances can be spun up on-demand for hours and terminated when tasks complete.

### Market Share & Industry Dominance

The global cloud infrastructure market exceeds **$190+ Billion USD** annually and continues to expand rapidly. The major cloud providers are:

| Cloud Provider                             | Market Share (Approx.) | Primary Characteristics & Strengths                                                                                          |
| ------------------------------------------ | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **Amazon Web Services (AWS)**              | **~28% – 32%**         | Pioneer of public cloud (launched 2006); largest catalog of services (200+); dominant enterprise adoption; global footprint. |
| **Microsoft Azure**                        | **~20% – 23%**         | Deep integration with Microsoft enterprise ecosystem (Active Directory, Windows Server, Office 365, OpenAI partnership).     |
| **Google Cloud Platform (GCP)**            | **~10% – 12%**         | Industry leader in data engineering, analytics, Kubernetes (Borg heritage), and AI/ML infrastructure (TPUs, Gemini).         |
| **Others (Alibaba, Oracle, Tencent, IBM)** | Remainder              | Regional dominance (Alibaba in Asia) or niche enterprise databases (Oracle Cloud Infrastructure).                            |

> **DevOps vs Cloud Engineer Roles:** While DevOps engineers focus heavily on end-to-end software delivery pipelines (CI/CD, observability, containers, automation), **Cloud Engineering** has evolved into a highly lucrative, standalone specialization focused on architecture, networking, security, cost optimization, and infrastructure provisioning.

---

## 3. On-Premises vs Cloud Computing: An Architectural Breakdown

### 3.1 What is On-Premises (On-Prem)?

In a traditional **On-Premises** setup, an organization hosts its compute, storage, and networking hardware within its own physical premises—typically a dedicated server room or private corporate data center.

Historical enterprise workloads hosted on-premise include:

- **ERP (Enterprise Resource Planning) Systems:** SAP, Oracle Applications.
- **CRM (Customer Relationship Management) Platforms:** Customer tracking and sales databases.
- **Internal File & Directory Servers:** Active Directory domain controllers, internal NAS/SAN storage.
- **Core Banking Systems:** Historically kept on-prem due to strict compliance, data sovereignty, and security mandates.

Prominent hardware and private cloud vendors include **HPE GreenLake, Oracle Bare Metal Servers, NetForChoice, Joyent Triton, and Dell PowerEdge**.

```
Traditional On-Premises Architecture:
┌────────────────────────────────────────────────────────┐
│ Corporate Office Server Room                          │
│                                                        │
│  ┌────────────┐   ┌────────────┐   ┌────────────┐      │
│  │ Server Rack│   │ SAN/NAS    │   │ Core       │      │
│  │ (Physical) │   │ (Storage)  │   │ Switch/FW  │      │
│  └──────┬─────┘   └──────┬─────┘   └──────┬─────┘      │
│         │                │                │            │
│  ───────┴────────────────┴────────────────┴──────────  │
│  HVAC Cooling Systems | Diesel Generators | UPS Units  │
│  Physical Access Badges | Fire Suppression Systems     │
└────────────────────────────────────────────────────────┘
```

### 3.2 The Four Fatal Bottlenecks of On-Premises

1. **Massive Procurement Lead Times (Supply Chain Dependency):**
   When an enterprise decides to launch a new service or needs additional compute capacity, hardware cannot be obtained immediately. Servers, fiber optic switches, RAID controllers, and hardware firewalls must be formally budgeted, approved, ordered from international manufacturers (e.g., China or the US), shipped across borders, cleared through customs, physically racked, cabled, and configured. This cycle regularly takes **4 to 12 weeks**. In dynamic software markets, a 3-month wait to deploy an application can kill a business.

2. **Inflexible Capacity Planning & Scaling Bottlenecks:**
   Workloads in software are inherently fluctuating. Consider an e-commerce platform during an Eid or Black Friday campaign where traffic surges by 1,000%.
   - Under On-Premises: You must purchase enough hardware to sustain the _absolute peak_ traffic.
   - The Economic Penalty: For the remaining 360 days of the year, those expensive machines sit idle in the server room consuming power, air conditioning, and depreciation—generating zero business value.

3. **Heavy Operational Maintenance (Hardware Depreciation & Failures):**
   Physical hardware fails constantly. Hard drives develop bad sectors, power supply units (PSUs) blow out, cooling fans seize, and backup tape drives degrade. The organization must employ round-the-clock physical IT staff, maintain cold-spare inventories, service HVAC (heating, ventilation, air conditioning) systems, and maintain diesel generator backups.

4. **Painful Disaster Recovery (DR) and Backup Complexity:**
   Backing up multi-terabyte production databases on-premise requires expensive tape libraries or secondary data center mirroring over dedicated leased lines. If a physical disaster strikes the headquarters (fire, flood, electrical grid failure), the business suffers catastrophic downtime.

### 3.3 CapEx vs OpEx & The Pay-As-You-Go Economic Model

The transition to cloud is fundamentally an architectural and economic revolution:

| Dimension                 | On-Premises Infrastructure                                                                                                                               | Cloud Computing (AWS)                                                                                                                      |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Financial Accounting**  | **CapEx (Capital Expenditure)**: Large upfront cash investment in hardware, data center leases, and facilities before making a single dollar in revenue. | **OpEx (Operational Expenditure)**: Zero upfront hardware costs. Infrastructure is treated as an operational operating utility expense.    |
| **Pricing Model**         | Sunk cost; fixed asset depreciation over 3–5 years regardless of actual utilization.                                                                     | **Pay-As-You-Go**: Billed per second or per hour for the exact resources consumed. Stop the server, and compute billing halts immediately. |
| **Provisioning Velocity** | Weeks or months to order, rack, cable, and configure physical hardware.                                                                                  | **Minutes or seconds**: Spin up virtual instances via Web Console, CLI, API, or Terraform.                                                 |
| **Elasticity**            | Static capacity; scaling requires purchasing new physical racks.                                                                                         | **Dynamic & Elastic**: Scale horizontally (add servers) or vertically (resize compute) automatically on demand.                            |
| **Hardware Maintenance**  | 100% customer responsibility (HVAC, power, cables, disk replacements).                                                                                   | 100% provider responsibility (AWS manages the physical data centers and underlying silicon).                                               |

---

## 4. AWS Global Infrastructure: Regions & The "Kolatoli Beach" Analogy

### 4.1 What is an AWS Region?

An **AWS Region** is a distinct, separate geographic area in the world where AWS clusters physical data centers.

- Examples: `us-east-1` (N. Virginia), `us-west-2` (Oregon), `ap-south-1` (Mumbai), `ap-southeast-1` (Singapore), `eu-west-1` (Ireland).
- Every AWS Region is completely isolated from other regions. This design guarantees the highest possible fault tolerance, stability, and failure isolation.
- Each Region contains **multiple Availability Zones (AZs)** (typically 3 or more). An AZ consists of one or more discrete physical data centers with redundant power, networking, and cooling, connected through low-latency fiber links.

```
AWS Global Infrastructure Hierarchy:
┌─────────────────────────────────────────────────────────────────┐
│ AWS Global Infrastructure                                       │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │ Region: ap-south-1 (Mumbai)                             │   │
│   │                                                         │   │
│   │   ┌───────────────┐  ┌───────────────┐  ┌─────────────┐ │   │
│   │   │ AZ: ap-south-1a│  │ AZ: ap-south-1b│  │AZ:ap-south-1c││   │
│   │   │ [Data Center] │  │ [Data Center] │  │[Data Center]││   │
│   │   └───────────────┘  └───────────────┘  └─────────────┘ │   │
│   │           ▲                  ▲                  ▲       │   │
│   │           └────────── High-Speed Fiber ─────────┘       │   │
│   └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### 4.2 The Default Trap: Why us-east-1 is the "Kolatoli Beach" of AWS

In class, the instructor introduced a brilliant and relatable analogy: **Kolatoli Beach in Cox's Bazar**.

When tourists travel to Cox's Bazar in Bangladesh and get off the bus, the rickshaw pullers instinctively take every newcomer straight to Kolatoli Beach because it is right by the main bus stand. It is the most crowded, most congested beach where everyone gathers by default.

**`us-east-1` (N. Virginia) is the "Kolatoli Beach" of AWS:**

1. **The Default Landing Spot:** When you create an AWS account and log into the AWS Management Console for the first time, AWS automatically drops your session into `us-east-1`. Beginners who do not know better end up launching all their workloads there.
2. **Oldest & Most Feature-Rich:** N. Virginia was AWS's very first region (launched in 2006). Almost all new AWS services and features are deployed to `us-east-1` first.
3. **Heavy Traffic & Congestion:** Because it hosts an enormous concentration of global enterprise workloads, major outages or platform throttling frequently originate in `us-east-1`.

### 4.3 Strategic Region Selection: Latency, Compliance, and Cost

As a professional DevOps or Cloud Engineer, you must never deploy infrastructure blindly into the default region. Region selection is guided by three core engineering factors:

1. **User Proximity & Latency:**
   - Network packets travel through undersea fiber optic cables at the speed of light. Distance directly introduces network latency (ping times).
   - If your application serves a user base in **Bangladesh** (e.g., a real-time food delivery app like Pathao or Foodpanda):
     - Connecting to `us-east-1` (Virginia) introduces **~220ms – 280ms** round-trip network latency. This sluggishness degrades user experience.
     - Deploying to `ap-south-1` (**Mumbai**) or `ap-southeast-1` (**Singapore**) cuts network latency down to **~35ms – 65ms**.
2. **Data Sovereignty & Legal Compliance:**
   - Many countries enforce data residency regulations (e.g., GDPR in Europe, RBI guidelines in India, central bank mandates in Bangladesh). Financial records, patient medical records, and citizen personal data must physically reside within national or approved regional boundaries.
3. **Cost Variation:**
   - AWS charges different prices across regions due to local electricity, real estate, taxation, and labor expenses. An instance running in `us-east-1` may cost slightly less per hour than the same instance running in `ap-southeast-1` or `sa-east-1` (São Paulo).

---

## 5. Amazon EC2 (Elastic Compute Cloud): Anatomy of a Virtual Server

### 5.1 Why is it called EC2 (Elastic Compute Cloud)?

The name originates from **E**lastic **C**ompute **C**loud (**E** with **two C's = EC2**):

- **Elastic:** The capacity can be dynamically stretched or shrunken on demand. If CPU spikes, scale up; when idle, scale down.
- **Compute:** Provides raw compute power (CPU cores and RAM) to execute instructions.
- **Cloud:** Hosted virtually on AWS global data center infrastructure.

At its core, an **EC2 instance** is an on-demand virtual machine (VM) powered by modern virtualization hypervisors (historically Xen, now the cutting-edge **AWS Nitro System**).

```
Physical Hardware vs Virtualization (AWS Nitro Architecture):
┌────────────────────────────────────────────────────────┐
│ EC2 Instance 1 (t3.micro)  │ EC2 Instance 2 (c5.large) │
│ [Guest OS: Ubuntu 24.04]   │ [Guest OS: Amazon Linux 2]│
├────────────────────────────┴───────────────────────────┤
│ AWS Nitro Hypervisor (Isolated Virtualization)         │
├────────────────────────────────────────────────────────┤
│ Physical Bare-Metal Server (Custom Intel/AMD/Graviton) │
│ High-Density DDR5 RAM | High-Speed PCIe Bus            │
└────────────────────────────────────────────────────────┘
```

### 5.2 AMI (Amazon Machine Image): The Immutable OS Blueprint

When creating a virtual server, AWS must know what operating system and initial disk configuration to clone onto the instance. This blueprint is called an **AMI (Amazon Machine Image)**.

- An AMI contains:
  1. A root volume template (operating system, kernel, system utilities).
  2. Launch permissions controlling which AWS accounts can use the AMI.
  3. Block device mappings determining default drive attachments.
- **AMI ID:** Every AMI possesses a globally unique identifier within its region, e.g., `ami-0c7217cdde317cfec` (Ubuntu 24.04 LTS in `us-east-1`).
- **Why the AMI ID is critical for DevOps:**
  In automated provisioning tools like **Terraform, CloudFormation, or Packer**, you never click a GUI dropdown. You supply the exact `ami-xxxxxxxx` string to tell AWS which OS blueprint to instantiate.

> **Class OS Standard:** Across 99% of modern DevOps workflows and throughout this course, the standard distribution used is **Ubuntu Linux LTS (Long Term Support)** due to its stability, extensive package ecosystem, and ubiquitous industry adoption.

### 5.3 Instance Types & Hardware Families: vCPU vs RAM

AWS categorizes EC2 instances into standardized families tailored for specific computing workloads. Instance names follow a clear syntactic naming convention:

$$\text{Instance Type} = \text{[Family]}\text{[Generation]}\text{.}\text{[Size]}$$

Example: `t3.medium` $\rightarrow$ Family `t` (General Purpose Burstable), Generation `3`, Size `medium` (2 vCPU, 4 GiB RAM).

| Instance Family | Purpose                     | Free Tier Eligible?     | Typical Configuration | Use Cases                                    |
| --------------- | --------------------------- | ----------------------- | --------------------- | -------------------------------------------- |
| **t2.nano**     | Ultra-lightweight burstable | No                      | 1 vCPU, 0.5 GiB RAM   | Microservices, dev testing                   |
| **t2.micro**    | General Purpose Burstable   | **YES (750 hrs/month)** | 1 vCPU, 1.0 GiB RAM   | Learning, light testing, bastion hosts       |
| **t2.small**    | Burstable General Purpose   | No                      | 1 vCPU, 2.0 GiB RAM   | Small staging APIs, background workers       |
| **t3.large**    | Next-gen Burstable          | No                      | 2 vCPU, 8.0 GiB RAM   | Production microservices, Docker nodes       |
| **c5.large**    | Compute Optimized           | No                      | 2 vCPU, 4.0 GiB RAM   | High-traffic web servers, batch processing   |
| **m5.large**    | Balanced General Purpose    | No                      | 2 vCPU, 8.0 GiB RAM   | Enterprise databases, app backends           |
| **r5.large**    | Memory Optimized            | No                      | 2 vCPU, 16.0 GiB RAM  | In-memory caches (Redis), big data analytics |

```
Instance Sizing Comparison:
t2.nano   [ 0.5 GB RAM ] 1 vCPU  ($)
t2.micro  [ 1.0 GB RAM ] 1 vCPU  ($ - Free Tier)
t2.small  [ 2.0 GB RAM ] 1 vCPU  ($$)
t3.large  [ 8.0 GB RAM ] 2 vCPU  ($$$$)
c5.large  [ 4.0 GB RAM ] 2 vCPU (High Compute Clock) ($$$$)
```

### 5.4 Storage Architecture: EBS (Elastic Block Store) vs System RAM

In class, the instructor emphasized an essential distinction that frequently trips up beginners: **Memory vs Storage**.

```
                ┌───────────────────────────────────┐
                │          THE EC2 INSTANCE         │
                │                                   │
                │  ┌─────────────┐  ┌─────────────┐ │
                │  │    vCPU     │  │  RAM/Memory │ │
                │  │ (Execution) │  │ (Volatile)  │ │
                │  └──────┬──────┘  └──────┬──────┘ │
                └─────────┼────────────────┼────────┘
                          │                │
            Virtual PCIe  ▼                ▼
       ┌────────────────────────────────────────┐
       │   Amazon EBS Volume (Persistent Disk)  │
       │   - 12 GB / 15 GB / 500 GB             │
       │   - Survives instance reboot & stop    │
       │   - Network-attached virtual HDD/SSD   │
       └────────────────────────────────────────┘
```

1. **System Memory (RAM):**
   - **Volatile workspace:** Holds active program code, running kernel data structures, and executing application variables.
   - When the instance is stopped or powered off, all data in RAM disappears completely.
   - Sizing: typically 1 GB to 64 GB depending on instance type.
2. **EBS Volume (Elastic Block Store):**
   - **Non-volatile storage:** The virtual hard drive (SSD/HDD) of your virtual machine.
   - Stores the operating system filesystem (`/`, `/bin`, `/var`, `/home`), installed software packages, and permanent files.
   - Persists data independently of the instance lifecycle. If you stop and restart the instance, your EBS volume remains fully intact.
   - Default minimum root volume for Ubuntu is 8 GiB (configured to 12 GiB and 15 GiB in class demonstrations).
   - Storage types: `gp2` / `gp3` (General Purpose SSDs).

---

## 6. Network Security & Access Control: Security Groups Deep Dive

### 6.1 What is an AWS Security Group?

A **Security Group (SG)** acts as a **virtual stateful firewall** at the instance level. It directly controls inbound and outbound network traffic to and from your EC2 instance.

Key Architectural Characteristics:

1. **Stateful Filtering:** If an inbound request is permitted through the security group, the return response traffic is automatically allowed out, regardless of outbound rules.
2. **Default Deny:** By default, a newly created security group blocks all incoming traffic from the public internet until explicit **Allow Rules** are added.
3. **Hypervisor-Level Enforcement:** Security groups operate at the AWS network virtualization layer _outside_ the guest OS. Packets blocked by a security group never reach the instance's network card, shielding the CPU from malicious connection floods.

```
Incoming Internet Traffic
          │
          ▼
┌──────────────────────────────────────────────┐
│ AWS Security Group (Stateful Firewall)       │
│                                              │
│  Port 22  (SSH)   ──► ALLOW (My IP Only)    │
│  Port 80  (HTTP)  ──► ALLOW (0.0.0.0/0)      │
│  Port 443 (HTTPS) ──► ALLOW (0.0.0.0/0)      │
│  Port 3306(MySQL) ──► DENY  (Blocked)        │
└──────────────────────┬───────────────────────┘
                       │ Allowed Packets
                       ▼
         ┌───────────────────────────┐
         │       EC2 Instance        │
         │     Ubuntu OS + Nginx     │
         └───────────────────────────┘
```

### 6.2 Inbound Rules vs Outbound Rules

- **Inbound Rules:** Define what external traffic is permitted to enter the instance. You must specify:
  1. **Protocol:** TCP, UDP, ICMP, or All.
  2. **Port Range:** The specific destination port (e.g., 22, 80, 443).
  3. **Source IP/CIDR:** Which client IP addresses are authorized to connect.
- **Outbound Rules:** Define what traffic the instance can initiate outward to the internet (e.g., reaching Ubuntu repositories to download updates). By default, AWS allows all outbound traffic (`0.0.0.0/0`).

### 6.3 Standard Ports Demystified (22, 80, 443, 25, 27017)

Network services listen on standard, standardized ports registered with the **Internet Assigned Numbers Authority (IANA)**:

| Port Number | Protocol / Service                       | Security Group Setting              | Plain-Language Purpose                                                      |
| ----------- | ---------------------------------------- | ----------------------------------- | --------------------------------------------------------------------------- |
| **22**      | **SSH (Secure Shell)**                   | Restrict to **My IP**               | Secure encrypted command-line terminal access to manage Linux servers.      |
| **80**      | **HTTP (HyperText Transfer Protocol)**   | Open to **0.0.0.0/0** (Anywhere)    | Standard unencrypted web traffic. Used to view web pages in a browser.      |
| **443**     | **HTTPS (HTTP Secure / TLS)**            | Open to **0.0.0.0/0** (Anywhere)    | Encrypted SSL/TLS web traffic. Standard for all modern production websites. |
| **25**      | **SMTP (Simple Mail Transfer Protocol)** | Restricted / Internal               | Email server communication and dispatch.                                    |
| **3306**    | **MySQL Database**                       | Restricted to App Server Private IP | Relational database client connections. **Never expose to 0.0.0.0/0!**      |
| **27017**   | **MongoDB Database**                     | Restricted to App Server Private IP | NoSQL document database connections. **Never expose to 0.0.0.0/0!**         |

### 6.4 Least Privilege Access: "My IP" vs 0.0.0.0/0

A fundamental tenet of DevOps and DevSecOps is the **Principle of Least Privilege**.

- **The Security Vulnerability:** Leaving Port 22 open to `0.0.0.0/0` (the entire internet) exposes your server to thousands of automated botnets executing relentless brute-force password and dictionary attacks every second.
- **The Best Practice:** Set the Port 22 SSH rule source to **"My IP"**. AWS automatically queries your current external public IP address and creates a rule formatted as `<your-public-ip>/32`. Only traffic originating from your physical internet connection can attempt an SSH handshake.

> **Dynamic IP Trap:** In residential broadband setups (common in Bangladesh and worldwide), internet service providers assign dynamic IP addresses. If your home router restarts or your connection drops, your public IP changes, and AWS Security Group will block your SSH connection!
> **The Fix:** Log into the AWS Console $\rightarrow$ EC2 $\rightarrow$ Security Groups $\rightarrow$ Edit Inbound Rules $\rightarrow$ Change Port 22 source back to "My IP" (or temporarily allow the new office/mobile IP).

---

## 7. SSH Cryptography & Key Management: Public vs Private Keys

### 7.1 How Asymmetric Key Pairs Work

AWS EC2 instances do not utilize insecure root passwords by default. Instead, access is authenticated using **Asymmetric Public-Key Cryptography (RSA)**:

```
               ┌───────────────────────────────┐
               │    AWS Key Pair Generation    │
               │    (e.g., nure-pc.pem)        │
               └──────────────┬────────────────┘
                              │
             ┌────────────────┴────────────────┐
             ▼                                 ▼
   ┌───────────────────┐             ┌───────────────────┐
   │    Private Key    │             │    Public Key     │
   │    (nure-pc.pem)  │             │  (OpenSSH format) │
   └─────────┬─────────┘             └─────────┬─────────┘
             │ Stays on                        │ Injected by AWS
             ▼ your computer                   ▼ into VM
   ┌───────────────────┐             ┌───────────────────┐
   │ Local Client      │   SSH       │ EC2 Remote Server │
   │ (~/Downloads/)    ├────────────►│ ~/.ssh/           │
   │ Private Key Stored│ Handshake   │ authorized_keys   │
   └───────────────────┘             └───────────────────┘
```

1. **The Private Key (`.pem` file):**
   - Generated when you create an EC2 key pair.
   - Downloaded to your local development machine (e.g., `nure-pc.pem`).
   - **Crucial Rule:** The private key **NEVER leaves your machine**. It is never sent over the wire, never uploaded to GitHub, and never copied onto the server.
2. **The Public Key:**
   - Retained by AWS.
   - When EC2 provisions the Ubuntu instance, AWS automatically injects this public key into the instance's `/home/ubuntu/.ssh/authorized_keys` file.
3. **The Cryptographic Handshake:**
   - When you initiate `ssh -i nure-pc.pem ubuntu@<IP>`, the server issues a random challenge encrypted with your public key.
   - Your local SSH client decrypts the challenge using the private key and proves identity. Access is granted without transmitting credentials over the network.

### 7.2 The Linux File Permission Requirement: `chmod 400`

When you download `nure-pc.pem` from AWS, your operating system assigns default POSIX file permissions (often `0644` or `-rw-r--r--`), meaning other users or background processes on your machine have read access.

If you attempt to connect immediately:

```bash
ssh -i nure-pc.pem ubuntu@54.210.12.34
```

The SSH client aborts with a severe security warning:

```text
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'nure-pc.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "nure-pc.pem": bad permissions
ubuntu@54.210.12.34: Permission denied (publickey).
```

#### Understanding POSIX Permission Notation:

$$\text{Permission Octal} = \text{[Owner]}\text{[Group]}\text{[Others]}$$

Where:

- Read ($r$) = $4$
- Write ($w$) = $2$
- Execute ($x$) = $1$

| Command                 | Numeric Code | Owner Permission            | Group Permission      | Others Permission     | Plain English Meaning                       |
| ----------------------- | ------------ | --------------------------- | --------------------- | --------------------- | ------------------------------------------- |
| `chmod 777 key.pem`     | `7 7 7`      | Read+Write+Exec ($4+2+1=7$) | Read+Write+Exec ($7$) | Read+Write+Exec ($7$) | Catastrophic failure: completely public     |
| `chmod 644 key.pem`     | `6 4 4`      | Read+Write ($4+2=6$)        | Read ($4$)            | Read ($4$)            | Insecure: anyone on OS can read key         |
| **`chmod 400 key.pem`** | **`4 0 0`**  | **Read Only ($4$)**         | **None ($0$)**        | **None ($0$)**        | **Gold Standard: Only file owner can read** |

```bash
# Secure the private key file
chmod 400 nure-pc.pem
```

### 7.3 The Handshake: Connecting from the Terminal

Once permissions are locked down and the Security Group has Port 22 open:

```bash
# SSH into Ubuntu EC2 instance
ssh -i "nure-pc.pem" ubuntu@<EC2-PUBLIC-IP-ADDRESS>
```

Upon first connection, OpenSSH prompts:
`Are you sure you want to continue connecting (yes/no/[fingerprint])?`
Type `yes` to store the server's host key in `~/.ssh/known_hosts`.

---

## 8. Automated Provisioning: Bootstrap Scripts with EC2 User Data

### 8.1 What is User Data (cloud-init)?

In traditional operations, an engineer launches a server, connects manually via SSH, and types commands one by one to install software. In DevOps, manual server configuration is considered an anti-pattern.

**EC2 User Data** is a mechanism that allows you to pass a bootstrap shell script to your instance during launch.

- Under the hood, a system service called **`cloud-init`** detects the script.
- The script executes **automatically on the instance's very first boot**.
- It runs with full **`root` administrative privileges** (no interactive password prompts needed).
- Execution output is logged to `/var/log/cloud-init-output.log` on the server for auditing and debugging.

```
EC2 Instance Launch Life Cycle:
1. Provision Hypervisor VM
      │
2. Attach EBS Volume & Mount Root Filesystem
      │
3. Cloud-Init Service Starts
      │
4. Read User Data Script from Metadata (http://169.254.169.254/latest/user-data)
      │
5. Execute Script as Root (apt update -> install nginx -> start service)
      │
6. Web Server Up & Responding on Port 80!
```

### 8.2 Writing the Bootstrap Bash Script

In class, the instructor demonstrated bootstrapping an Nginx web server directly through the EC2 **Advanced Details $\rightarrow$ User Data** console field:

```bash
#!/bin/bash
# -------------------------------------------------------------
# EC2 Bootstrap Script: Automated Web Server Deployment
# Runs automatically as root on initial instance boot
# -------------------------------------------------------------

# Step 1: Update the Advanced Package Tool (APT) repository index
apt update -y

# Step 2: Install Nginx HTTP Web Server non-interactively
apt install nginx -y

# Step 3: Ensure the Nginx systemd daemon is active and running
systemctl start nginx

# Step 4: Enable Nginx to automatically launch on system reboot
systemctl enable nginx
```

### 8.3 The Non-Interactive Flag: Why `-y` is Life or Death in DevOps

Notice the **`-y`** flag appended to `apt update -y` and `apt install nginx -y`.

When you run `apt install nginx` in an interactive terminal, the package manager pauses and outputs:
`After this operation, 45.2 MB of additional disk space will be used. Do you want to continue? [Y/n]`

In an automated User Data bootstrap environment:

1. There is no human attached to the terminal.
2. There is no interactive standard input (`stdin`) to supply the keystroke `Y`.
3. **The Disaster:** Without `-y`, the package manager waits indefinitely for user input. The script hangs silently in the background, `cloud-init` never finishes, and Nginx is never installed!
4. **DevOps Rule:** Any command placed inside User Data, Dockerfiles, or CI/CD pipelines must run in **non-interactive batch mode** (`-y`, `--yes`, or `DEBIAN_FRONTEND=noninteractive`).

### 8.4 Verifying Nginx on the Public Internet

Once the EC2 instance status transitions to `Running` (and status checks pass `2/2`):

1. Copy the instance's **Public IPv4 Address** from the EC2 console.
2. Open a web browser and navigate to:
   `http://<EC2-PUBLIC-IP>`
3. **Important:** Ensure the URL uses `http://` (Port 80) and not `https://` (Port 443), because no SSL certificate has been installed yet.
4. The default **"Welcome to nginx!"** landing page renders instantly.

---

## 9. Scaling Out: Launch Templates & Fleet Automation

### 9.1 The Problem: Provisioning 50 Servers by Hand

Imagine your team needs to deploy 10, 20, or 50 microservice nodes of identical configuration:

- Same Ubuntu AMI
- Same `t3.large` instance type
- Same `nure-pc.pem` key pair
- Same Security Group rules
- Same 15 GB EBS volume size
- Same User Data script installing Docker and Nginx

Clicking through the AWS Console wizard 50 times takes hours, is tedious, and is guaranteed to introduce human error.

### 9.2 The Analogy: Cupcake Molds & Brick Factory Dies

In class, the instructor used the vivid analogy of **Cupcake Baking Molds (কাপ কেকের ডাইস)** and **Brick Factory Molds (ইটের ছাঁচ)**:

- A baker does not shape each cupcake by hand from scratch. They pour batter into a precision mold tray and bake 50 identical cupcakes at once.
- In brick manufacturing, clay is pressed into a standardized mold. Every brick comes out with the exact same dimensions.

An **AWS Launch Template** is that exact industrial mold for your cloud infrastructure. You define every server specification once inside the template. Afterward, you simply tell AWS: _"Give me 50 instances from this template."_

```
                    ┌────────────────────────────┐
                    │    AWS Launch Template     │
                    │   (The "Cupcake Mold")     │
                    │                            │
                    │ - AMI: Ubuntu 24.04        │
                    │ - Type: t3.large           │
                    │ - Key: nure-pc.pem         │
                    │ - Storage: 15 GB EBS       │
                    │ - Script: Install Docker   │
                    └─────────────┬──────────────┘
                                  │
         ┌────────────────────────┼────────────────────────┐
         ▼                        ▼                        ▼
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│  Instance 1      │     │  Instance 2      │     │  Instance 50     │
│  (Identical VM)  │     │  (Identical VM)  │     │  (Identical VM)  │
└──────────────────┘     └──────────────────┘     └──────────────────┘
```

### 9.3 Building a Launch Template (Version 1)

In the AWS Console:

1. Navigate to **EC2 $\rightarrow$ Launch Templates $\rightarrow$ Create Launch Template**.
2. **Template Name:** `nure-pc-template`
3. **Template Version Description:** `v1 - Initial baseline with Nginx`
4. **AMI:** Ubuntu 24.04 LTS
5. **Instance Type:** `t3.large` (or `t2.micro` for Free Tier)
6. **Key Pair:** `nure-pc`
7. **Network Settings:** Select existing Security Group (Ports 22 & 80 open)
8. **Storage:** 12 GiB `gp3` root volume
9. **User Data:**
   ```bash
   #!/bin/bash
   apt update -y
   apt install nginx -y
   systemctl start nginx
   systemctl enable nginx
   ```
10. Click **Create launch template**.

### 9.4 Modifying & Versioning (Version 2 & 3)

One of the greatest engineering advantages of Launch Templates over older AWS Launch Configurations is **built-in version control**:

#### Creating Version 2 (Adding Docker):

When our tech stack evolves and we need Docker installed alongside Nginx:

1. Select `nure-pc-template` $\rightarrow$ **Actions $\rightarrow$ Modify template (Create new version)**.
2. **Version Description:** `v2 - Added Docker installation to User Data`
3. Update User Data script:

   ```bash
   #!/bin/bash
   apt update -y
   apt install nginx -y
   systemctl start nginx
   systemctl enable nginx

   # Install Docker Container Engine
   apt install docker.io -y
   systemctl start docker
   systemctl enable docker
   usermod -aG docker ubuntu
   ```

4. **Notice the AWS Console Bug Workaround:** Under Network / EBS configurations, ensure optional fields (such as Network Card Index) are set to _"Do not include in launch template"_ to avoid console validation blocks.
5. Click **Create template version**.

#### Creating Version 3 (Expanding Storage):

1. Create new version from `v2` $\rightarrow$ set version description `v3 - Upgraded EBS root volume to 15 GB`.
2. Under **Storage (Volumes)** $\rightarrow$ increase Size from `12 GiB` to `15 GiB`.
3. Set `v3` as the template default version.

### 9.5 Launching Multiple Instances Simultaneously from a Template

1. Select `nure-pc-template`.
2. Click **Actions $\rightarrow$ Launch instance from template**.
3. Select Template Version: `v2` or `v3`.
4. Under **Number of instances**, specify `2` (or `50`).
5. Click **Launch instance**.
6. AWS provisions all instances concurrently in parallel!
7. Within 60 seconds, multiple running instances appear with unique Instance IDs (`i-0a1b2c3d...`).

---

## 10. Real-World Gotchas, Bugs, & Troubleshooting in Class

During the live session, several practical issues arose. These scenarios replicate real-world DevOps environments:

### Gotcha 1: The Launch Template EBS Card Index Validation Trap

- **The Issue:** While attempting to save a modified Launch Template version, the AWS Console throws a validation error regarding the network interface or EBS card index.
- **Root Cause:** A quirk in the AWS Console UI where optional network card index parameters are auto-populated with empty objects rather than null defaults.
- **The Fix:** Explicitly set the configuration radio button to **"Do not include in launch template"** (`Do not include in launch template`). The template version saves cleanly.

### Gotcha 2: The Missing `-y` Flag in Bootstrap Scripts

- **The Issue:** A user data script appears to run, but when you SSH into the machine, Docker or Nginx is not installed.
- **Root Cause:** The script contained `apt install docker.io` without the `-y` flag. The non-interactive background shell froze indefinitely awaiting input confirmation.
- **The Fix:** Always verify every package manager and cleanup command has `-y` (`apt install docker.io -y`).

### Gotcha 3: SSH Connection Freezes / Timeouts

- **The Issue:** Running `ssh -i nure-pc.pem ubuntu@<IP>` hangs indefinitely without responding.
- **Root Cause:** Port 22 is missing or closed in the Security Group Inbound Rules, or the security group rule was locked to an outdated IP address.
- **The Fix:** Go to EC2 Console $\rightarrow$ Security Groups $\rightarrow$ Edit Inbound Rules $\rightarrow$ Add rule: Type `SSH`, Port `22`, Source `My IP` (or `0.0.0.0/0` temporarily for troubleshooting).

### Gotcha 4: Unprotected Private Key File (`chmod 400`)

- **The Issue:** SSH rejects connection with `Permissions 0644 for 'nure-pc.pem' are too open`.
- **Root Cause:** OpenSSH client enforces strict file permissions to ensure private keys cannot be read by other accounts or rogue processes on your local operating system.
- **The Fix:** Run `chmod 400 nure-pc.pem` in Linux/macOS terminal.

---

## 11. Questions Asked in Class (Q&A)

### Q1: How do I find my downloaded `.pem` key file in the Linux terminal without opening a GUI file manager?

**Answer:**
Use the Linux terminal's file listing and filtering commands. If downloaded to `~/Downloads`:

```bash
cd ~/Downloads
# Method 1: Wildcard globbing
ls -l nure-pc*

# Method 2: Case-insensitive grep filter
ls -la | grep -i "nure"
```

---

### Q2: If I move from my home to the office or my router restarts, will my SSH connection stop working?

**Answer:**
**Yes**, if your Security Group inbound rule was restricted to **"My IP"**. Most home ISPs dynamically assign IP addresses from a shared pool. When your router reconnects or when you switch to office Wi-Fi, your external public IP changes.

- **The Solution:** Open the AWS Console on your phone or laptop browser, navigate to your Security Group's inbound rules, and edit the Port 22 source. Select **"My IP"** again, which automatically populates your new current IP, then save.

---

### Q3: Are network ports like 22, 80, and 443 fixed and universal?

**Answer:**
**Yes.** These are known as **Well-Known Ports** (standardized by IANA in the range 0–1023). Every modern operating system and web browser expects web traffic on Port 80 (HTTP) or 443 (HTTPS), and secure terminal communication on Port 22 (SSH). While you _can_ configure a custom daemon to listen on an alternate port (e.g., SSH on port 2222), the industry standards remain universally recognized.

---

### Q4: If I launch 50 servers from a single Launch Template, does the private key get generated 50 times or copied across all machines?

**Answer:**
**Neither!** This is one of the most fundamental concepts in asymmetric cryptography:

1. **The Private Key (`nure-pc.pem`):** Stays strictly on **your local laptop**. It is **never** uploaded to AWS, never copied to the instances, and never shared across the network.
2. **The Public Key:** AWS retains the public counterpart. When AWS provisions 50 instances from your Launch Template, it automatically injects that single **Public Key** into `/home/ubuntu/.ssh/authorized_keys` across all 50 servers.
3. Your one local private key can now securely authenticate and unlock all 50 servers!

---

### Q5: Can multiple engineers access the same EC2 instance using the same private key?

**Answer:**
**Technically yes, but organizationally discouraged.**

- If you share `nure-pc.pem` with a teammate, they can run `chmod 400 nure-pc.pem` and connect to the instance identically.
- **Production Best Practice:** Never share private keys! Instead, each engineer generates their own key pair, and their individual public keys are added to `/home/ubuntu/.ssh/authorized_keys`, or organizations use **AWS IAM Identity Center** and **AWS Systems Manager (SSM) Session Manager** for audited, keyless SSH access.

---

## 12. Common Misconceptions, Corrected

### Misconception 1: "The Cloud is simply someone else's computer."

> **Correction:** While hardware in the cloud physically belongs to Amazon, Google, or Microsoft, the cloud is an entirely orchestrated, API-driven software fabric. It provides automated self-healing, multi-datacenter redundancy, programmatic software-defined networking (VPC), automated horizontal scaling, and managed security perimeters that cannot be matched by merely placing a physical computer in a remote closet.

### Misconception 2: "Memory and Storage mean the exact same thing."

> **Correction:** In cloud computing, **Memory** refers strictly to volatile **RAM** (e.g., 1 GB on `t2.micro`), which vanishes when an instance is powered down. **Storage** refers to non-volatile block storage (**EBS volumes**, e.g., 12 GB or 15 GB SSDs) that retains your files, operating system, and databases permanently across instance reboots and stops.

### Misconception 3: "Security Groups are firewalls running inside the Linux operating system (like UFW or iptables)."

> **Correction:** AWS Security Groups operate at the **hypervisor virtualization layer (AWS Nitro)** outside the virtual machine. Host-level firewalls (like `ufw` or `iptables`) run inside the Linux kernel. A packet rejected by a Security Group never even touches the instance's virtual network interface card (vNIC).

### Misconception 4: "Creating a Launch Template costs money and spins up instances immediately."

> **Correction:** A Launch Template is strictly a stored **blueprint (metadata definition)** in AWS. Creating, versioning, and storing Launch Templates is completely **free of charge**. You are billed only when you explicitly launch actual EC2 instances _from_ that template.

### Misconception 5: "The private key file is stored on the AWS server."

> **Correction:** AWS never stores or retains your private key. AWS generates the key pair once, delivers the private key `.pem` file to your browser for immediate download, and permanently discards its copy. Only the public key is stored on AWS and injected into EC2 instances. If you lose your private key file, it cannot be recovered from AWS.

---

## 13. Hands-On Practice Exercises

### Exercise 1: Manual EC2 Launch with Automated Nginx Bootstrapping

1. Log into your AWS Console and navigate to the region closest to you (e.g., `ap-south-1` Mumbai or `us-east-1`).
2. Launch a single EC2 instance with:
   - Name: `web-server-practice`
   - AMI: Ubuntu 24.04 LTS
   - Instance Type: `t2.micro` (Free Tier)
   - Key Pair: Create a new key pair named `practice-key.pem`
   - Security Group: Allow SSH (Port 22) from "My IP", Allow HTTP (Port 80) from Anywhere (`0.0.0.0/0`)
   - Storage: 10 GiB gp3
   - User Data: Include the automated Nginx installation script.
3. Once running, paste the Public IP into your browser to verify the Nginx welcome page.

### Exercise 2: SSH Security & Terminal Verification

1. Open your local terminal.
2. Locate `practice-key.pem` and verify its default permissions using `ls -l`.
3. Attempt to SSH without changing permissions and observe the security error.
4. Run `chmod 400 practice-key.pem`.
5. Connect via SSH: `ssh -i practice-key.pem ubuntu@<PUBLIC-IP>`.
6. Inspect the User Data execution log:
   ```bash
   cat /var/log/cloud-init-output.log
   ```
7. Check the Nginx service status:
   ```bash
   systemctl status nginx
   ```

### Exercise 3: Fleet Automation with Launch Templates (v1 to v2)

1. Create a Launch Template named `devops-fleet-template` with:
   - AMI: Ubuntu 24.04 LTS
   - Type: `t2.micro`
   - Key: `practice-key`
   - Storage: 12 GiB gp3
   - User Data (v1): Install Nginx.
2. Create **Version 2 (v2)** of the template:
   - Add Docker installation commands to User Data.
   - Expand EBS root volume to 15 GiB.
3. Launch **2 instances simultaneously** using Version 2.
4. SSH into both instances and verify Docker is functional:
   ```bash
   docker --version
   docker ps
   ```

---

## 14. DevOps Interview Corner

### Q1: What is the difference between AWS Security Groups and Network Access Control Lists (NACLs)?

**Model Answer:**

- **Security Groups:** Operate at the **instance/ENI level**, are **stateful** (return traffic is automatically allowed regardless of outbound rules), and support **allow rules only** (all other traffic is implicitly denied).
- **NACLs (Network ACLs):** Operate at the **subnet level**, are **stateless** (inbound and outbound rules must be explicitly defined for response traffic), and support both **allow and deny rules** processed in numerical rule order.

---

### Q2: What happens behind the scenes when an EC2 instance executes User Data?

**Model Answer:**
During the instance initialization sequence, the `cloud-init` service contacts the **Instance Metadata Service (IMDS)** at the link-local IP `http://169.254.169.254/latest/user-data`. It downloads the user data script, saves it to `/var/lib/cloud/instances/<instance-id>/`, and executes it with root privileges once during the initial boot cycle. Logs are captured in `/var/log/cloud-init-output.log`.

---

### Q3: Why does SSH require `chmod 400` on private key files?

**Model Answer:**
OpenSSH implements defensive security checks. If a private key file has group or world read permissions (such as `0644`), other unprivileged users or malicious processes running on the local operating system could read the key and compromise remote infrastructure. The `400` permission restricts read access exclusively to the file owner, satisfying OpenSSH's security audit.

---

### Q4: What is the difference between stopping and terminating an EC2 instance?

**Model Answer:**

- **Stopping an instance:** Performs a graceful OS shutdown. Compute (vCPU/RAM) allocation is released and hourly compute billing stops. However, the attached EBS root volume persists, and AWS continues to bill for EBS volume storage. The instance can be restarted later (often receiving a new dynamic Public IP unless an Elastic IP is attached).
- **Terminating an instance:** Permanently destroys the virtual server. By default, the attached EBS root volume is deleted (`DeleteOnTermination=true`), compute resources are released, and all billing halts permanently.

---

### Q5: How do Launch Templates facilitate Infrastructure as Code (IaC) and Auto Scaling?

**Model Answer:**
Launch Templates decouple instance configuration from scaling policies. In AWS Auto Scaling Groups (ASGs), the ASG references a Launch Template ID and version. When traffic increases, the ASG provisions new EC2 instances dynamically using the template blueprint. Furthermore, Launch Templates support versioning, allowing seamless blue/green deployments and rolling updates by updating the default version in Terraform or AWS CLI.

---

## 15. Bilingual Technical Glossary

| English Term                    | Bengali Translation          | Technical Definition & Practical Context                                                                                                                                                        |
| ------------------------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cloud Computing**             | ক্লাউড কম্পিউটিং             | অন-ডিমান্ড ইন্টারনেট ভিত্তিক আইটি রিসোর্স (কম্পিউট, স্টোরেজ, ডাটাবেস) ব্যবহারের প্রযুক্তি, যেখানে নিজস্ব হার্ডওয়্যার কেনার বদলে ব্যবহারে যতটুকু লাগে ততটুকুর জন্য পে করতে হয় (Pay-as-you-go)। |
| **On-Premises (On-Prem)**       | অন-প্রেমিসেস                 | প্রতিষ্ঠানের নিজস্ব অফিস বা ভবনের ভেতরে নিজস্ব সার্ভার রুম ও হার্ডওয়্যার স্থাপন করে আইটি সিস্টেম পরিচালনা করার সনাতন পদ্ধতি।                                                                   |
| **Elastic Compute Cloud (EC2)** | ইলাস্টিক কম্পিউট ক্লাউড      | অ্যামাজন ওয়েব সার্ভিসেসের (AWS) স্কেলেবল ও চাহিদামতো ছোট-বড় করার মতো ভার্চুয়াল সার্ভার প্রোভিশনিং সার্ভিস।                                                                                   |
| **Amazon Machine Image (AMI)**  | অ্যামাজন মেশিন ইমেজ          | একটি ভার্চুয়াল সার্ভার চালুর ব্লুপ্রিন্ট বা টেমপ্লেট; যার মধ্যে অপারেটিং সিস্টেম (যেমন Ubuntu 24.04), প্রাক-কনফিগারেশন ও বুট সেটিংস সংরক্ষিত থাকে।                                             |
| **Instance Type**               | ইন্সট্যান্স টাইপ             | একটি ভার্চুয়াল সার্ভারের হার্ডওয়্যার কনফিগারেশন স্পেসিফিকেশন (vCPU সংখ্যা ও RAM-এর অনুপাত), যেমন `t2.micro` বা `t3.large`।                                                                    |
| **Elastic Block Store (EBS)**   | ইলাস্টিক ব্লক স্টোর          | EC2 ইন্সট্যান্সের সাথে যুক্ত থাকা ভার্চুয়াল হার্ডড্রাইভ বা ব্লক স্টোরেজ, যা সার্ভার বন্ধ থাকলেও পারসিস্টেন্ট বা স্থায়ীভাবে ডাটা সংরক্ষণ করে।                                                  |
| **Security Group**              | সিকিউরিটি গ্রুপ              | ইন্সট্যান্স স্তরে কাজ করা ভার্চুয়াল স্টেটফুল ফায়ারওয়াল, যা কোন কোন নেটওয়ার্ক পোর্ট ও আইপি দিয়ে ট্রাফিক ঢুকবে (Inbound) বা বের হবে (Outbound) তা ফিল্টার করে।                               |
| **Inbound / Outbound Rules**    | ইনবাউন্ড / আউটবাউন্ড রুলস    | ফায়ারওয়ালে নির্দিষ্ট পোর্টে ইনকামিং ট্রাফিক প্রবেশের অনুমতি (Inbound) এবং সার্ভার থেকে ইন্টারনেটে ট্রাফিক বের হওয়ার অনুমতি (Outbound)।                                                       |
| **Asymmetric Cryptography**     | অ্যাসিমেট্রিক ক্রিপ্টোগ্রাফি | পাবলিক ও প্রাইভেট কি—এই দুটি আলাদা কিন্তু গাণিতিকভাবে সম্পর্কিত কী ব্যবহারের মাধ্যমে তথ্য এনক্রিপশন ও পাসওয়ার্ডহীন নিরাপদ অথেনটিকেশন ব্যবস্থা।                                                 |
| **Private Key (`.pem`)**        | প্রাইভেট কী                  | ক্লায়েন্ট মেশিনে কঠোর গোপনীয়তায় সংরক্ষিত ব্যক্তিগত ফাইল, যা কখনোই সার্ভার বা ইন্টারনেটে পাঠানো হয় না।                                                                                       |
| **Public Key**                  | পাবলিক কী                    | সার্ভারের `authorized_keys` ফাইলে সংরক্ষিত উন্মুক্ত অংশ, যা প্রাইভেট কী-এর সাথে গাণিতিক হ্যান্ডশেক সম্পন্ন করে অ্যাক্সেস দেয়।                                                                  |
| **User Data (cloud-init)**      | ইউজার ডাটা                   | EC2 সার্ভার প্রথমবার বুট হওয়ার সময় রুট প্রিভিলেজ নিয়ে স্বয়ংক্রিয়ভাবে চলা ব্যাশ স্ক্রিপ্ট, যা সফটওয়্যার ইনস্টলেশন ও অটোমেশন নিশ্চিত করে।                                                   |
| **Launch Template**             | লঞ্চ টেমপ্লেট                | সার্ভারের সব কনফিগারেশন (AMI, টাইপ, কি পেয়ার, ইউজার ডাটা, স্টোরেজ) একসাথে গুছিয়ে রাখা ভার্সন-নিয়ন্ত্রিত ব্লুপ্রিন্ট বা ডাইস, যা দিয়ে এক ক্লিকে বহু সার্ভার লঞ্চ করা যায়।                   |
| **Virtual Private Cloud (VPC)** | ভার্চুয়াল প্রাইভেট ক্লাউড   | AWS ক্লাউডের ভেতরে ইউজারের জন্য বরাদ্দকৃত সম্পূর্ণ আইসোলেটেড ও নিজস্ব নিয়ন্ত্রিত ভার্চুয়াল নেটওয়ার্ক।                                                                                        |
| **POSIX File Permissions**      | পসিক্স ফাইল পারমিশন          | লিনাক্স সিস্টেমে ফাইল বা ফোল্ডারের ওনার, গ্রুপ ও অন্যদের জন্য রিড (4), রাইট (2) ও এক্সিকিউট (1) ক্ষমতার সংখ্যাভিত্তিক সিকিউরিটি মডেল।                                                           |

---

## 16. One-Page AWS EC2 Cheat Sheet

### Core Terminal Commands

```bash
# 1. Locate downloaded private key
ls -l ~/Downloads/*.pem

# 2. Restrict private key permissions (Read-only for owner)
chmod 400 ~/Downloads/nure-pc.pem

# 3. Connect to EC2 Ubuntu Instance via SSH
ssh -i ~/Downloads/nure-pc.pem ubuntu@<EC2-PUBLIC-IP>

# 4. Inspect User Data execution log on remote instance
cat /var/log/cloud-init-output.log

# 5. Check and manage system services
systemctl status nginx
sudo systemctl restart nginx
docker ps
```

### Essential Security Group Rules Reference

| Port    | Protocol | Default Source for Production | Purpose                         |
| ------- | -------- | ----------------------------- | ------------------------------- |
| **22**  | TCP      | `<Your-Office-or-Home-IP>/32` | Administrative SSH Management   |
| **80**  | TCP      | `0.0.0.0/0` (Anywhere IPv4)   | Public HTTP Web Traffic         |
| **443** | TCP      | `0.0.0.0/0` (Anywhere IPv4)   | Secure Public HTTPS Web Traffic |

### Automated Bootstrap Template (Nginx + Docker)

```bash
#!/bin/bash
# Complete DevOps Bootstrap Script for EC2 User Data
set -e

# Update Repositories
apt-get update -y

# Install Core Tools
apt-get install -y nginx docker.io curl git

# Start & Enable Services
systemctl start nginx
systemctl enable nginx
systemctl start docker
systemctl enable docker

# Allow unprivileged 'ubuntu' user to run docker commands
usermod -aG docker ubuntu

echo "DevOps Environment Ready!" > /var/www/html/index.html
```

---

_End of Study Notes — AWS Class 01_
