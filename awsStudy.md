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

        - EC2 Spot Instance Requests:
            - Define max spot price and get the instance while current spot price < max
                - The hourly spot price varies based on offer and capacity
                - If the current spot price > your max price you can choose to stop or teminate your instance
            - Other strategy: Spot Block
                - "block"spot instance during a specified time frame (1 to 6 hours) without interruptions
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
        - Sport instances: the horel allows people to bid for the empty rooms and the highest bidder keeps the rooms. You can get kicked out at any time
        - Dedicated Hosts: We book an entire building of the resort
        - Capacity Reservations: You book a room for a period with full price even you don't staty in it

    - Spot Fleets = set of Spot Instances + (optional) On-Demand Instances
        - The Spot Fleet will  try to meet the target capacity with price constraints
            - Define possible launch pools: instance type (m5.large), OS, Availability Zona
            - Can have multiple launch pools, so t hat the fleet can choose
            - Spot Fleet stops lauching instances when reaching capacity or max cost
        - Strategies to allocato Spot Instances:
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
        - Or, as we will see later, use a Load Balancer and don't use a public IP

- Placement Groups
    - Sometimes you want control over the EC2 Instance placement strategy
    - That strategy can be defined using placement groups
    - When you create a placement group, you specify one of the following strategies for te group:
        - Cluster - clusters instances into a low-latency group in a single Availability Zone
            - Pros - Great network
            - Cons: If the rack failst, all instances fails at the same time
            
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
        - Long-rnning processing
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
    - Mae a backup (snapshot) of your EBS volume at a point in time
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
        - io1 / io2 (SSD): Highest-performance SSD volume for mission-critical low-latency or high-thoughput workloads
        - st1 (HDD): Low cost HHD volume designed for frequently accessed, throughput-intensive workloads
        - sc1 (HDD): Lowest cost HDD volume designed for lesse frequently accessed workloads

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
        - Achive higher application availability in clustered Lunux applications (ex: Teradata)
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

# Amazon EFS - Elastic File System

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




IOPS - I/O operations per second
rack = hardware
throutghput = taxa de transferencia

Doubts:
BYOL - Bring Your Own License?
