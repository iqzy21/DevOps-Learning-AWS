# DevOps-Learning-AWS
I learn AWS

##IAM (Identity and access managment)
users and groups 
Root Account

Created by default when the AWS account is set up.

Shouldn’t be used for everyday tasks.

Keep credentials secure; never share.

Users

Represent people or services in your organization.

Can be grouped for easier permission management.

Each has their own login & permissions.

Groups

Contain only users (no nested groups).

Assign permissions to the group → applies to all users in it.

A user:

Doesn’t have to belong to a group.

Can belong to multiple groups.

Example (From Diagram)

Developers Group: Michael, Jennifer, Joey

Audit Team Group: Zid

Operations Group: Zid, Ferns

No Group: Kasoon
<img width="497" height="259" alt="image" src="https://github.com/user-attachments/assets/85ba2466-e21d-4a8d-b647-31fc20b6d5eb" />

IAM: Permissions
IAM: Permissions
1. What Are Permissions?

Users or Groups can be assigned JSON documents called policies.

Policies = rulebooks that define what actions are allowed or denied.

Actions apply to specific AWS services/resources.

2. Example from Diagram

Allowed actions:

Describe EC2 instances.

Describe Elastic Load Balancing.

List/Get/Describe CloudWatch metrics.

3. Principle of Least Privilege

Only give the permissions needed to perform required tasks.

Don’t allow extra actions “just in case.”

Analogy: Give someone the key to the backseat, not the car ignition.

4. Why It Matters

Keeps AWS environment secure.

Prevents mistakes or misuse.

Scales well as your organization grows.
<img width="676" height="381" alt="Screenshot 2025-08-15 155212" src="https://github.com/user-attachments/assets/3bcbbd64-9d4b-4b7c-9f5c-1e578e4a28ca" />

IAM Policies – Inheritance
1. Group Permissions

Users inherit all permissions assigned to the group(s) they belong to.

Example:

Developers group → Alice, Bob, Charles

Permissions: e.g., EC2 access, S3 access.

Everyone in that group gets the same permissions.

2. Multiple Groups

A user can belong to more than one group.

They inherit permissions from all groups they are in.

Example:

Charles → Developers + DevOps Team → gets combined permissions.

David → Operations + DevOps Team → gets combined permissions.

3. Inline Policies

Attached directly to a specific user.

Not shared with any group.

Example:

Fred → in Operations group + has an inline policy for extra unique permissions.

4. Key Takeaways

Multiple group memberships = combined permissions.

Inline policies = user-specific extra access.

Best practice:

Use groups to manage most permissions.

Use inline policies only for special cases.
<img width="688" height="357" alt="image" src="https://github.com/user-attachments/assets/d8f3ad61-aa18-4aa4-bb15-cdda2bcc667b" />
IAM: Policies Structure
1. Main Components

Version (required) – Policy language version.

Always use: "2012-10-17".

Id (optional) – Identifier for the policy (e.g., name or reference).

Statement (required) – One or more blocks defining the actual permissions.

2. Statement Components

Sid (optional) – Label or identifier for the statement.

Effect (required) – "Allow" or "Deny".

Principal – Who the policy applies to (user, role, or account).

Action – List of actions the policy allows or denies.

Resource – List of resources the actions apply to.

Condition (optional) – Rules for when the policy is in effect (e.g., specific IP).

3. Example From Diagram

Version: "2012-10-17".

Id: "S3-Account-Permissions".

Principal: AWS root account (arn:aws:iam::1234:root).

Action: s3:GetObject, s3:PutObject.

Resource: arn:aws:s3:::mybucket/*.

4. Key Takeaways

A policy defines:
Who → Can do what → On which resources → Under what conditions.

AWS provides pre-built policies, but understanding the structure helps with custom setups.
<img width="802" height="451" alt="image" src="https://github.com/user-attachments/assets/f55023bd-e066-4aa0-8bdd-9bdde428151f" />

Password Policy
you can enforce a minimum passowrd length atleast 8-12 char
you can have specific requirments caps, no caps, numbers and special characters
Can allow IAM users to change their password
can require users to change password after some time
can require users to not reuse sam passoewrd
<img width="508" height="281" alt="image" src="https://github.com/user-attachments/assets/0d587820-8b8d-43e5-b518-cb128129a3f4" />

MFA- Multi factoer authentication
This is important because your AWS account contains important recourses 
MFA benefit - if account password is compromised accoutn cant be accesed unless it has an external login
this can be doine by a code, third party app or verification email
<img width="778" height="418" alt="image" src="https://github.com/user-attachments/assets/6f1bca3a-0d93-4d37-b17f-9da33de9e69b" />

MFA options in AWS
dVirtual MFA device - virtual authenticatore app e.g google 
they send a obne time code to access on the app on your smart phone

Universal 2nd factor security key - e.g yubikey
it is a USB device you plug inthe USB port that authenticates access without a code 1 key can access multiple acocunts

hardware key fob MFA  Device 
generates a code every 30 seconds  enter the code during log in and is abit old skool good forplaces where electrical devices are not allowed 

Hardware key fob MFA device for AWS gov cloud 
specificly for users in AWS govcloud in the US 
<img width="684" height="382" alt="image" src="https://github.com/user-attachments/assets/1d23c6ec-9b6a-476e-8435-fe1a4ef8df65" />

How users can access AWS:
3 ways
AWS managmen console protected by passowrd + MFA
AWS CLI protected by keys
AWS SDK softweare development kit for code:protected by acces keys

can generate keys formt he console and each user manages their key 
acces keys should not be shared 
access key id = username
secret acces key = passord
<img width="680" height="378" alt="image" src="https://github.com/user-attachments/assets/52f6367c-8fe1-4031-9313-2bf9f692221e" />

example of access key 
<img width="680" height="378" alt="image" src="https://github.com/user-attachments/assets/02872051-240f-474a-9e7a-a93b15a3930f" />

What is the aws cli:
The AWS cli lets you access to the API within your AWS management console how ever in a cli format it lets you run commands and it allows you to write scripts for resource managment. used in click ops to automate jobs 
<img width="796" height="408" alt="image" src="https://github.com/user-attachments/assets/1ce66a20-9a56-4098-b1ec-bab5883ee678" />


what is the AWS SDK?
AWS SDK – Key Points

What it is: AWS Software Development Kit – a set of pre-made code libraries for different programming languages.

Purpose: Lets you interact with AWS services from your code (no need to use the AWS Console).

Languages supported:

Python, JavaScript, PHP, .NET, Ruby, Java, Go, Node.js, C++.

Mobile SDKs: Android, iOS.

IoT SDKs: Arduino, Embedded C, etc.

How it works:

Embed AWS actions (e.g., upload to S3, create EC2) directly in your app’s code.

Write just a few lines instead of doing tasks manually in the console.

Example: The AWS CLI is built on top of the AWS SDK for Python (Boto3).

<img width="780" height="418" alt="Screenshot 2025-08-15 164952" src="https://github.com/user-attachments/assets/ce1aeda5-0271-43f3-8e61-70132d888166" />

IAM Roles for services
IAM Roles for Services
What are IAM Roles?

Allow AWS services to act on your behalf without needing long-term credentials.

Provide temporary access instead of hardcoding access keys/passwords.

Improve security and flexibility.

Why use them?

Services (like EC2, Lambda, CloudFormation) often need to interact with other AWS services.

Roles let them do this securely by attaching the right permissions.

Common Use Cases

EC2 Instance Roles

Attach a role when launching an instance.

Example: EC2 reads/writes to S3 or DynamoDB.

Lambda Function Roles

Attach a role to a Lambda function.

Example: Lambda writes logs to CloudWatch or accesses S3.

CloudFormation Roles

Allows CloudFormation to create/manage resources across multiple services.

Keeps credentials secure while automating resource creation.

Key Point

Think of IAM roles as temporary keys for services.

No hardcoded credentials.

Easy to control what permissions each service has.

IAM security tools
IAM Credentials report(account level)
a report that lists all your accounts users and status of their varius credentials

IA, access Advisor
shows the service permssions grated to users and when services were last acssesed 
you can use this info to revise your policies 

IAM guidlines and best poratices  
do not use root unless its for initial account set up use IAM user instead with correct
setup

one physical user = one aws user 
do not share logins and make sure everyone has an account
assign users to groups and assign permissions
crreate strong password policy
enforce MFA to keep accoutn secure
create and use roles for gving services permissions instead of hard coding
use access keys for progammatic access (CLI/SDK)
audit permissions of your account using IAM credentials report and IAM access advisor
never share IAM users or access keys 

summary: 
<img width="677" height="310" alt="image" src="https://github.com/user-attachments/assets/87fe4c73-b8f1-489d-ace9-3f97e8216755" />


