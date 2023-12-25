Amazon elastic compute cloud (EC2)

Amazon EC2 basics

Instance
An instance is a virtual server in the cloud.

Amazon EC2 provides a wide selection of instance types to enable you to choose the CPU, memory, storage, and GPU for our applications.
Here are the instance type categories:

General purpose:</br>
General purpose instances provide a balance of compute, memory, and networking resources.

Compute optimized:</br>
Compute optimized instances are ideal for compute-intensive workloads that require high performance processors..

Memory optimized:</br>
These instances are for memory-intensive workloads that process large datasets in memory.

Storage optimized:</br>
They are desinged for workloads that require sequential read and write access to very large data sets on local storage.

Accelerated computing:</br>
These are GPU instances that use hardware accelerators.

HPC optimized:</br>
These instances are optimized for high-performance computing workloads.

In addition, each instance type includes one or more instance sizes, allowing you to scale your resources to the requirements of your target workload.
![Screenshot 2023-12-13 at 11.43.49.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_A980CH%2FScreenshot%202023-12-13%20at%2011.43.49.png)

----------------------------

Amazon Machine Image (AMI)

An AMI provides the information required to launch an instance.
It is a  template that contains a software configuration such as an operating system, an application server, and applications.

We must specify an AMI when we launch an instance.
We can launch single instance or multiple instances from a single AMI when we need multiple instances with the same configuration.
![Screenshot 2023-12-13 at 12.04.34.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_A6FK6V%2FScreenshot%202023-12-13%20at%2012.04.34.png)

![Screenshot 2023-12-13 at 12.04.56.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_yw6rFD%2FScreenshot%202023-12-13%20at%2012.04.56.png)

Amazon EC2 basics

Amazon EC2 provides you with flexible, cost-effective, and easy-to-use data storage options for your instances.

Instances can access storage from disks that are physically attached to the host computer (or from disks on a network).

This disk storage is referred as instance store, which provides temporary block-level storage for your instance.
Also we can attach Amazon Elastic Block Storage (EBS) volume to our EC2 instance for persistent block-level storage.

EBS behaves like a raw, unformatted external block device.
Also our EBS data can be backed up using EBS snapshots and stored in Amazon S3.
EC2 also uses S3 for storing Amazon Machine Images, or AMIs.

In addition you can also create a file system for use with EC2.
Amazon Elastic File System (EFS) provides simple, scalable file storage.

Applications running on EC2 can access file system at the same time.

![Screenshot 2023-12-13 at 14.33.58.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_QMmxtA%2FScreenshot%202023-12-13%20at%2014.33.58.png)

Networking

Networking defines your network access to your EC2 instances.
Amazon Virtual Private Cloud (VPC) lets us provision a logically isolated area within thw AWS cloud, where we can launch EC2 instances on virtual networks that we define.
Alternatively, our AWS account comes with a preconfigured default VPC in each region.


Launch EC2 instances

1. To launch EC2 instances, first, we need to determine whether you want to run our EC2 in a single location or multiple locations, regions, or Availability Zones.

2. Select Amazon Machine Image (AMI)

3. then determine which EC2 instance types and sizes you want.

4. We'll also need to configure network access and specify any preferred subnet on our EC2 instances.


A default route storage is used to boot the instance. 
5. We can edit the root volume or attach additional storage to our instances

6. We can also add tags to our instances to help us identify them.

7. Configure security on our EC2 instances such as configure security groups.

We can also secure your login information for your instances using key pairs (public and private keys).

![Screenshot 2023-12-13 at 15.32.39.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_T5RfNi%2FScreenshot%202023-12-13%20at%2015.32.39.png)
![Screenshot 2023-12-13 at 15.32.58.png](..%2F..%2F..%2F..%2F..%2Fvar%2Ffolders%2Fs5%2Fbzs50xyx18x7tjz559xn_8h80000gn%2FT%2FTemporaryItems%2FNSIRD_screencaptureui_mROzup%2FScreenshot%202023-12-13%20at%2015.32.58.png)

