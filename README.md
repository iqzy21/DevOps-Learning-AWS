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

##  Storage 
Whats an EBS volume 
EBS is a network drove tht can be attached to EC2 instances 
Its like a virtual storage 
It allows your instance to persist data even if its off e.g saves data for your instances 
bound to AZs 
think of it like a networkUSBstick
why is it used
data persistance - data stays in there Even if an instance is terminated there will still be data saved in the EBS 
ideal for storing long term data like data bases 

AMI overview 
Amazon machine image 
AMI are a custyomisation of EC2 instances 
you can add your own software,config,OS and monitoring
good if you want a faster boot  config time because your software is pre packaged 

AMIs are built for specific reigons but can be copied across 

You can launch ec2 instances from 
A public AMI: AWS provided
Your own AMI: make and maintain you self
AWS market place: AMI that some one else made and sells 

Amazon EFS - elastic file system 
amazon managed solution for shared file storage 
Its a managed netwoirk file system that can be mounted onto multiple EC2 instances to share the same files
like a network drove that all instances can access 
EFS works with EC2 instances that are in in multi AZs 
Its highly avalible
scalable - grows as the data grows 
how ever it is expensive and should be used when only needed

#Load balancing & scalability 
Scalability & High Availability
scalability means that an application / system can handle greater laods by adapting its size
2 types
vertical - when more power is added e.g upgrading a computer such as adding more ram, cpu power ect while the machine staays the same
horizontal (elasticity) - add more instances of resource to handle the workload rather than making the server bigger you add more servers to balance the workload

scalability is about handling greaterloads
high avalibility ensuring the system is up and running even parts of it fail 

call center example:

Vertical scalability: One person with better tools to handle more calls.

Horizontal scalability: Hire more people to share the workload.

High availability (HA): Have backups/standby staff so operations continue if someone is unavailable.


virticle scalability
means increasing the size of an instance adding storage ram and cpu power
example 
if you have a t2.micro and need to upgrade you can scale and change to t2.large
virtical scalability is conmmon for non distributed systems such as databases 
DBS like RDS and elasticache are services thhat can scale vertically.
there is usually a hardware limit to how much you can scale 

horizontal scalability 

horizontal scalability means to increasse the number of instances/ systems for you application to handle more load
add more michances / instances based on work load 
each machine does part of the work allowing you to handle more data
easier to manage if one instance failsm you have back up
e.g if you haver website traffic you can start more instances to handle then stop once trafic lesens 
easy to impliment and can be automated than ks to amazon services and ec2 

high avalibility 
means having multiple locations for you application / system to ensure if one part of infra failes anotehr part can take over 
high avalibility and horizontal scaling go hand in hand because you need to have multiple instances across locations 
achieve this by running applications across 2 or more AZs 
Azs arer like independant dsata centres 
The goal of HA is to survive a data centre loss so if a data centre goes down then you have a back up to keep yopur app running 
<img width="681" height="340" alt="image" src="https://github.com/user-attachments/assets/f38496e3-1a71-41e6-a901-2b8616bc6dbe" />

Load balancing 
What is load balancing ?
loadbalancing is a way rtop distribute traffic across servers or ec2 instances 
acts like a traff=ic cop distrobuting requests to dofferent sewrvers 
when trafic comes in the load balancer checks between the ec2 instances doiwnstream to see which is up and healthy to take on more traficc
this process keeps you application avalible and working just incase one instance goes down  
load balancer will automaticly dovert traffic to healthy ec2 instances

reverse proxies
what are reverse procies ?
Definition:

A reverse proxy sits between users and your servers.

It handles requests on behalf of your servers.

Similar to a Load Balancer:

Both sit in the middle of client ↔ server communication.

Both can distribute traffic to servers.

Extra Functionality (compared to Load Balancer):

Can route traffic based on request content (e.g., URL path, headers).

Can send requests to different services or microservices.

Provides features like caching, SSL termination, authentication, compression (depending on the setup).

AWS Example:

Application Load Balancer (ALB) acts like a reverse proxy.

It supports content-based routing (e.g., /api → service A, /images → service B).

Useful in microservice architectures.

👉 Think of it like this:

Load Balancer = Just balances traffic.

Reverse Proxy = Balances traffic + adds smart routing & extra features.
<img width="299" height="198" alt="image" src="https://github.com/user-attachments/assets/dbb5b343-65be-464c-aadb-4e64f735d3e0" />

why use laod balancer?
spread load across multiple downstram instances prevents instances being ioverloaded bettering user experience 
expose single point of access DNS to you application 
handles falures of downstream instances redirects trafic to other healthy instances 
rdoes regular health checks on your instance 
provide SSL termination HTTPS for websites load balamncer deals with the heavy encryption with HTTPS websites
enforces stickeness with cookies - makes sures that each user is still on the same requst using cookies incase they are sent back
high avalibility accross zones - designed to be avalible 
seperate public traffic from provate traffic routes traffic different ways to enure what is external or uinternak trfafic

why use an elastic load balancer 
this is a managed load balancer 
AWS garuntees that it will be working 
AWS takes care of upgrades, maintanance and high avalability
AWS provides only a few configuriation knobs 

why DIY vs manage ones?
it costs less to set up your own balanver but it will be more effort you need to handle maintanence monitoring scaling and fault tolerance on ypur own 
with ELB AWS manages all that complexity 
ey AWS Services ELB integrates with:

Amazon Certificate Manager (ACM):

Handles SSL/TLS certificates.

ELB can terminate SSL for you (removes complexity).

Amazon CloudWatch:

Monitor ELB performance and health.

Amazon Route 53:

DNS routing integrated with ELB.

AWS WAF (Web Application Firewall):

Protects apps from attacks (SQL injection, XSS, etc.).

AWS Global Accelerator:

Improves performance and reliability for global traffic.

Why Use ELB?

Reliable & Low-Maintenance → AWS manages availability.

Time-Saving → No need to build/maintain your own load balancer.

Scalable → Works with Auto Scaling seamlessly.

More Costly than DIY solutions, but saves effort and operational overhead.

Health checks
key part of load balancing
the allow the load balancer to know idf instances are good enough to handle traffic and it is done via requests
a request is sent to a HTTP protocol on any port then to the end point 
the heath check is done on a port and a route(/health is common)
is the response is not 200 being OK then the intsance is unhealthy 
IF 200 IS RETURNED THEN ITS CONSIDERED HEALTHY

<img width="475" height="161" alt="image" src="https://github.com/user-attachments/assets/cd7b7aa4-646d-4820-bc94-5646b67f6291" />

types of load balancers on aws 
there are 4 kinds
clasic load balancer (v1 generation) 2009 CLB
handles the basics HTTP,HTTPS,TCP,SSL 
gets the job done 
lacks new generation features

Application load balancver (v2 new gen) 2016 - ALB
HHTP,HHTPS, Websocket
ideal for modern apps 
works on the application layer
smarter able to diorect traffic based on rewquestions liek URLS headers clear paramaters and cookies
perfect fpr microservicexzs or containers that need routing ]#

network load balancer (v2 - new gen) - 2017 NLB
TCP TLS, UDP
LAYER 4 LOAD BALANNCER 
designed for high performace scenarios where low latency is needed 
ideal for handling millions of requests per second
used for low latency applications e.g for real time gaming or high frequency tadingh systems
Gatwway laod balancer - 2020 - GWLB
operates at layer 3 network layer - IP protocal 
helps deploy scale and manage third party mnetwrok applications like firewalls intrusion detection systems and traffic analysers in your vpc 

Which AWS Load Balancer Should I Choose?

Classic Load Balancer (CLB)

Still works but old generation.

Limited features.

AWS recommends using ALB, NLB, or Gateway LB instead.

Internal vs External

Internal LB: Routes traffic inside your VPC (e.g., microservices, private apps).

External LB: Handles traffic from the public internet (e.g., websites, APIs).

Recommended Load Balancers

Application Load Balancer (ALB)

Best for: Web applications.

Features:

Content-based routing (e.g., /api → service A, /images → service B).

Supports modern protocols (HTTP/2, WebSockets).

Good for microservices & container-based apps.

Network Load Balancer (NLB)

Best for: High-performance, low-latency TCP/UDP traffic.

Features:

Handles millions of requests per second.

Very low latency.

Ideal for gaming servers, IoT, VoIP, or real-time apps.

Gateway Load Balancer (GWLB)

Best for: Advanced network applications.

Features:

Routes traffic to third-party appliances (e.g., firewalls, intrusion detection, deep packet inspection).

Useful in secure network setups.

👉 Quick Decision Guide:

Traditional web app? → ALB

Need ultra-fast TCP/UDP performance? → NLB

Advanced networking (firewalls, IDS, etc.)? → GWLB

Old system? → Only then use CLB (otherwise avoid).

load balancer security groups 
Load Balancer Security Group

Purpose: Expose the application to the internet safely.

Rules:

Allows HTTP (TCP, port 80) from 0.0.0.0/0 → anyone on the internet.

Allows HTTPS (TCP, port 443) from 0.0.0.0/0 → anyone on the internet.

Effect: Users can reach the load balancer using either HTTP or HTTPS.

Key point: The load balancer is the only public-facing component.

2. Application Security Group (EC2/Instances behind LB)

Purpose: Protect backend EC2/Istio instances.

Rules:

Allows HTTP (TCP, port 80), but only from the Load Balancer SG (not from the public internet).

In the example, the source is set to the load balancer’s SG (sg-054b5ff5ea02f2b6e).

Effect: The backend instances will not accept traffic directly from users. They only trust requests forwarded by the load balancer.

Why This Works (Analogy)

Think of it as:

Load Balancer = Front Door → Anyone can knock on it (open to the internet).

Application Servers = Private Rooms → Only the front door (LB) has the key; strangers can’t walk in directly.

This design ensures:
✅ Security – Backends aren’t exposed to direct attacks from the internet.
✅ Scalability – Load balancer can distribute traffic evenly to multiple instances.
✅ Flexibility – You can apply SSL termination, routing, or WAF (firewall rules) at the load balancer level.
<img width="770" height="388" alt="image" src="https://github.com/user-attachments/assets/82a22a67-d3e3-4627-8a97-caf3643d7bfd" />

application load balancer
Application Load Balancer (ALB) Notes + Examples

ALB operates at layer 7 HTTP layer
Example: it can inspect HTTP headers to decide routing, unlike a Classic Load Balancer which only works at TCP level.

Smart enough to understand what is being requested such as headers, cookies, URLs, strings and more
Example: a request with Cookie: user=premium could be routed to a premium server pool.

Load balance traffic to multiple HTTP applications across machines
Example: spreading traffic across 3 EC2 instances running the same web app so no single machine gets overloaded.

Load balance to multiple applications on the same machine
Example: one EC2 runs /app1 (Node.js) on port 3000 and /app2 (Flask) on port 4000 — ALB routes to the correct one.

Has support for HTTP/2 and WebSocket
Example: useful for chat apps needing WebSocket connections or faster loading with HTTP/2 multiplexing.

Supports redirects from HTTP to HTTPS as an example
Example: http://myshop.com automatically redirects to https://myshop.com.

Routing tables to different target groups
Example: /images/* → servers optimized for images, /api/* → backend servers.

Route based on path in URL
Example: /blog/* goes to blog servers, /shop/* goes to e-commerce servers.

Routing based on host name in URL
Example: api.myapp.com → API target group, admin.myapp.com → admin dashboard.

Routing based on query string, headers
Example: ?category=toys → toy catalog servers, X-Region: EU header → EU servers.

ALB are a great fit for microservices & container based applications e.g. Docker & Amazon ECS
Example: each ECS service (auth, payments, users) gets its own target group, ALB directs requests correctly.

Has a port mapping feature to redirect to dynamic port in ECS
Example: ECS assigns random container ports (32768, 32769, etc.), ALB maps incoming traffic automatically.

In comparison we need multiple classic load balancers per application
Example: 3 microservices (/auth, /cart, /profile) would need 3 CLBs, but only 1 ALB with routing rules.

how alb HANDLES HTTP BASED TRAFIC 
Users send HTTP requests → ALB sits in front of the application and receives the traffic.

ALB examines each request → decides where to route based on rules (path, host, headers, etc.).

Routes traffic to Target Groups:

A target group = a collection of resources (EC2, ECS tasks, or Lambda).

Each target group usually supports a specific service.

Example: one target group for user-related tasks (login, profile).

Another for product search or other functionality.

Health Checks:

ALB continuously runs health checks on targets.

If an instance fails health checks, ALB automatically stops sending traffic to it and routes requests to healthy ones.

Built for HTTP/HTTPS traffic:

Can handle many web-based services.

Smart enough to separate traffic by path, hostname, or query.

Why this matters:

You can route different requests to different parts of your application using one ALB.

Example:

https://example.com/profile → routes to User Service Target Group.

https://example.com/search → routes to Search Service Target Group.

Benefit:

Keeps services separate so one doesn’t interfere with the other.

Makes scaling and performance much easier (great for microservices)
<img width="547" height="307" alt="image" src="https://github.com/user-attachments/assets/189c5ef9-ca01-478e-9db1-de8316096a09" />

Application Load Balancer (Target Groups)
what are target groups
tagret groups are groups of resourves like ec2 lambda bunctionmns and esc tasks that your ALB routes traffic to 
they are distinaseion for requests that a load balancer handles 
each target group os toed to certain parts of an applicatioon allowing scalability of yopour system independatly 

different target groups
EEC2 instances ( can be managed by and auto scalling group ) - HTTP
ECS tasks (managed by ECS) - HTTP
lambda functions - HTTP request is translated into a JSON event
Ip addresses - must be private IPs
ALB can check route to multiple target groups
Health checks are at the target group level 

ALB good to know 
has a fixed host name provided by aws 
the application servers dont see the IP of the client directly 
The true IP of the client is inserted in the header X forwarded-for
We can also get Port (X-forwarded-port) and proto (X-forwarded-proto)
<img width="622" height="175" alt="image" src="https://github.com/user-attachments/assets/80cbebdc-cf9d-4b18-947a-483692779452" />

network load balancers
optoimised for handling extreme performance with low lantencyu 
works on the network layer layer 4
forward TCP & UDP traffic to your instances 
Handle millions of requests per seconds 
less latency - 100ms compared to ALB which has 400ms 
NLB has one static IP per AZ and supportas assigning alastic IP
NLB are used for extreme performance TCP or UDP traffic
#Not included in aws free tier 
used for stuff liek IOT gaming Real time processing 

network load balanced with TCP trafic 
works at layer 4 and routes raw tcp or udp traffic to instances without getting involved with higher level traffic likr HTTP and HTTPS

How Traffic is Routed

Incoming Request (WWW) → Enters NLB with TCP rules.

NLB forwards traffic to a Target Group:

Users Application → Uses TCP rules.

Search Application → Uses HTTP (inside TCP).

Key Points About NLB

Does not:

Inspect HTTP headers.

Do SSL termination (no HTTPS handling at Layer 7).

Only forwards traffic fast and efficiently without changing it.

When to Use NLB

✅ Best for raw performance and low latency.
✅ Can handle millions of requests per second.
✅ Ideal for:

Real-time apps (gaming, streaming, chat, etc.).

Applications sensitive to latency and connection speed.
✅ Good when different apps use different protocols (TCP vs HTTP).

NLB vs ALB

NLB (Layer 4): Speed, raw TCP/UDP forwarding.

ALB (Layer 7): Understands HTTP/HTTPS, can do SSL termination, read headers, smarter routing.

👉 Summary:
The NLB is like a super-fast traffic cop at the network layer. It doesn’t “look inside” the traffic (like ALB would), it just forwards TCP/UDP connections to the right target group based on rules. Great for performance-heavy apps needing speed and scalability.
<img width="767" height="322" alt="image" src="https://github.com/user-attachments/assets/b7214f2f-4350-4f81-a65a-00bcdfe08f03" />

sticky sessions (session afinity) 
sticky sessions ensure the client or user is router to the same instance behind the load balancer

works accross different load balancers 
the load balancer uses a cookie to keep track of which instance a client is connected to
each cookie binds a user to a certain server 
you can controoll their time and experation 
a use case when an application store session data that is not share across instances 
makes sure session data is not lost
for exampl a cart cookie on an e commerce site since this is a seperate instance a cookie will make sure the usedr doesnt loose their cart data 
may brinhgg imbalance ti the load over the back end of instances 
use if needed 

SSL/TLS basics 
crutial when dewaling with internet securty with load balancers 
SSL cert alloows traffic between your clients/users and load balanncer to be encrypted in transit(in-flight-encryption)

SSL refers to secure sockets layer, used to encrypt connections 
TLS refers to transport Layer security which is a newer version 
Nowdays TLS certificates are mainly used but people still refer to it as TLS

public SSL certs are issued by Certificate autheriries 
such as Comodo, symantec,Godaddy, global sign ect 

SSL certifications have experation dates you sey amd must be renewwed 

load balancer - SSL certificates 
load balancer uses a X509 cert SSL certificate 
AWS allows us to mange this using their certificate manager ACM 
ACM is a place where you can create renew and manage certicicates 
you can upload youw own certificates 
HTTPS Listener:
this is how we wensure traffic is encrypted 
you must specify defaultr certificate 
you can add an optional list of certs to support multiple domains
clients can use SNI server name indicatiob to specify the host name they reach 
can specify sdecurity polocies to support oldedr versions of SSL clients 

SSL -  SNI server name indication 
SNI solves the problem of loading multiople SSL certificates onto one web server (to server multiple websites)
Its a newer protocal and requires the client to indicate the host name of the target server int he initial SSL gghandshake 
The server will then find the crrect certidicate or return the default one
only works for ALB and NLB new gen, cloufront
doesnt not weork for CLB old gen 

ELB - SSL how do they work on differnt load balancers
CLB - classic load balanver v2
supports only one SSL cert
must use multiple CLB for multple host name with multiple SSL certificates 

ALB Application load balancer v2 
Supports multiple listner with multiple SSL certificates 
Uses server name indication SNI to make it work 

Newtork load balancer V2
supports multiple listeners with multiple ssl certs
uses server name indication sni to make it work 
