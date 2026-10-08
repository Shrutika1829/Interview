
# Project Title

A brief description of what this project does and who it's for

Absolutely. For a **1-year DevOps Engineer interview**, if I were the interviewer, I would not spend much time on obscure EBS/EFS features. I would test whether you can **choose the right storage, configure it on EC2, troubleshoot it, and explain how you used it in a project**.

Below is the **high-priority EBS + EFS question bank** I would expect you to prepare.

---

# AWS EBS & EFS — Top DevOps Interview Questions & Answers

## 🔥 Priority Guide

| Priority        | Topics                                                                         |
| --------------- | ------------------------------------------------------------------------------ |
| 🔴 Must Know    | EBS vs EFS, EBS types, snapshots, mounting, resizing, EFS mounting, EFS vs EBS |
| 🔴 Must Know    | EBS troubleshooting, EFS troubleshooting, AZ limitations, security             |
| 🟠 Important    | Performance, encryption, backup, Auto Scaling integration                      |
| 🟡 Good to Know | Multi-Attach, EFS throughput modes, lifecycle management                       |

---

# PART 1 — EBS

## 1. What is Amazon EBS?

### Interview Answer

> **Amazon EBS (Elastic Block Store)** is a persistent block storage service designed primarily for use with EC2 instances. It behaves like a virtual hard disk attached to an EC2 instance.
>
> EBS is commonly used for operating system disks, application data, and database workloads where low-latency block storage is required.
>
> The important point is that EBS data persists independently of the EC2 instance lifecycle, depending on the volume's delete-on-termination setting.

### Follow-up

**Interviewer:** Is EBS temporary or persistent?

**Answer:**

> EBS is persistent storage, unlike EC2 instance store, which is ephemeral.

---

# 2. EBS vs EFS — What is the difference?

🔥 **VERY IMPORTANT**

### Interview Answer

> EBS is **block storage**, whereas EFS is **managed shared file storage**.
>
> EBS is normally attached to an individual EC2 instance and is suitable for operating systems, applications, and databases.
>
> EFS uses the NFS protocol and allows multiple EC2 instances to access the same filesystem simultaneously, including instances across Availability Zones.
>
> So, if I need storage for a single EC2 or database, I would generally choose EBS. If multiple EC2 instances need shared files, I would choose EFS.

### Quick Comparison

| EBS                           | EFS                                      |
| ----------------------------- | ---------------------------------------- |
| Block storage                 | File storage                             |
| EC2-oriented                  | Shared across multiple compute resources |
| Usually tied to one AZ        | Regional service                         |
| Low-latency workloads         | Shared file workloads                    |
| OS/database/application disks | Shared uploads/files                     |
| `ext4`, `xfs`, etc.           | NFS                                      |

---

# 3. What are the different EBS volume types?

### Interview Answer

The important EBS volume types are:

### General Purpose SSD

* `gp3`
* `gp2`

### Provisioned IOPS SSD

* `io2`
* `io1`

### HDD

* `st1` — Throughput Optimized HDD
* `sc1` — Cold HDD

For most general-purpose workloads, I would prefer **gp3** because it allows me to provision IOPS and throughput independently of volume size.

For demanding database workloads requiring very high and consistent IOPS, I would evaluate **io2**.

---

# 4. Why would you choose gp3 over gp2?

### Interview Answer

> I would generally choose gp3 for new general-purpose workloads because it provides a predictable baseline of performance and allows IOPS and throughput to be configured independently of storage capacity.
>
> With gp2, performance is tied more closely to volume size.
>
> Therefore, gp3 gives better flexibility for performance and cost optimization.

### Interviewer Follow-up

**When would you use io2?**

> For workloads such as critical databases that require high and consistent IOPS and high durability.

---

# 5. How do you attach and mount an EBS volume to EC2?

🔥 **VERY COMMON**

### Interview Answer

I would follow these steps:

### Step 1 — Create EBS volume

Create the volume in the **same Availability Zone** as the EC2 instance.

### Step 2 — Attach it

Attach the volume to EC2.

### Step 3 — Check the device

```bash
lsblk
```

### Step 4 — Create filesystem

For a new volume:

```bash
sudo mkfs.ext4 /dev/xvdf
```

The actual device name should be verified using `lsblk`; don't blindly assume `/dev/xvdf`.

### Step 5 — Create mount point

```bash
sudo mkdir /data
```

### Step 6 — Mount

```bash
sudo mount /dev/xvdf /data
```

### Step 7 — Verify

```bash
df -h
```

### Step 8 — Persistent mount

Add the filesystem to `/etc/fstab`, preferably using its UUID.

---

# 6. Why do you need to format an EBS volume?

### Interview Answer

> A newly created EBS volume is raw block storage. Before storing files on it, I need to create a filesystem such as ext4 or XFS.
>
> The filesystem provides the structure required by Linux to organize, read, and write files.

---

# 7. What is the difference between attaching and mounting an EBS volume?

🔥 **Good interviewer trap**

### Interview Answer

> **Attaching** connects the EBS volume to the EC2 instance at the AWS infrastructure level.
>
> **Mounting** makes the filesystem on that volume accessible through a directory in the Linux filesystem.
>
> So attaching happens in AWS, while mounting is an operating-system operation.

Example:

```text
AWS

EBS
 ↓
Attach
 ↓
EC2
 ↓
Linux device
 ↓
Mount
 ↓
/data
```

---

# 8. What happens to EBS when an EC2 instance is terminated?

### Interview Answer

> It depends on the volume's **DeleteOnTermination** setting.
>
> For the root volume, the default is commonly to delete it when the instance is terminated, while additional EBS volumes may be configured to persist.
>
> Therefore, before terminating an instance, I would verify the DeleteOnTermination setting and ensure important data is backed up.

### Interviewer Follow-up

**What happens when EC2 is stopped?**

> EBS volumes normally remain attached and retain their data when the instance is stopped.

---

# 9. What is an EBS snapshot?

### Interview Answer

> An EBS snapshot is a point-in-time backup of an EBS volume.
>
> EBS snapshots are stored in AWS-managed infrastructure backed by Amazon S3, and subsequent snapshots are incremental at the block level.
>
> I can use a snapshot to create a new EBS volume, restore data, or support disaster recovery.

---

# 10. How would you recover deleted data from an EBS volume?

### STAR-style Answer

**Situation:**
Suppose important application data was accidentally deleted.

**Task:**
I need to recover the data without disturbing the production volume.

**Action:**
I would use the latest suitable EBS snapshot to create a new volume, attach it to a recovery EC2 instance, mount it, verify the required files, and restore only the necessary data.

**Result:**
The deleted data can be recovered while minimizing changes to the production environment.

---

# 11. Can you increase the size of an EBS volume without replacing it?

🔥 **Very important**

### Interview Answer

> Yes. EBS volumes can generally be modified to increase their size without replacing the volume.
>
> However, increasing the AWS volume size is only the first step. I also need to extend the partition, if applicable, and then grow the filesystem inside Linux.

For example:

```bash
lsblk
```

Then for ext4:

```bash
sudo resize2fs /dev/...
```

For XFS:

```bash
sudo xfs_growfs /mount-point
```

Finally:

```bash
df -h
```

---

# 12. Your EBS volume is 100% full. What will you do?

### STAR-style Answer

**Situation:**
The application server's filesystem has reached 100% utilization.

**Task:**
I need to restore available disk space without unnecessarily rebuilding the server.

**Action:**

First:

```bash
df -h
```

Then identify large directories:

```bash
sudo du -xhd1 / | sort -h
```

I would check:

* Application logs
* Temporary files
* Deleted-but-open files
* Core dumps
* Application data

If additional capacity is required, I would increase the EBS volume and extend the filesystem.

**Result:**
The application gets sufficient storage again, and I would implement log rotation/monitoring to prevent recurrence.

---

# 13. Can an EBS volume be attached to multiple EC2 instances?

### Interview Answer

> Normally, an EBS volume is designed to be attached to one EC2 instance at a time.
>
> However, certain Provisioned IOPS SSD volumes support **EBS Multi-Attach**, allowing a supported volume to be attached to multiple instances in the same Availability Zone.
>
> This is intended for specific clustered applications and requires an appropriate cluster-aware filesystem/application design.

### Important

Don't simply say:

> "EBS can never attach to multiple EC2s."

That's incomplete.

---

# 14. Can you attach an EBS volume from one AZ to an EC2 in another AZ?

### Interview Answer

> No, an EBS volume is AZ-specific and cannot be directly attached to an EC2 instance in another Availability Zone.
>
> If I need the data in another AZ, I can create a snapshot of the volume and then create a new EBS volume from that snapshot in the target AZ.

```text
EBS - AZ-A
     ↓
Snapshot
     ↓
EBS - AZ-B
```

---

# 15. EBS volume is attached, but Linux doesn't show it. What will you do?

### DPBE-style Answer

### Detect

```bash
lsblk
```

Also check:

```bash
sudo fdisk -l
```

### Possible Causes

* Volume wasn't attached correctly
* Wrong device assumption
* Device rescan issue
* OS/device mapping issue

### Best Fix

Verify attachment from AWS and compare with Linux block devices.

If it appears but has no filesystem:

```bash
sudo blkid
```

If it is a new volume, create the filesystem.

### Explain Result

Once the correct device is identified and mounted, verify:

```bash
df -h
```

---

# PART 2 — EFS

# 16. What is Amazon EFS?

### Interview Answer

> Amazon EFS is a fully managed, elastic file storage service for Linux workloads.
>
> It provides shared file storage using the NFS protocol, allowing multiple EC2 instances to access the same filesystem simultaneously.
>
> It is particularly useful for workloads where multiple servers need common files, such as shared application uploads.

---

# 17. Why would you use EFS instead of EBS?

🔥 **Very important**

### Interview Answer

> I would use EFS when multiple EC2 instances need simultaneous access to the same files.
>
> For example, if I have an Auto Scaling Group with five web servers and users upload profile pictures, storing those files on each server's local EBS would create inconsistent copies.
>
> With EFS, all five servers can access the same shared filesystem.

```text
             EFS
          /   |   \
        EC2  EC2  EC2
```

---

# 18. How does EFS work with Auto Scaling?

🔥 **Very common**

### Interview Answer

> EFS is useful with Auto Scaling because EC2 instances are dynamic and can be launched or terminated at any time.
>
> Instead of storing shared application files locally on each EC2 instance, all instances mount the same EFS filesystem.
>
> When Auto Scaling launches a new instance, the instance can mount the existing EFS filesystem and immediately access the shared files.

---

# 19. What are EFS Mount Targets?

### Interview Answer

> An EFS mount target is a network endpoint in a VPC through which EC2 instances access an EFS filesystem.
>
> For a highly available architecture, I would create mount targets in the Availability Zones where my application instances run.
>
> EC2 instances then connect to EFS using NFS.

---

# 20. Why does EFS use port 2049?

### Interview Answer

> EFS uses the NFS protocol, and NFS commonly uses TCP port **2049**.
>
> Therefore, the security group associated with the EFS mount target must allow inbound TCP 2049 from the appropriate client security group.

A good security design is:

```text
EC2 SG
   ↓
TCP 2049
   ↓
EFS SG
```

rather than allowing `0.0.0.0/0`.

---

# 21. EFS is not mounting. How would you troubleshoot it?

🔥 **Extremely important**

### DPBE Answer

### Detect

First check the mount error.

```bash
mount
```

and:

```bash
dmesg | tail
```

### Possible Causes

I would check:

1. EFS mount target exists.
2. EC2 and EFS are in the correct VPC/network setup.
3. Security Group allows TCP 2049.
4. DNS resolution works.
5. Required NFS/EFS utilities are installed.
6. Route/NACL configuration.
7. IAM authorization, if EFS IAM authorization is being used.

### Best Fix

Correct the networking, security group, DNS, or client configuration based on the actual error.

### Result

Retry the mount and verify:

```bash
df -h
```

---

# 22. Can EFS be accessed from different Availability Zones?

### Interview Answer

> Yes. EFS is designed as a regional file system and can be accessed by EC2 instances across Availability Zones.
>
> I would configure mount targets in the required Availability Zones so applications can access EFS through the VPC network.

---

# 23. Is EFS highly available?

### Interview Answer

> Yes. EFS is designed for high availability and durability within an AWS Region. It stores data redundantly across multiple Availability Zones for Regional EFS.
>
> Applications can access the same filesystem from multiple AZs, which makes it useful for highly available architectures.

---

# 24. Why shouldn't you normally use EFS for a MySQL database?

🔥 **Very common follow-up**

### Interview Answer

> I would generally not choose EFS as the primary storage layer for a MySQL database because database workloads typically require low-latency, high-performance block storage.
>
> For a self-managed database on EC2, I would generally evaluate EBS, such as gp3 or io2 depending on workload requirements.
>
> If I don't need to manage the database infrastructure myself, I would use Amazon RDS instead.

---

# 25. How would you secure EFS?

### Interview Answer

I would use multiple layers:

### Network Security

Security Group:

```text
EC2-SG → EFS-SG
TCP 2049
```

### IAM

Use IAM authorization where appropriate.

### Encryption

Enable:

* Encryption at rest
* Encryption in transit using TLS when mounting with the appropriate EFS client/configuration.

### File permissions

Use Linux:

```bash
chmod
chown
```

to control access.

---

# 26. EFS performance is slow. What would you check?

### Interview Answer

I would investigate:

* EFS throughput
* I/O workload
* Performance mode
* Throughput mode
* Client/network configuration
* Application access pattern
* CloudWatch EFS metrics

I would avoid immediately changing configuration without first identifying the workload bottleneck.

---

# 27. EBS vs EFS vs S3 — Which would you choose?

🔥 **Excellent architecture question**

### Interview Answer

> It depends on the type of storage required.

| Requirement                                  | Choice  |
| -------------------------------------------- | ------- |
| EC2 OS disk                                  | **EBS** |
| Database on EC2                              | **EBS** |
| Shared filesystem                            | **EFS** |
| User uploads requiring shared file semantics | **EFS** |
| Images/static objects                        | **S3**  |
| Backups/archives                             | **S3**  |
| Logs/large objects                           | **S3**  |

### Strong closing statement

> So I don't choose storage based simply on cost. I first identify whether the application needs **block, file, or object storage**, and then select the AWS service accordingly.

---

# 28. Explain how you used EBS and EFS in your project.

🔥 **You should definitely prepare this one.**

### STAR Answer

**Situation**

> I built a highly available AWS web application using EC2 instances across multiple Availability Zones behind an Application Load Balancer.

**Task**

> I needed persistent storage for the EC2 instances and shared storage for application uploads.

**Action**

> I used EBS as the persistent block storage for the EC2 instances. For shared application files, I created an EFS filesystem with mount targets in the required Availability Zones. I configured security groups to allow NFS traffic on TCP 2049 from the EC2 security group.
>
> I mounted EFS on the EC2 instances and verified that a file created from one instance was accessible from another instance.

**Result**

> The EC2 instances had persistent block storage through EBS, while the shared application files were accessible from multiple instances through EFS. This also made the storage architecture suitable for an Auto Scaling environment.

---

# 29. Production Scenario: User Uploads Disappear

### Interviewer

> You have 3 EC2 instances behind an ALB. A user uploads an image while connected to EC2-A. Later the image isn't visible when the request goes to EC2-B. What is happening?

### ⭐ Strong Answer

> I would suspect that the application is storing uploads on local EC2 storage, such as the instance's EBS filesystem.
>
> Since each EC2 has its own independent filesystem, a file created on EC2-A won't automatically exist on EC2-B.
>
> For shared file semantics, I would use EFS so all application instances mount the same filesystem. For many modern applications, I would also consider storing user-uploaded objects directly in S3, depending on the application's requirements.

---

# 30. Production Scenario: EFS Works on One EC2 but Not Another

### Interviewer

> EFS mounts successfully on EC2-A but fails on EC2-B. What do you check?

### ⭐ DPBE Answer

### Detect

Compare both instances.

### Check:

```text
EC2-A                    EC2-B
   │                        │
   ├─ Security Group        ├─ Security Group
   ├─ Subnet                ├─ Subnet
   ├─ Route                 ├─ Route
   ├─ DNS                   ├─ DNS
   └─ NFS client            └─ NFS client
```

I would specifically verify:

* EC2-B security group
* EFS security group
* TCP 2049
* DNS
* Route/NACL
* EFS mount target in the relevant AZ
* NFS/EFS utilities

Then compare the successful and failed configurations.

---

# 🎯 The 15 Questions I Would DEFINITELY Ask You

If I were interviewing you for a **1-year DevOps Engineer position**, these are the ones I would prioritize:

### 🔴 EBS

1. **What is EBS and why do we use it?**
2. **EBS vs EFS?**
3. **Explain gp2 vs gp3.**
4. **How do you attach and mount an EBS volume?**
5. **What happens when an EC2 instance is terminated?**
6. **What is an EBS snapshot?**
7. **How do you increase an EBS volume from 50 GB to 100 GB?**
8. **EBS volume is full. How do you troubleshoot it?**
9. **Can EBS be attached across AZs?**
10. **EBS is attached but Linux doesn't show it. What do you do?**

### 🔴 EFS

11. **What is EFS and why would you use it?**
12. **Why EFS instead of EBS for shared application files?**
13. **How do you mount EFS on EC2?**
14. **EFS is not mounting. How do you troubleshoot it?**
15. **Explain EFS mount targets and port 2049.**

### 🔥 Architecture

And I would almost certainly finish with:

> **"You have an Auto Scaling Group with 4 EC2 instances behind an ALB. Users upload images. Where would you store those images—EBS, EFS, or S3? Explain your choice."**

A strong answer is:

> "It depends on the application's access pattern. If the application requires a shared POSIX/NFS filesystem, I would use EFS. If the uploaded images are independent objects and the application can use object storage, I would generally prefer S3 because it is purpose-built for object storage and scales independently of the EC2 fleet. I would use EBS for instance-local block storage rather than shared uploads."

That last answer is particularly important because it shows you're thinking like a **DevOps engineer designing an architecture**, rather than simply memorizing *EBS = this, EFS = that*.
