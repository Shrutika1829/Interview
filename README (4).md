
# Project Title

A brief description of what this project does and who it's for

Absolutely. Since you have **only 1 week**, I’d focus on the EC2 questions that are most useful for a **DevOps Engineer interview with ~1 year of DevOps experience**—not every EC2 feature.

Below is a **GitHub-ready `README.md` format**, with answers designed to sound natural in an interview. I’ve also marked the highest-priority questions so you know what to revise first.

# AWS EC2 – Top 20 DevOps Interview Questions & Answers

> **Target Role:** DevOps Engineer
> **Experience Level:** ~1 Year DevOps Experience
> **Preparation Time:** 1 Week
> **Focus:** Frequently Asked + Scenario-Based EC2 Questions

---

# 📌 How to Use This Guide

Since the interview preparation time is only **1 week**, don't try to memorize every AWS EC2 feature.

For each question:

1. Understand the concept.
2. Remember the key points.
3. Practice answering it aloud.
4. Add a real-world example wherever possible.
5. For scenario questions, explain **problem → investigation → solution → prevention**.

### Interview Answering Techniques Used

| Technique                              | When to Use                         |
| -------------------------------------- | ----------------------------------- |
| **Direct Answer**                      | Simple conceptual questions         |
| **PREP**                               | Technical explanation/comparison    |
| **STAR**                               | Experience/scenario-based questions |
| **Problem → Investigation → Solution** | Troubleshooting questions           |

---

# ⭐ TOP 20 EC2 INTERVIEW QUESTIONS

## Priority Guide

### 🔴 Must Know

Questions: **1, 2, 3, 4, 5, 6, 7, 8, 9, 10**

### 🟠 Very Important

Questions: **11, 12, 13, 14, 15, 16**

### 🟡 Good to Know

Questions: **17, 18, 19, 20**

---

# 1. What is Amazon EC2?

### ⭐ Must Know

### Interview Answer — Direct Answer

> **Amazon EC2, or Elastic Compute Cloud, is a service provided by AWS that allows us to create and manage virtual servers in the cloud.**
>
> We can select the operating system, CPU, memory, storage, networking and security configuration according to our application requirements.
>
> As a DevOps engineer, I mainly use EC2 for deploying applications, configuring servers, installing required packages, managing storage, monitoring instances and integrating EC2 with services such as ALB, Auto Scaling and CloudWatch.

### Key Points

* EC2 = Virtual server in AWS
* Choose OS
* Choose instance type
* Configure networking
* Configure storage
* Configure security
* Start/stop/terminate instances
* Integrates with:

  * VPC
  * ALB
  * Auto Scaling
  * CloudWatch
  * IAM
  * EBS
  * EFS

---

# 2. What is an AMI?

### ⭐ Must Know

### Interview Answer — PREP

**P — Point**

> An AMI, or Amazon Machine Image, is a template used to launch EC2 instances.

**R — Reason**

> It contains the information required to create an instance, such as the operating system, installed software and configuration.

**E — Example**

> For example, if I configure an EC2 server with Nginx, Java and application dependencies, I can create an AMI from that instance.

**P — Point**

> Then I can use that AMI to launch multiple EC2 instances with the same configuration.

### Important Interview Point

**AMI is a template.**

**EC2 is the running virtual server created from that template.**

### Example

```text
AMI
 |
 +---- EC2 Instance 1
 |
 +---- EC2 Instance 2
 |
 +---- EC2 Instance 3
```

---

# 3. What is the difference between AMI and Snapshot?

### ⭐ Must Know

### Interview Answer — PREP

> An **AMI is used to launch EC2 instances**, whereas an **EBS snapshot is a backup of an EBS volume**.

| AMI                            | EBS Snapshot           |
| ------------------------------ | ---------------------- |
| Used to launch EC2 instances   | Backup of EBS volume   |
| Can contain OS + configuration | Contains volume data   |
| Can reference snapshots        | Is itself a backup     |
| Used for server templates      | Used for data recovery |

### Example

> Suppose I have an EC2 server with an application installed. If I want to create a reusable server template, I create an **AMI**.
>
> If I only want to back up the data stored on an EBS volume, I create an **EBS snapshot**.

### Easy Memory Trick

```text
AMI       → Launch Server
Snapshot  → Backup Volume
```

---

# 4. Explain EC2 Instance Types.

### ⭐ Must Know

### Interview Answer — Direct Answer

> EC2 instance types define the combination of compute resources such as CPU, memory, networking and storage available to an instance.
>
> AWS provides different instance families depending on the workload.

### Common Families

| Family                | Purpose                       |
| --------------------- | ----------------------------- |
| General Purpose       | Balanced CPU + Memory         |
| Compute Optimized     | CPU-intensive workloads       |
| Memory Optimized      | Memory-intensive applications |
| Storage Optimized     | High storage I/O workloads    |
| Accelerated Computing | GPU/ML workloads              |

### Examples

```text
t3 / t4g → General purpose
c7g      → Compute optimized
r7g      → Memory optimized
i4g      → Storage optimized
p4       → GPU/ML workloads
```

### Interview Example

> For a normal web application, I might start with a general-purpose instance. If the application becomes CPU-intensive, I would evaluate a compute-optimized instance instead.

---

# 5. What is the difference between Stop, Reboot and Terminate?

### ⭐ Must Know

### Interview Answer — PREP

> These are three different operations performed on an EC2 instance.

| Operation | Meaning                          |
| --------- | -------------------------------- |
| Reboot    | Restarts the operating system    |
| Stop      | Shuts down the instance          |
| Terminate | Permanently deletes the instance |

### Reboot

```text
Running
   ↓
Reboot
   ↓
Running
```

Useful when:

* OS needs restart
* Kernel update
* Application/service issue

### Stop

```text
Running
   ↓
Stopped
```

The instance can generally be started again later.

### Terminate

```text
Running
   ↓
Terminated
```

The instance is permanently removed.

### Interview Tip

> I would be very careful with **Terminate**, especially for production workloads, because termination is destructive and recovery depends on the available backups and configurations.

---

# 6. What is EBS?

### ⭐ Must Know

### Interview Answer — Direct Answer

> **Amazon EBS, or Elastic Block Store, provides persistent block storage for EC2 instances.**
>
> It behaves similarly to a hard disk attached to a virtual server.

### Example

```text
EC2
 |
 +---- Root EBS Volume
 |
 +---- Additional EBS Volume
```

### Common Use Cases

* OS disk
* Application data
* Database storage
* Logs
* Persistent application storage

### Important Point

> EBS volumes are persistent independently of the lifecycle of an EC2 instance in many configurations, so I can detach and reattach them when required.

---

# 7. What is the difference between EBS and Instance Store?

### ⭐ Must Know

### Interview Answer — PREP

> The main difference is **persistence**.

| EBS                                                  | Instance Store                                                              |
| ---------------------------------------------------- | --------------------------------------------------------------------------- |
| Persistent block storage                             | Temporary storage                                                           |
| Can survive instance stop                            | Data can be lost when instance is stopped/terminated depending on lifecycle |
| Can be detached/attached in supported configurations | Tied to host/instance                                                       |
| Good for persistent data                             | Good for temporary/cache data                                               |

### Example

> I would use EBS for application data that needs persistence, while instance store can be useful for temporary data such as cache or scratch space.

### Memory Trick

```text
EBS           → Persistent
Instance Store → Temporary
```

---

# 8. What is a Security Group?

### ⭐ Must Know

### Interview Answer — PREP

**Point:**

> A Security Group is a virtual firewall associated with AWS resources such as EC2 instances.

**Reason:**

> It controls inbound and outbound network traffic.

**Example:**

For a web server:

```text
Internet
   |
   | TCP 80
   ↓
Security Group
   |
   ↓
EC2
```

Example rules:

```text
HTTP   → 80
HTTPS  → 443
SSH    → 22
```

### Important Characteristics

* Stateful
* Inbound rules
* Outbound rules
* Allow rules only
* Can reference another Security Group

### Best Practice

> I avoid opening SSH port 22 to `0.0.0.0/0` in production. I prefer restricting access to trusted IPs, VPN/bastion access or using Systems Manager where appropriate.

---

# 9. Security Group vs NACL

### ⭐ Must Know

### Interview Answer — PREP

> Both Security Groups and Network ACLs control network traffic, but they operate at different levels.

| Security Group                       | NACL                                      |
| ------------------------------------ | ----------------------------------------- |
| Instance/ENI level                   | Subnet level                              |
| Stateful                             | Stateless                                 |
| Allow rules                          | Allow + Deny rules                        |
| Return traffic automatically allowed | Return traffic must be explicitly allowed |
| Rules are evaluated together         | Rules evaluated by rule number            |

### Example

```text
Internet
   |
   ↓
NACL
   |
   ↓
Subnet
   |
   ↓
Security Group
   |
   ↓
EC2
```

### Easy Memory Trick

```text
SG   → Instance level + Stateful
NACL → Subnet level + Stateless
```

---

# 10. What happens when you launch an EC2 instance?

### ⭐ Must Know

This is a very common conceptual question.

### Interview Answer — PREP

> When I launch an EC2 instance, AWS creates the virtual server based on the configuration I provide.

### Flow

```text
Select AMI
    ↓
Select Instance Type
    ↓
Configure Network/VPC/Subnet
    ↓
Configure Security Group
    ↓
Configure Storage
    ↓
Configure IAM Role
    ↓
Configure User Data
    ↓
Select Key Pair / Access Method
    ↓
Launch EC2
    ↓
Instance Starts
```

### Interview Explanation

> First I select the AMI, which determines the operating system and base configuration.
>
> Then I select the instance type based on CPU and memory requirements.
>
> Next I configure the VPC and subnet, security group, storage and IAM role.
>
> I can also provide user data for bootstrapping.
>
> Once the instance is launched, I verify its health checks, networking and application availability.

---

# 11. What is an Elastic IP?

### 🟠 Very Important

### Interview Answer — Direct Answer

> An Elastic IP is a static public IPv4 address allocated to my AWS account that I can associate with an EC2 instance.

### Why use it?

Normally, when an EC2 instance is stopped and started, its public IPv4 address can change.

With Elastic IP:

```text
Static Public IP
       |
       ↓
     EC2
```

### Example

> If an application requires a fixed public IP address, I can associate an Elastic IP with the EC2 instance.

### Important Best Practice

> I would avoid using Elastic IP unnecessarily because AWS charges for public IPv4 addresses, and in production I would generally prefer an ALB or other architecture where appropriate.

---

# 12. What is User Data in EC2?

### 🟠 Very Important

### Interview Answer — PREP

> EC2 User Data is a mechanism used to execute initialization commands when an EC2 instance starts.

### Example

```bash
#!/bin/bash

apt update -y
apt install nginx -y
systemctl enable nginx
systemctl start nginx
```

### Use Cases

* Install packages
* Configure services
* Download application code
* Configure agents
* Perform server bootstrapping

### Real-World Example

> Instead of manually installing Nginx on every server, I can put the installation commands in User Data and automatically configure the server when it launches.

---

# 13. What is an IAM Role for EC2?

### 🟠 Very Important

### Interview Answer — PREP

> An IAM role allows an EC2 instance to securely access AWS services without storing permanent AWS access keys on the server.

### Example

```text
EC2
 |
 | IAM Role
 ↓
S3
CloudWatch
SSM
Secrets Manager
```

### Example Scenario

Suppose an EC2 instance needs to upload logs to S3.

Instead of:

```text
AWS Access Key
AWS Secret Key
```

I would attach an IAM role with the required S3 permissions.

### Best Practice

> I follow the principle of least privilege and give the EC2 role only the permissions required by the application.

---

# 14. How do you connect to an EC2 Linux instance?

### 🟠 Very Important

### Common Method — SSH

```bash
ssh -i my-key.pem ec2-user@<public-ip>
```

For Ubuntu:

```bash
ssh -i my-key.pem ubuntu@<public-ip>
```

### Before connecting, I verify:

1. Instance is running
2. Public IP / reachable network path exists
3. Security Group allows TCP 22
4. Network ACL/routing is correct
5. Correct username
6. Correct private key
7. Key permissions are correct

Example:

```bash
chmod 400 my-key.pem
```

Then:

```bash
ssh -i my-key.pem ubuntu@<public-ip>
```

### Production Approach

> For production environments, I would prefer secure access methods such as AWS Systems Manager Session Manager where possible, instead of exposing SSH publicly.

---

# 15. Scenario: You cannot SSH into an EC2 instance. How will you troubleshoot?

### 🔥 Very Important Scenario Question

### Interview Answer — Problem → Investigation → Solution

> First, I would not immediately assume that the EC2 server is down. I would troubleshoot the issue layer by layer.

### Step 1 — Check Instance Status

```text
EC2 → Running?
```

Check:

* Instance state
* System status check
* Instance status check

### Step 2 — Check Network

Verify:

```text
VPC
 ↓
Subnet
 ↓
Route Table
 ↓
Internet Gateway / NAT as applicable
```

For direct public SSH access, verify the instance has appropriate public connectivity.

### Step 3 — Check Security Group

Verify:

```text
TCP 22
Source = My IP
```

Example:

```text
SSH
Port: 22
Source: <your-public-IP>/32
```

### Step 4 — Check NACL

Verify inbound and outbound traffic isn't blocked.

### Step 5 — Check SSH Service

If I have another access mechanism:

```bash
systemctl status ssh
```

or:

```bash
systemctl status sshd
```

### Step 6 — Check OS Firewall

For example:

```bash
sudo ufw status
```

### Step 7 — Check Key/User

Verify:

* Correct `.pem` file
* Correct username
* Correct IP
* Correct file permissions

### Strong Interview Ending

> So my troubleshooting approach is to move from **AWS infrastructure → networking → security → operating system → SSH configuration**, instead of randomly changing settings.

---

# 16. Scenario: EC2 instance is running, but the application is not accessible on port 80. What will you check?

### 🔥 Very Important

### Interview Answer — Problem → Investigation → Solution

I would check the following layers:

```text
Client
  ↓
Internet
  ↓
Route Table
  ↓
NACL
  ↓
Security Group
  ↓
EC2
  ↓
OS Firewall
  ↓
Application
  ↓
Port 80
```

### Step 1 — Security Group

Check:

```text
Inbound
HTTP
TCP
80
Source: Required CIDR / Load Balancer SG
```

### Step 2 — Application

On the server:

```bash
sudo systemctl status nginx
```

### Step 3 — Check Port

```bash
sudo ss -lntp
```

or:

```bash
sudo netstat -lntp
```

### Step 4 — Test Locally

```bash
curl localhost:80
```

If this works:

```text
Application is running
```

but external access fails, I investigate networking/security.

### Step 5 — Check OS Firewall

```bash
sudo ufw status
```

### Strong Interview Answer

> I would isolate whether the problem is with the **application, OS, security group or network path** instead of changing multiple configurations at once.

---

# 17. What are EC2 Status Checks?

### 🟡 Good to Know

### Interview Answer

> EC2 provides status checks to determine whether an instance and the underlying AWS infrastructure are functioning properly.

There are two main categories:

### 1. System Status Check

Checks the underlying AWS infrastructure.

Examples:

* Hardware issue
* Network issue
* Power issue

### 2. Instance Status Check

Checks the instance itself.

Examples:

* OS problem
* Networking configuration issue
* Instance-level failure

### Simple Memory Trick

```text
System Check
    ↓
AWS Infrastructure

Instance Check
    ↓
Your EC2 Instance
```

---

# 18. What is an EC2 Launch Template?

### 🟡 Good to Know

### Interview Answer — PREP

> A Launch Template is a reusable configuration that defines how EC2 instances should be launched.

It can contain:

* AMI
* Instance type
* Security Group
* IAM role
* Key pair
* User Data
* EBS configuration
* Network settings

### Example

```text
Launch Template
       |
       +---- EC2 Instance
       +---- EC2 Instance
       +---- EC2 Instance
```

### Real-World Use

> Launch Templates are commonly used with Auto Scaling Groups so that new instances are launched consistently with the same configuration.

---

# 19. What is Auto Scaling Group and how does it work with EC2?

### 🟡 Good to Know

### Interview Answer — PREP

> An Auto Scaling Group automatically maintains the desired number of EC2 instances based on the configuration and scaling policies.

Example:

```text
Minimum = 2
Desired = 2
Maximum = 5
```

If CPU utilization increases:

```text
2 EC2
  ↓
High CPU
  ↓
Auto Scaling
  ↓
3 EC2
```

If demand decreases:

```text
3 EC2
  ↓
Low CPU
  ↓
Auto Scaling
  ↓
2 EC2
```

### Common Architecture

```text
             ALB
              |
       ----------------
       |      |       |
      EC2    EC2     EC2
       \      |       /
        Auto Scaling
             |
      Launch Template
```

### Interview Tip

> I would use an Auto Scaling Group rather than manually launching individual instances when the application requires elasticity and high availability.

---

# 20. How would you troubleshoot an EC2 instance with high CPU utilization?

### 🔥 Very Important Scenario

### Interview Answer — Problem → Investigation → Solution

> First, I would confirm the CPU utilization using CloudWatch and then identify which process is consuming CPU on the instance.

### Step 1 — Check CloudWatch

Look at:

```text
CPUUtilization
```

Check:

* Current CPU
* Duration
* Pattern
* Whether it is continuous or a spike

### Step 2 — Connect to EC2

```bash
ssh ...
```

### Step 3 — Check Processes

```bash
top
```

or:

```bash
htop
```

### Step 4 — Identify Process

For example:

```text
Java process → 90% CPU
```

Then investigate:

* Application issue
* Infinite loop
* High traffic
* Background job
* Memory pressure
* Bad deployment

### Step 5 — Check Logs

```bash
journalctl
```

Application logs:

```bash
tail -f application.log
```

### Step 6 — Immediate Mitigation

Depending on the situation:

* Restart affected service
* Stop runaway process
* Scale out
* Increase instance size
* Fix application issue

### Step 7 — Long-Term Solution

> I would identify the root cause and then implement monitoring and scaling so the same issue does not repeatedly affect production.

### Strong Interview Ending

> I wouldn't simply increase the instance size and consider the issue solved. I would first identify the root cause and then decide whether scaling, optimization or application changes are required.

---

# 🎯 BONUS QUESTIONS YOU SHOULD KNOW IF THE INTERVIEWER GOES DEEPER

These aren't part of the main 20, but revise them if you have time.

## Bonus 1. What is the difference between Public and Private EC2?

### Answer

> A public EC2 instance has a network path that allows internet connectivity, typically through an Internet Gateway and appropriate routing, whereas a private EC2 instance does not have direct inbound internet connectivity.

Typical architecture:

```text
Internet
   |
   ↓
ALB / Bastion
   |
   ↓
Private EC2
```

---

# Bonus 2. How can a private EC2 instance access the internet?

> A private EC2 instance can access the internet for outbound traffic through a **NAT Gateway** in a public subnet.

```text
Private EC2
    |
    ↓
Private Route Table
    |
    ↓
NAT Gateway
    |
    ↓
Internet Gateway
    |
    ↓
Internet
```

---

# Bonus 3. What is the difference between vertical and horizontal scaling?

### Vertical Scaling

Increase instance size:

```text
t3.medium
    ↓
t3.large
```

### Horizontal Scaling

Increase number of instances:

```text
1 EC2
 ↓
3 EC2
```

### Interview Answer

> Vertical scaling increases the resources of an individual instance, whereas horizontal scaling increases the number of instances.

---

# Bonus 4. What is the difference between On-Demand, Reserved and Spot Instances?

| Type                       | Use                         |
| -------------------------- | --------------------------- |
| On-Demand                  | Flexible workloads          |
| Reserved / Savings options | Predictable long-term usage |
| Spot                       | Interruptible workloads     |

### Interview Example

> For a production workload that requires predictable capacity, I would evaluate On-Demand or a commitment-based pricing option. For fault-tolerant batch workloads, Spot Instances can reduce cost.

---

# Bonus 5. What is the difference between Public IPv4 and Elastic IP?

| Public IPv4                         | Elastic IP                             |
| ----------------------------------- | -------------------------------------- |
| Generally automatically assigned    | Allocated to your AWS account          |
| Can change after stop/start         | Static while allocated/associated      |
| Suitable for temporary connectivity | Suitable when a fixed IPv4 is required |

---

# 🧠 EC2 INTERVIEW RAPID REVISION SHEET

Before your interview, make sure you can explain these without looking at notes:

```text
EC2
 │
 ├── AMI
 │
 ├── Instance Types
 │
 ├── EBS
 │
 ├── Security Group
 │
 ├── NACL
 │
 ├── IAM Role
 │
 ├── User Data
 │
 ├── Key Pair / SSH
 │
 ├── Public IP
 │
 ├── Elastic IP
 │
 ├── Status Checks
 │
 ├── Launch Template
 │
 └── Auto Scaling
```

---

# 🔥 MOST IMPORTANT SCENARIOS TO PRACTICE

For a DevOps interview, don't only prepare definitions.

Practice explaining these scenarios aloud:

### Scenario 1

> **EC2 is running but I cannot SSH into it. What will you check?**

Remember:

```text
Instance
 ↓
Network
 ↓
Route Table
 ↓
Security Group
 ↓
NACL
 ↓
OS Firewall
 ↓
SSH Service
 ↓
Username / Key
```

---

### Scenario 2

> **Application is running on EC2 but port 80 is not accessible.**

Remember:

```text
Security Group
 ↓
NACL
 ↓
OS Firewall
 ↓
Application
 ↓
Listening Port
```

---

### Scenario 3

> **EC2 CPU utilization suddenly reaches 95%.**

Remember:

```text
CloudWatch
 ↓
SSH
 ↓
top / htop
 ↓
Identify Process
 ↓
Check Logs
 ↓
Mitigate
 ↓
Root Cause
 ↓
Prevent
```

---

### Scenario 4

> **You need to create 10 identical EC2 instances. What will you do?**

Strong answer:

```text
Configure EC2
      ↓
Create AMI
      ↓
Create Launch Template
      ↓
Create Auto Scaling Group
      ↓
Launch Instances
```

---

### Scenario 5

> **How will you securely allow EC2 to access S3?**

Strong answer:

```text
EC2
 ↓
IAM Role
 ↓
IAM Policy
 ↓
S3
```

Avoid storing:

```text
AWS Access Key
AWS Secret Key
```

on the server.

---

# 📅 7-DAY EC2 PREPARATION PLAN

Since you have only one week, I recommend this schedule.

## Day 1 — EC2 Fundamentals

Study:

* What is EC2?
* AMI
* Instance types
* Launching EC2
* Stop vs Reboot vs Terminate
* Status checks

Practice explaining each answer aloud.

---

## Day 2 — Storage

Study:

* EBS
* EBS volume types
* EBS snapshots
* AMI vs Snapshot
* Instance Store
* EBS attach/detach
* Mounting an EBS volume on Linux

### Hands-on

```text
Create EBS
 ↓
Attach to EC2
 ↓
lsblk
 ↓
Create filesystem
 ↓
Mount
 ↓
Verify
```

---

## Day 3 — Networking

Study:

* VPC
* Subnet
* Route Table
* Internet Gateway
* NAT Gateway
* Security Group
* NACL
* Public vs Private EC2

Practice:

> "Why can't I SSH into my EC2?"

---

## Day 4 — Security & Access

Study:

* IAM Role
* Security Groups
* SSH
* Key Pairs
* Session Manager
* Least Privilege

Practice:

> "How can EC2 access S3 securely?"

---

## Day 5 — Automation & Scaling

Study:

* User Data
* AMI
* Launch Template
* Auto Scaling Group
* ALB
* CloudWatch

Understand this architecture:

```text
                 Internet
                    |
                   ALB
                    |
          -------------------
          |        |        |
         EC2      EC2      EC2
          \        |        /
           Auto Scaling Group
                    |
            Launch Template
                    |
                   AMI
```

---

## Day 6 — Troubleshooting

Practice these without notes:

### 1.

> EC2 is running but SSH doesn't work.

### 2.

> Application is running but port 80 is inaccessible.

### 3.

> EC2 CPU is 95%.

### 4.

> EC2 cannot access S3.

### 5.

> EC2 has no internet access.

### 6.

> EC2 is continuously restarting.

### 7.

> EBS volume is attached but not visible inside Linux.

### 8.

> EC2 is healthy but application is down.

---

# Day 7 — Mock Interview

Don't study new topics.

Take the **20 questions** above and answer each one verbally.

### Round 1

Answer in:

```text
30–45 seconds
```

### Round 2

Answer scenario questions in:

```text
1–2 minutes
```

### Round 3

Ask yourself:

> "Why?"

For example:

**Interviewer:** What is a Security Group?

You:

> Security Group is a stateful virtual firewall...

Then interviewer:

> Why is it stateful?

You should be able to explain.

---

# ⭐ FINAL INTERVIEW PRIORITY

If you become short on time, study in this exact order:

```text
1.  EC2 fundamentals
2.  AMI
3.  Instance Types
4.  EBS
5.  AMI vs Snapshot
6.  Security Groups
7.  SG vs NACL
8.  EC2 Launch Process
9.  User Data
10. IAM Role
11. SSH Troubleshooting
12. Application Port Troubleshooting
13. Launch Template
14. Auto Scaling
15. CloudWatch / CPU Troubleshooting
16. Public vs Private EC2
17. NAT Gateway
18. Elastic IP
19. Status Checks
20. Stop vs Reboot vs Terminate
```

# 💡 One Important Interview Tip

Don't answer EC2 questions like you are reading AWS documentation.

Instead of:

> "Amazon EC2 is a web service that provides resizable compute capacity..."

Say:

> **"EC2 is basically a virtual server in AWS. As a DevOps engineer, I use it to deploy applications, configure servers, manage storage and networking, and integrate it with services such as ALB, Auto Scaling and CloudWatch."**

Then give a small practical example.

That makes your answer sound much more like a **DevOps engineer with hands-on exposure** rather than someone who has only studied the AWS documentation.

For your **one-week preparation**, I’d strongly recommend that you do **hands-on EC2 troubleshooting alongside these 20 questions**. The interviewer is very likely to move from *“What is EC2?”* to *“Your EC2 is running but SSH/application access is failing—what do you check?”* That transition is where your preparation should be strongest.
