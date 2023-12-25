# Amazon Elastic Block Store Overview

EBS is a service that provides block level storage volumes for use with EC2 instances.

EBS volumes behave like raw, unformatted block device in the cloud.
We can mount EBS volumes as a device on our EC2 instances.

Each EBS volume can be attached to only one instance at a time, but multiple volumes can be attached to a single instance.

EBS volumes are designed for both throughput and transaction intensive workloads at any scale.

All the data on the attached EBS volume remains avaliable even after we stop or terminate an EC2 instance.

EBS provides two major storage volume types.

- Solid State Drive (SSD)
- Hard Disk Drive (HDD)
SSD-backed volumes and HDD-backed volumes.

SSD-backed storage is optimized for transactional workloads, which involve frequent read/write operations with small I/O size.
Examples: boot volumes, databases.

HDD-backed storage is for throughput-intensive workloads.
HDD-backed vlumes include throughput-optimized HDD (st1) for frequently accessed, throughput-intensive workloads 
sc11

--------------------------------------------

EBS features


EBS Elastic Volumes

Elastic volumes allows you to dynamically increase capacity, tune performance, and change volume types with no downtime or performance impact.

Simply create a volume with the capacity and performance needed today, knowiing you have the ability to modify your volume configuration in the future.

By using Amazon CloudWatch with AWS Lambda, we can automate volume changes to meet the changing needs of our applications.

--------------------------------------------

Amazon EBS-Optimized Instances

We can launch certain EC2 instances types as EBS-optimized instances.
EBS-Optimized Instances deliver dedicated throughput between EC2 and EBS with options between 500 Mbps and 19,000 Mbps depending on the instance type used.
The dedicated throughput minimizes contention between EBS I/O and other traffic from our EC2 instance, providing the best performance for our EBS volumes.

--------------------------------------------

EBS Snapshots

We can back up the data on our EBS volumes to S3 by taking point-in-time snapshots to enable disaster recovery.
Snapshots are incremental backups, which means that only the blocks on the device that have changed after our most recent snapshot are saved.
We can create one or multiple EBS volumes based on snapshot and resize EBS volumes.

Here is a key features for EBS snapshots:
 - Direct read access
    - we can use EBS direct API to read data off snapshots. It's makes easy to track incremental changes between two EBS snapshots, without needing to create EBS volumes and EC2 instan
 - Create EBS snapshots from any block storage
    - we can create EBS snapshots from any block storage, regardless of where it resides,  including data on premises. In addition, after a volume is created from EBS snapshots, our attached instance can have immidiate access to the EBS volume data.
 - Fast snapshot restore (FSR)
   - If we enable fast snapshot restore (FSR) capability, EBS volumes restored from FSR-enabled snapshots instantly receive their full performance.
   - EBS snapshots can also be shared across AWS accounts and copied across AWS regions, enabling geographical expansion.

In addition when we delete a snapshot, only the data unique to that snapshot is removed.
![Screenshot 2023-12-13 at 17.48.11.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_HmD5cW%2FScreenshot%202023-12-13%20at%2017.48.11.png)

--------------------------------------------

Features
Amazon Data Lifecycle Manager (DLM)

Amazon Data Lifecycle Manager (DLM) for EBS Snapshots provides a simple, automated way to back up data stored on EBS volumes by ensuring that EBS snapshots are created and deleted on a custom schedule.
Snapshots are cleaned up regularly to keep costs under control
Simply tag your EBS volumes and start creating lifecycle policies for creation and management of backups.
We can also use CloudWatch to monitor our policies and ensure that ouw backups are being created successfully

--------------------------------------------

Features

Key benefits:

 - Performance for any workload
   - EBS volumes are performant for your most demanding workloads, including mission-critical applications, relational and non-relational databases, enterprise applications, containerized applications, big data analytics engines, file systems, and media workflows are also widely deployed on EBS.
   - We can choose from different volume types to balance optimal price and performance.
 - Highly available and durable
   - Designed for mission-critical systems, EBS volumes are replicated within an Availability Zone and offer 99.999% availability.
   - EBS offers a high-durability volume io2 for customers that need 99.999% durability, especially for their business-critical applications.
   - Since EBS volumes are for persistent data, it is important to back up the data. EBS snapshot with Amazon Lifecycle Manager, or DLM, provides a simple and robust backup solution
 - Cost-effective
   - EBS offers different volumes at various price points and performance benchmark. 

Additionaly, EBS snapshots are stored incrementally which means you are billed only for changed blocks stored and save storage cost.
![Screenshot 2023-12-13 at 18.12.05.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_et4hBz%2FScreenshot%202023-12-13%20at%2018.12.05.png)
--------------------------------------------

EBS volumes easy to create, use, encrypt, and protect.

Elastic volumes is a feature that allows you to easily adapt your storage volumes as the needs of your application change.
We can also create backups of our volume using EBS snapshot and easyly automate snapshot management using DLM without any additional overhead or cost.

Virtually unlimited scale

We can build applications that require as little as a single gb storage or scale up to pb of data.
Snapshots can be used to quickly restore new volumes, enabling rapid scale.
![Screenshot 2023-12-13 at 18.21.03.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_lr1hdB%2FScreenshot%202023-12-13%20at%2018.21.03.png)

--------------------------------------------
Secure

Access to EBS volumes is integrated with AWS IAM which enables access control to your EBS volumes.
Newly created EBS volume can be encripted by default with a single setting in your account, beside it they support encryption of data at rest, data in transit and all volume backups

--------------------------------------------

EBS volume types

Let's compare EBS volume types:

The general purpose SSD volume gp3 and gp2 are designed to offer single-digit millisecond latency.

GP2 is designed to deliver a consistent baseline performance of 3 IOPS per GB and can burst up yo 3000 IOPS.
It also provides a maximum of 16000 IOPS per volume and up to 250 MB/s of maximum throughput per volume.

GP3 enables you to provision performance independent of storage.
It delivers a baseline performance of 3000 IOPS and can scale up 16000 IOPS and throughput up to 1000 MB/s which is four times more throughput than gp2.

gp3 and gp2 have the same volume size of one gigabyte to 16 terabytes.

Next let's look at the provisioned IOPS SSD volumes io2 and io1.
They are designed to deliver up to maximum of 64000 IOPS, four times more than gp3 and 1000 MB/s of throughput per volume, along with single-digit millisecond latency.
They have same volume size of four gigabytes to 16 terabytes.

However, io2 is designed to provide 100 times more durability of 99.999% compared to that of io1 and all other volume types.

Customers that need sub-millisecond latency or need to go beyond the current single-volume peak performance and throughput can use io2 Block Express which is designed to deliver four times higher IOPS, throughput, and capacity than io2 volumes along with a high durability as io2.

## Let's look at the throughput optimized HDD volumes.

st1 can burst up to 250 MB/s per TB with a baseline throughput of 40 MB/s per TB. 
It offers maximum troughput of 500 MB/s and up to 500 IOPS per volume.

Simmlar to st1, cold HDD sc1 provides a burst model.
These volumes can burst up to 80 MB/s per TB with a baseline throughput of 12 MB/s per TB, and a maximum throughput of 250 MB/s for volume. The maximum IOPS per volume for sc1 is 250 IOPS.

Both st1 and sc1 have the same volume size of 500 gigabytes to 16 terabytes.

