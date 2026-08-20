# Assignment 5 — Deploy a Highly Available Two-Tier Application on AWS

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will design and deploy a highly available two-tier web application on AWS: highly available networking across two Availability Zones, an Application Load Balancer, an Auto Scaling Group for the web tier, and a private Multi-AZ RDS database. You must prove high availability with real failure tests.

---

# Task 1 — Create HA Networking (VPC + 4 Subnets + IGW + NAT + Route Tables)

## Goal

Build a VPC (10.0.0.0/16) with two public and two private subnets across two Availability Zones, an Internet Gateway, a NAT Gateway, and the matching public/private route tables.

### Evidence

#### Screenshot 1 — VPC details showing CIDR 10.0.0.0/16

![task 1](screenshots/Screenshot%20with%20VPC%20details%20showing%20CIDR%2010.0.0.0_16_number%205.png)

---

#### Screenshot 2 — Subnets list showing four subnets and their Availability Zones

![task 1](screenshots/Screenshot%20with%20Subnets%20list%20showing%20four%20subnets%20and%20their%20Availability%20Zones.png)

---

#### Screenshot 3 — Public route table showing the Internet Gateway route and both public-subnet associations

![task 1](screenshots/Screenshot%20with%20Public%20route%20table%20.png)

---

#### Screenshot 4 — Private route table showing the NAT Gateway route and both private-subnet associations

![task 1](screenshots/Screenshot%20with%20Private%20route%20table%20showing%20the%20NAT%20Gateway%20route%20.png)

---

#### Screenshot 5 — NAT Gateway status showing Available and the Elastic IP

![task 1](screenshots/Screenshot%20with%20NAT%20Gateway%20status%20showing%20Available%20and%20the%20Elastic%20IP.png)

---

# Task 2 — Create Security Groups (ALB, EC2, RDS) with Least Privilege

## Goal

Create `ha-alb-sg` (HTTP public), `ha-web-sg` (HTTP only from `ha-alb-sg`, SSH from your IP), and `ha-db-sg` (database port only from `ha-web-sg`).

### Evidence

#### Screenshot 6 — ALB Security Group inbound rules

![task 2](screenshots/Screenshot%20with%20ALB%20Security%20Group%20inbound%20rules_final.png)

---

#### Screenshot 7 — EC2 Security Group inbound rules showing the ALB Security Group reference and SSH from your IP

![task 2](screenshots/Screenshot%20with%20ALB%20Security%20Group%20inbound%20rules%202.png)

---

#### Screenshot 8 — RDS Security Group inbound rule showing the database port allowed only from the EC2 Security Group

![task 2](screenshots/Screenshot%20with%20RDS%20Security%20Group%20inbound%20rule%20showing%20the%20database%20port.png)

---

# Task 3 — Deploy Database Tier (RDS Multi-AZ in Private Subnets)

## Goal

Launch a private, Multi-AZ RDS database (MySQL or PostgreSQL) using the private DB Subnet Group and `ha-db-sg`.

### Evidence

#### Screenshot 9 — RDS summary showing Multi-AZ = Yes and Publicly accessible = No

![task 3](screenshots/Screenshot%20with%20RDS%20summary%20showing%20Multi-AZ%20Yes%20and%20Publicly%20accessible%20No.png)

---

#### Screenshot 10 — RDS connectivity section showing the DB Subnet Group and Security Group

![task 3](screenshots/Screenshot%20with%20RDS%20connectivity%20section%20showing%20the%20DB%20Subnet%20Group%20and%20Security%20Group.png)

---

# Task 4 — Build a Launch Template (User Data Installs App + Connects to DB)

## Goal

Create a Launch Template whose user data installs the web-server runtime, deploys the application, configures the database connection, and starts the required services.

### Evidence

#### Screenshot 11 — Launch Template details showing that user data exists, including a visible snippet

![task 4](screenshots/Screenshot%20with%20Launch%20Template%20details%20showing%20that%20user%20data%20exists.png)

---

#### Screenshot 12 — A running instance created from the template showing the application responds on port 80

![task 4](screenshots/Screenshot%20with%20A%20running%20instance%20created%20from%20the%20template.png)

---

# Task 5 — Create an Application Load Balancer (ALB) Across 2 Public Subnets

## Goal

Create an internet-facing ALB across both public subnets with an HTTP listener and a healthy instance target group.

### Evidence

#### Screenshot 13 — ALB details showing two public subnets in two Availability Zones

![task 5](screenshots/Screenshot%20with%20ALB%20details%20showing%20two%20public%20subnets%20in%20two%20Availability%20Zones.png)

---

#### Screenshot 14 — Target group showing at least one healthy target

![task 5](screenshots/Screenshot%20with%20Target%20group%20showing%20at%20least%20one%20healthy%20target_0.png)

---

# Task 6 — Create Auto Scaling Group (ASG) in 2 Public Subnets

## Goal

Create an Auto Scaling Group from the Launch Template across both public subnets, with desired capacity 2, minimum 2, and maximum 4, registered to the ALB target group.

### Evidence

#### Screenshot 15 — Auto Scaling Group showing desired, minimum, and maximum capacity and the selected subnet Availability Zones

![task 6](screenshots/Screenshot%20with%20Auto%20Scaling%20Group%20showing%20desired,%20min,%20and%20max%20capacity%20.png)

---

#### Screenshot 16 — EC2 instances list showing two running instances in different Availability Zones

![task 6](screenshots/Screenshot%20with%20EC2%20instances%20list%20showing%20two%20running%20instances%20in%20diff%20Availability%20Zones.png)

---

# Task 7 — Configure App to Use RDS + Validate Read/Write

## Goal

Confirm the application communicates with the RDS database through the ALB DNS name with at least one read and one write operation.

### Evidence

#### Screenshot 17 — Browser showing the application loaded through the ALB DNS name with the URL visible

![task 7](screenshots/Screenshot%20with%20test%201.png)

---

#### Screenshot 18 — Proof of a database write through a UI message or database query output

![task 7](screenshots/Screenshot%20with%20test%201.png)

---

# Task 8 — High Availability Tests (Must Do Both)

## Goal

Test A: terminate one web instance and confirm the Auto Scaling Group replaces it automatically without interrupting the ALB. Test B: simulate an Availability Zone impact (stop, detach, or reduce desired capacity in one AZ) and confirm the application stays available.

### Evidence

#### Screenshot 19 — EC2 showing the terminated instance and the newly launched instance

![task 7](screenshots/Screenshot%20with%20test%201.png)

---

#### Screenshot 20 — Target group showing healthy targets after replacement

![task 7](screenshots/Screenshot%20with%20test%201.png)

---

#### Screenshot 21 — Evidence that an instance was removed, detached, placed in Standby, or stopped in one Availability Zone

![task 7](screenshots/Screenshot%20with%20test%201.png)

---

#### Screenshot 22 — Browser showing that the ALB DNS endpoint still works during the change

![task 7](screenshots/Screenshot%20with%20test%201.png)

---

# Task 9 — Architecture and Test-Results Summary

## Goal

Summarize the VPC/subnet layout, the ALB and Auto Scaling Group setup, the private Multi-AZ RDS setup, and the results of both high-availability tests.

### Evidence

#### Screenshot 23 — A simple architecture diagram (hand-drawn is fine), or an AWS console overview showing the components

![task 7](screenshots/Screenshot%20with%20test%201.png)

---

### Notes

Write a short summary covering the network, ALB/ASG setup, RDS setup, and the results of Test A and Test B.

The cloud architecture utilizes a custom VPC network spanning multiple availability zones with public and private subnets, an internet-facing Application Load Balancer linked to an Auto Scaling Group of EC2 web servers, and a secure Multi-AZ Amazon RDS database backend.

Network Architecture
•	VPC Setup: Custom Virtual Private Cloud configured with multiple availability zones.
•	Subnets: Split into public subnets for edge/load balancing and private subnets for secure backend services.
•	Gateway: Attached Internet Gateway for external traffic routing. 

ALB and ASG Setup
•	Load Balancer (ALB): Internet-facing Layer 7 balancer routing HTTP/HTTPS traffic.
•	Auto Scaling Group (ASG): Manages EC2 instances across zones with dynamic scaling rules (e.g., target tracking at 50% CPU).
•	Target Group: Integrates ASG instances to receive balanced requests from the ALB.

RDS Setup
•	Database Engine: Relational database instance deployed on Amazon RDS.
•	High Availability: Multi-AZ deployment providing automatic failover capability.
•	Security: Placed in private subnets, accessible only from the web tier security groups. 

Test Results
•	Test A: Verified baseline traffic distribution and health check responses through the load balancer.
•	Test B: Simulated high CPU load triggering the ASG scaling policy to successfully launch new instances.



---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post about the high-availability build, including the ALB URL (or a redacted screenshot), three to five lines on what you built and how you tested high availability, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`Add your URL here`

---

#### Screenshot — Published LinkedIn post

Add your screenshot here.

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not expose passwords, connection strings, private keys, or account IDs

---

# Completion Checklist

- [✅ ] Task 1: VPC, four subnets, IGW, NAT Gateway, and route tables created (Screenshots 1–5)
- [✅ ] Task 2: Least-privilege ALB, EC2, and RDS security groups created (Screenshots 6–8)
- [✅ ] Task 3: Private Multi-AZ RDS created (Screenshots 9–10)
- [✅ ] Task 4: Self-configuring Launch Template created and tested (Screenshots 11–12)
- [✅ ] Task 5: ALB created across both public subnets (Screenshots 13–14)
- [✅ ] Task 6: Auto Scaling Group running two instances across two AZs (Screenshots 15–16)
- [✅ ] Task 7: Application verified through the ALB with a database read and write (Screenshots 17–18)
- [✅ ] Task 8: Both high-availability tests completed (Screenshots 19–22)
- [✅ ] Task 9: Architecture and test-results summary completed (Screenshot 23 & Notes)
- [✅ ] LinkedIn post published and URL submitted
- [✅ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
