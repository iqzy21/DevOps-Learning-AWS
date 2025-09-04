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

## containers
# AWS Containers, Serverless & Networking Guide

## Table of Contents
- [Containers](#containers)
- [Serverless](#serverless)
- [Amazon Networking](#amazon-networking)

---

## Containers

### Docker Recap

**Docker** = a platform to build, ship, and run apps in a consistent way.

Packages everything your app needs (code, libraries, dependencies, runtime) into a container.

Runs the same everywhere: laptop, server, or cloud.

### What is a Container?

A lightweight, portable box holding:
- App code
- Libraries & dependencies
- Runtime environment

Ensures the app behaves the same across all systems.

Solves the classic "works on my machine" problem.

### Why Use Docker?

- **Portability** – works on any system with Docker installed
- **Consistency** – same behavior in dev, staging, production
- **Speed** – starts fast, uses fewer resources than VMs
- **Simplicity** – everything bundled and easy to ship/deploy

### How Docker Works

- **Container Image** = blueprint of the app (code + dependencies)
- **Container** = running instance of that image

Works across:
- Local servers
- Cloud environments
- Other machines

### Docker on an Operating System

![Docker Architecture](https://github.com/user-attachments/assets/9a3c67c3-54b5-4ff7-a3fe-2a2d01a97708)

1. **Server (the big box)**
   - Could be an EC2 instance, local machine, or any host
   - Runs the Docker Engine (container runtime)

2. **Containers (the smaller boxes inside)**
   - Each container runs one application with its own dependencies
   - Examples shown in the diagram:
     - Java apps (multiple versions or instances)
     - Nginx web server
     - MySQL database

3. **Key Points from the Diagram**
   - **Isolation**: Each app is in its own container, no interference between Java, Nginx, or MySQL
   - **Shared host resources**: All containers use the same host OS kernel, CPU, memory, and networking
   - **Multiple instances**: You can run multiple versions of the same app (e.g., 3 Java containers) without conflicts
   - **Lightweight**: Unlike VMs, containers don't need their own OS — they share the host's OS kernel

### Where Are Docker Images Stored

**Docker Images Recap**

Docker Image = a blueprint for your app.

Contains code, libraries, dependencies, configs.

Once built → can run as a container.

#### Where Are Images Stored? → Repositories

A repository = folder for Docker images.

Can be public or private.

#### 1. Docker Hub
- Largest public repo for Docker images
- Like GitHub for Docker images
- Hosts base images (Ubuntu, Nginx, MySQL, etc.)
- Anyone can push or pull images

#### 2. Amazon ECR (Elastic Container Registry)
- AWS-managed repository
- Designed for AWS users
- Stores images securely (private by default)
- Can also publish images publicly via ECR Public Gallery
- Integrates with other AWS services (ECS, EKS, etc.)

#### Public vs. Private Repositories

**Public repos** (e.g., Docker Hub, ECR Public):
- Anyone can access/download images
- Good for sharing or using common base images

**Private repos** (e.g., ECR Private):
- Restricted access (only authorized users)
- Used by organizations to control security & access

**Summary:**
Build image → store in a repo (public or private).
- Docker Hub = biggest public repo
- Amazon ECR = secure, AWS-focused repo with private + public options

### Docker vs Virtual Machines

![Docker vs VMs](https://github.com/user-attachments/assets/ea0cf5c9-198c-4e0a-9d4e-372920ea8278)

#### Virtual Machines (VMs)

**What they are**: Full virtual systems that mimic physical hardware.

**Components:**
- Each VM has its own guest OS (Ubuntu, Windows, etc.)
- Managed by a hypervisor (e.g., VMware, Hyper-V)
- Resources: Each VM gets its own chunk of CPU, RAM, storage

**Downsides:**
- Heavy → each VM carries a full OS
- Slow boot times
- High resource usage

#### Containers (Docker)

**What they are**: Lightweight, OS-level virtualization.

**Components:**
- Share the host OS kernel
- Managed by the Docker daemon (lighter than a hypervisor)
- Resources: Multiple containers run side-by-side, each with its own app + dependencies

**Benefits:**
- Fast start/stop times
- Lightweight → no extra OS per container
- Portable → run anywhere Docker is installed
- Efficient → run many more containers per server compared to VMs

#### Key Difference

- **VMs** → Virtualize hardware (each has its own OS)
- **Containers** → Virtualize the OS (apps isolated but share same kernel)

### Getting Started with Docker

![Docker Workflow](https://github.com/user-attachments/assets/e489e36c-2ded-4964-9b8a-4b581ae3862b)

#### 1. Dockerfile → The Recipe

Defines how to build your Docker image.

**Common instructions:**
- `FROM` → base image (e.g., ubuntu:18.04)
- `COPY` → copy app files into the image
- `RUN` → run commands during build (install deps, compile)
- `CMD` → default command when container starts (e.g., run Python script)

#### 2. Build the Image

**Command:**
```bash
docker build -t myapp .
```

This takes the Dockerfile → produces an image.

Image = blueprint you can reuse to start containers.

#### 3. Run the Container

**Command:**
```bash
docker run myapp
```

This creates a container = running instance of your image.

Example: container running a Python application.

#### 4. Push & Pull Images

Push images to a registry (cloud storage for images).
- Docker Hub (public repo)
- Amazon ECR (private/public repo for AWS)

Pull them later to use anywhere (local, servers, cloud).

**Summary Flow:**
Dockerfile (blueprint) → Build (Image) → Run (Container) → Push/Pull (Registry like Docker Hub / ECR)

### Container Related Services

#### Amazon Elastic Container Service (ECS)
- Amazon's own platform
- Fully managed service that allows you to run docker containers without having to install and manage orchestration software
- You can define container number, images to use, how they should interact through a simple interface

#### Elastic Kubernetes Service Amazon (EKS)
- Amazon managed kubernetes
- With EKS you can take advantage

#### AWS Fargate
- Amazon own serverless container platform
- Works with ECS and EKS

#### Amazon ECR
- Stores container images

### Amazon ECS - EC2 Launch Type

![ECS EC2 Launch Type](https://github.com/user-attachments/assets/8c66c9fd-d269-4055-a723-8119c881b928)

ECS (Elastic Container Service)

Launch docker container on AWS = launch ECS tasks on ECS clusters

**EC2 launch type**: you must provision and maintain the infrastructure (the EC2 instances)
- Each EC2 instance must run the ECS agent to register in the ECS cluster
- AWS takes care of starting/stopping containers

### Amazon ECS Fargate Launch Type

Launch docker containers on AWS

You do not provision the infrastructure no EC2 instances to manage

It's all serverless

You just create task definitions

AWS just runs ECS tasks for you based on the CPU / RAM you need

To scale, just increase the number of tasks simple- no more EC2 instance tasks

### Amazon ECS IAM Roles for ECS

![ECS IAM Roles](https://github.com/user-attachments/assets/0369381e-3e64-49b4-9583-a8aa8fcd28cb)

Used by the ECS agent:
- Makes API calls to ECS service
- Send container logs to cloudwatch logs
- Pull docker image from ECR
- Reference sensitive data in secrets manager or SSM parameter store

**ECS TASK ROLE**
- ALLOWS EACH TASK TO HAVE A SPECIFIC ROLE
- USE DIFFERENT ROLES FOR THE DIFFERENT ECS SERVICES YOU RUN
- TASK ROLE IS DEFINED IN THE TASK DEFINITION

### Amazon ECS Load Balancer Integration

![ECS Load Balancer](https://github.com/user-attachments/assets/f65a601f-6bea-4056-8bd3-5babbdb76ee4)

**Application Load Balancer** supported and works for most use cases the go to choice as it operates at layer 7
- Works with https/http
- Supports path based routing

**NLB** recommended for high performance or to pair with AWS private link doesn't have advance feature

**CLB** supported not recommended used for legacy systems

### ECS Service Auto Scaling

#### What is ECS Service Auto Scaling?

Automatically adjusts the number of ECS tasks running based on demand.
- Scales up when traffic/CPU goes up
- Scales down when things calm down → saves money
- Uses AWS Application Auto Scaling under the hood

#### Metrics Used for Scaling

- CPU utilization
- Memory utilization
- ALB (Application Load Balancer) request count per target
- Other custom CloudWatch metrics

#### Scaling Strategies

**Target Tracking (simple & dynamic)**
- Set a target (e.g., keep CPU at 40%)
- AWS automatically adjusts tasks to maintain that target
- Easiest to use

**Step Scaling (fine-grained control)**
- Define thresholds and actions:
  - If CPU > 70% → add 2 tasks
  - If CPU < 30% → remove 1 task
- More manual but gives full control

**Scheduled Scaling (predictable traffic)**
- Pre-plan scaling events
- Example: If traffic spikes every Tuesday 5 PM → scale up before, scale down after

#### ECS with Fargate

- No need to manage EC2 instances
- You only define how many tasks you want
- AWS manages infrastructure scaling for you

**Summary**

ECS auto scaling = handles spikes, saves costs.

Options: Target Tracking (simple), Step Scaling (precise), Scheduled Scaling (predictable).

Even easier with Fargate since AWS manages the servers.

### Amazon ECR

![Amazon ECR](https://github.com/user-attachments/assets/83b4f707-f144-40be-b5b5-8b323ffa303e)

#### What is ECR?

- Cloud storage system for Docker images
- Works like a container image hub (similar to Docker Hub but on AWS)
- Designed to integrate directly with ECS
- Stores images securely (backed by Amazon S3)

#### Types of Repositories

**Private Repositories**
- Store internal/sensitive Docker images
- Access controlled by IAM

**Public Repositories**
- For open-source or shared images
- Amazon ECR Public Gallery provides free/public images

#### ECS Integration

- ECS tasks can pull images directly from ECR
- Makes deployment smooth and automatic

#### Security & Access

- Permissions controlled with IAM policies
- Common issue: if pulling fails → check IAM permissions

#### Extra Features

- **Vulnerability Scanning** → scans images for security risks
- **Versioning & Tagging** → manage multiple image versions (e.g., v1.0, latest)
- **Lifecycle Management** → automatically deletes old images to save space

**Summary:**
ECR = secure, scalable Docker image storage on AWS. Integrated with ECS, controlled by IAM, with extra features like scanning, versioning, and lifecycle cleanup.

### Amazon EKS

#### What is EKS?

- Fully managed Kubernetes service on AWS
- AWS manages the control plane (updates, patching, scaling)
- You just focus on running apps, not cluster management

#### Why Kubernetes?

- Open-source container orchestration platform
- Automates deployment, scaling, and management of containers
- Helps run containers at scale

#### EKS vs ECS

**ECS (Elastic Container Service)**
- AWS-specific, simpler API
- Deep integration with AWS services

**EKS (Elastic Kubernetes Service)**
- Based on Kubernetes (open-source, cloud agnostic)
- Easier migration between AWS, GCP, Azure (AKS), or on-prem
- Ideal if you already use Kubernetes elsewhere

#### Launch Types in EKS

- **EC2 Launch Type** → You manage worker nodes (EC2), AWS manages control plane
- **Fargate Launch Type** → Serverless, AWS runs the containers, no need to manage EC2

#### Use Cases

- Migrating from on-prem or another cloud while keeping Kubernetes
- Running workloads across multiple clouds (cloud agnostic)
- Multi-region deployments → 1 EKS cluster per region

#### Monitoring & Insights

- Integrated with CloudWatch Container Insights
- Tracks logs, metrics, performance
- Helps troubleshoot & optimize workloads

### EKS Architecture

![EKS Architecture](https://github.com/user-attachments/assets/6e28614a-2f0c-4ace-9a2d-c0d4c80363b4)

#### Core Components

**EKS Worker Nodes**
- The yellow boxes = EC2 instances running Kubernetes worker processes
- These nodes actually run your pods (smallest unit in Kubernetes)

**Pods**
- Groups of containers that perform specific tasks
- Hosted inside the worker nodes

**Auto Scaling Group (ASG)**
- Worker nodes are part of an ASG
- Scales nodes up/down automatically based on load

#### Networking & Security (VPC setup)

**VPC (Virtual Private Cloud)**
- Isolated private network where the entire EKS setup runs
- Ensures secure communication between components

**AZs (Availability Zones)**
- Multiple AZs used for high availability
- If one AZ goes down, others keep the app running

**Private Subnets**
- Worker nodes are deployed here for security
- Not directly exposed to the public internet

**NAT Gateway**
- One in each AZ
- Lets worker nodes access the internet outbound (e.g., to pull Docker images, updates)
- Prevents inbound public traffic from reaching nodes directly

#### Traffic Management

**ELB (Elastic Load Balancer)**
- Distributes incoming user traffic across nodes
- Sends requests to healthy nodes only
- Improves performance + fault tolerance

#### Why Multiple AZs?

- Provides resilience & fault tolerance
- If one AZ fails, traffic is routed to nodes in other AZs
- Keeps the application always available

**Summary**

This architecture is a secure, scalable, highly available setup for Kubernetes on AWS.

EKS nodes + pods → Run your workloads.
ELB → Balances user traffic.
NAT Gateways → Allow safe outbound internet access.
VPC + private subnets → Provide security and isolation.
Multiple AZs → Ensure uptime even if one zone fails.

### Amazon EKS Node Types

**Managed Node Groups**
- Creates and manages nodes (EC2 instances) for you
- Nodes are part of an ASG managed by EKS
- Supports on demand or spot instances

**Self Managed Nodes**
- Nodes created by you and register to the EKS cluster and managed by ASG
- You can use prebuilt AMI amazon EKS optimised AMI
- Support on demand or spot instances

**AWS Fargate**
- No maintenance required no nodes managed its serverless all you need to do is define cpu and memory requirements for containers

---

## Serverless

### Serverless Overview

Serverless is when developers don't have to manage servers anymore

They just deploy code

They deploy functions

Serverless is a FAAS (Function as a Service)

Serverless was pioneered by AWS lambda but now includes anything that's managed

Serverless does not mean there are no servers

It means you don't manage provision or see them

### Serverless in AWS

![Serverless in AWS](https://github.com/user-attachments/assets/dc42629a-d581-4636-9d46-86a51f346076)

**Lambda** - US the core of serverless offerings
You just write your functions deploy the lambda function and it does the job

**DynamoDB** - fully managed serverless nosql database that scales automatically

**Cognito** - helps manage user authentication easier to handle logins and signups on applications

**API Gateway** - acts like a bridge between users and lambdas functions you can create and monitor apis which interact with your back end services

**S3** - serverless offering for storing files and content

**SNS & SQS** - SNS handles notifications SQS queues messaging between services they are messaging systems

**Kinesis Data Fire hose** - used to lower streaming data in the AWS for analysis and storage

**Aurora Serverless** - fully managed serverless database that auto scales on demand

**Step Functions** - step functions helps you manage multiple lambda functions

**Fargate** - serverless compute option AWS handles your ec2 instances

### Why AWS Lambda

#### EC2 (Elastic Compute Cloud)

- Virtual servers (VMs) in the cloud
- You choose CPU & memory for each instance
- Always running → you pay even when idle
- Scaling requires manual setup (or auto scaling configs)
- Good for long-running applications or when you need full control

#### Lambda (Serverless Functions)

- No servers to manage → AWS handles it
- Runs on-demand, only when triggered
- Billed only for execution time (no idle cost)
- Execution limit → max 15 minutes per function
- Best for short, event-driven tasks (e.g., S3 upload trigger, API call, stream processing)
- Automatic scaling → handles 1 to 1M+ requests seamlessly

**Summary**

EC2 = Full control, but you manage servers, scaling, and costs (can pay for idle).

Lambda = Serverless, auto-scaling, cost-efficient, but limited to short-lived, event-driven workloads.

### Benefits of Lambda

**Easy pricing** - pay for what you use
- Free tier give 1000000 requests and 4000000Gb of compute time
- Integrated with the whole aws suite of services
- Integrated with many programming languages
- Easy monitoring through cloudwatch
- Easy to get more resources per functions
- Increasing ram will improves CPU and network

#### Built-in Supported Languages

- **Node.js (JavaScript)** → great for event-driven apps
- **Python** → widely used for data processing, automation, serverless web apps
- **Java** → popular for enterprise-level apps
- **C#, .NET Core, PowerShell** → strong fit for Microsoft developer background
- **Ruby** → good for scalable web apps (e.g., with Rails)

#### Custom Runtimes

- Use the Lambda Runtime API to bring your own language
- Example: Go (Golang) and other non-officially supported languages
- Lets you choose a language that fits your project best

#### Lambda Container Images

- Package your app into a Docker container image
- Ideal for:
  - Complex app setups
  - Custom dependencies
  - More control over runtime environment
- Requirement: container must implement Lambda Runtime API

If you want full flexibility with Docker containers (beyond Lambda limits), use ECS Fargate instead.

**Summary**

AWS Lambda supports multiple languages out-of-the-box, plus custom runtimes and container images for flexibility. This means you can run serverless functions in the language and environment you're most comfortable with.

### Example of Serverless

#### Example: Serverless Thumbnail Creation

![Serverless Thumbnail Creation](https://github.com/user-attachments/assets/d7c0fb02-609e-4b75-87cf-e59f7034d32c)

**Flow of Events**

1. **Upload Image to S3**
   - User uploads an image (e.g., beach photo) to an S3 bucket
   - This is the starting point

2. **Trigger Event**
   - The S3 upload event automatically triggers a Lambda function
   - No manual step → fully event-driven

3. **Lambda Function Executes**
   - Lambda takes the uploaded image
   - Generates a smaller thumbnail version

4. **Push to S3**
   - The new thumbnail is stored back into S3 (often in a different bucket/folder)
   - Now you have both the full-size image + the thumbnail

5. **Store Metadata in DynamoDB**
   - Lambda also records details about the image (name, size, timestamp, etc.)
   - Stored in DynamoDB for easy querying later

**Why This Is Cool**

- **Fully Automated** → just upload the image, everything else happens automatically
- **Serverless** → no servers to manage, AWS handles scaling
- **Cost-Efficient** → pay only when Lambda runs + storage costs
- **Scalable** → works whether 1 image or 1 million images are uploaded

#### Example: Scheduled Task with EventBridge + Lambda

![Scheduled Task](https://github.com/user-attachments/assets/47c98910-dc61-43ab-98e7-e7fab861abe4)

**Flow**

1. **EventBridge (CloudWatch Events)**
   - Acts like a scheduler/cron job
   - Example: trigger every 1 hour
   - Can be set to run at specific times/dates using cron expressions

2. **Lambda Function**
   - Triggered automatically by EventBridge
   - Runs your code only when scheduled
   - Can perform tasks like:
     - Backups
     - Cleanup jobs
     - Generating reports
     - Interacting with other AWS services (e.g., S3, DynamoDB, EC2)

**Why This Is Useful**

- **Event-driven** → runs only when triggered
- **No servers** → no need for a VM running 24/7
- **Cost-effective** → you only pay when Lambda runs
- **Flexible scheduling** → run every minute, hour, day, or custom cron timing

**Summary:**
EventBridge = scheduler
Lambda = worker
Together → fully managed, cost-efficient way to automate recurring tasks in AWS.

---

## Amazon Networking

### Understanding CIDR - IPv4

Classless Inter Domain Routing

CIDR is a method for allocating IP addresses

CIDR help us define IP ranges

If an IP address has XX.XX.XX.XX/(ANYNUMBER) - 1 IP

If an ip is 0.0.0.0/0 is is public

#### CIDR consists of two components

**BASE IP**
- Represents an IP container range
- Example 10.0.0.0,192168.0.0

**Subnet mask**
- Defines how many bits and IP can change
- Example /0/24/32
- Can take 2 forms
  - /8 → 255.0.0.0
  - /16 → 255.255.0.0
  - /24 → 255.255.255.0
  - /32 → 255.255.255.255

### Visual Breakdown

![CIDR Visual](https://github.com/user-attachments/assets/bbbeccd5-da9d-4a88-9e51-54a213992221)

Here's a visual breakdown of CIDR:

- **Blue** = network bits (fixed, define the subnet)
- **Gray** = host bits (variable, used for devices inside the subnet)

- /8 → Huge network, many hosts (16M+)
- /16 → Medium network (65K hosts)
- /24 → Small network (254 hosts, common in LANs)
- /32 → Single device (no host bits left)

This way, you can literally see how the prefix length (/X) shrinks the host space and increases network specificity.

### Understanding CIDR - Subnet Mask

![Subnet Mask](https://github.com/user-attachments/assets/bc6023a1-715e-4af6-b137-74334891b7a4)

A subnet mask (or CIDR slash notation) defines how many IP addresses exist in a subnet.

- /32 → Only 1 IP (usually assigned to a single host)
- /31 → 2 IPs
- /30 → 4 IPs
- /29 → 8 IPs
- /28 → 16 IPs
- /27 → 32 IPs
- /26 → 64 IPs
- /25 → 128 IPs
- /24 → 256 IPs
- /16 → 65,536 IPs
- /0 → Covers all IPs (entire IPv4 space)

Notice how each step doubles the number of available IPs. That's because each time you reduce the subnet mask (make it less restrictive), you free up one more host bit, which doubles the possible addresses.

#### Octet Relationship

Think of an IP address as 4 octets (192.168.0.0 for example).

- /32 → No octet can change → one IP
- /24 → Last octet can change (0–255) → 256 IPs
- /16 → Last two octets can change (0–255.0–255) → 65,536 IPs
- /8 → Last three octets can change → 16.7 million IPs
- /0 → All octets can change → every IPv4 address

#### Example Walkthrough

Let's take 192.168.0.0/30:

- Subnet mask: 255.255.255.252
- Binary: 11111111.11111111.11111111.11111100
- Host bits: 2 (the last two zeroes)
- Total IPs = 2² = 4

That gives you:
- 192.168.0.0 → Network address
- 192.168.0.1 → Usable host
- 192.168.0.2 → Usable host
- 192.168.0.3 → Broadcast address

So /30 means 2 usable IPs for devices.

#### Why This Matters (AWS & VPC Example)

In AWS, when you create a VPC (Virtual Private Cloud) or subnets, you must plan how many IPs you'll need:

- /16 → Large network, ~65K IPs (used for VPC range)
- /24 → Smaller subnet, 256 IPs (commonly used for private or public subnets)

This way, you don't run out of addresses, and you don't waste too many.

**Summary:**

- Subnet masks tell you how many IPs are in a range
- The number of IPs doubles each time you increase host bits by 1
- CIDR /X → smaller number means more hosts, larger number means more specific, fewer hosts
- AWS typically uses /16 (for VPCs) and /24 (for subnets)

### Public vs Private IPv4

#### 1. Private IPv4 Addresses

The IANA (Internet Assigned Numbers Authority) reserved specific IP ranges for private use only. These addresses are not routable on the public internet — they are meant for local networks (like your home Wi-Fi, office LAN, or cloud VPC).

The reserved private ranges are:

- 10.0.0.0 → 10.255.255.255 (10/8 range)
- 172.16.0.0 → 172.31.255.255 (172.16/12 range)
- 192.168.0.0 → 192.168.255.255 (192.168/16 range)

**Example:**
Your home router might give your laptop an IP like 192.168.1.10.
That IP works inside your house, but no one on the internet can reach it directly.

**Special note:**
127.0.0.1 = Loopback address → used to test networking on your own computer ("localhost").

#### 2. Public IPv4 Addresses

Any IPv4 address outside those private ranges is a public IP.

These can be accessed from anywhere on the internet (as long as firewalls or ISPs don't block them).

Websites, servers, and cloud services usually use public IPs so users around the world can connect.

**Example:**
- Google DNS: 8.8.8.8 → Public
- A website's server might be 203.0.113.25 → Public

#### 3. How They Work Together

- Your home devices use private IPs (e.g., 192.168.1.20)
- Your router has a public IP assigned by your ISP (e.g., 81.2.69.142)
- NAT (Network Address Translation) maps your private IPs → public IP, so you can browse the internet
- Without NAT and private IP ranges, we would've run out of IPv4 addresses decades ago

**Summary:**

- **Private IPs** = Local networks only (10.x.x.x, 172.16–31.x.x, 192.168.x.x)
- **Public IPs** = Routable on the internet, used by websites & servers
- **Loopback** (127.0.0.1) = Your own machine, for local testing
- **NAT** lets private IP devices share one public IP to connect online

### Default VPC

**Default VPC** (what you get automatically)

- Comes with your AWS account (1 per region)
- Already has internet access via an Internet Gateway
- Has 1 public subnet per AZ
- EC2s launched here get a public + private IPv4 by default
- EC2s also get DNS names (public + private)
- Includes a default security group + route table

### VPC in AWS

**AWS VPC (Virtual Private Cloud) Notes**

A VPC = your own private network in AWS (like a mini data center).

You can have up to 5 VPCs per region by default (limit can be increased).

#### CIDR Range Limits

- **Smallest**: /28 → 16 IPs
- **Largest**: /16 → 65,536 IPs

#### Allowed Private IP Ranges

- 10.0.0.0/8 → very large networks
- 172.16.0.0 – 172.31.255.255 (/12) → AWS commonly uses 172.31.0.0/16 for default VPCs
- 192.168.0.0/16 → common for home networks

#### Important Rule

**VPC CIDR must not overlap with:**
- Other VPCs
- Your corporate network
- Any connected network

Otherwise, routing will break.

**In short:**
A VPC gives you a custom private network in AWS, with flexible IP ranges, but you need to plan the CIDR carefully to avoid overlap.

### VPC - Subnet IPv4

**Subnets in a VPC**

- Subnets = smaller networks inside your VPC
- Spread across Availability Zones (AZs) for redundancy & high availability
- Can be:
  - **Public subnet** → internet access allowed
  - **Private subnet** → no direct internet access

#### AWS Reserves 5 IPs in Every Subnet

Example: 10.0.0.0/24 → 256 IPs total, but 5 are reserved:

- .0 → Network address
- .1 → VPC router
- .2 → Amazon DNS
- .3 → Future use
- .255 → Broadcast (AWS doesn't use broadcast, but still reserved)

So usable IPs = total - 5.

#### Example of Planning

- /27 = 32 IPs → usable = 27 (too small for 29 instances)
- /26 = 64 IPs → usable = 59 (fits 29 instances)

### Internet Gateway

Allows resources in a vpc to connect ot the internet

Scales horizontally and is highly available and redundant must be created separately from a VPC

One VPC can only be attached to one IGW and etc

IGW on their own do not allow internet access

Route tables must also be updated

### Bastion Hosts

![Bastion Host](https://github.com/user-attachments/assets/3ae70930-5bfc-4952-960f-8e4313d784d2)

#### What is a Bastion Host?

- A bastion host = a special EC2 instance in a public subnet
- Acts as a secure bridge to reach private EC2 instances in a private subnet
- You SSH into the bastion first, then from there into your private instances

#### Why Do We Need Them?

- Private EC2 instances (databases, backends) should not be exposed to the internet
- But admins still need a way to log in for maintenance/updates
- Bastion hosts provide a controlled, secure entry point

#### How It Works (with your diagram)

- Bastion Host sits in a public subnet
- You SSH into the bastion from your office/home IP
- From the bastion, you SSH into private EC2s inside the private subnet

#### Security Best Practices

- **Bastion Security Group** → only allow inbound SSH from trusted IPs (e.g., office IP range)
- **Private EC2 Security Group** → only allow inbound SSH from the bastion host (not the whole internet)
- Use key pairs or even MFA for stronger access control

**Summary:**
A bastion host = secure gateway to private EC2s. It prevents exposing sensitive systems directly to the internet while still allowing admins safe access when needed.

#### Walkthrough of the Diagram

**User (Admin)**
- You (the admin) are outside the VPC
- You connect via SSH into the bastion host

**Public Subnet (Top Box)**
- Contains the bastion host EC2 instance
- Security Group (BastionHost-SG) only allows SSH from trusted IPs (like your office or home)

**Private Subnet (Bottom Box)**
- Contains the private EC2 instances (databases, backend servers)
- These cannot be reached from the internet directly
- Security Group (LinuxInstance-SG) allows SSH only from the bastion host

**Flow**
- You → SSH into Bastion Host (public subnet)
- Bastion Host → SSH into Private EC2s (private subnet)

**Key Point**

The bastion host acts as the middleman:
- Publicly accessible but restricted (secure)
- Lets you safely reach private instances without exposing them to the internet

### NAT Gateway

#### What is a NAT Gateway?

- **NAT** = Network Address Translation
- Lets private subnet instances access the internet (updates, APIs, patches)
- Blocks inbound traffic → internet cannot reach private instances

#### Why It's Important

- Keeps private resources secure (not exposed)
- Still allows outbound internet access when needed

#### Key Features

- **Fully managed by AWS** → no patching, scaling, or maintenance
- **High bandwidth** → 5 Gbps baseline, auto-scales up to 100 Gbps
- **AZ specific** → must deploy 1 per Availability Zone for redundancy
- Uses an Elastic IP for outbound connections
- Requires an Internet Gateway (NAT gateway → Internet Gateway → Internet)
- No Security Groups needed (unlike NAT instances)

#### Limitations

- Instances in the same subnet as the NAT gateway can't use it
- Billed hourly + per GB of traffic → can get costly at scale

#### Typical Setup

Private Subnet → routes outbound traffic → NAT Gateway (in Public Subnet) → Internet Gateway → Internet.

**Summary:**
A NAT Gateway = a secure, managed bridge for private subnet instances to reach the internet outbound only, while keeping them safe from inbound internet traffic

### NAT Gateway with High Availability

![NAT Gateway High Availability](https://github.com/user-attachments/assets/74f9fd5f-9753-497f-8ac4-c7a0d2fa3cb1)

#### NAT Gateway & High Availability

**1. The Setup (from the diagram)**

VPC has:
- **Public Subnets** (top row) → each with a NAT Gateway
- **Private Subnets** (bottom row) → EC2 instances
- NAT Gateways connect → Internet Gateway → Internet
- Router handles the routing between subnets

**2. The Problem (Single NAT Gateway)**

- NAT Gateways are AZ-specific
- If you only have one NAT Gateway in AZ-A, and AZ-A goes down:
  - ✅ EC2s in AZ-B are still running
  - ❌ But they lose internet access, because their traffic depended on the NAT Gateway in AZ-A

**3. The Solution (Multiple NAT Gateways)**

Deploy a NAT Gateway in each AZ where you have resources.

Example:
- NAT Gateway in AZ-A → serves EC2s in AZ-A
- NAT Gateway in AZ-B → serves EC2s in AZ-B

If one AZ fails, the other AZ still has internet connectivity.

#### Why This Matters

- **High Availability** → avoids single points of failure
- **Disaster Recovery** → ensures your private workloads (databases, app servers) can still reach the internet for updates/APIs

**Summary:**
A NAT Gateway is only highly available within its own AZ. To achieve true high availability across multiple AZs, deploy one NAT Gateway per AZ.

### NAT Gateway vs NAT Instance

![NAT Gateway vs NAT Instance](https://github.com/user-attachments/assets/da1c6e6d-6adb-4aee-afa8-8c9a9e51d19e)

| Feature              | **NAT Gateway** 🟦                                | **NAT Instance** 🟧                          |
| -------------------- | ------------------------------------------------- | -------------------------------------------- |
| **Availability**     | HA within an AZ (deploy 1 per AZ for multi-AZ HA) | No built-in HA (needs failover scripts)      |
| **Bandwidth**        | Up to **100 Gbps**, auto-scales                   | Limited by EC2 instance type                 |
| **Maintenance**      | Fully managed by AWS (no patches/updates)         | You manage OS, patches, scaling              |
| **Cost Model**       | Simple: hourly + per GB data transfer             | Based on EC2 instance type + network charges |
| **Elastic IP**       | Requires Elastic IP                               | Requires Elastic IP                          |
| **Security Groups**  | Not needed (AWS manages security)                 | Can attach SGs like any EC2                  |
| **Bastion Host Use** | ❌ No (only forwards traffic)                      | ✅ Yes (can double as bastion host/jump box)  |
| **Scaling**          | Auto-scales                                       | Manual (resize EC2 or add more)              |

**Summary**

- **NAT Gateway** → Best for production: scalable, simple, low maintenance, but higher cost
- **NAT Instance** → More flexible & cheaper for small setups, but requires manual management and gives you extra options (like doubling as a bastion host)

### NACLs (Network Access Control Lists)

#### 1. Basics

- Work at the subnet level (1 NACL per subnet)
- Act like a big filter that allows or denies traffic
- Every subnet gets a default NACL, but you can create custom ones

#### 2. Key Characteristics

- **Stateless** → return traffic isn't automatically allowed (must add both inbound + outbound rules)
- Rules are numbered (1–32766) →
  - Lower numbers = higher precedence
  - First match wins (no further rules are checked)
- Catchall rule (*) at the end → denies everything not explicitly allowed

#### 3. Example

- Rule #100: ALLOW 10.0.0.10/32
- Rule #200: DENY 10.0.0.10/32

👉 Rule #100 takes precedence (lower number wins).

#### 4. Best Practices

- Use rule numbers in increments of 100 (100, 200, 300, …) → easy to insert new rules later
- Use NACLs for broad subnet-level blocking (e.g., block malicious IPs)

#### 5. Why Use NACLs?

- Extra layer of security on top of Security Groups
- Good for subnet-wide rules (affects all resources in that subnet)
- Useful for blocking specific IPs/ranges before they reach your instances

**In summary:**
NACLs = stateless subnet firewalls. They control traffic in/out of a subnet with numbered rules (first match wins). Great for broad blocking and defense in depth.

### Security Groups & NACLs

![Security Groups & NACLs](https://github.com/user-attachments/assets/9efecad3-9785-4add-bf7c-23ae381511ee)

#### SGs vs NACLs (based on diagram)

**Scope**
- **SG (Security Group)** → Works at instance level (controls traffic to/from EC2)
- **NACL (Network ACL)** → Works at subnet level (controls traffic for all resources in subnet)

**Incoming Request (Left side of diagram)**
- Traffic hits NACL inbound rules (stateless)
- Must be explicitly allowed
- If denied → blocked immediately
- If allowed → traffic reaches SG inbound rules (stateful)
- If allowed by SG → request accepted
- For the response:
  - SG automatically allows it back (stateful)
  - NACL outbound must also allow it (stateless)

**Outgoing Request (Right side of diagram)**
- Instance sends traffic → SG outbound rules checked
- If allowed → traffic leaves the instance
- Then passes through NACL outbound rules (stateless)
- Must explicitly allow it
- When the response returns:
  - Must pass NACL inbound rules (stateless)
  - Then SG inbound rules (stateful)

**Key Difference**
- **SGs are stateful** → If inbound is allowed, return traffic is allowed automatically
- **NACLs are stateless** → Must allow both inbound + outbound explicitly

**Summary (diagram in words):**

NACLs filter first at the subnet edge → stateless (need rules in both directions).
SGs filter next at the instance → stateful (return traffic auto-allowed).
Together they provide layered security: subnet-wide + instance-level control.

### VPC Peering

![VPC Peering](https://github.com/user-attachments/assets/36edf5bc-6bf2-4870-a622-681c7547f50d)

Privately connect two vpc using aws network
Make them behave as if they were in the same network
Must not overlap CIDRs
VPC peering connection is not transitive (must be established for each VPC that need to communicate with other one)
You must update route tables in each VPC subnets to ensure EC2 instances can communicate with each other

#### VPC Peering Explained (with Diagram)

**VPC-A and VPC-B**
- There's a VPC Peering (A–B) connection
- This allows private communication between A and B over AWS's internal network (not the internet)

**VPC-B and VPC-C**
- There's also a VPC Peering (B–C) connection
- So B and C can talk to each other privately

**VPC-A and VPC-C**
- Notice: there is no direct peering connection between A and C
- Even though A connects to B, and B connects to C → peering is non-transitive
- Meaning A cannot automatically talk to C through B
- 👉 If you want A and C to communicate, you must create a direct VPC Peering (A–C) (as shown on the left side of the diagram)

#### Key Notes from the Diagram

- **Non-transitive rule**: Just because A ↔ B and B ↔ C are connected, does NOT mean A ↔ C works
- Route tables must be updated in each VPC so traffic knows to flow through the peering connection
- No overlapping CIDRs allowed (A, B, and C must have different IP ranges)

**In summary (diagram in words):**

VPC Peering creates private, secure links between VPCs.
But communication is point-to-point only (non-transitive).
If multiple VPCs need to talk, you must peer each pair directly.

### VPC Peering Good to Know

#### Cross-Account & Cross-Region Peering

You can peer VPCs across:
- Different AWS accounts
- Different regions

Useful for large orgs where teams/departments have separate accounts but need secure communication.

#### 2. Security Group (SG) References Across Accounts

You can reference a security group in a peered VPC, even across accounts, as long as both VPCs are in the same region.

Example: Instead of whitelisting IPs, you reference an SG ID from another account → traffic rules are more dynamic and secure.

#### 3. Why This Matters

Example:
- Prod account VPC + Dev account VPC
- Normally isolated (for security)
- But sometimes Dev team needs access to Prod resources

With VPC peering + SG references:
- Only specific traffic is allowed
- The rest of the infrastructure stays isolated

**Summary:**

- VPC peering works across accounts and regions
- You can use cross-account SG references (same region only) for easier, more secure traffic management
- Great for multi-account orgs where environments (Dev, Prod, etc.) need selective, secure communication

### VPC Endpoints (AWS PrivateLink)

![VPC Endpoints](https://github.com/user-attachments/assets/7abef26a-d9f7-47dc-8139-7ed092cca290)

#### What is a VPC Endpoint?

- Powered by AWS PrivateLink
- Lets you connect to AWS services (S3, SNS, DynamoDB, etc.) privately inside AWS's network
- No internet exposure → traffic never leaves AWS

#### Why Use It?

- **Security** → avoids sending traffic over the public internet
- **Reliability** → redundant + horizontally scalable
- **No NAT/IGW required** → private instances can still reach AWS services without internet gateways or NAT gateways

#### Two Options (Accessing AWS Services)

**Without VPC Endpoint**
- Private instance → NAT Gateway → Internet → AWS service
- Uses public internet path

**With VPC Endpoint**
- Private instance → VPC Endpoint → AWS service
- Stays fully within AWS private network

#### Common Issues

- **DNS resolution** → must be enabled for endpoints to resolve correctly
- **Route tables** → must route traffic to the endpoint, not the internet

**Summary:**
VPC Endpoints let your resources in private subnets securely access AWS services without using the internet, improving security, compliance, and reliability.

#### VPC Endpoints in Action (Diagram Explanation)

**Public Subnet Path (Top)**
- The public EC2 instance connects to the internet through an Internet Gateway
- From there, it reaches Amazon SNS directly (public path)
- This is the "normal" way, but it exposes traffic to the public internet

**Private Subnet – Option 1 (via NAT Gateway)**
- The private EC2 instance has no direct internet access
- To reach Amazon SNS, it must send traffic → NAT Gateway → Internet Gateway → Amazon SNS
- This path works but still routes through the public internet

**Private Subnet – Option 2 (via VPC Endpoint)**
- Instead of using NAT, the private EC2 instance connects directly to Amazon SNS using a VPC Endpoint
- This keeps traffic inside AWS's private network (powered by PrivateLink)
- More secure, reliable, and cost-effective, since no NAT/IGW is needed

**In summary (diagram in words):**

- **Public EC2** → IGW → SNS (public route)
- **Private EC2**:
  - Option 1 → NAT + IGW → SNS (internet path)
  - Option 2 → VPC Endpoint → SNS (private path, more secure)

### Types of Endpoints

#### 1. Interface Endpoints (Powered by PrivateLink)

![Interface Endpoint](https://github.com/user-attachments/assets/5db38d85-87de-4dd5-b44b-192a727f3cc4)

- Creates an ENI (Elastic Network Interface) in your subnet
- Provides a private IP for connecting to AWS services
- Supports most AWS services (e.g., SNS, SQS, API Gateway, etc.)
- Security Groups required → since traffic flows via the ENI
- Cost: Charged hourly + per GB of data processed

👉 **Best for:** private access to many AWS services (beyond S3/DynamoDB)

#### 2. Gateway Endpoints

![Gateway Endpoint](https://github.com/user-attachments/assets/4e6c33a5-6348-4e99-9059-2a76369ce3bb)

- No ENI → works by updating route tables
- Supports only S3 and DynamoDB
- No Security Groups needed
- Free → no hourly or data charges

👉 **Best for:** cheap, simple access to S3/DynamoDB from private subnets

#### Diagram Explanations

**📌 Diagram 1: Interface Endpoint (first image)**

- Inside a private subnet, you have an EC2 instance
- The EC2 connects to a VPC Endpoint (Interface)
- This endpoint creates an ENI (PrivateLink) with a private IP
- Through this ENI, the EC2 instance can securely connect to Amazon SNS without leaving the AWS network

👉 **Key point:** Requires SGs for traffic management, since it uses an ENI

**📌 Diagram 2: Gateway Endpoint (second image)**

- Again, inside a private subnet, you have an EC2 instance
- Instead of ENIs, the subnet's route table points traffic to the VPC Endpoint (Gateway)
- That gateway provides direct private access to S3 or DynamoDB
- No ENIs or security groups involved — just route configuration

👉 **Key point:** Free to use, but only supports S3 and DynamoDB

**In summary:**

- **Interface Endpoint** (PrivateLink + ENI) → flexible, supports many services, but costs money
- **Gateway Endpoint** (Route-based) → free, simple, but only for S3/DynamoDB

### IPv6

#### IPv6 Basics

- **Successor to IPv4** (IPv4 = ~4.3 billion addresses, running out)
- **IPv6** = 3.4 × 10³⁸ addresses → practically unlimited
- Designed for the future: supports massive growth of connected devices

#### IPv6 in AWS

- Every IPv6 address is **public + internet-routable**
- 👉 No "private" IPv6 ranges like in IPv4
- You must use security controls (SGs, NACLs, firewalls) to protect resources

#### IPv6 Format

Uses hexadecimal (0–9, A–F).

Sections separated by colons (:), not dots.

**Example:**
```
2001:0db8:0000:0000:0000:ff00:0042:8329
```

Can shorten zeros with `::`:
```
2001:db8::ff00:42:8329 (double colon = multiple zero blocks)
```

Only one `::` allowed per address.

#### Why IPv6?

- **Scalability** → billions of new IoT devices, cloud systems, mobile networks
- **Future-proof** → solves IPv4 exhaustion problem
- **AWS networking** → IPv6 preferred for massive scaling (global apps, multi-region)

**Summary:**
IPv6 solves the address shortage of IPv4, introduces a new hex-based format, and in AWS all IPv6 addresses are public, so securing them is critical. It's the standard moving forward for large-scale cloud networks.

### IPv6 in VPC

#### 1. IPv4 Always On

- You cannot disable IPv4 in a VPC or subnet
- Enabling IPv6 = dual stack mode → both IPv4 + IPv6 active together

#### 2. Dual Stack Mode

- Resources can use both IPv4 and IPv6 at the same time
- Provides compatibility (IPv4) + future-proofing (IPv6)

#### 3. EC2 Instances with IPv6

Each instance gets:
- **Private IPv4** → internal VPC communication
- **Public IPv6** → internet-routable

Internet Gateway supports both protocols, allowing outbound + inbound traffic.

#### 4. Why It Matters

- Smooth transition from IPv4 to IPv6
- Ensures your apps/services can communicate with both IPv4 and IPv6 clients
- Future-proofs your setup while avoiding breakage of existing IPv4 systems

**Summary:**
In AWS, IPv6 runs in dual stack mode with IPv4. EC2s always keep a private IPv4 for VPC traffic, plus a public IPv6 for global reach. This gives flexibility during migration and ensures compatibility with both current (IPv4) and future (IPv6) internet standards.

### Problems with IPv6

**Problem**

You launch an EC2 instance (e.g., Istio) in a subnet, but it fails.

At first, you might think: "Is it because there's no IPv6 space?"

❌ **Wrong** → The issue is not IPv6.

✅ **The real issue** = no available IPv4 addresses left in the subnet.

**Why This Happens**

- AWS VPCs always need IPv4 space, even when IPv6 is enabled
- IPv6 space is virtually unlimited, so you won't run out
- But IPv4 space is limited (CIDR ranges like /28, /24, etc.)
- If all IPv4s are used, you can't launch new instances — even if IPv6 is enabled

**Solution**

- Expand the IPv4 CIDR range of the subnet
- Add more IPv4 addresses so new instances can launch
- Keep in mind: AWS requires IPv4 to remain active → you can't go IPv6-only yet

**Summary:**
In AWS, running out of IPv4 addresses (not IPv6) is a common cause of subnet issues. Even with dual-stack IPv6, you still need IPv4 space. Fix = add/expand the IPv4 CIDR block in your subnet.

### Egress-only Internet Gateway

![Egress-only Internet Gateway](https://github.com/user-attachments/assets/4084a8d7-0eb0-4cca-98b7-0806c46223be)

**USED FOR IPv6 ONLY**

Similar to NAT Gateway but for IPv6

Allows instances in your VPC outbound connections over IPv6 while preventing the internet to initiate and IPv6 connection to your instances

You must update the route tables

#### Diagram Walkthrough

**Public Subnet (Left Box)**
- Instance has an IPv6 address (2001:db8::b1c2)
- Connected via a normal Internet Gateway
- With this setup → connections can be initiated both ways:
  - The instance can connect to the internet
  - The internet can also initiate connections back (inbound)

**Private Subnet (Right Box)**
- Instance also has an IPv6 address (2001:db8::e1c3)
- Uses an Egress-Only Internet Gateway
- With this setup → only outbound connections are allowed:
  - Instance can connect out to the internet
  - Internet cannot initiate inbound connections

**Key Point**

- Egress-Only IGW = NAT Gateway equivalent for IPv6
- Ensures outbound-only communication for private subnets
- Protects resources from unwanted inbound IPv6 traffic

**Summary (Diagram in Words)**

- **Left side** (Public Subnet + Internet Gateway) → two-way communication possible
- **Right side** (Private Subnet + Egress-Only IGW) → outbound-only communication, inbound is blocked

This makes Egress-Only IGWs perfect for securing IPv6-enabled private subnets.

### IPv6 Routing

![IPv6 Routing](https://github.com/user-attachments/assets/30667217-3f2d-4113-9e2e-14b7e27f10d5)

#### IPv6 Routing in Dual-Stack VPC

**VPC Setup**
- Dual-stack: supports both IPv4 and IPv6
- Each subnet has an IPv4 CIDR and an IPv6 CIDR
- Two subnets: Public and Private

**Public Subnet**
- Has IPv4 + IPv6 enabled
- EC2 instance gets:
  - Private IPv4
  - Elastic IP (public IPv4)
  - Public IPv6 address
- Can access the internet with both IPv4 and IPv6

**Private Subnet**
- No direct internet access
- Instances get a private IPv4 and IPv6
- Outbound traffic:
  - IPv4 → NAT Gateway (in public subnet)
  - IPv6 → Internet Gateway (direct)

**NAT Gateway**
- Needed only for IPv4 outbound from private subnet
- Not required for IPv6 – instances use Internet Gateway directly

**Route Tables**

**Public subnet route table:**
- Local routes for internal VPC CIDRs (IPv4 + IPv6)
- All other IPv4 + IPv6 → Internet Gateway

**Private subnet route table:**
- IPv4 → NAT Gateway
- IPv6 → Internet Gateway

**Key Difference (IPv4 vs IPv6)**
- IPv4 private instances need NAT Gateway for internet
- IPv6 private instances use Internet Gateway directly (no NAT needed)

### IPv6 Architecture

![IPv6 Architecture](https://github.com/user-attachments/assets/ccc426da-f770-4702-a1d8-d63ee0ceae12)

#### VPC (with IPv4 + IPv6 ranges)

- The container for your AWS network
- Has IPv4 CIDR (e.g., 10.0.0.0/16) and IPv6 CIDR (e.g., 2001:db8:1234:1a00::/56)
- Subnets inside it inherit both ranges

#### 2. Public Subnet

EC2 instances here have:
- Private IPv4 (for local VPC communication)
- Elastic IP (EIP) to map their IPv4 to the internet
- IPv6 address that is always publicly routable

They can connect to the internet directly over both IPv4 + IPv6.

#### 3. Private Subnet

EC2 instances here have:
- Private IPv4 only (not directly routable)
- IPv6 address but secured with outbound-only routing

They cannot directly talk to the internet — they need gateways.

#### 4. NAT Gateway (for IPv4)

- Lives in the public subnet with an Elastic IP
- Lets private instances send traffic to the internet (e.g., OS updates, APIs)
- Translates private IPv4 → public IPv4
- Blocks inbound connections (internet can't reach private instances)

#### 5. Internet Gateway (IGW)

- Attached to the VPC
- Provides two-way communication between VPC resources and the internet
- Used by:
  - Public IPv4 traffic (via Elastic IPs)
  - All IPv6 traffic (since IPv6 addresses are public by default)

#### 6. Egress-Only Internet Gateway (EIGW)

- Special gateway for IPv6 traffic from private subnets
- Works like a NAT gateway but for IPv6
- Allows outbound-only connections (EC2 → internet)
- Blocks inbound connections (internet → EC2)

#### 7. Route Tables

**Public Subnet RT:** sends all 0.0.0.0/0 (IPv4) and ::/0 (IPv6) to the Internet Gateway.

**Private Subnet RT:**
- IPv4 → NAT Gateway
- IPv6 → Egress-Only Internet Gateway

**Summary**

- **NAT Gateway** → IPv4 outbound only for private subnets
- **IGW** → Direct internet access (both IPv4 + IPv6)
- **EIGW** → IPv6 outbound only for private subnets
- **Public Subnet** → Direct internet access
- **Private Subnet** → Secured, needs NAT (IPv4) or EIGW (IPv6) to talk out

### VPC Summary

#### Core VPC Concepts Recap

**CIDR (Classless Inter-Domain Routing)**
- Defines IP address ranges (IPv4 & IPv6)
- Sets the boundaries for IP allocation in your VPC

**VPC (Virtual Private Cloud)**
- Your isolated, private slice of AWS network
- You define IP ranges (CIDRs) for resources

**Subnets**
- Divide a VPC into smaller networks
- Each subnet tied to an Availability Zone (AZ)
- Can assign unique CIDRs per subnet

**Internet Gateway (IGW)**
- Provides VPC internet access (IPv4 & IPv6)
- Attached at the VPC level

**Route Tables**
- Control traffic flow
- Routes traffic to IGWs, NAT gateways, VPC peering, or endpoints

**Bastion Host**
- Public EC2 used as a secure jump server to access private EC2 instances

**NAT Instance**
- Legacy way to give private EC2s internet access (IPv4)
- Requires manual setup (e.g., disable source/dest check)

**NAT Gateway**
- AWS-managed NAT service (scalable, HA)
- Lets private instances reach the internet via IPv4

**NACLs (Network ACLs)**
- Stateless firewalls at the subnet level
- Inbound & outbound rules evaluated separately

**Security Groups (SGs)**
- Stateful firewalls at the instance level
- Allow rules automatically allow return traffic

**VPC Peering**
- Connects two VPCs via AWS internal network
- Non-transitive (A↔B and B↔C ≠ A↔C)

**Transit Gateway**
- Hub-and-spoke model
- Provides transitive connectivity across VPCs, VPNs, Direct Connect

**VPC Endpoints**
- Private access to AWS services (e.g., S3, DynamoDB) without using the internet

**PrivateLink (VPC Endpoint Services)**
- Connects consumer VPC to provider VPC privately
- Requires NLB or ENIs
- No need for peering, NAT, or internet

**Egress-Only Internet Gateway**
- For IPv6 outbound only
- Blocks inbound internet traffic (like NAT Gateway but IPv6)

**In short:**

- **CIDRs & Subnets** define the network
- **IGWs, NAT, Egress-only** control internet connectivity
- **Route Tables** direct traffic
- **SGs & NACLs** secure traffic
- **Peering, TGWs, Endpoints, PrivateLink** connect networks & services


















































































