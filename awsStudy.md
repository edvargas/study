- AZ - > Availability Zones


# IAM: User and groups
- IAM -> Identity and Access Management, global service
- Root account -> crated by default, shouldn't userd or shared
- Users/groups can be assinged to json documentos(policy), to refeer whith user/group is allowed to do on the AWS account
- Policies define permissions, don't give more permission that the user needs
- IAM is a global service, users and groups are created in a global fashion

https://edvargas-aws.signin.aws.amazon.com/console

- IAM policies structure:
    - Version - policiy language version, awways inclde "2012-10-17"
    - Id - identifier optional
    - Statment - one or more (required)
        - Sid: an identifier for the statment (optional)
        - Effect: whether the statement allows or denies access(Allow,Deny)
        - Principal: account/user/role to whitch this policy applied to
        - Action: list of actions
        - Resource: list of resources
        - Condition: condition for when this policy is in effect

- Password policy -> in AWS we can set a password policy, increasing the security for the account. We can setupt a password policy
    - Minimum password length
    - Require specific character types
    - Allow all IAM user to change their own passwords
    - Password expiration
    - Prevent password re-use

- Multi Factor Authentication - MFA
    - combination password + secutiry device you own
    - Virtual MFA device: an APP in the smartphone like Google authenticatior
    - Universal 2nd Factor(U2F)/Hardware Key fob MFA: it's an phisycal key make by a 3rd party company, support for multiple root and IAM users using a single key

- IAM Roles for Services
    - Some AWS services will need to perform some actions on your behald, for Eg.: EC2 instance escalating memory
    - To do so, we will assing permissions to AWS services with IAM Roles
    - Commons roles:
        - EC2 Instance roles
        - Lambda Function roles
        - Roles for CloudFormation

- IAM Security Tools
    - IAM Credentials Report (account-level): a report that lists all your account's users and the status of their various credentials
    - IAM Access Advisor (user-level): Acccess advisor shows the service permissions granted to a user and when those services were last accessed, you can use this info to revise your policies

- IAM Guideline and Best Practices
    - Don't use the root account except for AWS account setup
    - One physical user = One AWS user
    - Assign users to groups and assign permissions to groups
    - Create a strong password policy
    - Use and enforce the use of MFA
    - Create and user Roles for giving permissions to AWS services
    - Use Access Key for Programmatic Access (CLI/SDK)
    - Audit permissions of your account with the IAM Credentials Report
    - Never share IAM users and Access Keys


# AWS CLI - Command line interface and AWS Shell
- AWS CLI/Shel are tools that enables to interact with AWS services using commands in your command-line shell
- On AWS CLI case, we need to sing in on terminal, using the user's access key and secret key
- On the AWS Shell case, it's available on the AWS console for specific regions


# AWS EC2 - Elastic Compute Cloud
- EC2: Elastic Compute Cloud -> Infra as a Service
    - Renting virtual machines(EC2)
    - Storing data on virtual drives(EBS)
    - Distributing load across machines(ELB)
    - Scaling the services using an auto-scaling group(ASG)

- Sizing and configuration options
    - SO: Linux, Windows or Mac OS
    - How much CPU
    - How much RAM
    - How much storage
        - Network-attached(EBS and EFS)
        - hardware (EC2 Instance store)
    - Network card: speed of the card, public IP address
    - Firewall rules
    - Bootstrap script(configura at first launch): EC2 User data
        - Bootstrapping means launching commands when a machine starts
        - Thats script is only run once at the instance firts start
        - EC2 user data is used to automate boot tasks such as:
            - Installing updates
            - Installing software
            - Downloading commons files from the internet
            - Anything you can think of
        - The EC2 User Data SCript runds with the root user
- The Instance's public IP may change, but the private IP is aways be the same

- EC2 Instance Types:
    - We can use different ypes of EC2 instances that are optimised for different use cases
    - AWS has the following naming convention, for E.g.: m5.2xlarge:
        - m: instance class
        - 5: generation (AWS imrpvoes them over time)
        - 2xlarge: size within the instance class
    - List of EC2 instance types:
        - General Purpose: great for a diversity of  workloads such as web servers or code repositories, balance between:
            - Compute
            - Memory
            - Networking
        - Compute Optimized: great for compute-intensive tasks that require high performance processors:
            - Batch processing workloads
            - Media transcoding
            - High performance web servers
            - High performance computing (HPC)
            - Scientific modeling and machine learning
            - Dedicated gaming servers
        - Memory Optimized: fats performance for workloads that process large data sets in memory, use cases:
            - High performance, relational/non-relational databases
            - Distributed web scale cache stores(elasticache for example)
            - In-memory databases optmized for BI(business intalligence)
            - Applications performing real-time processing of big unstructured date
        - Storage Optimized: great for storage-intesive tasks tha require high, sequential read and write access to large data sets on local storage, use cases:
            - High frequency online transaction rocessing (OLTP) systems
            - Relational and NoSQL databases
            - Cache for in-memory databases (for example, Redis)
            - Data warehousing applications
            - Distributed file systems

- Security Groups: are the fundamentals of network security in AWS
    - They control how traffic is allowed into or out of our EC2 Instances
    - Security groups only contain allow rules
    - Security groups rules can reference by IP or by security group
    - Security groups are acting as a "firewall" on EC2 instances, they regulate:
        - Access to Ports
        - Authorised IP ranges - IPv4 or IPv6
        - Cotrol of inbound network (from other to the instance)
        - Control of outbound network (from the instance to other)
    - Security group can be attached to multiple instances
    - Locked down to a region /VPC combination
    - Does live "outside" the EC2 - if traffic is blocked the EC2 instance won't see it
    - It's good to maintain one separate security group for SSH access
    - If your application is not accessible (time out), then it's a security group issue
    - If your application gives a "connection refused" error, then it's an application error or it's not launched
    - All inbound traffic is blocked by default
    - All outbound traffic is authorised by default
    - Classic Ports to know:
        - 22 = SSH (Secure Shell) - log into a Linux instance
        - 21 = FTP (File Transfer Protocol) - upload files into a file share
        - 22 = SFTP (Secure File Transfer Protocol) - upload files using SSH
        - 80 = HTTP - access unsecured websites
        - 443 = HTTPS - access secured websites
        - 3389 = RDP (Remote Desktop Protocol) - log into a Windows instance

- EC2 SSH connection - that is the command to access EC2 from SSH:
    - ssh -i EC2Turorial.pem ec2-user@44.204.66.19
    - if we're using key pair on the EC2 instance, it's necessary to user "-i [keyPair]", otherwise, just ignore it
    - and after that, ec2-user@[instanceIP]

- EC2 Instance Roles - instead use credentials when accessing EC2 instance, we should use IAM Roles on the instance to have access to the AWS functionalities

- EC2 Instances Purchasing Options:
    - On-Demand Instances - short workload, predictable pricing, pay by second
        - Pay for what you use
        - Has the highets cost but no upfront payment
        - No long-term commitment
        - Recommended for short-term and un-interrupted workloads, where you can't predict how the application will behave
    - Reserved Instances (1 and 3 years) - Up to 72% discount comparated to On-demand
        - Reserved Instances - long workloads
        - We reserve a specific instance attributes (Instance Type, Region, Tenancy, OS)
        - Payment Options - No upfront, Partitial upfront, All upfront
        - Reserved Instance's Scope - Regional or Zonal (reserver capactity in an AZ)
        - Recommended for steady-stated usage applications (think database)
        - We can buy and sell in the Reserved Instance Marketplace

        - Convertible reserved instances - long workloads with flexible instances
            - Can change the EC2 instance type, instance family, OS, scope and tenancy
            - Up to 66% discount
    - Saving Plans (1 and 3 years) - commitment to an amount of usage, long workload
        - Get a discount based on long-term usage (up to 72%)
        - Commit to a certaing type of usage ($10/hour for 1 or 3 years)
        - Usage beyond EC2 Savings Planas is billed at the On-Demand price

        - Locked to a specific instance family and AWS region (e.g, M5 in us-east-1)
        - Flexible across:
            - Instance Size (e.g., m5.xlarge to m5.2xlarge)
            - OS (e.g., Linux to Windows)
            - Tenancy (Host, Dedicated, Default)
    - Spot Instances - short workloads, cheap, can lose instances (less reliable)
        - Can get a discount of up to 90% comparated to On-demand
        - Instances that you can "lose" at any point of time if your ma price is less than the current spot price
        - The MOST cost-efficient instances in AWS

        - Useful for workloads that are resilient to failure
            - Batch jobs
            - Data analysis
            - Image processing
            - Any distributed workloads
            - Workloads with a flexible start and end time
        - Not Suitable for critical jobs or databases
    - Dedicated Hosts - book an entire physical server, control instance placement
        - A physical server with EC2 instance capacity fully dedicated to your use
        - Allows you address compliance requirements and use your existing server-bound software licences (per-socket, per-core, pe-VM software licenses)
        - Purchasing Options:
            - On-demand - pay per second
            - Reserved - 1 or 3 years (No Upfront, Partial Upfront, All Upfront)
        - The most expensive option

        - Useful for software that have complicated licensing model (BYOL - Bring Your Own License)
        - Or for companies that have strong regulatory or compliance needs
        
        - I have my own Instance on my own Hardware
    - Dedicated Instances - no other customer will share your hardware
        - Instances run on hardware that's dedicated to you
        - May share hardware with other instances in same account
        - No control over instance placement (can move hardware after Stop / Start)

        - I physical server itself, that's gives lower level visibility
    - Capacity Reservations - reserve capacity in a specific AZ for any duration
        - Reserve On-Demand instances capacity in a specific AZ for any duration
        - We always have access to EC2 capacity when you need it
        - No time commitment (create/cancel anytime), no billing discounts
        - Combine with Regional Reserved Instances and Saving Plans to benefit from billing discounts
        - We're charged at On-Demand rate wheter you run instances or not

        - Suitable for short-tem, uninterrupted workloads that needs to be in a specific AZ





Doubts:
BYOL - Bring Your Own License?
