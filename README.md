IAM (Identity Access Management)



Users
Groups
Roles
Policies
It is universal, does not apply to regions
New user has no permissions
Key id and access key are used to access AWS API and command line


Policy JSON has

Version
Statement
Effect
Action
Resource
 



S3



object based
File can be from 0 bytes to 5TB
Unlimited storage
Files are stored in buckets
S3 is a universal namespace (name must be unique globally), it creates dns
Not suitable to install OS or database
You can turn on MFA delete, to protect your object and accidentally delete your objects
Multipart upload, recommended for files over 100mb, required for files over 5gb
Will give you http 200 code on successful upload
By default new bucket are private, you can setup access using bucket policies or ACL
You can configure access log, which log all request to s3 buckets, this can be sent to another bucket or even another bucket in another account
Read after write consistency for put new object
Eventual consistency for overwrite puts and deletes (takes time to propagate)


S3 url

{bucket name}.s3.Region.amazonaws.com/{key name} -virtual hosted style
s3.region.amazonaws.com/bucketname/keyname - path style


storage classes

standard, stored across multiple devices in multiple facilities, instant retrieval
IA (infrequently access), less frequent access but requires rapid access, retrieval fee, instant retrieval 99,999999999% durability, 99.99% availability, 30 days
One zone IA - no multiple availability zone data resilience, instant retrieval
Intelegent tiering - optimize cost by automatically moving data to the most cost effective access tier, instant retrieval
Glacier, retrieval time from minutes to hours, 90 days
Glacier deep archieve, retrieval times 12 hours, 120 days


billing

Storage
Requests
Storage management pricing
Data transfer pricing
Transfer acceleration
Cross region replication
S3 has

Key - name of the object
Value - data, sequence of byte
Version id, object store before you enable version will have version id of null
Metadata
Subresources; access control list (using bucket ACL or using bucket policies)
read after write consistency - after you upload you can access immediately 
Eventual consistency for overwrite puts and delete, can take time to propagate, you might get the old object


encryption

In transit - SSL/TSL



At rest:

Server side

s3 managed keys - SSE-S3 - strongest avail, 256bit
Awk key management service - SSE-KMS, there is a limit, region specific 5.5k, 10k or 30k, provide audit trail
Server side encryption with customer provided keys - SSE-C


Client side

client encrypt then send to server




AWS organisation

enable MFA on root
Use strong and complex password
Paying account should be for billing only
enable/disable was services using service control policies (SCP) either on organisational unit or individual accounts




Cross region replication

requires versioning
Files in an existing buckets are not replicate automatically
All subsequent updated files will be updated automatically
Delete markers are not replicated
Deleting individual version or delete markers will not be replicated


Versioning

backup tool
Once enabled can’t be disabled
MFA delete capability, additional layer of security
You need versioning enabled for cross region replication


Lifecycle management

automates moving your objects between different storage tiers
Can be used in conjunction with versioning
Can be applied to the current and previous versions


Object lock

Store object using worm model (write once read many), it can prevent objects from being deleted or modified for fixed amount of time or indefinitely 



Share buckets across accounts

using bucket policies and identity access management (IAM) programatic access only
Using bucket ACLS and IAM (individual objects) programmatic access only
Cross account IAM roles, programming and console access 


Transfer acceleration

Instead of uploading directly to s3, you will use a distinct url to upload directly to an edge location, which will then transfer the file to S3



Data Sync

Used to move large amount of data from on premises to AWS
Used with NFS and SMB compatible systems
Replication; hourly, daily, weekly
Install dataSync agent to start replication
Can be used to replicate EFS to EFS


CloudFront



Is amazon CDN
It can be used to deliver your website, dynamic, stating, streaming, and interactive content
Edge location - location where the content will be cached, this is separate to an AWS region
Origin - the origin of the file that the CDN will distribute, can be s3, ec2, elastic load balancer or route53
Distribution type: web distribution; website AND RTMP; media streaming
Object are cached for the life of the TTL (Time to live)
You can clear cached, but you will be charged


Signed url and cookies

When we create a signed url or cookies, we attach a policy; url expiration, ip ranges, or trusted signers (which was account can created signed URLs)
It can be used to restrict content access
It uses origin access identity (OAI)
signed url is fo individual file
Signed cookie is for multiple files


Cloudfront Signed url

can have different origins
Key-pair is account wide and managed by root user
Utilise caching
Filtering; date, path, ip, etc
Use when your origin is EC2

S3 Signed url

issue a requests as IAM user who create the presigned url
use when your origin is S3


Snowball

petabyte scale
import/export to S3
50TB or 80TB
Tamper resistant enclosure
256 bit encryption
Trusted platform module (TPM)


Snowball edge; 100TB, with onboard storage and compute capabilities


Snowmobile; exabyte scale, up to 100PB per snowmobile



Storage gateway

hybrid storage between aws and on premise
File gateway, volume gateway, and tape gateway
Stored on S3


File gateway

NFS & SMB - storing files

Volume gateway (iSCIS)

Stored volumes - entire data is store on site and backed up to s3

cached volumes -  entire data set is stored on s3, and most frequent access data is cached on site



Tape gateway

To replace existing tape based backup i.e. netbackup, backup exec, seem



Athena vs Macie

Athena

Analyse and query data in S3 using SQL
Serverless
Use to analyse log data stored in s3
No need to extra/transform/load (ETL)


Macie

Uses ai to recognise s3 object that contains PII (personally identifiable information)
Great for PCI-DSS and preventing ID theft
Can be use to analyse cloud trail for suspicious API activity


Elastic Compute Cloud (EC2)



on Demand, pay by hour/second
Reserved, contract terms of 1 or 3 years, significant hourly charge discount
Standard reserved instances; 75% off on demand instances, the more you pay up front the longer the contract, the greater the discount
Convertible reserved instances; up to 54% off on demand capability to change the attributes of the instance, as long as the exchange results in the creation of reserved instance of equal or greater value
Scheduled reserved instances; available to launch based on specific time windows


Spot, bid for a price, but the price changes
Application with flexible start and end times
Low compute prices
Urgent computing need for large amount of computing capacity
If it is termination by AWS you will not be charged to the partial hour, but if you terminated the instance yourself you will be charged for the partial hour the instance ran
Dedicated host, physical dedicated server.
Regulatory requirements
Great for licensing which does not support multi-tenancy
Can be purchased on demand(hourly)
Can be purchased as reservation, 70% off the on demand price


Termination protection is turned off by default, remember to turn it on
On EBS backed instance, the default action is for the root EBS volume to be melted when instance is terminated
EBS root volume can be encrypted


Security group

change made take affect immediately 
All inbound is blocked by default
All outbound is allowed
It is stateful, what ever is allowed inbound, will be allowed outbound. NACL is stateless, which mean inbound and outbound will need to be configured
Can’t block individual ip and can’t block individual port, you can do this using NACL
You can specify allow rules, but you can’t specify deny rules




EBS (Elastic Block Store)

virtual HD in the cloud
Termination protection is turn off by default (DeleteOnTermination)
Volume exist on EBS
Snapshots exists on s3, it is incremental (delta), it is a point of copies of volumes
To create snapshot for root devices, best practice is to stop the instance before taking snapshot, however you can still do it while instance is running
You can create AMI’s from snapshots
You can change volume size on the fly
Volume will always be in the same availability zone as the EC2 instance
To move EC2 volume from one AZ to another, you need to create a snapshot
TO move EC2 volume from one region to another, create a snapshot, create an AMI, copy the AMI to another region, then use the AMI on the other region


Types of EBS

General purpose (SSD) - gp2; up to 16k iops 1ib -16tib
Provisioned IOPS (SSD) - io1; up to 64K iops, for databases - 4gib - 16igb
Throughput optimised HDD - st1; 500IOPS, for big data, data warehouse, Throughput intensive workloads
Cold HDD - sc1; 250IOPS, less frequent access, for file server
EBS Magnetic HDD - Standard; 40-200 IOPS, infrequently access


When you terminate an EC2 instance, by default the root ebs volume will also be terminated however, the attached volumes persist. There is an option to make the root volume persist on termination (DeleteOnTermination)



EBS Snapshot

Snapshot exist on S3

Are point in time copies of volumes

Are incremental - only the block that have changed since your last snapshots are moved to S3

First snapshot will take longer to create

When the EBS volumes are root device, you should stop the instance before taking snapshot

You can create ami from both volumes and snapshot

You can change abs volume on the fly

Volumes will always be in the same availability zone as the EC2 instance



Migrating EBS

to move EC2 volume from one az to another take a snapshot of it, create an AMI from the snapshot, use the AMI to launch the EC2 instance in a new AZ
To move EC2 volume from one region to another, take a snapshot, create AMI, copy to another region, use the AMI to launch the EC2 in the a new region


EBS Encryption

Snapshots of encrypted volumes are encrypted automatically

Volumes restored from encrypted snapshots are encrypted automatically

You can share snapshots only if they are unencrypted



Instance store

Ephemeral store - will be deleted if the instance is stopped

Can be rebooted



AMI

You can select AMI based on

Region
OS
Architecture (32 sion
Storage for the root device
Instance Store (Ephemeral Storage); root device is an instance store volume created from a template store in amazon S3, cannot be stopped, will loose data when stopped, can reboot, can’t keep root volume on termination
EBS backed Volumes; root device is an EBS volume created from an Amazon EBS snapshot, can be stopped, you will not loose data when it is stopped, can reboot, can keep root volume on termination if the option to keep is selected


ENI VS ENA VS EFA

ENI (elastic network interface)

Virtual network card


ENA (enhanced networking)

10GBPS - 100GBPS

Use single root I/O virtualisation (SR-IOV) to provide high performance networking

There are 2 types

ENA  - elastic network adapter— 100gbps
Virtual Function - 10gbps


EFA (elastic fabric adapter)

A network device you can attach to your amazon EC2 to accelerate high performance computer (HPC) and machine learning applications, or OS by-pass

Only on linux



Encrypted root device volumes

Snapshot of encrypted volumes are encrypted automatically
Volumes restored from encrypted snapshot, are encrypted automatically
You can share only unencrypted volume


Spot instances and Spot Fleets

Spot instances, uses unused ec2 capacity in AWS, up to 90% cheaper compared to on demand instances.
Spot instance will be provisioned as long as the sport price is below your maximum spot price; you will have 2 mins to decide if the spot instance will be terminated or not
Spot block; stop your instance to be terminated, currently can be set to 1-6 hours
it is useful for big data, analytics, containerised workloads, ci/cdand testing, web services, image and media rendering, high performance computing
It is not good for persistent workloads, critical jobs, and databases.


Spot request

max price
Number of instances
Launch specification
Request type: one time | persistent, one time will only launch once, persistent will relaunch the instances once the spot prices goes down
Valid from, valid until


Spot fleets

Collection of spot instances and optionally on demand instances



Spot fleet strategies

capacityOptimized; comes from the pool with optimal capacity for the number of instances launching
LowestPrice; come from the pool with the lowest price, this is the default strategy
Diversified; distributed across  all pools
instancePoolToUseCount; distributed across the number of spot instances pools you specify, this is only valid when used in combination with lowestPrice


EC2 Hibernate

To use hibernation the root volume needs to be encrypted
Make sure you have enough space on your root volume to store you RAM, on hibernate, RAM will be saved into the root volume
Much faster to boot up
Instance ram must be< 150gb
Can’t be hibernate for more than 60 days


Cloudwatch

monitor your AWS resources and application that you run on AWS
When to use cloudtrail - when you want to monitors activities (auditing), compliance, API calls
When to use cloudWatch - when you want to monitors performance
By default it will monitor events every 5 mins, we can set it to do 1 mins intervals
You can create alarms, raise events, and logs data




AWS Command Line (CLI)

you need to set up access in IAM (identity access management)


Identity Access Management Roles

Role is more secure than access key and secret access key
Easier to manage
Roles can be created to ec2 instance
Roles are universal, can be use in any region


Instance Metadata

Is used to get information about an instance

169.254.169.254.169/latest/meta-data



Instance user data

169.254.169.254.169/latest/user-data



EFS (Elastic File System)

Similar to EBS, but can be shared between EC2 instances
Storage is elastic, it grows and shrinks automatically 
Good for file server
Supports NFSV4 protocol
You only pay for storage that you are using
Scale up to petabytes
Data store in multiple AZ
Read after write consistency
Can support thousands of concurrent NFS connections
Shared storage
For linux based application


Amazon FSx for windows and Amazon FSx for lustre



FSX for windows:

managed windows server that run window server message block SMB
Designed for windows and windows application i,e, sharepoint, mussel, its, etc
Supports ad users, access control list, groups and security policies along with distributed file system namespaces and replication
EFS:

managed NAS filer for EC2 instances based on NFSV4
One of the first network file sharing protocols native to unix and linux




FSX lustre

full managed file system optimised for compute intensive workloads i.e. machine learning, financial modelling, video processing, electronic design automation
High speed high capacity distributed storage (HPC)
Can store data directly to S3


EC2 Placement Groups

the way of placing your ec2 instance
There are 3 types: clustered placement group, spread placement group, and partitioned
Name must be unique
Only certain types of instances can be launched -in a placement group (Compute Optimised, GPU, Memory Optimzed, storage optimzed)
Clustered placement group

Single availability zone
EC2 instances are put together
For application with low network latency, high network throughput, or both


Spread placement group

Individual critical group
recommended for application that have a small number of trivial instances that should be kept separate from each other
EC2 instances are spread
Only single instance in each group
Partitioned

Similar to spread placement group, but can have multiple instances in each group i.e. HDFD, HBase, Cassandra




High performance computer (HPC)

data transfer
snowball, snowmobile
AWS datasync
Direct connect; dedicated network connection from your premises to AWS
Compute and networking
EC2
EC2 fleets (spot instances and spot fleets)
Placement groups
Enhanced networking
Elastic network adapter
Elastic fabric adapter
Storage
EBS; up 64K IOPS
Instance store; millions of IPS, low latency
Network storage; s3, efs, fax for lustre
Orchestration and automation
AWS batch
AWS ParallelCluster


Web application Firewall (WAF)

monitor http and https requests
Lets you control access to your content
Layer 7 (HTTP/HTTPS)
Block cross scription, sql injection


You can block malicious ip using

WAF
NACL


Database

SQL Service
Oracle
mySQL
PostgreSQL
Aurora (Amazon proprietary) -serverless
MariaDB
dynamoDB (noSQL) not RDS, serverless


Redshift - petabytes data warehouse online analytics processing, columnar



Replication

multi region - aurora async, other RDS sync
Multi a/z - sync
Read-replicate -async


It has 2 key features

multi az, it has automatic fail-over, avail for ms sql, oracle, mysql, posgresql, and MariaDB
Read replicas (performance), no automatic fail-over, you need to create a new url and update your ec2 to point to the new url. The advantage is you can create a read replica to scale your database. It will improve your DB performance, you need to have backup turn on


RDS:

is used for online transactional processing
You can ssh or remote desktop 
Virtually machine
Not serverless except for AURORA

Encryption is done using AWS Key Management Service (KMS), once its s done, everything else i.e. snapshot, backup, will be encrypted as well.



Multi AZ is for DR only, not performance



Read replicas

can be multi az
Use to increase performance
Must have backup turned on
Can be in different region
Support mySQL, postgresSQL, mariaDB, oracle and aurora
Can be promoted to master, this will break read replica


Non relational Database - dynamoDB

Collection - table
Document - row
Key value pair - columns


ElastiCache

Web service that makes it easy to deploy, operate and scalene in memory cache in the cloud. Allowing you to retrieve information from fast managed in memory caches instead of relying on your database slow disk

It supports 2 open source in memory caching engines

Memcached - simple -not hippa multithread
Redis - more advanced - hippa, for leaderboard, game etc replication, backup, multi AZ, back up restore


Backups

Automated backup, restore backup at any point of time within a retention period(1-35 days), enabled by default, backup data is stored in S3 (free), DB might suffer in latency during backup
Snapshot; done manually, they are stored even after you delete the original database, unlike automated backup


When you restore, you will get a new RDS instance with a new DNS endpoint and default security group



Dynamo DB

Fast and flexible noSQL database service for application that need consistent, single digit millisecond latency at any scale.



stored on ssd storage
Spread across 3 geographically distinct data centres
Eventual consistent reads, if your application won’t read your data within 1 second
Strongly consistent reads, if your application need to read the data within 1 second


DynamicDB accelerator (DAX) - cached
Transactions - multiple all or nothing operations, financial operations, fulfilling orders. 
On Demand capacity - pay per request pricing, balance cost and performance, no minimum capacity, use for new product launches


Redshift

Fully managed petabyte data warehouse service.
Business intelligence
Only 1 az
Backup enabled by default with 1 day retention, max retention 35 days
Always maintain at least 3 copies of your data
Can asynchronously replicate your snapshot to another region for disaster recovery
Suitable for OLAP
Enable enhanced VPC routing to monitor COPY and Unload traffic




Aurora

amazon own database
Mysql and postgressql compatible RDS
Cost effective
5 times faster than mysql, 3 times faster compare to postgreSQL
Start with 10gb up to 64TB
2 copies of your data, with a minimum 3 availability zones, in total you will always have 6 copies, can have 15 replicas max
Self healing
Can share snapshot to other was account
Serverless
Automatically start, stop, and scale up or down
Good for infrequent, intermittent, or unpredictable work
3 types of replicas avail, Aurora replicas, mySQL replicas, postgresQL replicas
Automated failover only avail in Aurora replicas
Automated backup turned on by default

AWS database migration service (DMS)

A way to migrate you data into the AWS cloud.
Support homogenous and heterogenous migration


Caching Strategies

Cloudfront - media, image, videos at edge location
Api gateway
Elasticache - memcached and rediss
dynamoDB accelerator (DAX)


You use redis when you need multi AZ, you can do backup and restore

You use memcached if you need to scale horizontally



Amazon EMR (Elastic mapReduce)

Big data platform 
Consist of master node, core node and optionally task node
By default log data is stored in master node
You can replicate to s3 on 5 minutes interval for all log from master log, can only be configured on cluster creation


AWS Directory Service

family of managed services
Connect was resources with on premise AD
Use existing corporate credentials
Can be use to login to was console
SSO to any domain joined EC2


Types:

Microsoft AD
Simple AD


Active Directory:

Based on LDAP and DNS
Support kerberos, LDAP, and NTLM
Highly available


AD compatible service

Managed Microsoft AD, ad connector, simple AD



Not AD compatible service

Cloud directory, Cognition user pools

Microsoft AD



AWS managed

multi AZ deployment
Patch, monitor, recover
Instance rotation
Snapshot and restore


Customer

user, groups, and group policy objects
Standard AD tools
Scale out Domain Controller
Trusts (resource forest) 
Certificate authorities (LDAP)
Federation
Simple AD

stand alone managed directory]
Basic ad features
type: small <= 500, large <= 5k users
Easier to manage EC2
Linux workloads that need LDAP
Does not support trust (cant join on premises AD)


AD Connector

Directory gateway (proxy) for on premise AD
Allow on premises user to login to AWS using AD


IAM Policies

Amazon Resource Name (ARN)

ARNs begins with:

arn:partion:service:region:account_id



ARNs end with:”

Resource

resource_type/resource

Resource_type/resource/qualifier

Resource_type/resource:qualifier

Resource_type:resource

Resource_type:resource:qualifier



Route53

DNS is on route 53, that’s where route53 comes from
DNS - domain name system
ELBs do not have predefined IPV4 addresses, you resolve them using a dns name
Alias record vs CNAMES, always choose alias record (A for IPv4, AAAA for IPv6)

Domain Name

You can buy from AWS
Can take up 3 days to register

Routing Policies

Simple routing
Weighted routing
Latency-based routing
Failover routing
Geolocation routing
Geoproximity routing (traffic flow only)
Multivalue answer routing
Simple Routing

you can have one record for multiple ip address. If you have multiple values, route53 will return all the values to user in random order
Can’t have health check

Weighted Routing

Split your traffic to different region based on weight assign. i.e. 20% to us-east-1 and 80% to us-west-1
You can set health check on individual records set, if it fails, it will be removed from route53 until it becomes healthy again
You can set SNS notification to send you a notification when health check fail


Latency-based Routing

Allow you to route your traffic based on the lowest rank latency
You create a latency resource record set, route53 will look at the lowest record set and direct user

Failover Routing

when you want to create active/passive setup, route53 will monitor your primary site health check, when it fails it will failover to the passive site


Geolocation Routing

lets you choose where your traffic will be sent based on the geographic location of your users


Geoproximity Routing 

route traffic based on geographic location of your user and also your resources.
You can also choose to route more or less traffic to a resource by specifying a value, known as bias.
Must use route 53 traffic flow


Multivalue Answer Routing

Specify multi values to any record, but allows you to put health check on each record set to ensure route53 only returns a healthy one


VPC (Virtual Private Cloud)

Logical data center in AWS
Consist of Internet gateways or virtual private gateways, route tables, NACL, subnet, security group
subnet, one subnet = one availability zone
Assign custom ip
Configure route tables
Create internet gateway, you can only have 1 internet gateway per VPC
Security
Instance security groups - stateful
NACL - stateless
No transitive peering
US-east-1A in your AWS account can be completely different US-East-1A in another AWS account, AWS randomised




VPC peering - allow you to connect one vpc to another, you can’t have transitive peering, each vpc need to peer to another, in order to communicate



Amazon reserved 5 ip addresses

10.0.0.0 - network address
10.0.0.1 - vpc router
10.0.0.2 - Reserved
10.0.0.3 - for future use
10.0.0.255 - b


When you create a VPC, a default route table, NACL, and a default security group are created.



It will not create subnet nor default internet gateway



VPC - add route table to enable traffic between instances

VPC Endpoint - Private subnet communication without making it public 



Create nat gateway in the public subnet


NAT Instances and NAT Gateway



NAT - Network address Translation


NAT Instance 

Single EC2 Instance
can be a bottle neck (increase the site of nat instance to resolve)
you need to disable source/destination check on the instance,
Must be a route out of the private subnet to the nat instance
it must be in a public subnet
need to patch


NAT Gateway 

allow your private subnet to communicate to the internet without making it public
its highly available
redundant availability zone
preferred by enterprise
no need to patch
automatic public ip
no need to disable source/destination check
Remember to update your route table
5Gbps - 45 Gbps




Network access Control list (NACL)



VPC automatically comes with NACL, and by default allows all outbound and inbound traffic
You can create custom NACL, by default each custom NACL denies all inbound and outbound traffic
Each subnet must be associated with a NACL, if not then by default it will be associated to the default NACL
Block IP addresses using NACL,  you cannot do this with security group
You can associate NACL with multiple subnet, however subnet can only be associated with one NACL at a time.
NACL has separate inbound and outbound rules
NACL contains numbered list of rules, it is evaluated in order starting with the lowest numbered rules
Denied alway win vs allow
It is stateless


Custom VPCs and ELBs



Type of ELBS

Classic ELB
Network ELB
Application ELB


You need a minimum 2 public subnets to deploy an internet ELB



VPC Flow logs



you cannot enable flow logs for vpc that are peered with your vpc unless the peer vpc is your account
You can tag flow logs
You cannot change configuration after you created flow logs i.e. can’t associate a different IAM role
Not all traffic is monitored; 
Traffic generated by instances when they contact amazon dns, if you use your own dns traffic is logged
Traffic generated by windows instance for amazon window license activation
Traffic to and from 169.254.169.254
DHCP traffic
Traffic to reserved IP addresses for default VPC router


Bastions

Jump box, used to securely administer ec2 instances (using SSH or RDP)
nat gateway or nat instance is used to provide internet traffic to ec2 instances in private subnets
You cannot use a NAT gateway as a bastion host


Direct Connect

dedicated private connection between AWS and your premise
Useful for high throughput workloads
Secure and reliable connection


How to create

create a virtual interface in the direct connect console, this is public virtual interface
Go to VPN console and then to VPN connections, create a customer gateway
Create a virtual private gateway
Attach the virtual private gateway to the desired VPC
Select vpn connections and create new vpn connection
Select virtual private gateway and the customer gateway
Once vpn is available, setup the vpn on the customer gateway or firewall






Global Accelerator

improve availability and performance of your application for local and global users
Assigned 2 ip adresses
You can control traffic using traffic dials
You can control weighting to individual endpoints using weights 


VPC Endpoint

enable private connection to your VPN to supported AWS services and VPC endpoint services powered by private link without requiring internet gateway, etc


There are 2 types

Interface endpoints
Gateway endpoints; s3 and dynamoDB


AWS PrivateLink

Peering to multiple VPC, doesn’t require VPC Peering



Transit Gateway

Allow transitive peering between VPC

Works on hub and spoke model

Works on regional but you can have it across multi regions

You can use it across multiple AWS accounts using RAM (Resource Access Manager)

Works with direct connect and VPN

Support IP Multicast (not supported by any other AWS Services)

AWS VPN CloudHub

Multiple Site with multiple VPN connection use cloudHUB

Operates over public internet, but traffic is encrypted



AWS Network Cost

use private ip address over public
Group your ec2 instances in the same availability zone and use private address, this will be cost free. However you will have a  single failure issue


Elastic Load Balancer

Class and application load balancer doesn’t have IP, only DNS



Elastic Load Balancing provides access logs that capture detailed information about requests sent to your load balancer. Each log contains information such as the time the request was received, the client's IP address, latencies, request paths, and server responses. You can use these access logs to analyze traffic patterns and troubleshoot issues.

Access logging is an optional feature of Elastic Load Balancing that is disabled by default. After you enable access logging for your load balancer, Elastic Load Balancing captures the logs and stores them in the Amazon S3 bucket that you specify as compressed files. You can disable access logging at any time.



Type of ELBS

Classic ELB
Network ELB (layer 4)
Application ELB (layer 7)

Application ELB

operates on layer 7 (http/https)
application aware


Network ELB

operates on layer 4 (TCP)
Static IP address
Extreme performance




Classic ELB

can use layer 7, but not application aware, to get ip address use the X-Forwaded-For header
Can use layer 4, for application that rely purely on the TCP
Cost under control


504 means gateway timeout



Advanced load balance

Classic load balance routes each request independently to the registered EC2 instance with the smallest load.



Sticky session,  allow you to bind a user’s session to a specific EC instance, to ensure all requests from the user for that session is sent to the same instance



You can enable stick session for Application load balancer as well, but the traffic will be sent at the target group level.



Cross zone load balancing, sending traffic across different availability zone



Path patterns, a listener with rules to forward request based on the url path to different EC2 instance



Auto Scaling



It has 3 components

Group; logical component i.e. web server group, application group, database group
Configuration templates; launch template
Scaling options; ways to scale your groups


Scaling options:

maintain current instance levels at all time, if there is an unhealthy instance it will terminate and launch a new one
Scale manually, specify max, min or desired capacity
Scale based on schedule
Scale based on demand
Use predictive scaling




HA Architecture

plan for failure
Use multiple az and multiple region when you can
Know the difference between multi-az and read replaces for ads
Know the difference between calling out and scaling up
Cost element
Different s3 storage classes


HA with bastion

Scenario 1 (normally being used for production): 2 ec2 instances, two availability zones bastion host, network load balancer 
Scenario 2: 1 ec2 instance, 2 availability zone bastion host, auto scaling group
You can use application load balancer (application load balancer is layer 7), you need to use layer 4, so you can only use network load balancer




On Premises Services with AWS

High level services you can use on-premises

Database migration service (DMS)
Server migration service (SMS)
AWS application discovery services
VM import/export (hyperV or vmware)
Download amazon linux2 as ISO


DMS

allow you to migrate database to/from AWS
Might have your DR in AWS and your primary on premise
Support homogenous and heterogenous migration


SMS

incremental replication of your on premise servers in to AWS
Can be use as backup tool, multi site strategy


AWS application discovery service

it will build server utilisation map and dependency map of your on premise environment
The data collected can be use to estimate total cost of running your premise in AWS and plan your migration


VM Import/export

Migrate existing application to ec2
Export AWS instance to on premise




.504 error means gateway has timed out.



Load balances have their own DNS name, you are never given an IP



Instance monitored by ELB are reported as InService or OutofService



If you need the ipv4 address of your end user, look for the x-forwarded-for header



Cloud formation - completely scripting your cloud environment, support version control, 2 types of update, directly update or change sets (if you need to preview)



Elastic beanstalk - quickly deploy and manage applications in AWS, without worrying about the capacity provisioning, load balancing, scaling, and application health monitoring (Aim for dev who just want to get their stuffs to the cloud quickly)





SQS (Simple queue System)

use to decouple your infrastructure
Message can contain up to 256kb, of text in any format
Any component can later retrieve the message programmatically using the amazon SQS API
It is pull based not pushed based, you need to have an ec2 instance to pull the queue
Messages can be kept in queue from 1 min to 14 days, default retention period is 4 days
It guarantees that your message will be processed at least once
SQS long polling is a way to retrieve message from your amazon SQS. While the regular short polling returns immediately , the long polling doesn’t return a response until a message arrives in the message queue, or the long poll times out
Anytime you see scenario about “decoupling” your infrastructure, think SQS




Types of queue;

standard queues; unlimited number of transaction per second, there might be more than one copy of the message (duplicate), and its might be delivered our of order
Fifo queues (first in first out); will do exactly once processing (no duplicate), limited to 300 transactions per second


Visibility timeout - the amount of time that the message is invisible in the SQS queue after a read picks up that message. Provided that the job is processed before visibility queue expires, the message will be deleted, other wise the  message will be come visible again and another read will process it. This could result the message to be delivered twice. 



Visibility timeout maximum is 12 hours



FIFO can’t be used as s3 event notification destination





Simple WorkFlow (SWF) Service

use cases vs SQS
Coordinate work across distributed application components
There is a human element to it


SWF VS SQS

- SQS has a retention period for up to 14 days; SWF workflow execution can last up to 1 year

SWF pressent task-oriented API, SQS offers message oriented API
SWF ensure that the task is only executed once and no duplicated, SQS ensure that the task is executed at least once, you need to handle duplication manually
SWF keep track all the tasks and event in an application, SQS you need to implement your own application level tracking, especially if your application uses multi queues


SWF actors

Workflow starters - an application that can initiate a workflow
Decider - control the flow
Activity workers -carry out the tasks
Simple notification Service (SNS)

provides developer with a highly scalable, flexible, and cost-effective capability to publish messages from an application and immediately deliver them to subscribers or other applications
Allows you to do push notifications, and also allows you to push SMS, or any http endpoint

SNS VS SQS

both are messaging services
SNS - push
SQS - pools (pull)

Elastic Transcoder

media transcoder in the cloud
Convert media files to different format that will play on smart phones, tablet, PCs, etc
Pay based on the minute that you transcode


API Gateway

fully managed service that makes it easy for developer to publish, maintain, monitor, and secure APIS at any scale


Has caching capabilities to increase performance
Low cost and scales automatically
You can throttle api gateway to prevent attacks
You can log results to CloudWatch
If you are using Javascript or Ajax, that uses multiple domains with API gateway, you need to enable CORS on API gateway
CORS is enforced by the client


Kinesis

Streaming data that is generated continuously by thousands of data sources. i.e. purchases from online stores, stock prices, game data, social network data


types:

kinesis streams - consist of shards; 5 transactions/s for reads, up to a maximum total data read rate of 2MB/s and up to 1000 records/s, up to maximum total data write rate of 1 MB/s, data persistent, persist data up to 24hours
Kinesis firehose, analysing on the fly (real-time), you need to find a place to store it i.e. s3, redshift, elastic search, splunk


Kinesis analytics, if you want to analyse the data in both kineses stream and kinesis firehouse




Web identity Federation and Cognito

Web identity federation - let user access to AWS resources after they have successfully authenticated with a web-based identity provider like Amazon, facebook or google.



Amazon cognito provides web identity federation with the following feature

sign up and sign in to your apps
Access for guest users
Acts as an indemnity broker between your application and web ID providers
Sync user data fro multiple devices, Uses SNS to send notification whenever the user data changes
Recommended for all mobile application AWS services
User authenticate first with the Web ID provider, and received an authentication token


User pools - use to managed sign up and sign in for mobile and web applications, account recovery. Successful authentication with generate a JSON web token (JWT)



Identify pools - provide temporary was credentials to acmes was services like S3 or dynamoDB (authorise access to your AWS resources)





Even Processing Patterns



- Event driven architecture - Publish/Subscribe messaging - decoupled system running in response to event



Dead-letter queue (DLQ) - when all retry and error handling fails


SNS - message published to a topic that fail to deliver are sent to SQS queue, held for further analysis
SQS - message sent to SQS that exceed the queues maxReceiveCount are sent to a DLQ (another message queue)
Lambda - result from failed asynchronous invocation; will retry twice and send to either an SQS queue or SNS topic


- Fan out pattern - instead of talking directly to SNS queue, the published talk to an SNS topic, which then send queue to the SNS



- S3 event notifications

Object created
Object removed
Object restored
RRS object lost
Replication




Reducing Security Threats





Key Management Service (KMS)

Regional secure key management (encrypt and decrypt)
Manages customer master key (CMKs)
Ideal for s3 objects, database password, api key
Encrypt and decrepit data up to 4kb size
Pay per API call
Audit capability using cloudtrail
FISP 140-2 Level 2
Regional service, it won’t work in other region, you need to decrypt, move to other region and then encrypt it in that region
Multi tenant


Types of CMK

Customer managed; can view, managed and dedicated to your account
AWS Managed, can view, dedicated for your account, can’t managed
AWS owned, can managed, can’t view, is not dedicated for your account


Types of encryption:

Symmetric
same key for both encrypt and decrypt 
AES 256
Never leaves was unencrypted
Must call the KMS API to use
AWS services integrated with KMS uses symmetric CMKS
Encrypt decrypt and re-encrypt data
Generate data key, key pairs and random byte string
Import your own key material
Asymmetric
Public and private key pair
RSA and elliptic curve cryptography(ECC)
Must call KMS api using the private key
Download the public key and use outside AWS
AWS service integrated with KMS does not support asymmetric CMKS
Sign messages and verify signatures

Cloud HSM (Dedicated Hardware security Module)

FIPS 140-2 Level 3
Manage your own key
Single tenant
No access to was managed component
Regulatory compliance requirements


System Manager Parameter Store

Allow to save different environment configs



Secret Manager

Similar to system manager parameter store

No charge, parameter store change

Automatically rotate secrets

Apply new key/password in RDS for you

Generate random secrets



AWS shield

DDOS attack



AWS shield standard

No cost

Protect against layer 3 and 4 (SYN/UDP floods, reflection attacks)

DDOS



AWS shield advanced

3K/month per org

24/7 access to dos response team Avail for business and enterprise account







AWS-Xray is to debug your serverless 





Fx for lustre automatically backup to s3





Lambda - SNS: Async

Lambda - SQS: Sync



Cloudtrail event: data event and management event



VPC one region - transit gateway

VPC multi region - virtual private gateway



Default security group - allow outbound deny inbound

Default NACL - allow both inbound and outbound

Custom NACL - deny outbound deny inbound



If you see SAML 2.0 pick SSO



Cloudwatch and cloud trail logs are encrypted by default 
