# Assignment 6 — Capstone Assignment — Deploy Book Review App (Three-Tier Architecture) on AWS

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

This is the most important assignment of the course. You will deploy the Book Review App in a fully production-style three-tier architecture on AWS: a Next.js Web Tier behind Nginx and a public ALB, a private Node.js/Express App Tier behind an internal ALB, and a private Multi-AZ MySQL RDS database with a read replica. You are expected to design, deploy, isolate, debug, and document the result independently.

---

# Task 1 — Architecture Diagram

## Goal

Create an architecture diagram showing the custom VPC (10.0.0.0/16), the six subnets across two Availability Zones (two public Web Tier, two private App Tier, two private Database Tier), the public ALB, Web Tier EC2/Nginx, internal ALB, private App Tier EC2, private Multi-AZ RDS with its read replica, and the permitted traffic flow.

### Evidence

#### Diagram image or link

![task 1](screenshots/Screenshot%20with%20Launch%20Template%20details%20showing%20that%20user%20data%20exists.png)
---

# Task 2 — AWS Region & Services Used

## Goal

Record the AWS Region used and list every AWS service used across networking, compute, load balancing, security, and the database.

### Notes

**Region:**

US_East_2(Ohio)

---

**Services:**

Category	    AWS services
Networking	    Amazon VPC, Subnets, Internet Gateway, Route Tables
Compute	        Amazon EC2
Load Balancing	Elastic Load Balancing – Application Load Balancer (ALB)
Database	    Amazon RDS for MySQL
Security	    Security Groups, IAM
Deployment	    EC2 Launch Templates (if used)



---

# Task 3 — Public Entry Point

## Goal

Confirm the Book Review App loads through the public ALB DNS name.

### Evidence

#### Public ALB DNS

Paste your public ALB DNS name here:

`24.184.164.77`

---

# Task 4 — Evidence Screenshots

## Goal

Capture visual proof of every tier and load balancer.

### Evidence

#### Web EC2

![task 4](screenshots/Screenshot%20with%20EC2%20instances%20list%20showing%20two%20running%20instances%20in%20diff%20Availability%20Zones.png)

---

#### App EC2

![task 4](screenshots/Screenshot%20with%20EC2%20instance%20sumary%20showin%20the%20public%20IPv4%20address%20subnt%20and%20security%20grp.png)

---

#### Public ALB

![task 4](screenshots/Screenshot%20with%20ALB%20details%20showing%20two%20public%20subnets%20in%20two%20Availability%20Zones.png)

---

#### Internal ALB

![task 4](screenshots/Screenshot%20with%20ALB%20Security%20Group%20inbound%20rules.png)

---

#### RDS + Replica

![task 4](screenshots/Screenshot%20with%20RDS%20instance%20summary%20showing%20Publicly%20accessible%20No.png)

---

#### App UI proof

![task 4](screenshots/Screenshot%20with%20AWS%20Budget%20setup%20page%20showing%20the%20budget%20name.png)

---

# Task 5 — Summary

## Goal

Summarize what worked in the final deployment, the issues encountered and how each was fixed, and the tools or sources used to research and debug.

### Notes

**What worked:**

The deployment of the Book Review application using a three-tier AWS architecture was successfully implemented.

The custom VPC was configured with six subnets distributed across two Availability Zones, separating the Web, Application, and Database tiers.

The public Application Load Balancer was configured as the entry point for incoming application traffic and distributed requests to the Web Tier EC2 instances running the Next.js application behind Nginx.

The private Application Tier was designed to receive backend requests through an internal Application Load Balancer, with Node.js/Express running on private EC2 instances.

Amazon RDS for MySQL was configured within the private Database Tier, with Multi-AZ availability and a read replica to support database resilience and read scalability.

Security groups were used to control communication between the different tiers, limiting direct exposure of internal resources to the public internet.

Overall, the project demonstrated how to design and deploy a scalable, secure, and highly available cloud application using AWS networking, compute, load balancing, and managed database services.

---

**Issues + fixes:**

**Issues + Fixes:**

During the deployment, several configuration and connectivity challenges were encountered and investigated.

1. **Network connectivity and subnet configuration:** Ensured that the public and private subnets had the appropriate route table associations and that internet-facing resources were separated from internal application and database resources.

2. **Security group configuration:** Reviewed and adjusted inbound and outbound security group rules to ensure that each tier could communicate only with its permitted upstream and downstream components.

3. **Load balancer connectivity:** Checked the public and internal ALB configurations, listener rules, target groups, and health checks to ensure that traffic was routed to the correct instances.

4. **Application deployment and configuration:** Reviewed the application configuration, service availability, and communication between the Next.js frontend and Node.js/Express backend to troubleshoot potential connectivity issues.

5. **Database connectivity:** Verified that the RDS database was deployed in the private database environment and that the application tier had the appropriate network permissions to connect to MySQL.

These troubleshooting steps helped reinforce the importance of systematic debugging, correct AWS networking configuration, security isolation, and validating communication between application components.

---

**Tools/sources used:**

**Tools/Sources Used:**

The following tools and resources were used throughout the project:

- **AWS Management Console:** For provisioning, configuring, and monitoring AWS infrastructure.
- **Amazon EC2:** For deploying and managing the Web Tier and Application Tier servers.
- **Amazon VPC:** For creating the custom network, subnets, route tables, and network isolation.
- **Elastic Load Balancing (ALB):** For distributing incoming traffic across the application infrastructure.
- **Amazon RDS:** For configuring the MySQL database, Multi-AZ deployment, and read replica.
- **Security Groups and IAM:** For implementing access control and securing communication between resources.
- **Ubuntu Terminal and SSH:** For connecting to EC2 instances, installing dependencies, configuring services, and troubleshooting.
- **Nginx:** For reverse proxy configuration and routing requests to the Next.js application.
- **Node.js and Express:** For running the backend application.
- **Next.js:** For deploying the frontend application.
- **Claude AI and ChatGPT:** For technical research, understanding AWS configurations, troubleshooting, and validating deployment approaches.
- **AWS Documentation:** For referencing official AWS service documentation, architecture guidance, and troubleshooting procedures.

The combination of hands-on AWS implementation, command-line troubleshooting, technical documentation, and AI-assisted research helped guide the deployment process.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post sharing the capstone deployment, including the public ALB DNS (or a redacted screenshot), three to five lines on what you built and why it is production-style, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/er4mYa6i`

---

#### Screenshot of LinkedIn post

![linkedIN](screenshots/Screenshot%20with%20capstone%20linkedIn.png)

---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, RDS credentials, connection strings, private keys, or account IDs

---

# Completion Checklist

- [✅] Task 1: Architecture diagram completed
- [✅] Task 2: AWS Region and services documented
- [✅] Task 3: Public ALB DNS confirmed working
- [✅] Task 4: All six evidence screenshots captured (Web Tier, App Tier, both ALBs, RDS + replica, app UI)
- [✅] Task 5: Deployment summary completed (what worked, issues/fixes, tools/sources)
- [✅] LinkedIn post published and URL submitted
- [✅] App Tier and Database Tier confirmed not publicly accessible
- [✅] No sensitive data exposed

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

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*