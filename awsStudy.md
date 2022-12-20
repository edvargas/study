# Amazon AWS Study Solution Architecture Certification

## IAM: User and groups
- IAM -> Identity and Access Management, global service
- Root account -> crated by default, shouldn't used or shared
- Users/groups can be assinged to json documents(policy), to refeer whitch user/group is allowed to do on the AWS account
- Policies define permissions, don't give more permission that the user needs
- IAM is a global service, users and groups are created in a global fashion

- URL IAM example https://edvargas-aws.signin.aws.amazon.com/console

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
    - Some AWS services will need to perform some actions on your behald, for e.g.: EC2 instance escalating memory
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


## AWS CLI - Command line interface and AWS Shell
- AWS CLI/Shel are tools that enables to interact with AWS services using commands in your command-line shell
- On AWS CLI case, we need to sing in on terminal, using the user's access key and secret key
- On the AWS Shell case, it's available on the AWS console for specific regions


## AWS EC2 - Elastic Compute Cloud
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
        - General Purpose: great for a diversity of workloads such as web servers or code repositories, balance between:
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
        - Control of inbound network (from other to the instance)
        - Control of outbound network (from the instance to other)
    - Security group can be attached to multiple instances
    - Locked down to a region / VPC combination
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
        - Instances that you can "lose" at any point of time if your max price is less than the current spot price
        - The MOST cost-efficient instances in AWS

        - Useful for workloads that are resilient to failure
            - Batch jobs
            - Data analysis
            - Image processing
            - Any distributed workloads
            - Workloads with a flexible start and end time
        - Not Suitable for critical jobs or databases

        - EC2 Spot Instance Requests:
            - Define max spot price and get the instance while current spot price < max
                - The hourly spot price varies based on offer and capacity
                - If the current spot price > your max price you can choose to stop or teminate your instance
            - Other strategy: Spot Block
                - "block" spot instance during a specified time frame (1 to 6 hours) without interruptions
                - In rare situations, the instance may be reclaimed
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

    - Which purchasing option is right for me? (Using a resort as example)
        - On demand: coming and staying in resort whenever we like, we pay the full price
        - Reserved: like planning ahead and if we plan to star for a long time, we may get a good discount
        - Saving Plans: pay a certain amount per hour for certain period and stay in any room type
        - Spot instances: the hotel allows people to bid for the empty rooms and the highest bidder keeps the rooms. You can get kicked out at any time
        - Dedicated Hosts: We book an entire building of the resort
        - Capacity Reservations: You book a room for a period with full price even you don't staty in it

    - Spot Fleets = set of Spot Instances + (optional) On-Demand Instances
        - The Spot Fleet will try to meet the target capacity with price constraints
            - Define possible launch pools: instance type (m5.large), OS, Availability Zone
            - Can have multiple launch pools, so that the fleet can choose
            - Spot Fleet stops lauching instances when reaching capacity or max cost
        - Strategies to allocate Spot Instances:
            - lowestPrice: from the pool with the lowest price (cost optimization, short workload)
            - diversified: distributed across all pools (great for availability, long workloads)
            - capacityOptimized: pool with the optimal capacity for the numbers of instances
        - Spot Fleets allow us to automatically request Spot Instances with the lowest price

- EC2 - Elastic IPs
    - When you stop and then start an EC2 instance, it can change its public IP
    - If you need to have a fixed public IP for your incesntace, you need an Elastic IP
    - An Elastic IP is a public IPv4 IP you own as long as you don't delete it
    - You can attach it to one instance at a time
    - With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account
    - You can only have 5 Elastic IP in your account (you can ask AWS to increase that)

    - Overall, try to avoid using Elastic IP
        - They often reflect poor architectural decisions
        - Instead, use a random public IP and register a DNS name to it
        - Or, use a Load Balancer and don't use a public IP

- Placement Groups
    - Sometimes you want control over the EC2 Instance placement strategy
    - That strategy can be defined using placement groups
    - When you create a placement group, you specify one of the following strategies for te group:
        - Cluster - clusters instances into a low-latency group in a single Availability Zone
            - Pros - Great network
            - Cons: If the rack fails, all instances fails at the same time
            
            - Use case:
                - Big Data job that needs to complete fast
                - Application that need extremely low latency and high network throughput
        - Spread - spreads instances across underlying hardware (max 7 instances per group per AZ) - critical applications
            - Pros:
                - Can span across Availability Zones
                - Reduced risk is simultaneous failure
                - EC2 Instances are on different physical hardware
            - Const:
                - Limited to 7 instances per AZ per placement group
            
            - Use case:
                - Application that need to maximize high availability
                - Critical Applications where each instance must be isolated from failure from each other
        - Partition - spreads instances across many different partitions (which rely on different sets of racks) within an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)
            - Up to 7 partitions per AZ
            - Can span across multiple AZs in the same region
            - Up to 100s of EC2 instances
            - The instances in a partition do not share racks with the instances in the other partitions
            - A partitions failure can affect many EC2 but won't affect other partitions
            - EC2 instances get access to partition information as metadata
            - Use cases: big data applications, HBase, Cassandra, Kafka

- Elastic Network Interfaces (ENI)
    - Logical component in a VPC that represents a virtual network card
    - The ENI can have the following attributes:
        - Primary private IPv4, one or more secondary PVv4
        - One Elastic IP (IPv4) per private IPv4
        - One Public IPv4
        - One or more security groups
        - A MAC address
    - We can create ENI independendtly and attach them on the fly (move them) on EC2 instances for failover
    - Bound to a specific availability zone (AZ)

- EC2 Hibernate
    - The in-memory (RAM) stated is preserved
    - The instance boot is much faster
    - Under the hood: the RAM state is written to a file in the root EBS volume
    - The root EBS volume must be encrypted

    - User cases:
        - Long-running processing
        - Saving the RAM state
        - Services that take time to initialize

- EBS Volume - Elastic Block Store
    - An EBS is a network drive you can attach to your instances while they run
    - It allows your instances to persist data, even after their termination
    - They can only be mounted to one instance at a time (at the CCP level)
    - They are bound to a specific availability zone

    - Analogy: Think of them as a "network USB stick"
    - Free tier: 30 GB of free EBS storage of type General Purpose (SSD) or Magnetic per month

    - It's a network drive (i.e. not a physical drive)
        - It uses the network to communicate the instance, which means there might be a bit of latency
        - It can detached from an EC2 instance and attached to another one quickly
    - It's locked to an AZ
        - An EBS Volume in us-east-1a cannot be attached to us-east-1b
        - To move a volume across, you first need to snapshot it
    - Have a provisioned capacity (size in GBs, and IOPS)
        - We get billed for all the provisioned capacity
        - We can increase the capacity of the drive over time

    - EBS - Delete on Terminations attribute
        - Controls the EBS behaviour when an EC2 instance terminates
            - By default, the root EBS volume is deleted (attribute enabled)
            - By default, any other attached EBS volume is not deleted (attribute disabled)
        - This can be controlled by the AWS console / AWS CLI
        - User case: preserve root volume when instance is terminated

- EBS Snapshots
    - Make a backup (snapshot) of your EBS volume at a point in time
    - Not necessary to detach volume to do snapshot, but recommended
    - Can copy snapshots (backups) across AZ or Region

    - EBS Snapthos Feature
        - EBS Snapshot Archive
            - Move a Snapshot to and "archive tiers" that is 75% cheaper
            - Takes within 24 to 72 hours for restoring the archive
        - Recycle Bin for EBS Snapshots
            - Setup rules to retain deleted snapshots so you can recover them after an accidental deletion
            - Specify retention (from 1 day to 1 year)
        - Fast Snapshot Restore (FSR)
            - Force full initialization of snapshot to have no latency on the first use ($$$)

- EBS Volume Types
    - EBS Volume come in 6 types:
        - gp2 / gp3 (SSD): General purpose SSD volume that balances price and performance for a wide variety of workloads
        - io1 / io2 (SSD): Highest-performance SSD volume for mission-critical, low-latency or high-thoughput workloads
        - st1 (HDD): Low cost HHD volume designed for frequently accessed, throughput-intensive workloads
        - sc1 (HDD): Lowest cost HDD volume designed for less frequently accessed workloads

    - EBS Volumes are characterized in Size | Throughpu IOPS
    - When in doubt always consult the AWS documentation
    - Only gp2/gp3 and io1/io2 can be used as boot volumes

    - EBS Volume Types Use Cases:
        - General Purpose:
            - Cost effective storage, low-latency
            - System boot volumes, Virtual desktops, Development and test environments
            - 1 GiB - 16 TiB
            - gp3:
                - Baseline of 3,000 IOPS and throughput of 126MiB/s
                - Can increase IOPS up to 16,000 and throughput up to 1000 MiB/s independently 
            - gp2:
                - Small gp2 volumes can burst IOPS to 3,000
                - Size of the volume IOPS are linked, max IOPS is 16,000
                - 3 IOPS per GB, means at 5,334 GB we are at the max IOPS
        
        - Provisioned IOPS (PIOPS) SSD:
            - Critical business applications with sustained IOPS performance
            - Or applications that need more than 16,000 IOPS
            - Great for databases workloads (sensitive to storage perf and consistency)
            - io1/io2 (4 GiB - 16 Tib):
                - Max PIOPS: 64,000 for Nitro EC2 instances and 32,000 for other
                - Can increase PIOPS independently from storage size
                - io2 have more durability and more IOPS per GiB (at the same price as io1)
            - io2 Block Express (4 GiB - 64 TiB):
                - Sub-millisecond latency
                - Max PIOPS: 256,000 with an IOPS: GiB ratio of 1,000:1
            - Supports EBS Multi-attach
        
        - Hard Disk Drives (HDD): 
            - Cannot be a boot volume
            - 125 Mib to 16 TiB
            - Throughtput Optimized HDD (st1)
                - Big Data, Data Warehouses, Log Processing
                - Max throughput 500 MiB/s - max IOPS 500
            - Cold HDD (sc1)
                - For data that is infrequently accessed
                - Scenarios where lowest cost is important
                - Max throughput 250 MiB/s - max IOPS 250

- EBS Multi-Attach - io1/io2 family
    - Attach the same EBS volume to multiple EC2 instances in the same AZ
    - Each instance has full read and write permissions to the high-performance volume
    - Use case:
        - Achive higher application availability in clustered Linux applications (ex: Teradata)
        - Applications must manage concurrent write operations
    - Up to 16 EC2 Instances at a time
    - Must use a file system that's cluster-aware (not XFS, EX4, etc...)

- EBS Encryption
    - When you create an encrypted EBS volume, you get the following:
        - Data at rest is encrypted inside the volume
        - All the data in flight moving between the instance and the volume is encrypted
        - All snapshots are encrypted
        - All volumes created from the snapshot
    - Encryption and decryption are handled transparently, EC2 and EBS behind the scenes
    - Encryption has a minimal impact on latency
    - EBS Encryption leverages key from KMS (AES-256)
    - Copying an unencrypted snapshot allows encryption
    - Snapshots of encrypted volumes are encrypted

- AMI - Amazon Machine Image
    - AMI are customization of an EC2 instance
        - You add your own software, configuration, operating system, monitoring...
        - Faster boot/configuration time because all your software is pre-packaged
    - AMI are built for a specific region (and can be copied across regions)
    - You can launch EC2 instances from:
        - A Public AMI: AWS provided
        - Your own AMI: you make and maintain them yourself
        - An AWS Marketplace AMI: an AMI someone else made (and potentially sells)
    
    - AMI Process (from an EC2 instance)
        - Start an EC2 instance and customize it
        - Stop the instance (for data integrity)
        - Build an AMI - this will also create EBS snapshots
        - Launch instances from other AMIs

- EC2 Instance Store
    - EBS volumes are network drives with good but "limited" performance
    - If you need a high-performance hardware disk, use EC2 Instance Store

    - Better I/O performance
    - EC2 Instance Store lose their storage if they-re stopped (ephemeral)
    - Good for buffer / cache / scratch data / temporary content
    - Risk of data loss if hardware fails
    - Backups and Replications are your responsability

- Instance Metadata
    - AWS EC2 Instance Metadata is powerful but one of the least known features to developers
    - It allows AWS EC2 instance to "learn about themselves" without using an IAM Role for that purpose
    - The URL is http://169.254.169.254/latest/meta-data
    - You can retrieve the IAM Role name from the metadata, but you CANNOT retrieve the IAM Policy
    Metadata = Info about the EC2 Instance
    - Userdata = launch script of the EC2 instance


## Amazon EFS - Elastic File System

- Managed NFS (network file system) that can be mounted on many EC2
- EFS works with EC2 instances in multi-AZ
- Highly available, scalable, expensive (3x gp2), pay per use

- Use cases: content management, web serving, data sharing, Wordpress
- Uses NFSv4.1 protocol
- Uses security group to control access to EFS
- Compatible with Linux based AMI (not Windows)
- Encryption at rest using KMS

- POSINX file system (Linux) that has a standard file API
- File system scales automatically, pay-per-use, no capacity planning, pay for each GB used

- EFS - Performance and Storage Classes
    - EFS Scale
        - 1000s of concurrent NFS clients, 10 GB+/s throughtput
        - Grow to Petabyte-scale network file system, automatically
    - Performance mode (set at EFS creation time)
        - General purpose (default): latency-sensitive use cases (web servers, CMS, etc...)
        - Max I/O - Highet latency, throughput, highly prallell (big data, media processing)
    - Throughput mode
        - Bursting (1 TB = 50MiB/s + burst of up to 100MiB/s)
        - Provisioned: set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage
    
    - Storage Tiers (lifecycle management feature - move file after N days)
        - Standard: for frequently accessed files
        - Infrequent access (EFS-IA) with a Lifecycle Policy
    - Availability and durability
         - Standard: Multi-AZ, great for prod
         - One Zone: One AZ, great for dev, backup enabled by default, compatible with IA (EFS One Zone-IA)
        - Over 90% in cost savings

- EFS vs EBS vs Instance Store
    - EFS is for network file system mounted in a multiple instances 
    - EBS is for a network volume that in mounted in one instance and it is locked to an AZ
    - Instance Store maximum amount of IO in EC2 instance, but it's something that you lose, ephemeral


## AWS Elastic Load Balancer - ELB
- Load Balances are servers that forward traffic to multiple servers (e.h., EC2 instances) downstream

- Health Checks
    - health Checks are crucial for Load Balancers
    - They enable the load balancer to know if instances it forwards traffic to are available to reply to requests
    - The health check is done on a port and a route(/heath is common)
    - If t he response is not 200 (OK), then the instance is unhealthy

- Types of load balancer on AWS
    - Classic Load Balancer (v1 - old generation) - 2009 - CLB
        - HTTP, HTTPS, TCP, SSL
    - Application Load Balancer (v2 - new generation) - 2016 - ALB
        - HTTP, HTTPS, WebSocket
    - Network Load Balancer ( v2 - new generation) - 2017 - NLB
        - TCP, TLS, UDP
    - Gateway Load Balancer - 202 - GWLB
        - Operates at layer 3 (Network layer) - IP Protocol
    
    - Overall, it is recommended to use the newer generation load balancers as they provide more features
    - Some load balancers can be setup as internal (private) or external (public) ELBs

- Load Balancer Security Groups
    - Load Balancer has the own Securty group
    - For example, we can allow the users from anywhere access the Load Balancer, but the EC2 Instances only allow traffic comming only from the Load Balancer, because de EC2 Security Group will allow HTTP traffic only from the Load Balancer Security Group, this way linking Security Groups, for e.g. in images/img1

- Application Load Balancer (v2) - ALB
    - Application load balancers is Layer 7 (HTTP)

    - Load Balancing to multiple HTTPS applications across machines (target groups)
    - Load balancing to multiple applications on the same machine (ex: containers)
    - Support for HTTP/2 and WebSocket
    - Support redirects (from HTTP to HTTPS for example)

    - Routing tables to different targe groups:
        - Routing based on path in URL (example.com/users & example.com/posts)
        - Routing based on hostname in URL (one.example.com & other.example.com)
        - Routing based on Query String, Headers (example.com/users?id=123&order=false)
    - ALB are a great fit for micro services & container-based application (example: Docker & Amazon ECS)
    - Has a port mapping feature to redirect to a dynamic port in ECS
    - Has a port mapping feature to redirect to a dynamic port in ECS
    
    - Target Groups
        - EC2 instances (can be managed by an Auto Scaling Groups) - GTTP
        - ECS tasks (managed by ECS itself) - HTTP
        - Lambda functions - HTTP request is translated into a JSON event
        - IP Addresses - must be private IPs

        - ALB can route to multiple target groups
        - Health checks are at the target group level
    
    - Good to Know
        - Fixed hostname (XXX.region.elb.amazonaws.com)
        - The application servers don't see the IP of the client directly
            - The true IP of the client is inserted in the header X-Forwarded-For
            - We can also get Pot (X-Forwarded-Port) and proto (X-Forwarded-Proto)

- Network Load Balancer (v2) - NLB
    - Network Load balancers (Layer 4) allow to:
        - Forward TCP & UDP traffic to your instances
        - Handle millions of request per second
        - Less latency ~100 ms ( vs 400 ms for ALB)

    - NLB has one static IP per AZ, and supports assigning Elastic IP (helpful for whitelisting specific IP)

    - NLB are used for extreme performance, TCP or UDP traffic
    - Not included in the AWS free tier

    - NLB Target Groups
        - EC2 instances
        - IP Addresses - must be private IPs
        - Application Load Balancer(yeah, an ALB)
        - Health Checks support the TCP, HTTP and HTTPS Protocols

- Gateway Load Balancer - GWLB
    - Deploy, sacle ,and manage a fleet of 3rd party network virtual appliances in AWS
    - Example: Firewalls, Intrusion Detection and PRevention Systems, Deep Packaet Inspection Systems, payload manipulation,...

    - Operates at Layer 3 (Network Layer) - IP Packats
    - Combines the following functions:
        - Transparent Network Gateway - single entry/exit for all traffic
        - Load Balancer - distributes traffic to your virtual appliances
    - Uses the GENEVE protocol on port 6081

    - Target Groups:
        - EC2 instances
        - IP Addresses - must be private IPs

- Sticky Sessions (Session Affinity)
    - It is possible to implement stickiness so that the same client is always redirected to the same instance behind a load balancer
    - This works for CLB and ALB
    - The "cookie" used for stickiness has an expiration date you control
    - Use case: make sure the user doesn't lose his session data
    - Enabling stickiness may bring imbalance to the load over the backend EC2 instances

    - Cookie Names
        - Application-based Cookies
            - Custom cookie
                - Generated by the target
                - Can include any custom attributes required by the application
                - Cookie name must be especified individually for each target group
                - Don't use AWSALB, AWSALBAPP or AWSALBTG (reserved for use by the ELB)
            - Application cookie
                - Generated by the load balancer
                - Cookie name is AWSALBAPP
        - Duration-based Cookie
            - Cookie generated by the load balancer
            - Cookie name is AWSALB for ALB, AWSELB for CLB
    
- Cross-Zone Load Balancing
    - images/img2
    - With Cross Zone Load Balancing - each instance distributes evenly across all registered instances in all AZ
    - Without Cross Zone Load Balancing - requests are distributed in the instance of the node of the Elastic Load Balancer

    - Application Load Balancer - ALB
        - Enabled by default (can be disabled at the Target Group level)
        - No charges fo inter AZ data
    - Network Load Balancer & Gateway Load Balancer - NLB & GWLB
        - Disabled by default
        - You pay charges ($) for inter AZ data if enabled
    - Classic Load Balancer CLB
        - Disabled by default
        - No charges for inter AZ data if enabled

- SSL/TLS
    - An SSL Certificate allows traffic between your clients and your load balancer to be encrypted in transit (in-flight encryption)

    - SSL refers to Secure Sockets Layer, used to encrypt connections
    - TLS refers to Transport Layer Security, witch is a newer version
    - Nowadays, TLS certificates are mainly used, but people still refer as SSL

    - Public SSL certificates are issued by Certificate Authorities (CA)
    - Comodo, Synmantec, GoDaddy, GlobalSign, Digicert, Letsencrypt, etc...


- SSL Certificates
    - The load balancer uses an X.509 certificate (SSL/TLS server certificate)
    - You can manage certificates using ACM (AWS Certificate Manager)
    - You can create upload your own certificates alternatively
    - HTTPS listener:
        - You must specify a default certificate
        - You can add an optional list o certs to support multiple domains
        - Clients can use SNI (Server Name Indication) to specify the horname they reach
        - Ability to specify a security policy to support older version of SSL / TLS (legacy clients)

    - Server Name Indication - SNI
        - SNI solves the problem of loading multiple SSL certificates onto one web server (toserve multiple websites)
        - It's a "newer" protocol, and requires the client to indicate the horname of the target server in the initial SSL handshake
        - The server will then find the correct certificate, or return the default one

        * Note
        - Only works for ALB & NLB (newer generation), CloudFront
        - Does not work for CLB (older gen)

    - ELB - SSL certificates
        - Classic Load Balancer - CLB (v1)
            - Support only one SSL certificate
            - Must user multiple CLB for multiple horname with multiple SSL certifcates
        - Application Load Balancer - ALB (v2)
            - Supports multiple listeners with multiple SSL certificates
            - Uses Server Name Indication (SNI) to make it work
        - Network Load Balancer (v2)
            - Supports multiple listeners with multiple SSL certificates
            - Uses SNI to make it work

- Connection Draining
    - Feature naming
        - Connection Draining - for CLB
        - Deregistration Delay - for ALB and NLB
    - Time to complete "in-flight requests" while the instance is de-registering or unhealthy
    - Stops sending new requests to the EC2 instance which is de-registering
    - Between 1 to 3600 seconds (default: 300 seconds)
    - Can be disabled (set value to 0)
    - Set to a low value if your requests are short

## AWS Auto Scaling Group - ASG
- In real-life, the load on your websites and applications can change
- In the cloud, yo ucan create and get rid of servers very quickly
- images/img3
- ASG + ELB - images/img4 

- The goal of an Auto Scaling Group is to:
    - Scale out (add EC2 instances) to match an increased load
    - Scale in (remove EC2 instances) to match a decreased load
    - Ensure we have a minimum and a maximum number of EC2 instances running
    - Automatically register new instances to a load balancer
    - Re-create an EC2 instance in case a previous one is terminated (ex: if unhealthy)
- ASG are free (you only pay for the underlying EC2 instances)

- ASG Attributes
    - A Launch Template
        - AMI + Instance Type
        - EC2 User Data
        - EBS Volumets
        - Security Groups
        - SSH Key Pair
        - IAM Roles for your EC2 Instances
        - Network + Subnets Information
        - Load Balancer Information
    - Min Size / Max Size / Initial Capacity
    - Scaling Policies

- Auto Scaling - CloudWatch Alarms & Scaling
    - It is possible to scale an ASG based on CloudWatch alarms
    - An alarm monitor a metric (such as Avarage CPU, or a custom metric)
    - Metricis such as Avarage CPU are computed for the overall ASG instances
    - Based on the alarm:
        - We can create scale-out policies (increase the number of insances)
        - We can create scale-in policies (decrease the number of instances)

- ASG - Dynamic Scaling Policies
    - Target Tracking Scaling
        - Most simple and easy to set-up
        - Example: I want to avarage ASG CPU to stay at around 40%
    - Simple / Step Scaling
        - When a CloudWatch alarmig is triggered (example CPU > 70%), then add 2 units
        - When a CloudWatch alarmig is triggered (example CPU < 30%), then add 1 units
    - Scheduled Actions
        - Anticipate a scaling based on known usage patterns
        - Example: increase the min capacity to 10 at 5 pm on Fridays

- ASG - Predictive Scaling
    - Predictive sacaling: continuously forecast load and schedule scaling ahead (machine learning powered)

- Good metrics to scale on
    - CPUUtilization: avarage CPU utilization across your instances
    - RequestCountPerTarget: to make sure the number of requests per EC2 instances is stable
    - Avarage NEtwork In / Out (if your're application is network bound)
    - Any custom metric (that you push using CloudWatch)

- ASG - Scaling Cooldowns
    - After a scaling activity happens, you are in the cooldown period (default 300 seconds)
    - During the cooldown period, the ASG will not launch or terminate additional instances (to allow for metrics to stabilize)
    
    - Advice: Use a ready-to-use AMI to reduce configuration time in order to be serving request fasters an reduce the cooldown period


## AWS Relational Database Service - RDS
- RDS stands for Relational Database Service
- It's a managed DB service for DB use SQL as a query language
- It allows you to create databases in the cloud that are managed by AWS
    - Postgres
    - MySQL
    - MariaDB
    - Oracle
    - Microsoft SQL Server
    - Aurora

- Advantage over using RDS versus deploying DB on EC2
    - RDS is a managed service
        - Automated provisioning, OS patching
        - Continuous backups and restore to specific timestamp (Point in Time Restore)
        - Monitoring dashboards
        - Read replicas for improved read performance
        - Multi AZ setup for DR (DIsaster Recovery)
        - Maintenance windows for upgrades
        - Scaling capability (vertical and horizontal)
        - Storage backed by EBS (gp2 or io1)
    - But you can't SSH into your instances

- RDS - Storage Auto Scaling
    - Helps you increase storage on your RDS DB instance dynamically
    - When RDS detects you are running out of free database storage, it scales automatically
    - Avoid manually scaling your database storage
    - You have to set Maximum Storage Threshold (maximum limit for DB storage)
    - Automatically modify storage if:
        - Free storages is less than 10% of allocated storage
        - Low-storage lasts at lesast 5 minutes
        - 6 hours have passed since last modification
    - Useful for applications with unpredictable workloads
    - Supports all RDS database engines (MariaDB, MySQL, PostgreSQL, SQL Server, Oracle)

- RDS Read Replicas, for read scalability
    - Up to 5 Read Replicas
    - Within AZ, Cross AZ or Cross Region
    - Replication is ASYNC, so reads are eventually consistent
    - Replicas can be promoted to their own DB
    - Applications must update the connection string to leverage read replicas

    - Use cases
        - You have a production database that is taking on normal load
        - You want to run a reporting application to run som analytics
        - Your create a Read Replica to run the new workload there
        - The production application is unaffected
        - Read replicas as used for SELECT (=read) only kind of statements (not INSERT, UPDATE, DELETE)
    
    - Network Cost
        - In AWS there's a network cost when data goes from one AZ to another
        - For RDS Read Replicas within the same region, you don't pay that fee
    
- RDS Multi AZ (DIsaster Recovery)
    - SYNC replication
    - One DNS name - automatic app failover to standby
    - Increase availability
    - Failover in case of loss of AZ, loss of network, instance or storage failure
    - No manual intervention in apps
    - Not used for scaling

    * Note
        - The Read Replicas be setup as Multi AZ for Disaster Recovery (DR)
    
- RDS - From Single-AZ to Multi-AZ
    - Zero downtime operation (no need to stop the DB)
    - Just click on "modify" for the database
    - The following happens internally
        - A snapshot is taken
        - A new DB is retored from the snapshot in a new AZ
        - Synchronization is established between the two databases

- RDS Custom
    - Managed Oracle and Microsoft SQL Server Database with OS and database customization
    - RDS - Automates setup, operation, and scaling of database in AWS
    - Custom - access to the underlying database and OS so you can:
        - Configure settings
        - Install patches
        - Enable native features
        - Access the underlying EC2 Instance using SSH or SSM Session Manager
    - De-activate Automation Mode to perform your customization, better to take a DB snapshot before
    - RDS vs RDS Custom
        - RDS: entire database and the OS to be managed by AWS
        - RDS Custom: full admin access to the underlying OS and the database

- Amazon Aurora
    - Aurora is a proprietary technology from AWS
    - Postgres and MySQL are both supported as Aurora DB (means your drivers will work as if Aurora was a Postgres or MySQL database)
    - Aurora is "AWS cloud optimized"and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS
    - Aurora storage automatically grows in increments of 10GB, up to 128 TB
    - Aurora can have 15 replicas while MuSQL has 5, and the replication process is faster (sub 10 ms replica lag)
    - Failover in Aurora is instantaneous. It's HA (high availability) native
    - Aurora costs more than RDS (20% more) - but is more efficient

    - Aurora High Availability and Read Scaling
        - 6 copies of your data across 3 AZ
            - 4 copies out of 6 needed for writes
            - 3 copies out of 6 need for reads
            - Self healing with peer-to-peer replications
            - Storage is striped across 100s of volumes
        - One Aurora Instance takes writes (master)
        - Automated failover for master in less than 30 seconds
        - Master + up to 15 Aurora Read Replicas serve reads
        - Support for Cross Region Replication
    
    - Aurora DB Cluster
        - images/img5
        - Writer Endpoint - POinting to the master
        - Reader Endpoint - Connection Load Balancing to the read replicas
        - Replicas are Auto Scaling
    
    - Features of Aurora
        - Automatic fail-over
        - Backup and Recovery
        - Isolation and security
        - Industry compliance
        - Push-button scaling
        - Automated Patching with Zero Downtime
        - Advanced Monitoring
        - Routine Maintenance
        - Backtrack: restore data at any point of time without using backups

    - Aurora Replicas - Auto Scaling
        - Once the Reader Endpoint has a lot of requests and the Reader replicas increase de CPU usage, Auto Scaling creates Endpoint Extended to resolve this situation
    
    - Aurora - Custom Endpoints
        - Define a subset of Aurora Instances as a Custom Endpoint
        - Example: Run analytical queries on specific replicas
        - The Reader Endpoint is generally not used after defining Custom Endpoints
    
    - Aurora Servless
        - Automated database instantiation and auto-scaling based on actual usage
        - Good for infrequent, intermittent or unpredictable workloads
        - No capacity planning needed
        - Pay per second, can be more cost-effective
    
    - Autora Multi-Master
        - In case you want immediate failover for write node (HA)
        - Every node does R/W - vs promoting a RR as the new master
    
    - Global Aurora
        - Aurora Cross Region Read Replicas
            - Useful for disaster recovery
            - Simple to put in place
        - Aurora Global Database (recommended)
            - 1 Primary Region (R/W)
            - Up to 5 secondary (read-only) regions, replication lag is less than 1 second
            - Up t o16 Read Replicas per secondary region
            - Helps for decreasing latency
            - Promoting another region (for disaster recovery) has an RTO of < 1 minute
            - Typical cross-region replication takes less than 1 second
    
    - Aurora Machine LEarning
        - Enables you to add ML-based predictions to your applications via SQL
        - Simple, optimized, and secure integration between Aurora and AWS ML services
        - Supported services
            - Amazon SageMaker (use with any ML model)
            - Amazon Comprehend (for sentiment analysis)
        - You don't need to have ML experience
        - Use cases: fraud detection, ads targeting, sentiment analysis, product recommendations

- RDS Backups
    - Automated backups:
        - Daily full backup of the database (during the backup window)
        - Transaction logs are backed-up by RDS every 5 minutes
        - => ability to restore to any point in time (from oldest backup to 5 minutes ago)
        - 1 to 35 days of retention, set 0 to disable automated backups
    - Manual DB Snapshots
        - Manually triggered by the user
        - Retention of backup for as long as you want
    
    * Trick: in a stopped RDS database you will still pay for storage. If you plan on stopping it for a long time, you should snapshot and restore instead

- Aurora Backups
    - Automated bakcups
        - 1 to 35 days (cannot be disabled)
        - point-in-time recovery in that timeframe
    - Manual DB Snapshots
        - Manyally triggered by the user
        - Retention of backup for as long as you want
    
- RDS & Aurora Restore options
    - Restoring a RDS / Aurora backup or a snapshot creates a new database
    
    - Restoring MySQL RDS database from S3
        - Create a backup of your on-premises database
        - Store it on Amazon S3
        - Restore the backup file onto a new RDS instance running MySQL
    
    - Restoring MySQL Aurora cluster from S3
        - Create a backup of your on-premises database using Percona XtraBackup
        - Store the backup file on Amazon S3
        - Restore the backup file onto a new Aurora cluster running MySQL

- Aurora Database Cloning
    - Create a new Aurora DB Cluster from an existing one
    - Faster than snapshot and restore
    - The new DB cluster uses the same cluster volume and data as the original but will change when data updates are made
    - Very fast and cost-effective
    - Useful to create a "staging" database from a "production" database without impacting the production database

- RDS and Aurora Security
    - At-rest encryption
        - Database master and replicas encryption using AWS KMS - must be defined as launch time
        - If the master is not encrypted, the read replicas cannot be encrypted
        - To encrypt an un-encrypted database, go through a DB snapshot and restore as encrypted
    - In-flight encryption: TLS-ready by default, use the AWS TLS root certificates client-side
    - IAM Authentication: IAM roles to connect to your database (instead of username/pw)
    - Security Groups: Control Network access to your RDS/ Aurora DB
    - No SSH available excpet on RDS Custom
    - Audit Logs can be enabled an sent to CloudWatch Logs for longer retention

- RDS Proxy
    - Fully managed database proxy for RDS
    - Allows apps to pool and share DB connections established with database
    - Improving database efficiency by reducing the stress on database resources (e.g., CPU, RAM) and minimize open connections (and timeouts)
    - Serverless, autoscaling, highly available (multi-AZ)
    - Reduced failover time by up 66%
    - Supports RDS (MySQL, PostgresSQL, MariaDB) and Aurora
    - No code changes required for most apps
    - Enforce IAM Authentication for DB, and securely store credentuas in AWS Secrets Manager
    - RDS Proxy is never publicly accessible (must be accessed from Virtual Privated Cloud - VPC)


## AWS ElastiCache 
- The same way RDS is to get managed Relational Databases
- ElastiCache is to get managed Redis or MemCached
- Caches are in-memory databases with really high performance, low latency
- Helps reduce load off of databases for read intensive workloads
- Helps make your application stateless
- AWS takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups
- Using ElastiCache involves heavy application code changes

- ElastiCache Solution Architecture - DB Cache
    - images/img6
    - Applications queries ElastiCache, if not available, get from RDS and store in ElastiCache
    - Helps relieve load in RDS
    - Cache must have an invalidation strategy to make sure only the most current data is used in there

- ElastiCache Solution Architecture - User Session Store
    - images/img7
    - User logs into any of the application
    - The application writes the session data into ElastiCache
    - The user hits another instance of our application
    - The instance retrieves the data and the user is already logged in

- ElastiCache - Redis vs Memcached
    - Redis
        - Multi AZ with Auto-Failover
        - Read Replicas to scale reads and have high availability
        - Data Durability using AOF persistence
        - Backup and restore features
    - Memcached
        - Multi-node for partitioning of data (sharding)
        - No high availability (repliaction)
        - Non persistent
        - No backup and restore
        - Multi-threaded architecture

- Cache Security
    - All caches in ElastiCache
        - Do not support IAM authentication
        - IAM policies on ElastiCache are only used for AWS API-level security
    - Redis AUTH
        - You can set a "password/token" when you create a Redis cluster
        - This is an extra level of security for your cache
        - Support SSL in flight encryption
    - Memcached
        - Supports SASL-based authentication (advanced)
    
- Patters for ElastiCache
    - Lazy Loading: all the read data is cached, data can become stale in cache
    - Write Through: adds or update data in the cache when written to a DB (no stale data)
    - Session Srote: store temporary session data in a cache (using TTL features)

    * Note: There are only two hard things in Computer Science: cache invalidation and naming things

- Redis Use Case
    - Gaming Leaderboards are computtationally complex
    - Redis Sorted sets guarantee both uniqueness and element ordering
    - Each time a new element added, it's ranked in real time, then added in correct order


## AWS Route 53
- A highly available, scalable, fully managed and Authoritative DNS 
    - Authoritative = the customer can update the DNS records
- Route 53 is also a Domain Registrer
- Ability to check the health of your resources
- The only AWS service wich provides 100% availability SLA
- Why Route 53? 53 is a reference to the traditional DNS port

- Records
    - How you want to route traffic for a domain
    - Each record contains:
        - Domain / subdomain Name - e.g., example.com
        - Record Type - e.g., A or AAAA
        - Value - e.g., 12.34.56.78
        - Routing Policy - how Route %3 responds to queries
        - TTL - amount of time the record cached at DNS Resolvers
    - Rout 53 supports the following DNS record types
        - (must know) A / AAAA / CNAME / NS
    
- Record Types
    - A - maps a hostname to IPv4
    - AAAA - maps a hostname to IPv6
    - CNAME - maps a hostname to another hostname
        - The targe is a domain name wich must have an A or AAAA record
        - Can't create a CNAME record for the top node of a DNS namespace (Zona Apex)
        - Example: you can't create for example.com, but you can create for www.example.com
    - NS - Name Servers for the Hosted Zone
        - Control how traffica is routed for a domain
    
- Hosted Zones 
    - A container for records that define how to route traffic to a domain and its subdomains 

    - Public Hosted Zones - contains records that specify how to route traffic on the Internet (public domain names) - application1.myplublicdomaincom
    - Private Hosted Zones - contain records that specify how you route traffic within one or more VPCs (private domain names) - application1.company.internal

    - You pay $0.50 per month per hosted zone

- Public vs Private Hosted Zones
    - images/img8

- Records TTL (Time To Live)
    - images/img9
    - High TTL - e.g., 24h
        - Less traffic on Route 53
        - Possibly outdated records
    - Low TTL - e.g., 60 sec.
        - More traffic on Route 53 ($$$)
        - Records are outdated for less time
        - Easy to change records
    
    - Except for Alias records, TTL is mandatory for each DNS record

- CNAME vs Alias
    - AWS Resources (Load Balancer, CloudFront...) expose an AWS hostname: lalala.elb.amaon.com and you want lalala.mydomain.com

    - CNAME:
        - Points a hostname to any other hostname (app.mydomain.com => lalala.anything.com)
        - ONLY FOR NON ROOT DOMAIN (aka.something.mydomain.com)
    - Alias:
        - POints a hostname to an AWS Resource (app.domain.com => lalala.amazonaws.com)
        - Works for ROOT DOMAIN and NON ROOT DOMAIN (aka.mydomain.com)
        - Free of charge
        - Native health check
    
- Alias Records 
    - Maps a hostname to an AWS resource
    - An extension to DNS functionality
    - Automatically recognizes changes in the resource's IP addresses
    - Unlike CNAME, it can be used for the top node of a DNS namescpace (Zone Apex), e.g.: example.com
    - Alias Record is always of type A/AAAA for AWS resources (IPv4 / IPv6)
    - TTL is set automatically, you can't change

    - Targets:
        - Elastic Load Balancers
        - CloudFront Distributions
        - API Gateway
        - Elastic Beanstalk environments
        - S3 Websites
        - VPC Interface Endpoints
        - Global Accelerator
        - Rout 53 record in the same hosted zone

        - You cannot se an ALIAS record for an EC2 DNS name

- Routing Policies
    - Define how Route 53 responds to DNS queries
    - Don't get confused by the word "Routing"
        - It's not the same as Load Balancer Routing wich routes the traffic
        - DNS does not route any traffic, it only responds to the DNS queries
    - Route 53 Supports the following Routing Policies
        - Simple
        - Weighted
        - Failover
        - Latency based
        - Geolocation
        - Multi-Value Answer
        - Geoproximity (using Route 53 Traffic Flow feature)
    
    - Simple
        - Tipically, route traffic to a single resource
        - Can specify multiple values in the same record
        - If multiple values are returned, a random one is chosen by the client
        - When Alias enabled, specify only one AWS resource
        - Can't be associated with Health Checks

    - Weighted
        - Control the % of the requests that go to each specific resource
        - Assign each record a relative weight:
            - traffic (%) = (Weight for a specific record) / (Sum of all the weights for all records)
            - Weights don't need to sum up to 100
        - DNS records must have the same name and types
        - Can be associated with Health Checks
        - Use cases: load balancing between regions, testing new applications versions...
        - Assign a weight of 0 to a record to stop sending traffic to a resource
        - If all records have weight of 0, then all records will be returned equally

    - Latency-based
        - Redirect to the resource that has the least latency close to us
        - Super helpful when latency for users is a priority
        - Latency is based on traffic between users and AWS regions
        - Germany users may be redirected to the US (if that's the lowest latency)
        - Can be associated with HEalth Checks (has a failover capability)

    - Failover (Active-Passive)
        - Once one mandatory Instance is associate with Route 53, and we get an unhealthy statate, Route 53 automatically redirect to another Instance, this secondary instance also can be associated with a Health Check

    - Geolocation
        - Different from Latency-based
        - This routing is based on user location
        - Specify location by Continen, Country or by US State (if ther's overlapping, most precise location selected)
        - Should create a "Default" record (in case there's no match on location)
        - Use Cases: website localization, restrict content distribution, load balancing, ...
        - Can be associated with Health Checks
    
    - Geoproximity
        - images/img10
        - Route traffic to your resources based on the geographic location of users and resources
        - Ability to shift more traffic to resources based on the defines bias
        - To change the size of the geographic region, specify bias values:
            - To expand (1 to 99) - more traffic to the resource
            - To shrink (-1 to -99) - less traffic to the resource

        - Resources can be:
            - AWS resources (specify AWS region)
            - Non-AWS resources (speficy Latitude and Longitude)
        - You must use Route 53 Traffic Flow (advanced) to use this feature

    - Multi-Value
        - Use when routing traffic to multiple resources
        - Route 53 return multiple values/resources
        - Can be associated with Health Checks (return only values for healthy resources)
        - Up to healthy records are returned for each Multi-Value query
        - Multi-Value is not a substitute for having an ELB, the idea it's to be a client-side load balancing

- Health Checks
    - HTTP Health Checks are only for public resources
    - Health Check => Automated DNS Failover:
        1. Health checks that monitor an endpoint (application, server, other AWS resource)
        2. Health checks that monitor other health checks (Calculated Health Checks)
        3. Health checks that monitor CloudWatch Alarms (full control) - e.g., throttles of DynamoDB, alarms on RDS, custom metrics,... (helpful for private resources)
    
    - Health Checks are integrated with CW (CloudWatch) metrics

    - Monitor an Endpoint
        - About 15 global health checkers will check the endpoint health
            - Healthy / Unhealthy Threshold - 3 (default)
            - Interval - 30 sec (can set to 10 sec - higher cost)
            - Supported protocol: HTTP, HTTPS and TCP
            - If > 18% of health checkers report the endpoint is healthy, Rout 53 considers it Healthy. Otherwise, it's Unhealthy
            - Ability to choose wich locations you want Route 53 to use
        - Health Checks pass only when the endpoint responds with the 2xx and 3xx status codes
        - Health Checks can be setup to pass / fail based on the text in the first 5120 bytes of the response
        - Configure your router / firewall to allow incoming requests from Route 53 Health Checkers

    - Calculated Health Checks
        - Combine the results of multiple Health Checks into a single Health Check
        - You can use OR, AND, or NOT
        - Can monitor up to 256 Child Health Checks
        - Specify how many of the health checks need to pass to make the parent pass
        - Usage: perform maintenance to your website without causing all health checks to fail

    - Private Hosted Zones
        - Route 53 health checkers are outside the VPC
        - They can't access private endpoints (private VPC or on-premises resource)

        - You can create a CloudWatch Metric and associate a CloudWatch Alarm, then create a Health Check that checks the alarm itself

- Domain Registrar vs. DNS Service
    - You buy or register you domain name with a Domain Registrar typically by paying annual charges (e.g, GoDaddy, Amazon Registrar Inc., ...)
    - The Domain Registrar usually provides you with a DNS service to manage your DNS records
    - But you can use another DNS service to manage your DNS records
    - Example: purchase the domain from GoDaddy and user Route 53 to namage your DNS records

    - 3rd Party Registrar with Amazon Route 53
        - If you buy your domain on a 3rd party registrar, you can still use Route 53 as the DNS Service provider

        1. Create a Hoste Zone in Route 53
        2. Update NS Records on 3rd party website to use Route 53 Name Servers

        - Domain Registrar != DNS Service
        - But every Domain Registrar usually comes with some DNS features


## AWS Elastic Beanstalk
- Elastic Beanstalk is a developer centric view of deploying an application on AWS
- It uses all the component's regarding application deployment: EC2, ASG, ELB, RDS, ...
- Managed service
    - Automatically handles capacity provisioning, load balancing, scaling, application health monitoring, instance configuration, ...
    - Just the application code is the responibility of the developer
- We still have full control over the configuration
- Beanstalk is free but you pay for the underlying instances

- Components
    - Application: collection of Elastic Beanstalk component (environments, versions, configurations, ...)
    - Applications Version: an iteration of your application code
    - Environment
        - Collection of AWS resources running an application version (only one application version at a time)
        - Tiers: Web Server Environment Tier And Worker Environment Tier
        - You can create multiple environments (dev, test, prod, ...)
    
- Suppoerted Platforms 
    - Go, Java SE
    - Java with Tomcat
    - .NET Core on Linux
    - .NET on Windows Server
    - Node.js
    - PHP
    - Python
    - Ruby
    - Packer Builder
    - Single Container Docker
    - Multi-container Docker
    - Preconfigured Docker

- Web Server Tier vs Worker Tier
    - images/img31

## AWS S3 
- Amazon S3 is one of the main building blocks of AWS
- It's advertised as "infinitely scaling" storage

- Many websites use Amazon S3 as a blackbone
- Many AWS services use Amazon S3 as an integration as well

- User Cases
    - Backup ans storage
    - Disaster Recovery
    - Archive
    - Hybrid Cloud storage
    - Application hosting
    - Media hosting
    - Data lakes and big data analytics
    - Software delivery
    - Static website

- Buckets
    - Amazon S3 allows people to store objects (files) in "buckets" (directories)
    - Buckets must have a globally unique name (across all regions and accounts)
    - Buckets are defined at the region level
    - S3 looks like a global service but buckets are created in a region
    - Naming convention
        - No uppercase, No underscore
        - 3-63 characters long
        - Not an IP
        - Must start with lowercase letter or number
        - Must NOT start with the prefix xn--
        - Must NOT end with the suffix -s3alias

- Objects
    - Objects (files) have a key
    - The key is the FULL path
        - s3:my-bucket/my_file.txt
    - The key is composed of prefix + object name
    - There's no concept of "directories" within buckets (although the UI will trick you to think otherwise)
    - Just keys with very long names that contains slaches ("/")

    - Object values are the content of the body:
        - Max. Object Size is 5TB
        - If uploading more than 5GB, must use "multi-part upload"
    
    - Metadata (list of text key / value pair - system or user metadata)
    - Tags (Unicode key / value pair - up to 10) - useful for security / lifecycle
    - Version ID (if versioning is enabled)

- Security
    - User-Based
        - IAM Policies - wich API calls should be allowed for a specific user from IAM
    
    - Resource-Based
        - Bucket Policies - bucket wide rules from the S3 console - allows cross account
        - Object Access Control List (ACL) - finer grain (can be disabled)
        - Bucket Access Control List (BCL) - less common (can be disabled)
        
    - Note: an IAM principal can access an S3 object if
        - The user IAM permissions ALLOW it OR the resource policy ALLOWS it
        - AND ther's no explicit DENY
    
    - Encryption: encrypt objects in Amazon S3 using encryption keys

- S3 Bucket Policies
    - JSON based policies
        - Resources: buckets and objects
        - Effect: Allow / Deny
        - Actions: Set of API to Allow or Deny
        - Principal: The account or user to apply the policy to
    
    - Use S3 bucket for policty to:
        - Grant public access to the bucket
        - Force objects to be encrypted at upload
        - Grant access to another account
    
- Static Website Hosting
    - S3 can host static websites and have them accessible on internet
    
    - If you get a 403 Forbidden error, make sure the bucket policy allows public reads

- Versioning
    - You can version your files in Amazon S3
    - It is enabled at the bucket level
    - Same key overwrite will change the "version": 1, 2, 3...
    - It's best pratice to version your buckets
        - Protec against unintended deletes (ability to restor a version)
        - Easy roll back to previous version
    
    * Notes:
        - Any file that is not versioned prior to enabling versioning will have version "null"
        - Suspending versioning does not delete the previous versions

- Replication (CRR & SRR)
    - Must enable Versioning in source and destination buckets
    - Cross-Region Replication (CRR)
    - Same-Region Replication (SRR)
    - Buckets can be in different AWS accounts
    - Copying is asynchronous
    - Must give proper IAM permissions to S3
    
    - Use cases:
        - CRR - compliance, lower latency access, replication across accounts
        - SRR - log aggregation live replication between production and test accounts

    - After you enable Replication, only new objects are replicated
    - Optionally, you can replicate existing objects using S3 Batch Replication
        - Replicates existing objects and objectts that failed replication
    
    - For DELETE operations
        - Can replicate delete markers from source to targe (optional setting)
        - Deletions with a version ID are not replicated (to avoid malicious deletes)
    
    - There is no "chaining" of replication
        - If bucket 1 has replication into bucket 2, wich has replication into bucket 3, then objects created in bucket 1 are not replicated to bucket 3

- Storage Classes
    - Amazon S3 Standard - General Purpose
    - Amazon S3 Standard-Infrequent Access (IA)
    - Amazon S3 One Zone-Infrequent Access
    - Amazon S3 Glacier Instant Retrieval
    - Amazon S3 Glacier Flexible Retrieval
    - Amazon S3 Glacier Deep Archive
    - Amazon S3 Intelligent Tiering

    - Can move between classes manually or using S3 Lifecycle configurations

    - S3 Durability and Availability
        - Durability: High durability (99,99%) of objects across multiple AZ

        - Availability: 
            - Measures how readily available a service is
            - Varies depending on storage class

    - S3 Standard - General Purpose
        - 99,99% of Availability
        - User for frequently accessed data
        - Low latency and high throughput
        - Sustain 2 concurrent facility failures

        - Use cases: Big Data analytics, mobile & gaming applications, content distribution...

    - S3 Storage Classes - Infrequent Access
        - For data that is less frequently accessed, but requires rapid access when needed
        - Lower cost thant S3 Standard

        - Amazon S3 Standard-Infrquent Access (S3 Standard-IA)
            - 99.9% Availability
            - Use cases: Dissaster Recovery, backups

        - Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)
            - High durability (99.99999999999%) in a sigle AZ; data lost when AZ is destroyed
            - 99.5% Availability
            - Use cases: storing secondary backup copies of on-premise data, or data you can recreate

    - S3 Glacier Storage Classes
        - Low-cost object storage meant for archiving / backup
        - princing: price for stare + object  retrieval cost

        - Amazon S3 Glacier Instant Retrieval
            - Milisecond retrieval, great for data accessed once a quarter
            - Minuum storage duration of 90 days
        - Amazon S3 Glacier Flexible Retrieval (formerly Amazon S3 Glacier)
            - Expedited (1 to 5 minutes), Standard (3 to 5 hours), Bulk (5 to 12 hours) - free
            - Minimum storage duration of 90 days
        - Amazon S3 Glacier Deep Archive - for long term storage:
            - Standard (12 hours), Bulk (48 hours)
            - Minimum storage duration of 180 days

    - S3 Intelligent-Tiering
        - Small monthly monitoring and auto-tiering fee
        - Moves objects automatically between Access Tiers based on usage
        - There are no retrieval charges in S3 Intelligent-Tiering

        - Frequent Access tier (automatic): default tier
        - Infrequent Access tier (automatic): objects not accessed for 30 days
        - Archive Instant Access tier (automatic): objects not accessed for 90 days
        - Archive Access tier (optional): configurable from 90 days to 700+ days
        - Deep Archive Access tier (optional): configurable from 180 days to 700+ days

- Moving between Storage Classes
    - You can transition objects between storage calsses
    
    - For infrequently accessed object, move them to Standard IA
    - For archive objects that you don't need fast access to, move them to Glacier or Glacier Deep Archive

    - Moving Objects can be automated using a Lifecycle Rule

    - Lifecyle Rules
        - Transition Actions - configure objects to transition to another storage class
            - Move objects to Standard IA class 60 days after creation
            - Move to Glacier for archiving after 6 months

        - Expiration actions - configure objects to expire (delete) after some time
            - Access log files can be set to delete after a 365 days
            - Can be used to delete old versions of files (if versioning is enabled)
            - Can be used to delete incomplete Multi-Part uploads

        - Rules can be created for a crtain prefix (example:s3://mybucket/mp3/*)
        - Rules can be created for certain objects Tags (example:Department:Finance)

        - Scenario 1:
            - Your application on EC2 creates images thimbnails after profile photos are uploaded to AS3. These thumbnails can be easily recreated, and only need to be kept for 60 days. The source images should be able to be immediately retrieved for these 60 days, and afterwards, the user can wait up to 6 hours. How would you design this?

            - S3 source images can be on Standard, with a lifecycle configuration to transition them to Glacier after 60 days
            - S3 thumbnails can be on One-Zone IA, with a lifecycle configuration to expire them after 60 days.
        
        - Scenario 2:
            A rule in your company states that you should be able to recover your deleted S3 objects immediately for 30 days, although this may happen rarely. After this time, and for up to 365 days, deleted objects should be recoverable within 48 hours.

            - Enable S3 Versioning in order to have object versions, so that "deleted objects" are in fact hidden by a "delete marker" and ca ben recovered
            - Transition the "noncurrent versions" of the object to Standard IA
            - Transition afterwards the "noncurrent versions" to Glacier Deep Archive
    
    - Storage Class Analysis
        - help you decide when to transition objects to the right sotrage class
        - Recommendations for Standard and Standard IA
            - Does NOT work for One-Zone IA or Glacier
        - Report is updated daily
        - 24 to 48 hours to start seeing data analysis

        - Good first step to put together Lifecycle Rules

- Requester Pays
    - images/img32
    - In general, bucket owners pay for all Amazon S3 storage and data transfer costs associated with their bucket
    - With Requester Pays buckets, the requester instead of the bucket owner pays the cost of the request and the data download from the bucket
    - Helpful when you want to share large datasets with other accounts
    - The requester must be authenticated in AWS (cannot be anonymous)

- Event Notifications
    - Events are things shuch as: S3: S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication...
    - Object name filtering possible (*.jpg)
    - Use case: generate thumbnails of images uploaded to S3
    - The events could be triggered to SNS, SQS, Lambda Functions, ...
    - Can create as many "S3 events" as desired

    - S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer

    - Integration with Amazon EventBridge
        - images/img33
        - Advanced filtering options with JSON rules (metadata, object size, name, ...)
        - Multiple Destinations - ex Step Functions, Kinesis Streams / Firehose ...
        - EventBridge Capabilities - Archive, Replay Events, Reliable delivery

- Baseline Performance
    - Amazon S3 automatically sacles to high request rates, latency 100-200 ms
    - Your Application can achieve at least 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix in a bucket
    - There are no limits to the number of prefixes in a bucket
    
- Performance
    - Multi-Part upload:
        - Recommended for files > 100MB, must use for files > 5GB
        - Can help parellelize uplodas (speed up transfers)
    - S3 Transfer Acceleration
        - Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region
        - Compatible with multi-part upload
    
    - S3 By-Range Fetches
        - Parallelize GETs by requesting specific byte ranges
        - Better resilience in case of failures

        * Can be user to speedup downloads
        * Can be used to retrieve only partial data (for example the head of a file)

- S3 Select & Glacier Select
    - Retrieve less data using SQL by performing server-side filtering
    - Can filter by rows & columns (simple SQL statements)
    - Less network transfer, less CPU cost client-side

- Batch Operations
    - Perform bulk operations on existing S3 objects with a single request, example:
        - Modify object metadata & roperties
        - Copy objects between S3 buckets
        - Ecnrypt un-encrypted objects
        - Modify ACLs, tags
        - Restore objects from S3 Glacier
        - Invoke Lambda function to perform custom action on each object
    - A job consists of a list of objects, the action to perform, and optional parameters
    - S3 Batch Operations manages retries, tracks progress, send completion notifications, generate reports ...
    - You can use S3 Inventory to get object list and use S3 Select to filter your objects

- Object Encryption
    - You can ecnrypt objects in S3 buckets using one of 4 methods

    - Server-Side Encryption (SSE)
        - Server-Sider Encryption with Amazon S3-Managed Keys (SSE-S3)
            - Encrypts S3 objects using keys handled, managed, and owned by AWS
        - Server-Side Encryption with KMS Key stored in AWS KMS (SSE-KM)
            - Leverage AWS Key Management Service (AWS KMS) to manage encryption keys
        - Server-Side Encryption with Customer-Provided Keys (SSE-C)
            - When you want to manage your own encryption keys
        - Client-Side Encryption

        - It's important to understand wich ones are for wich situation for the exam

    - SSE-S3
        - Encryption using keys handled, managed, and owned by AWS
        - Object is encrypted server-side
        - Encryption type is AWS-256
        - Must set header "x-amz-server-side-encryption": "AES256"

    - SSE-KSM
        - Encryption using keys handled and managed by AS KMS (Key Management Service)
        - KSM advantages: user control + audit key usage using CloudTrail
        - Object is encrypted server side
        - Must set header "x-amz-servier-side-encryption": "aws:kms"

        - Limitations
            - If you use SSE-KMS, you may be impacted by the KMS limits
            - When you upload, it calls the GenerateDataKey KMS API
            - When you download, it calls the Decrypt KSM API
            - Count towards the KMS quota per second (5500, 10000, 30000 req/s based on region)
            - You can request a quota increase using the Service Quotas Console
    
    - SSE-C
        - Server-Side Encryption using keys fully managed by the customer outside of AWS
        - Amazon S3 does NOT store the encryption key you provide
        - HTTTPS must be used
        - Encryption key must be provided in HTTP headers, for every HTTP request made

    - Client-Side Encryption
        - Use client libraries such as Amazon S3 Client-Side Encryption Library
        - Clients must encrypt data themselves before sending to Amazon S3
        - Clients must decrypt data themselves when retireving from Amazon S3
        - Customer fully manages the keys and encryption cycle

    - Encryption in transit (SSL/TLS)
        - Amazon S3 exposes two endpoints:
            - HTTPS Endpoint - non encrypted
            - HTTPS Endpoint - encryption in flight

            - HTTPS is recommended
            - HTTPS is mandatory for SSE-C
            - Most clients would use the HTTPS endpoint by default

    - Default Encryption vs. Bucke Policies
        - One way to "force encryption" is to use a bucket policy and refuse any API call to PUT and S3 object without encryption headers
        - Policy example: images/img34
        - Another way is to use the "default encryption" option in S3
        * Note: Bucket POlicies are evaluated before "default encryption"

    - S3 CORS
        - CORS (Cross-Origin Resource Sharing)
            - Origin = schem (protocol) + host (domain) + port
                - example: https://www.example.com (implied port is 443 for HTTPS, 80 for HTTP)
            - Web Browser based mechanism to allow request to other origin while visiting the main origin
            - Same origin: http://example.com/app1
             and http://example.com/app2
            - Different origins: http://example.com and http://otherexample.com
            - The requests won't be fulfilled unless the other origin allows for the requests, using CORS Headers (example: Access-Control-Allow-Origin)
        - If a client makes a cross-origin request on our S3 bucket, we need to enable the correct CORS headers
        - It's a popular exam question
        - You can allow for a specific origin or for * (all origins)
    
    - MFA Delete
        - force users to generate a code on a device before doing important operations on S3
        - MFA will be required to:
            - Permanently delete an object version
            - Suspend Versioning on the bucket
        - MFA won't be required to:
            - Enable Versioning
            - List deleted versions
        - To use MFA Delete, Versioning must be enabled on the bucket
        - Only the bucket owner (root account) can enable/disable MFA Delete
- Access Logs
    - For audit purpose, you may want to log all access to S3 buckets
    - Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket
    - That data can be analyzed using data analysis tools...
    - The target loggin bucket must be in the same AWS region

    - Warning
        - Do not set your logging bucket to be the monitored bucket
        - It will create a logging loop, and your bucket will grow exponentially

- Pre-Signed URLs
    - Generated pre-signed URLs using the S3 Console, AWS CLI or SDK
    - URL Expiration
        - S3 Console - 1min up to 720 mins (12 hours)
        - AWS CLI - configure expiration with --expires-in parameter in seconds (default 3600 secs, max 604800 secs ~ 168 hours)
    - Users given a pre-signed URL inherit the permissions of the user that generated the URL for GET / PUT

    - Examples:
        - Allows only logged-in users to download a premium video from your S3 bucket
        - Allow and ever-changing list of users to download files by generations URLs dynamically
        - Allow temporarily a user to upload a file to a precise location in your S3
    
- S3 Glacier Vault Lock
    - Adopt a WORM (Write Once Read Many) model
    - Create a Vault Lock Policy
    - Lock the policy for future edits (can no longer be changed or deleted)
    - Helpful for compliance and data retention

- S3 Object Lock (versioning must be enabled)
    - Adopt a WORM model
    - Block an object version deletion for a specified amount of time
    - Retention mode - Compliance:
        - Objects versions can't be overwritten or deleted by any user, including the root user
        - Objects retention modes can't be changed, and retention periods can't be shrtened
    - Retention mode - Governance:
        - Most users can't overwrite or delete an object version or alter its lock settings
        - Some users have special permissiions to charge the retention or delete the object
    - Retention Period: protect the object for a fixed period, it can be extended
    - Legal Host:
        - Protect the object indefinitely, independent from retention period
        - can be freely placed and removed using the s3:PutObjectLegalHold IAM permission
    
- Access Points
    - images/img35
    - Each Access Point gets its own DNS and policy to limit who can access it
        - A specific IAM user / group
        - One policy per ACcess Point => Easier to manage than complex bucket policies

    - S3 Object Lambda
        - images/img36
        - Use AWS Lambda Function to change the object before it is retrieved by t he caller application
        - Only one S3 bucket is needed, on top of wich we create S3 Access Point and S3 Object Lambda ACcess Points
        
        - Use Cases:
            - Redacting personally identifiable information for analytics or non-production environments
            - Converting across data  formats, sucha as converting XML to JSON
            - Resizing and watermarking images on the fly using caller-specific details, such as the user who requested the object
    

## AWS CloudFront
- Content Delivery Network (CDN)
- Imrpoves read performance, content is cached at the edge
- Improves users experience
- 216 Point of Presence globally (edge locations)
- DDoS protection (because worldwide), integration with Shield, AWS Web Application Firewall

- Origins
    - S3 bucket
        - For distributing files and caching them at the edge
        - Enhanced security with CloudFront Origin Access Control (OAC)
        - OAC is replacing Origin Access Identity (OAI)
        - CloudFront can be used as an ingress (to upload files to S3)
    
    - Custom Origin (HTTP)
        - Application Load Balancer
        - EC2 instance
        - S3 website (must first enable the bucket as a static S3 website)
        - Any HTTP backend you want

    - CloudFront at a high level
        - images/img36

    - S3 as an Origin
        - images/img37

    - CloudFront vs S3 Cross Region Replication
        - CloudFront:
            - Global Edge network
            - Files are cached for a TTL (maybe a day)
            - Great for static content that must be available everywhere
        - S3 Cross Region Replication:
            - Must be setup for each region you want replication to happen
            - Files are updated in near real-time
            - Ready only
            - Great for dynamic content that need to be available at low-latency in few regions

    - ALB or EC2 as an origin
        - images/img38

- Geo Restriction
    - You can restric who can access your distribution
        - Allowlist: Allow your users to access your content only if they're in one of  the countries on a list of approved countries
        - Blocklist: Prevent your users from accessing your content if the're in one of the countries on a list of banned countries
    - The "country" is determined using a 3rd party Geo-IP database
    - Use cases: Copyright Laws to control access to content

- Pricing
    - CloudFront Edge locations are all around the world
    - The cost of data out per edge location varies

    - Price Classes
        - You can reduce the number of edge locations for cost reduction
        - Three price classes:
            1. Price Class All: all regions - best performance
            2. Price Class 200: most regions, but excludes the most expensive regions
            3. Price Class 100: only the least expensive regions
    
- Cache Invalidations
    - images/img39
    - In case you update the back-end origin, CloudFront doesn't know about it and will only get the refreshed content after the TTL has expired
    - However, you can force an entire or partial cache refresh (thus bypassing the TTL) by performing a CloudFront Invalidation
    - You can invalidate all files (*) or a special path (/images/*)



## AWS Global Accelerator
- Leverage the AWS internal network to route to your application
- 2 Anycast IP are created for your application
- The Anycast IP send traffic directly to Edge Locations
- The Edge locations send the traffic to your application

- Works with Elastic IP, EC2 instances, ALB, NLB, public or private
- Consistent Performance
    - Intelligent routing to lowest latency and fast regional failover
    - No issue with client cache (because the IP doesn't change)
    - Internal AWS network
- Health Checks
    - Global Accelerator performs a health check of your applications
    - Helps make your application global (failover less than 1 minute for unhealthy)
- Security
    - only 2 external IP need to be whitelisted
    - DDoS protection thanks to AWS Shield

- AWS Global Accelerator vs CloudFront
    - They both use the AWS global network and its edge locations around the world
    - Both services integrate with AWS Shield for DDoS protection

    - CloudFront
        - Improves performance for both cacheable content (such as images and videos)
        - Dynamic content (such as API acceleration and dynamic site delivery)
        - Content is served at the edge
    - Global Accelerator
        - Improves performance for a wide range of applications over TCP or UDP
        - Proxying packets at the edge to applications running in one or more AWS Regions
        - Good f it for non-HTTP use cases, such as gaming (UDP, IoT) (MQTT), or Voice over IP
        - Good for HTTP use cases that require static IP addresses
        - Good for HTTP use cases that required deterministic, fast regional failover



## AWS Snow Family
- Highly-secure, portable devices to collect and process data at the edge, and migrate data into and out of AWS

- Data migration (physical devices):
    - Snowcone
    - Snowball Edge
    - Snowmobile

- Edge computing:
    - Snowcone
    - Snowball edge

- Data migration with AWS Snow Family
    - images/img40
    - AWS Snow Family: offline devices to eprform data migrations
    - If it takes more than a week to transfer over the network, use Snowball devices

    - AWS Snowball Edge (for data transfers)
        - Physical ata transport solution: move TBs or PBs of data in or out of AWS
        - Alternative to moving data over the network (and paying network fees)
        - Pay per data transfer job
        - Provide block storage and Amazon S3-compatible object storage
        
        - Snowball Edge Storage Optimized
            - 80 TB of HDD capacity for block volume and S3 compatible object storage
        - Snowball Edge Compute Optimized
            - 42 TB of HDD capacity for block volume and S3 compatible object storage
        
        - Use cases: large data cloud migrations, DC decommision, disaster recovery

    - AWS Snowcone
        - Small, portable computing, anywhere, rugged & secure, withstands harsh envrionments
        - Light, 2.1kg
        - Device used for edge computing, storage, and data transfer
        - 8 TBs of usable storage
        - Use Snowcone, where Snowball does not fit (space-constrained environment)
        - Must provide your own battery / cables

        - Can be sent back to AWS offline, or connect it to intenet and use AWS DataSync to send data

    - AWS Snowmobile (it's actually a truck!!)
        - Transfer exabyte of data (1 EB = 1,000 PB)
        - Each Snowmobile has 100 PB of capacity (use multiple in parallel)
        - High security: temperature controlled, GPS, 24/7 video surveillance
        - Better than Snowball if you transfer more than 10 PB
    
    - Usage Process
        1. Request Snowball devices from the AWS console for delivery
        2. Install the snowball client / AWS OpsHub on your servers
        3. Connect the snowball to your servers and copy files using the client
        4. Ship back the device when you're done (goes to the right AWS facility)
        5. Data will be loaded into an S3 bucket
        6. Snowball is completely wiped
    
- Edge Computing
    - Process data while it's being created on an edge location
        - example: A truck on the road, a ship on the sea, a mining stating underground...
    - These locations may have
        - Limited / no internet access
        - Limited / no easy access to computing power
    - AWS setup a Snowball Edger / Snowcone device to do edge computing
    - Use cases of Edge Computing:
        - Preprocess data
        - Machine learning at the edge
        - Transcoding media streams
    - Eventually (if need be) we can ship back the device to AWS (for transferring data for example)

    - Snowcone (smaller)
        - 2 CPUs, 4 GB of memory, wired or wireless access
        - USB-C power using a cord or the optional battery
    - Snowball Edge - Compute Optimized
        - 52 vCPUs, 208 GiB of RAM
        - Optional GPU
        - 42 TB usable storage
    - Snowball Edge - Storage Optimized
        - Up to 40 vCPUs, 80 GiB of RAM
        - Objects storage clustering available
    - All: can run EC2 Instances & AWS Lambda functions (using aWS IoT Greengrass)
    - Long-term deployment options: 1 and 3 years discounted pricing

- AWS OpsHub
    - Historically, to use Snow Family devices, you needed a CLI
    - Today, you can use AWS OpsHub (a software you install on your computer / laptop) to manage your Snow Family Device
        - Unlocking and configuring single or clustered devices
        - Transferring files
        - Launching and managing instances running on Snow Family Devices
        - Monitor device metrics (storage capacity, active instances on your device)
        - Launch compatible AWS services on your devices (ex: Amazon EC2 instances, AWS DataSync, Network File System NFS))



## Amazon FSx
- Launch 3rs party high-performance file systems on AWS
- Fully managed service

- FSx for Windows (File Serve)
    - FSx for Windows if a fully managed Windows file system share drive
    - Supports SMB protocol & Windows NTFS
    - Microsoft Active Directory integration, ACLs, user quotas
    - Can be mounted on Linux EC2 instances
    - Supports Microsoft's Distributed File SYstem (DFS) Namespaces (group files across multiple FS)

    - Scale up to 10s of GB/s, millions of IOPS, 100s PB of data
    - Storage Options:
        - SSD - latency sensitvie workloads (databses, media processing, data analytics, ...)
        - HDD - broad spectrum of workloads (home directory, CMS, ...)
    - Can be accessed from your on-premises infrastructure (VPN or Direct Connect)
    - Can be configured to be Multi-AZ (high availability)
    - Data is backed-up daily to S3

- FSx for Lustre
    - Lustre is a type of prallel distributed file system, for large-scale computing
    - The name Lustre is derived from "Linux" and "cluster"

    - Machine Learning, High Performance Computing (HPC)
    - Video Processing, Financial Modeling, Electronics Design Automation
    - Scales up to 100s GB/s, millions of IOPS, sub-ms latencies
    - Storage Options:
        - SSD - low-latency, OPS intensive workloads, small & random file operations
        - HDD - throughput-intensive workloads, large & sequential file operations
    - Seamless integration with S3
        - Can "read S3" as a file system (through FSx)
        - Can write the output of the computations back to S3 (through FSx) 
    - Can be used from on-premises servers (VPN or Direct Connect)

- FSx File System Deployment Options
    - Scratch File System
        - Temporary storage
        - Data is not replicated
        - High burst
        - Usage: short-term processing, optimize costs
    - Persistent File System
        - Long-term storage
        - Data is replicated within same AZ
        - Replace failed files within minutes
        - Usage: long-term processing, sensitive data

- FSx for NetApp ONTAP
    - Managed NetApp ONTAP on AWS
    - File System compatible with NFS, SMB, iSCSI protocol
    - Move workloads running on ONTAP or NAS to AWS
    - Works with:
        - Linux
        - Windows
        - MacOS
        - VMware cloud
        - Amazon Workspace & AppStream 2.0
        - Amazon EC2, ECS and EKS
    - Storage shrinks or grows automatically
    - Snapshots, replication, low-cost, compression and data de-duplication
    - Point-in-time instatntaneous cloning (helpful for testing new workloads)

- FSx for OpenZFS
    - Managed OpenZFS file system on AWS
    - File System compatible with NFS (v3, v4, v4.1, v4.2)
    - Move workloads running on ZFS to AWS
    - Works with:
        - Linux
        - Windows
        - MacOS
        - VMware cloud
        - Amazon Workspace & AppStream 2.0
        - Amazon EC2, ECS and EKS
    - Up to 1,000.000 IOPS with < 0.5ms latency
    - Snapshots, compression and low-cost
    - Point-in-time instantaneous cloning






## Classic Solutions Architecture Examples
- These solutions architectures are some example how to all the technologies below work togheter

- Stateless Web App: WhatsIsTheTime.com
    - Stateless Web App: WhatIsTheTime.com
        - images/img11
        - WhatIsTheTime.com allows people to know what time it is
        - We don't need a database
        - We want to start small and can accept downtime
        - We want to fully scale vertically and horizontally, no downtime

        - Scalling proccess:
            - images/img11 to images/img18
            - Topics used on the scalling proccess:
                - Public vs Private IP and EC2 instances
                - Elastic IP vc Route 53 vs Load Balancers
                - Route 53 TTL, A records and Alias Records
                - Maintaining EC2 instances manually vcs Auto Scaling Groups
                - Multi AZ to survive disasters
                - ELB Health Checks
                - Security Group Rules
                - Reservation of capacity for costing saving when possible
                - We're considering 5 pillars for a well architected application: cost, performance, reliability, security, operational excellence
    
    - Stateful Web App: MyClothes.com
        - MyClothes.com allows people to buy clothes online
        - There's a shopping cart
        - Our website is having hundreads of users at the same time
        - We need to scale, maintain horizontal scalability and keep our web application as stateless as possible
        - Users should not lose their shopping cart
        - Users should have their details (address, etc) in a database

        - Scalling proccess, 3-tier architectures for web applications
            - images/img19 to images/img25
            - EBLA sticky sessions
            - Web clients for storing cookies and making our web app stateless
            - ElastiCache
                - For storing sessions (alternatvie: DynamoDB)
                - For caching data from RDS
                - Multi AZ
            - RDS
                - For storing user data
                - Read replicas for scaling reads
                - Multi AZ for disasters recovery
            - Tight Security with security groups referencing each other

    - Stateful Web App: MyWordPress.com
        - We area trying to create a fully scalable WordPress website
        - We want that website to access and correctly display picture uploads
        - Our user data, and the blog content should be stored in a MySQL database

        - Scalling proccess:
            - images/img26 to images/img30
            - Aurora Database to have easy Multi-AZ and Read-Replicas easier
            - Storing data in EBS (single instance application)
            - Vs Storing data in EFS (distributed application)
            
- Instantiating Applications quickly
    - When launching a full stack (EC2, EBS, RDS), it can take time to:
        - INstall applications
        - Insert initial (or recovery) data
        - Configure everything
        - Launch the application

    - We can take advantage of the cloud to speed that up

    - Instantiatin Applications quickly
        - EC2 Instances:
            - Use a Golden AMI: INstall your applications, OS dependencies etc... beforehand and launch your EC2 instance from the Golden AMI
            - Bootstrap using User Data: For dynamic configuration, use User Data scripts
            - Hybrid: mix GOlden AMI and User Data (Elastic Beanstalk)
        - RDS databases:
            - Restore from a snapshot: the database will have schemas and data ready
        - EBS Volumes:
            - Restore from a snapshot: the disk will already be formatted and have data



## Annotations and doubts
IOPS - I/O operations per second
rack = hardware
throutghput = taxa de transferencia


Doubts:
BYOL - Bring Your Own License?

## Scalability and High Availability

- Scalability means that an applications / system can handle greater loads by adapting
- There are two kins of scalability
    - Vertical Scalability
        - Vertically scalability means increasing the size of the instance
        - For example, your application runs on a t2.micro, scaling that applications vertically means running it on a t2.large
        - Vertical scalability is very common for non distributed systems, such as a database
        - RDS, ElastiCache are services that can scale vertically
    - Horizontal Scalability (= elasticity)
        - Horizontal Scalability means increasing the number of instances / systems for your application
        - Horizontal scaling implies distributed systems
        - This is very common for web applications / modern applications
        - It's easy to horizontally scale thanks the cloud offerings such as Amazon EC2
    - High Availability
        - High Availability usualy goes hand in hand with horizontal scaling
        - High availability means running your data applications / system in at least 2 data centers (== AZs)
        - The goal of high availability is to survive a data center loss
        - The high availability can be passive (for RDS Multi AZ for example)
        - The high availability can be active (for horizontal scaling)
- Scalability is linked but different to High Availability




## Unicast IP vs Anycast IP
- Unicast IP: one server holds one IP address

- Anycast IP: all servers hold the same IP address and the client is routed to the nearest one