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
