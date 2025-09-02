# DevOps-Learning-AWS
I learn AWS

## IAM (Identity and Access Management)

### Users and Groups 

#### Root Account
- Created by default when the AWS account is set up
- Shouldn't be used for everyday tasks
- Keep credentials secure; never share

#### Users
- Represent people or services in your organization
- Can be grouped for easier permission management
- Each has their own login & permissions

#### Groups
- Contain only users (no nested groups)
- Assign permissions to the group → applies to all users in it
- A user:
  - Doesn't have to belong to a group
  - Can belong to multiple groups

#### Example (From Diagram)
- **Developers Group**: Michael, Jennifer, Joey
- **Audit Team Group**: Zid
- **Operations Group**: Zid, Ferns
- **No Group**: Kasoon

![IAM Users and Groups Structure](https://github.com/user-attachments/assets/85ba2466-e21d-4a8d-b647-31fc20b6d5eb)

---

## IAM: Permissions

### 1. What Are Permissions?
- Users or Groups can be assigned JSON documents called policies
- Policies = rulebooks that define what actions are allowed or denied
- Actions apply to specific AWS services/resources

### 2. Example from Diagram
**Allowed actions:**
- Describe EC2 instances
- Describe Elastic Load Balancing
- List/Get/Describe CloudWatch metrics

### 3. Principle of Least Privilege
- Only give the permissions needed to perform required tasks
- Don't allow extra actions "just in case"
- **Analogy**: Give someone the key to the backseat, not the car ignition

### 4. Why It Matters
- Keeps AWS environment secure
- Prevents mistakes or misuse
- Scales well as your organization grows

![IAM Permissions Example](https://github.com/user-attachments/assets/3bcbbd64-9d4b-4b7c-9f5c-1e578e4a28ca)

---

## IAM Policies – Inheritance

### 1. Group Permissions
- Users inherit all permissions assigned to the group(s) they belong to
- **Example:**
  - Developers group → Alice, Bob, Charles
  - Permissions: e.g., EC2 access, S3 access
  - Everyone in that group gets the same permissions

### 2. Multiple Groups
- A user can belong to more than one group
- They inherit permissions from all groups they are in
- **Example:**
  - Charles → Developers + DevOps Team → gets combined permissions
  - David → Operations + DevOps Team → gets combined permissions

### 3. Inline Policies
- Attached directly to a specific user
- Not shared with any group
- **Example:**
  - Fred → in Operations group + has an inline policy for extra unique permissions

### 4. Key Takeaways
- Multiple group memberships = combined permissions
- Inline policies = user-specific extra access
- **Best practice:**
  - Use groups to manage most permissions
  - Use inline policies only for special cases

![IAM Policy Inheritance](https://github.com/user-attachments/assets/d8f3ad61-aa18-4aa4-bb15-cdda2bcc667b)

---

## IAM: Policies Structure

### 1. Main Components
- **Version** (required) – Policy language version
  - Always use: `"2012-10-17"`
- **Id** (optional) – Identifier for the policy (e.g., name or reference)
- **Statement** (required) – One or more blocks defining the actual permissions

### 2. Statement Components
- **Sid** (optional) – Label or identifier for the statement
- **Effect** (required) – `"Allow"` or `"Deny"`
- **Principal** – Who the policy applies to (user, role, or account)
- **Action** – List of actions the policy allows or denies
- **Resource** – List of resources the actions apply to
- **Condition** (optional) – Rules for when the policy is in effect (e.g., specific IP)

### 3. Example From Diagram
- **Version**: `"2012-10-17"`
- **Id**: `"S3-Account-Permissions"`
- **Principal**: AWS root account (`arn:aws:iam::1234:root`)
- **Action**: `s3:GetObject`, `s3:PutObject`
- **Resource**: `arn:aws:s3:::mybucket/*`

### 4. Key Takeaways
- A policy defines: **Who** → Can do **what** → On **which resources** → Under **what conditions**
- AWS provides pre-built policies, but understanding the structure helps with custom setups

![IAM Policy Structure](https://github.com/user-attachments/assets/f55023bd-e066-4aa0-8bdd-9bdde428151f)

---

## Password Policy

You can enforce:
- A minimum password length at least 8-12 characters
- Specific requirements: caps, no caps, numbers and special characters
- Allow IAM users to change their password
- Require users to change password after some time
- Require users to not reuse same password

![Password Policy Settings](https://github.com/user-attachments/assets/0d587820-8b8d-43e5-b518-cb128129a3f4)

---

## MFA - Multi Factor Authentication

This is important because your AWS account contains important resources.

**MFA benefit**: If account password is compromised, account can't be accessed unless it has an external login. This can be done by a code, third party app or verification email.

![MFA Overview](https://github.com/user-attachments/assets/6f1bca3a-0d93-4d37-b17f-9da33de9e69b)

### MFA Options in AWS

#### Virtual MFA Device
- Virtual authenticator app e.g., Google Authenticator
- They send a one time code to access on the app on your smart phone

#### Universal 2nd Factor Security Key
- e.g., YubiKey
- It is a USB device you plug in the USB port that authenticates access without a code
- 1 key can access multiple accounts

#### Hardware Key Fob MFA Device
- Generates a code every 30 seconds
- Enter the code during log in and is a bit old school
- Good for places where electrical devices are not allowed

#### Hardware Key Fob MFA Device for AWS GovCloud
- Specifically for users in AWS GovCloud in the US

![MFA Options](https://github.com/user-attachments/assets/1d23c6ec-9b6a-476e-8435-fe1a4ef8df65)

---

## How Users Can Access AWS

### 3 Ways:
1. **AWS Management Console** - protected by password + MFA
2. **AWS CLI** - protected by keys
3. **AWS SDK** - Software Development Kit for code: protected by access keys

### Access Keys:
- Can generate keys from the console and each user manages their key
- Access keys should not be shared
- **Access Key ID** = username
- **Secret Access Key** = password

![AWS Access Methods](https://github.com/user-attachments/assets/52f6367c-8fe1-4031-9313-2bf9f692221e)

### Example of Access Key
![Access Key Example](https://github.com/user-attachments/assets/02872051-240f-474a-9e7a-a93b15a3930f)

---

## What is the AWS CLI?

The AWS CLI lets you access to the API within your AWS management console however in a CLI format. It lets you run commands and it allows you to write scripts for resource management. Used in click ops to automate jobs.

![AWS CLI Overview](https://github.com/user-attachments/assets/1ce66a20-9a56-4098-b1ec-bab5883ee678)

---

## What is the AWS SDK?

### AWS SDK – Key Points

**What it is**: AWS Software Development Kit – a set of pre-made code libraries for different programming languages.

**Purpose**: Lets you interact with AWS services from your code (no need to use the AWS Console).

**Languages supported**:
- Python, JavaScript, PHP, .NET, Ruby, Java, Go, Node.js, C++
- **Mobile SDKs**: Android, iOS
- **IoT SDKs**: Arduino, Embedded C, etc.

**How it works**:
- Embed AWS actions (e.g., upload to S3, create EC2) directly in your app's code
- Write just a few lines instead of doing tasks manually in the console

**Example**: The AWS CLI is built on top of the AWS SDK for Python (Boto3).

![AWS SDK Overview](https://github.com/user-attachments/assets/ce1aeda5-0271-43f3-8e61-70132d888166)

---

## IAM Roles for Services

### What are IAM Roles?
- Allow AWS services to act on your behalf without needing long-term credentials
- Provide temporary access instead of hardcoding access keys/passwords
- Improve security and flexibility

### Why use them?
- Services (like EC2, Lambda, CloudFormation) often need to interact with other AWS services
- Roles let them do this securely by attaching the right permissions

### Common Use Cases

#### EC2 Instance Roles
- Attach a role when launching an instance
- **Example**: EC2 reads/writes to S3 or DynamoDB

#### Lambda Function Roles
- Attach a role to a Lambda function
- **Example**: Lambda writes logs to CloudWatch or accesses S3

#### CloudFormation Roles
- Allows CloudFormation to create/manage resources across multiple services
- Keeps credentials secure while automating resource creation

### Key Point
- Think of IAM roles as temporary keys for services
- No hardcoded credentials
- Easy to control what permissions each service has

---

## IAM Security Tools

### IAM Credentials Report (Account Level)
A report that lists all your account's users and status of their various credentials.

### IAM Access Advisor
Shows the service permissions granted to users and when services were last accessed. You can use this info to revise your policies.

---

## IAM Guidelines and Best Practices

- Do not use root unless it's for initial account setup - use IAM user instead with correct setup
- **One physical user = one AWS user**
- Do not share logins and make sure everyone has an account
- Assign users to groups and assign permissions
- Create strong password policy
- Enforce MFA to keep account secure
- Create and use roles for giving services permissions instead of hard coding
- Use access keys for programmatic access (CLI/SDK)
- Audit permissions of your account using IAM credentials report and IAM access advisor
- **Never share IAM users or access keys**

---

## Summary

![IAM Summary](https://github.com/user-attachments/assets/87fe4c73-b8f1-489d-ace9-3f97e8216755)

---

# Amazon Compute

## Amazon EC2 

### What is EC2?
- **EC2** = Elastic Compute Cloud
- Part of AWS IaaS (Infrastructure as a Service)
- Lets you rent virtual machines (servers) from AWS
- Flexible & scalable – used by startups and large enterprises
- Pay only for what you use

### What EC2 Helps You Do

#### Rent Virtual Machines
- Choose OS and configure as needed

#### EBS (Elastic Block Store)
- Virtual hard drive for EC2 instances
- Stores data alongside VMs

#### ELB (Elastic Load Balancer)
- Distributes incoming traffic across multiple EC2 instances
- Prevents one machine from being overloaded

#### ASG (Auto Scaling Group)
- Automatically scales instances up or down
- **Scale out** = add more instances when traffic increases
- **Scale in** = remove extra instances when traffic decreases
- Saves cost – pay only for what's running

### Why EC2 is Important
- Core AWS service, fundamental to cloud computing
- Introduces key cloud concepts:
  - Virtual machines
  - Scaling
  - Load balancing

---

## EC2 Configurations & Options 

### 1. Operating System (OS)
- Choose from Linux, Windows, macOS, and others
- Decision depends on application requirements and what you're comfortable managing

### 2. Compute Power (CPU & Cores)
- Decide how many CPUs/cores and how powerful they should be
- **Example:**
  - Small workloads → fewer cores
  - Heavy tasks (ML, big data) → more cores & processing power

### 3. Memory (RAM)
- Select how much RAM the instance will have
- More RAM = better performance for large apps or real-time data processing

### 4. Storage Options
- **EBS (Elastic Block Store)**: like a hard drive attached to one VM
- **EFS (Elastic File System)**: shared storage, accessible from multiple VMs
- **Instance Store**: physical storage on the host machine. Very fast, but temporary (data lost when instance stops)

### 5. Networking
- Configure network card speed (affects traffic handling)
- Option for a public IP address (makes instance accessible from internet)

### 6. Security Groups (SGs)
- Act as firewalls for your instance
- Control who can access and what type of traffic is allowed
- Critical for securing EC2 instances

### 7. Bootstrap Script / User Data
- Script that runs automatically when the instance first launches
- Can be used to:
  - Install software
  - Run updates
  - Configure tasks

⚠️ **Key Point**: Right configuration matters → prevents underperformance or overpaying for unused resources.

---

## EC2 Instance Types - Depends on What Job You Want to Do

- **General Purpose** - used for small databases, small web servers or general work loads
- **Compute Optimised** - used if lots of processing power needed, gives extra CPU for tasks like heavy computing or calculation processing
- **Memory Optimised** - used when app needs a lot of memory such as memory databases, big data processing or high performance computing work loads
- **Storage Optimised** - designed for fast and high throughput storage used for large data sets or databases that require fast processing and big storage
- **Accelerated Computing** - this is for the GPU side and is used for machine learning, video processing or scientific simulation
- **HPC Optimised** - provides high performance computing used for tasks that require a lot of processing power for intensive computing tasks and fast networking

### Naming Convention Breakdown

**AWS Instance Naming Convention (Example: M5.2xlarge)**

#### 1. Instance Class (Letter at the start)
- Tells you the purpose/type of instance
- **Examples:**
  - **M** = General Purpose
  - **C** = Compute Optimized
  - **R** = Memory Optimized
  - **T** = Burstable General Purpose

#### 2. Generation (Number after the letter)
- Shows the version/generation of the instance
- Higher number = newer, faster, more efficient
- **Example**: 5 = 5th generation

#### 3. Size (after the generation)
- Defines how big the instance is (CPU, memory, network)
- **Examples:** small, large, xlarge, 2xlarge … up to 32xlarge
- Bigger size = more resources = more cost

⚖️ **Key Point: Choosing the Right Instance**
- Too small → app may run out of resources, poor performance
- Too big → wasted resources, higher costs
- **Goal** = balance performance & cost

![EC2 Instance Naming](https://github.com/user-attachments/assets/7f92149e-4311-4e9b-9a7f-e53b1d9b066d)

---

## EC2 Purchasing Options 

### 🔹 On-Demand Instances
- Pay per second (no long-term commitment)
- Best for short-term, unpredictable workloads
- ✅ **Example**: Testing a new app, temporary projects
- **Pros**: Flexible, predictable pricing
- **Cons**: Most expensive in the long run

### 🔹 Reserved Instances (RIs)
- Commit for 1 or 3 years (longer = cheaper)
- **Types:**
  - **Standard RI** → lowest price, fixed instance type
  - **Convertible RI** → allows switching instance types
- ✅ **Example**: Steady, long-term production workloads
- **Pros**: Big discounts
- **Cons**: Less flexible than On-Demand

### 🔹 Savings Plans
- Commit to spend for 1 or 3 years
- More flexible than RIs (can apply across instance types & regions)
- ✅ **Example**: Long-term use, but want flexibility
- **Pros**: Discounts + flexibility
- **Cons**: Still requires long-term commitment

### 🔹 Spot Instances
- Bid for unused EC2 capacity at very low prices
- Can be terminated by AWS anytime
- ✅ **Example**: Batch jobs, big data analysis, workloads that can be interrupted
- **Pros**: Cheapest option
- **Cons**: Unreliable, no guarantee of uptime

### 🔹 Dedicated Hosts
- Get an entire physical server
- ✅ **Example**: Compliance, software licensing tied to hardware
- **Pros**: Full control of hardware
- **Cons**: Expensive

### 🔹 Dedicated Instances
- Run on hardware not shared with other customers
- ✅ **Example**: Security or compliance needs
- **Pros**: Isolation from others
- **Cons**: No control over physical server itself

### 🔹 Capacity Reservations
- Reserve capacity in a specific Availability Zone (AZ)
- ✅ **Example**: Mission-critical workloads that must run during peak demand
- **Pros**: Guaranteed availability
- **Cons**: Pay even if not used

### ✅ Quick Recommendations
- **Unpredictable workloads** → On-Demand
- **Long-term predictable workloads** → Reserved Instances / Savings Plans
- **Interruptible workloads** → Spot Instances
- **Compliance/licensing needs** → Dedicated Hosts
- **Security isolation** → Dedicated Instances
- **Guarantee capacity in AZ** → Capacity Reservations

---

## Security Groups & Cloud Networking 

Security groups control how traffic flows in and out of an EC2 instance. It's like firewalls however firewalls allow and deny, security groups can only allow what has been set. Anything that's not set as allowed but is allowed by default won't pass to the EC2 instance. Anything allowed on security groups for input will also be allowed for output.

**Example**: You create a website using EC2 and you allow port 80 or 443 but not SSH.

![Security Groups Basic Concept](https://github.com/user-attachments/assets/61ca939b-b422-4ff6-a725-3b942c585d74)

### Security Group Deeper Dive

They have access to ports and control which ports are open. For example, you're making a webserver and you need website traffic, you would open port 80 instead of every other port or it would cause unwanted traffic.

**Authorised IP ranges** such as IPv4 or IPv6 - this can be set to which IP has the ability to access your instance. For example, if you use an SSH login it will only allow it from an authorised IP you have set like home or office.

**Control of inbound network and control of outbound network** - controlling what your instance is allowed to do and access.

#### Table Breakdown 

**Rule 1: HTTP**
- **Protocol**: TCP (since HTTP runs over TCP)
- **Port Range**: 80 (standard HTTP port)
- **Source**: 0.0.0.0/0 → open to the entire internet (anyone can access)
- **Meaning**: Allows inbound web traffic on port 80 from anywhere
- **Use Case**: Hosting a public website

⚠️ **Security Note**: Since it's open to 0.0.0.0/0, anyone can reach the server on port 80. This is normal for websites but make sure you only expose needed ports.

**🔹 Rule 2: SSH**
- **Protocol**: TCP
- **Port Range**: 22 (default SSH port)
- **Source**: 122.149.196.85/32 → a single specific IP
- **Meaning**: Only the machine with that exact IP can connect via SSH
- **Use Case**: Secure remote admin access for one trusted user

✅ **Security Best Practice**: This is a tight rule (restricted to one IP), which is good. Avoid 0.0.0.0/0 here, as that would expose SSH to the world.

**🔹 Rule 3: Custom TCP Rule**
- **Protocol**: TCP
- **Port Range**: Custom-defined (based on your app's needs)
- **Source**: Can be set to a specific IP, subnet, or 0.0.0.0/0
- **Meaning**: You open a specific TCP port for a particular application (e.g., 5000 for a Flask app, 3306 for MySQL, etc.)
- **Use Case**: Application-specific needs

⚠️ **Security Note**: Only open ports you absolutely need, and limit to trusted IPs whenever possible.

#### ✅ Key Takeaways from the Image
- **HTTP (80)** → Open to the world for web access
- **SSH (22)** → Restricted to one IP for secure admin
- **Custom TCP** → Flexible, but should be tightly controlled
- **Best Practice**: Keep rules minimal. More open = more exposure = higher risk

![Security Groups Rules Table](https://github.com/user-attachments/assets/47c0bc80-b393-4090-919a-88f088cf5515)

### Security Group Diagram 

#### 1. EC2 Instance & Security Groups
- In the center, you see an EC2 Instance with its public IP (XX.XX.XX.XX)
- Attached to it are two Security Groups:
  - **Inbound** (top box): Filters traffic coming in
  - **Outbound** (bottom box): Filters traffic going out
- Think of these SGs as firewalls at the instance level

#### 2. Inbound Rules (Top Path)
**Example from the diagram:**
- Your Computer (IP XX.XX.XX.XX) tries to connect on Port 22 (SSH)
- Since the IP + Port match the inbound rule, the SG allows it ✅
- Now you can SSH into the instance
- Another computer also tries to connect on Port 22, but its IP is not authorized
- The SG blocks this ❌ → No SSH access

👉 **Takeaway**: Inbound rules decide who can enter and on which port.

#### 3. Outbound Rules (Bottom Path)
- The instance itself may need to reach the Internet (WWW)
- Outbound SG rules decide what traffic can leave
- By default, outbound is set to any IP on any port, so:
  - The EC2 instance can download updates, fetch APIs, etc.

⚠️ **Best practice**: Instead of leaving "all open," restrict to required IPs/ports.

👉 **Takeaway**: Outbound rules decide where your instance can go out to.

#### 4. Summary of Diagram
- **Inbound Security Group (SG1)**: Acts like a door guard – lets in only specific IPs and ports
- **Outbound Security Group (SG2)**: Controls what leaves the instance – open by default but can be tightened
- **Your Computer** (top-right): Authorized because its IP + Port 22 matches rule
- **Other Computer**: Blocked because not authorized
- **Internet** (bottom-right): Allowed outbound on any port by default

✅ **Big Picture:**
- Security Groups = filters for traffic
- **Inbound**: Who can talk to your EC2
- **Outbound**: Where your EC2 can talk to outside
- **By default** → Inbound = blocked, Outbound = open

![Security Groups Diagram](https://github.com/user-attachments/assets/101977ae-5068-423a-98bf-04ca2f17fa50)

### Security Groups Good to Know

#### 1. Reusability
- One SG can be attached to multiple instances
- Makes access management easier across many instances

#### 2. Region & VPC Bound
- SGs are tied to a specific Region + VPC
- ❌ You cannot use the same SG across regions or VPCs

#### 3. Where They Live
- SGs exist outside the EC2 instance
- If SG blocks traffic → it never reaches your instance
- ✅ First place to check if you can't connect to your instance

#### 4. Best Practices
- Keep a separate SG just for SSH (port 22)
- Easier to tightly control admin access without touching app/web rules

#### 5. Troubleshooting Tips
- **Timeout** (no response): Usually SG blocking traffic
- **Connection refused**: Likely an app/service issue on the instance, not SG

#### 6. Default Behavior
- **Inbound**: 🚫 Blocked by default (must explicitly allow ports like 22, 80, 443)
- **Outbound**: ✅ Allowed by default (instance can reach internet/others unless restricted)

SGs are reusable firewalls at the instance level. They default to blocking inbound and allowing outbound, and most connectivity issues (timeouts) are caused by SG misconfigurations.

### Security Groups Referencing 

#### 1. Normal SG Rules
- Usually, SGs allow traffic based on IP addresses + ports
- **Problem**: IPs can change (e.g., auto-scaling, dynamic environments)

#### 2. Referencing Other SGs
- Instead of IPs, you can allow traffic from another SG
- **Example in diagram:**
  - SG1 inbound rule allows traffic from SG1, SG2, SG3 on port 123
  - ✅ Any instance in SG1, SG2, or SG3 can talk to the instance using SG1 on that port

#### 3. Why It's Useful
- **Dynamic scaling**: Instances come and go, but SG-based rules still work
- **Microservices:**
  - App tier SG ↔ Database tier SG
  - All app instances can securely talk to all DB instances
- **Simpler management**: No need to update firewall rules for changing IPs

#### 4. Key Takeaways
- SGs can reference other SGs instead of IPs
- This keeps communication secure and flexible
- **Best for:**
  - Auto-scaling groups
  - Clusters
  - Multi-tier architectures (App ↔ DB ↔ Cache)

👉 **Easy way to remember:**
- **Normal SGs** = Lock the door by IP
- **SG referencing** = Lock the door by "group membership" instead

![Security Groups Referencing](https://github.com/user-attachments/assets/e95e177e-2279-415d-9568-2e09d09da086)

---

## Classic Ports to Know 

### 🔐 SSH
- **Port**: 22
- **Use**: Secure login to Linux instances
- **Tip**: Restrict access to trusted IPs only (never open to the world)

### 📂 FTP / SFTP
- **FTP** (File Transfer Protocol): Port 21 – insecure, being phased out
- **SFTP** (Secure FTP): Port 22 – runs over SSH, encrypted & secure

### 🌍 HTTP / HTTPS
- **HTTP**: Port 80 – Unsecured web traffic
- **HTTPS**: Port 443 – Secured web traffic (TLS/SSL), encrypts data
- **Tip**: Always use HTTPS for sensitive apps/data

### 🌐 DNS
- **Port**: 53 (UDP/TCP)
- **Use**: Resolves domain names → IP addresses
- **Critical for**: Any app that uses domain names

### 🖥 RDP (Remote Desktop Protocol)
- **Port**: 3389
- **Use**: Log in to Windows instances remotely

### 📧 SMTP (Simple Mail Transfer Protocol)
- **Port**: 25
- **Use**: Sending emails

### 🗄 Databases
- **MySQL**: Port 3306
- **PostgreSQL**: Port 5432

# AWS Storage and Load Balancing Guide

## Storage

### What's an EBS Volume

EBS is a network drive that can be attached to EC2 instances.

It's like a virtual storage.

It allows your instance to persist data even if it's off e.g saves data for your instances.

Bound to AZs.

Think of it like a network USB stick.

#### Why is it used

**Data persistence** - data stays in there even if an instance is terminated there will still be data saved in the EBS.

**Ideal for storing long term data** like databases.

---

## AMI Overview

### Amazon Machine Image

AMI are a customisation of EC2 instances.

You can add your own software, config, OS and monitoring.

Good if you want a faster boot config time because your software is pre packaged.

AMIs are built for specific regions but can be copied across.

**You can launch EC2 instances from:**

- **A public AMI:** AWS provided
- **Your own AMI:** make and maintain yourself
- **AWS marketplace:** AMI that someone else made and sells

---

## Amazon EFS - Elastic File System

Amazon managed solution for shared file storage.

It's a managed network file system that can be mounted onto multiple EC2 instances to share the same files.

Like a network drive that all instances can access.

EFS works with EC2 instances that are in multi AZs.

It's highly available.

Scalable - grows as the data grows.

However it is expensive and should be used when only needed.

---

# Load Balancing & Scalability

## Scalability & High Availability

Scalability means that an application/system can handle greater loads by adapting its size.

**2 Types:**

**Vertical** - when more power is added e.g upgrading a computer such as adding more ram, cpu power etc while the machine stays the same.

**Horizontal (elasticity)** - add more instances of resource to handle the workload rather than making the server bigger you add more servers to balance the workload.

Scalability is about handling greater loads.

High availability ensuring the system is up and running even parts of it fail.

### Call Center Example:

**Vertical scalability:** One person with better tools to handle more calls.

**Horizontal scalability:** Hire more people to share the workload.

**High availability (HA):** Have backups/standby staff so operations continue if someone is unavailable.

### Vertical Scalability

Means increasing the size of an instance adding storage ram and cpu power.

**Example:** If you have a t2.micro and need to upgrade you can scale and change to t2.large.

Vertical scalability is common for non distributed systems such as databases.

DBS like RDS and ElastiCache are services that can scale vertically.

There is usually a hardware limit to how much you can scale.

### Horizontal Scalability

Horizontal scalability means to increase the number of instances/systems for your application to handle more load.

Add more machines/instances based on work load.

Each machine does part of the work allowing you to handle more data.

Easier to manage if one instance fails, you have back up.

**Example:** If you have website traffic you can start more instances to handle then stop once traffic lessens.

Easy to implement and can be automated thanks to amazon services and ec2.

### High Availability

Means having multiple locations for your application/system to ensure if one part of infra fails another part can take over.

High availability and horizontal scaling go hand in hand because you need to have multiple instances across locations.

Achieve this by running applications across 2 or more AZs.

AZs are like independent data centres.

The goal of HA is to survive a data centre loss so if a data centre goes down then you have a back up to keep your app running.

<img width="681" height="340" alt="image" src="https://github.com/user-attachments/assets/f38496e3-1a71-41e6-a901-2b8616bc6dbe" />

---

## Load Balancing

### What is Load Balancing?

Load balancing is a way to distribute traffic across servers or EC2 instances.

Acts like a traffic cop distributing requests to different servers.

When traffic comes in the load balancer checks between the EC2 instances downstream to see which is up and healthy to take on more traffic.

This process keeps your application available and working just in case one instance goes down.

Load balancer will automatically divert traffic to healthy EC2 instances.

### Reverse Proxies

#### What are reverse proxies?

**Definition:**

A reverse proxy sits between users and your servers.

It handles requests on behalf of your servers.

**Similar to a Load Balancer:**

Both sit in the middle of client ↔ server communication.

Both can distribute traffic to servers.

**Extra Functionality (compared to Load Balancer):**

Can route traffic based on request content (e.g., URL path, headers).

Can send requests to different services or microservices.

Provides features like caching, SSL termination, authentication, compression (depending on the setup).

**AWS Example:**

Application Load Balancer (ALB) acts like a reverse proxy.

It supports content-based routing (e.g., /api → service A, /images → service B).

Useful in microservice architectures.

> 👉 Think of it like this:
>
> Load Balancer = Just balances traffic.
>
> Reverse Proxy = Balances traffic + adds smart routing & extra features.

<img width="299" height="198" alt="image" src="https://github.com/user-attachments/assets/dbb5b343-65be-464c-aadb-4e64f735d3e0" />

#### Why Use Load Balancer?

Spread load across multiple downstream instances prevents instances being overloaded bettering user experience.

Expose single point of access DNS to your application.

Handles failures of downstream instances redirects traffic to other healthy instances.

Does regular health checks on your instance.

Provide SSL termination HTTPS for websites load balancer deals with the heavy encryption with HTTPS websites.

Enforces stickiness with cookies - makes sure that each user is still on the same request using cookies in case they are sent back.

High availability across zones - designed to be available.

Separate public traffic from private traffic routes traffic different ways to ensure what is external or internal traffic.

### Why Use an Elastic Load Balancer

This is a managed load balancer.

AWS guarantees that it will be working.

AWS takes care of upgrades, maintenance and high availability.

AWS provides only a few configuration knobs.

#### Why DIY vs Managed Ones?

It costs less to set up your own balancer but it will be more effort you need to handle maintenance monitoring scaling and fault tolerance on your own.

With ELB AWS manages all that complexity.

#### Key AWS Services ELB integrates with:

**Amazon Certificate Manager (ACM):**

Handles SSL/TLS certificates.

ELB can terminate SSL for you (removes complexity).

**Amazon CloudWatch:**

Monitor ELB performance and health.

**Amazon Route 53:**

DNS routing integrated with ELB.

**AWS WAF (Web Application Firewall):**

Protects apps from attacks (SQL injection, XSS, etc.).

**AWS Global Accelerator:**

Improves performance and reliability for global traffic.

#### Why Use ELB?

**Reliable & Low-Maintenance** → AWS manages availability.

**Time-Saving** → No need to build/maintain your own load balancer.

**Scalable** → Works with Auto Scaling seamlessly.

**More costly** than DIY solutions, but saves effort and operational overhead.

### Health Checks

Key part of load balancing.

They allow the load balancer to know if instances are good enough to handle traffic and it is done via requests.

A request is sent to a HTTP protocol on any port then to the end point.

The health check is done on a port and a route (/health is common).

If the response is not 200 being OK then the instance is unhealthy.

If 200 is returned then it's considered healthy.

<img width="475" height="161" alt="image" src="https://github.com/user-attachments/assets/cd7b7aa4-646d-4820-bc94-5646b67f6291" />

---

## Types of Load Balancers on AWS

There are 4 kinds:

### Classic Load Balancer (v1 generation) 2009 - CLB

Handles the basics HTTP, HTTPS, TCP, SSL.

Gets the job done.

Lacks new generation features.

### Application Load Balancer (v2 new gen) 2016 - ALB

HTTP, HTTPS, WebSocket.

Ideal for modern apps.

Works on the application layer.

Smarter able to direct traffic based on requests like URLs headers clear parameters and cookies.

Perfect for microservices or containers that need routing.

### Network Load Balancer (v2 - new gen) - 2017 NLB

TCP TLS, UDP.

Layer 4 Load Balancer.

Designed for high performance scenarios where low latency is needed.

Ideal for handling millions of requests per second.

Used for low latency applications e.g for real time gaming or high frequency trading systems.

### Gateway Load Balancer - 2020 - GWLB

Operates at layer 3 network layer - IP protocol.

Helps deploy scale and manage third party network applications like firewalls intrusion detection systems and traffic analysers in your VPC.

---

## Which AWS Load Balancer Should I Choose?

### Classic Load Balancer (CLB)

Still works but old generation.

Limited features.

AWS recommends using ALB, NLB, or Gateway LB instead.

### Internal vs External

**Internal LB:** Routes traffic inside your VPC (e.g., microservices, private apps).

**External LB:** Handles traffic from the public internet (e.g., websites, APIs).

### Recommended Load Balancers

#### Application Load Balancer (ALB)

**Best for:** Web applications.

**Features:**

Content-based routing (e.g., /api → service A, /images → service B).

Supports modern protocols (HTTP/2, WebSockets).

Good for microservices & container-based apps.

#### Network Load Balancer (NLB)

**Best for:** High-performance, low-latency TCP/UDP traffic.

**Features:**

Handles millions of requests per second.

Very low latency.

Ideal for gaming servers, IoT, VoIP, or real-time apps.

#### Gateway Load Balancer (GWLB)

**Best for:** Advanced network applications.

**Features:**

Routes traffic to third-party appliances (e.g., firewalls, intrusion detection, deep packet inspection).

Useful in secure network setups.

> 👉 Quick Decision Guide:
>
> Traditional web app? → ALB
>
> Need ultra-fast TCP/UDP performance? → NLB
>
> Advanced networking (firewalls, IDS, etc.)? → GWLB
>
> Old system? → Only then use CLB (otherwise avoid).

---

## Load Balancer Security Groups

### Load Balancer Security Group

**Purpose:** Expose the application to the internet safely.

**Rules:**

Allows HTTP (TCP, port 80) from 0.0.0.0/0 → anyone on the internet.

Allows HTTPS (TCP, port 443) from 0.0.0.0/0 → anyone on the internet.

**Effect:** Users can reach the load balancer using either HTTP or HTTPS.

**Key point:** The load balancer is the only public-facing component.

### Application Security Group (EC2/Instances behind LB)

**Purpose:** Protect backend EC2/Istio instances.

**Rules:**

Allows HTTP (TCP, port 80), but only from the Load Balancer SG (not from the public internet).

In the example, the source is set to the load balancer's SG (sg-054b5ff5ea02f2b6e).

**Effect:** The backend instances will not accept traffic directly from users. They only trust requests forwarded by the load balancer.

### Why This Works (Analogy)

Think of it as:

**Load Balancer** = Front Door → Anyone can knock on it (open to the internet).

**Application Servers** = Private Rooms → Only the front door (LB) has the key; strangers can't walk in directly.

This design ensures:

✅ **Security** – Backends aren't exposed to direct attacks from the internet.

✅ **Scalability** – Load balancer can distribute traffic evenly to multiple instances.

✅ **Flexibility** – You can apply SSL termination, routing, or WAF (firewall rules) at the load balancer level.

<img width="770" height="388" alt="image" src="https://github.com/user-attachments/assets/82a22a67-d3e3-4627-8a97-caf3643d7bfd" />

---

## Application Load Balancer

### Application Load Balancer (ALB) Notes + Examples

**ALB operates at layer 7 HTTP layer**

Example: it can inspect HTTP headers to decide routing, unlike a Classic Load Balancer which only works at TCP level.

**Smart enough to understand what is being requested** such as headers, cookies, URLs, strings and more

Example: a request with Cookie: user=premium could be routed to a premium server pool.

**Load balance traffic to multiple HTTP applications across machines**

Example: spreading traffic across 3 EC2 instances running the same web app so no single machine gets overloaded.

**Load balance to multiple applications on the same machine**

Example: one EC2 runs /app1 (Node.js) on port 3000 and /app2 (Flask) on port 4000 — ALB routes to the correct one.

**Has support for HTTP/2 and WebSocket**

Example: useful for chat apps needing WebSocket connections or faster loading with HTTP/2 multiplexing.

**Supports redirects from HTTP to HTTPS as an example**

Example: http://myshop.com automatically redirects to https://myshop.com.

**Routing tables to different target groups**

Example: /images/* → servers optimized for images, /api/* → backend servers.

**Route based on path in URL**

Example: /blog/* goes to blog servers, /shop/* goes to e-commerce servers.

**Routing based on host name in URL**

Example: api.myapp.com → API target group, admin.myapp.com → admin dashboard.

**Routing based on query string, headers**

Example: ?category=toys → toy catalog servers, X-Region: EU header → EU servers.

**ALB are a great fit for microservices & container based applications** e.g. Docker & Amazon ECS

Example: each ECS service (auth, payments, users) gets its own target group, ALB directs requests correctly.

**Has a port mapping feature to redirect to dynamic port in ECS**

Example: ECS assigns random container ports (32768, 32769, etc.), ALB maps incoming traffic automatically.

**In comparison we need multiple classic load balancers per application**

Example: 3 microservices (/auth, /cart, /profile) would need 3 CLBs, but only 1 ALB with routing rules.

### How ALB Handles HTTP Based Traffic

**Users send HTTP requests** → ALB sits in front of the application and receives the traffic.

**ALB examines each request** → decides where to route based on rules (path, host, headers, etc.).

**Routes traffic to Target Groups:**

A target group = a collection of resources (EC2, ECS tasks, or Lambda).

Each target group usually supports a specific service.

Example: one target group for user-related tasks (login, profile).

Another for product search or other functionality.

**Health Checks:**

ALB continuously runs health checks on targets.

If an instance fails health checks, ALB automatically stops sending traffic to it and routes requests to healthy ones.

**Built for HTTP/HTTPS traffic:**

Can handle many web-based services.

Smart enough to separate traffic by path, hostname, or query.

#### Why this matters:

You can route different requests to different parts of your application using one ALB.

**Example:**

https://example.com/profile → routes to User Service Target Group.

https://example.com/search → routes to Search Service Target Group.

**Benefit:**

Keeps services separate so one doesn't interfere with the other.

Makes scaling and performance much easier (great for microservices).

<img width="547" height="307" alt="image" src="https://github.com/user-attachments/assets/189c5ef9-ca01-478e-9db1-de8316096a09" />

---

## Application Load Balancer (Target Groups)

### What are target groups

Target groups are groups of resources like EC2 Lambda functions and ECS tasks that your ALB routes traffic to.

They are destination for requests that a load balancer handles.

Each target group is tied to certain parts of an application allowing scalability of your system independently.

### Different target groups

**EC2 instances** (can be managed by an auto scaling group) - HTTP

**ECS tasks** (managed by ECS) - HTTP

**Lambda functions** - HTTP request is translated into a JSON event

**IP addresses** - must be private IPs

ALB can check route to multiple target groups.

Health checks are at the target group level.

### ALB good to know

Has a fixed host name provided by AWS.

The application servers don't see the IP of the client directly.

The true IP of the client is inserted in the header X-Forwarded-For.

We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto).

<img width="622" height="175" alt="image" src="https://github.com/user-attachments/assets/80cbebdc-cf9d-4b18-947a-483692779452" />

---

## Network Load Balancers

Optimised for handling extreme performance with low latency.

Works on the network layer layer 4.

Forward TCP & UDP traffic to your instances.

Handle millions of requests per seconds.

Less latency - 100ms compared to ALB which has 400ms.

NLB has one static IP per AZ and supports assigning Elastic IP.

NLB are used for extreme performance TCP or UDP traffic.

Not included in AWS free tier.

Used for stuff like IoT gaming Real time processing.

### Network Load Balanced with TCP Traffic

Works at layer 4 and routes raw TCP or UDP traffic to instances without getting involved with higher level traffic like HTTP and HTTPS.

#### How Traffic is Routed

**Incoming Request (WWW)** → Enters NLB with TCP rules.

**NLB forwards traffic to a Target Group:**

**Users Application** → Uses TCP rules.

**Search Application** → Uses HTTP (inside TCP).

#### Key Points About NLB

**Does not:**

Inspect HTTP headers.

Do SSL termination (no HTTPS handling at Layer 7).

Only forwards traffic fast and efficiently without changing it.

#### When to Use NLB

✅ **Best for raw performance and low latency.**

✅ **Can handle millions of requests per second.**

✅ **Ideal for:**

Real-time apps (gaming, streaming, chat, etc.).

Applications sensitive to latency and connection speed.

✅ **Good when different apps use different protocols (TCP vs HTTP).**

#### NLB vs ALB

**NLB (Layer 4):** Speed, raw TCP/UDP forwarding.

**ALB (Layer 7):** Understands HTTP/HTTPS, can do SSL termination, read headers, smarter routing.

> 👉 **Summary:**
>
> The NLB is like a super-fast traffic cop at the network layer. It doesn't "look inside" the traffic (like ALB would), it just forwards TCP/UDP connections to the right target group based on rules. Great for performance-heavy apps needing speed and scalability.

<img width="767" height="322" alt="image" src="https://github.com/user-attachments/assets/b7214f2f-4350-4f81-a65a-00bcdfe08f03" />

---

## Sticky Sessions (Session Affinity)

Sticky sessions ensure the client or user is routed to the same instance behind the load balancer.

Works across different load balancers.

The load balancer uses a cookie to keep track of which instance a client is connected to.

Each cookie binds a user to a certain server.

You can control their time and expiration.

A use case when an application store session data that is not shared across instances.

Makes sure session data is not lost.

For example a cart cookie on an e commerce site since this is a separate instance a cookie will make sure the user doesn't lose their cart data.

May bring imbalance to the load over the back end of instances.

Use if needed.

---

## SSL/TLS Basics

Crucial when dealing with internet security with load balancers.

SSL cert allows traffic between your clients/users and load balancer to be encrypted in transit (in-flight-encryption).

SSL refers to secure sockets layer, used to encrypt connections.

TLS refers to transport Layer security which is a newer version.

Nowadays TLS certificates are mainly used but people still refer to it as TLS.

Public SSL certs are issued by Certificate authorities such as Comodo, Symantec, GoDaddy, Global Sign etc.

SSL certifications have expiration dates you set and must be renewed.

### Load Balancer - SSL Certificates

Load balancer uses a X509 cert SSL certificate.

AWS allows us to manage this using their certificate manager ACM.

ACM is a place where you can create renew and manage certificates.

You can upload your own certificates.

**HTTPS Listener:**

This is how we ensure traffic is encrypted.

You must specify default certificate.

You can add an optional list of certs to support multiple domains.

Clients can use SNI server name indication to specify the host name they reach.

Can specify security policies to support older versions of SSL clients.

### SSL - SNI Server Name Indication

SNI solves the problem of loading multiple SSL certificates onto one web server (to serve multiple websites).

It's a newer protocol and requires the client to indicate the host name of the target server in the initial SSL handshake.

The server will then find the correct certificate or return the default one.

Only works for ALB and NLB new gen, CloudFront.

Does not work for CLB old gen.

### ELB - SSL How Do They Work on Different Load Balancers

#### CLB - Classic Load Balancer v2

Supports only one SSL cert.

Must use multiple CLB for multiple host name with multiple SSL certificates.

#### ALB Application Load Balancer v2

Supports multiple listener with multiple SSL certificates.

Uses server name indication SNI to make it work.

#### Network Load Balancer V2

Supports multiple listeners with multiple SSL certs.

Uses server name indication SNI to make it work.

---

## Connection Draining

**Purpose:** Prevents cutting off ongoing user requests when removing an instance from a load balancer.

**Terminology:**

Classic Load Balancer (CLB) → Connection Draining

Application Load Balancer (ALB) & Network Load Balancer (NLB) → Deregistration Delay

### How it Works

When an instance is deregistered (unhealthy, scaling down, etc.):

The load balancer stops sending new requests to that instance.

Any in-flight requests (already connected) are allowed to complete.

### Timing Control

Can set the wait time: 1–3,600 seconds (default = 300 seconds = 5 minutes).

**Use Cases:**

Short requests → Set lower value to speed up removal.

Instant removal → Set to 0.

Long-running requests → Keep higher value to avoid user disruption.

✅ **Key Benefit:** Ensures smooth user experience during scaling or instance health changes by letting active sessions finish before removal.

---

## Auto Scaling Groups

### What is an Auto Scaling Group ASG

In real life the load on your application and website can change.

In the cloud you can create and get rid of servers very quickly.

**The goal of ASG:**

Scale out add EC2 instance to match increase load.

Scale in remove EC2 instances to match decrease load.

Ensure there is a min and max of EC2 instances running.

Auto register new instances to a load balancer.

Re create an EC2 instance in case a previous one is terminated.

ASG is free you only pay for the EC2 instance.

### Auto Scaling Group

📌 **Key Settings:**

#### Minimum Capacity

The fewest instances that must always run.

Ensures your app is always available, even during low traffic.

#### Desired Capacity

The "target" number of instances for normal load.

ASG tries to keep this number steady under typical conditions.

#### Maximum Capacity

The upper limit of instances ASG can create.

Prevents overspending and unnecessary scaling.

### Scaling Behavior

#### Scale Out (Traffic ↑)

ASG adds new EC2 instances when demand rises.

Can scale up until it reaches the maximum capacity.

#### Scale In (Traffic ↓)

ASG removes instances when demand drops.

Reduces costs but never goes below the minimum capacity.

✅ **Result:**

ASGs automatically balance performance and cost:

Keep enough instances running for reliability.

Add instances during spikes.

Remove them when demand falls.

### Auto Scaling Group in AWS with Load Balancer

#### 1. Users

Send traffic to your application (website, mobile app, or service).

#### 2. Elastic Load Balancer (ELB / ALB)

Distributes traffic evenly across EC2 instances.

Prevents overload on any single instance.

**Performs health checks:**

Continuously monitors EC2 instances.

If an instance is unhealthy → stops sending traffic to it.

Once fixed/replaced → resumes routing traffic.

#### 3. Auto Scaling Group (ASG)

Automatically adjusts the number of instances based on demand.

Scale Out → adds instances during high traffic (e.g., sales day).

Scale In → reduces instances when traffic drops to save costs.

Works with the ELB to ensure only healthy instances serve traffic.

#### 4. Amazon Machine Image (AMI)

Custom AMIs can be used to launch preconfigured instances.

Every new instance created by ASG is ready-to-go with:

Your software

Configurations

Dependencies

✅ **Big Picture:**

**Load Balancer** = smart traffic director.

**ASG** = dynamic capacity manager.

**AMI** = ensures consistency across all instances.

**Together** → they create a scalable, fault-tolerant, and cost-efficient system.

<img width="670" height="318" alt="image" src="https://github.com/user-attachments/assets/836935b6-7075-414d-aaf3-d00fbca605f8" />

---

## Auto Scaling Group Attributes

### Launch Template includes:

**AMI** → Pre-configured Amazon Machine Image (ensures all EC2s start with same setup).

**Instance Type** → Defines resources (CPU, memory, storage capacity).

**EBS Volumes** → Persistent storage attached to EC2s.

**Security Groups** → Virtual firewalls for inbound/outbound traffic.

**SSH Key Pair** → Secure login credentials for EC2 access.

**IAM Role** → Grants EC2 permissions to use AWS services securely.

**VPC + Subnets** → Network placement for instances.

**Load Balancer** → Registers instances for traffic distribution.

### Other ASG attributes:

**Min Size / Max Size / Initial Capacity** → Controls number of running instances.

**Scaling Policies** → Rules that tell ASG when to scale in/out based on demand.

<img width="208" height="241" alt="image" src="https://github.com/user-attachments/assets/4f5d8424-962e-43bb-ba0e-2a4633a4d013" />

---

## Auto Scaling - CloudWatch Alarms & Scaling

CloudWatch alarms keep an eye on certain metrics across ASG like power and CPU usage or a custom metric.

Once alarm is triggered AWS can take action.

You can use scaling policies such as scale out and scale in which increase and decrease instances.

### Auto Scaling Group Scaling Policies

#### Dynamic Scaling

##### Target Tracking Scaling

Simple to set up.

Example: I want the average ASG usage CPU usage to stay around 40%.

##### Simple/Step Scaling

When CloudWatch alarm is triggered example CPU > 70% add 2 units.

When CloudWatch alarm is triggered example CPU < 30% then remove 1.

#### Schedule Scaling

Anticipate a scaling based on known usage patterns.

Example: increase the min capacity to 10 at 5pm on Fridays.








