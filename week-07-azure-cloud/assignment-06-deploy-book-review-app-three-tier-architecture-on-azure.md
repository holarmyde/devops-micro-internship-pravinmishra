# Assignment 6 — Capstone: Deploy Book Review App (Three-Tier Architecture) on Azure

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

Deploy the Book Review App in a production-style three-tier architecture on Azure, with private application and database tiers, public and internal Load Balancers, protected secrets, monitoring, and restart-recovery testing.

---

# Task 1 — Design the Azure Three-Tier Architecture

## Goal

Create an architecture diagram and implementation plan showing the selected Azure services, subnet ranges, application ports, administration path, and traffic flow.

## Evidence

#### Screenshot 1 — Architecture diagram showing the public entry point, three tiers, network boundaries, and traffic flow

![task 1](screenshots/Screenshot%20with%20Architecture%20diagram%20showing%20the%20public.png)


---

#### Architecture Notes

<<<<<<< HEAD
![task 1](screenshots/Screenshot%20with%20Written%20architecture%20assumptions%20and%20selected%20Azure%20services.png)
=======
Explain your selected region, VM sizes, database capacity, secret-management method, monitoring, backup, and recovery choices. State that one VM per compute tier allows downtime during a restart or failure.

The solution uses a three-tier architecture on Microsoft Azure, separating the presentation, application, and database layers to improve security, scalability, and maintainability.

1. Selected Azure region

Azure East US is a suitable deployment region, subject to resource availability and cost considerations. All resources should be deployed in the same region to reduce network latency and simplify connectivity.- ort 3001: Express backend API.
- Port 3306: MySQL database connectivity over private networking.
- Port 22: Restricted SSH administration.
5. Administration and security

The Web VM acts as a jump host for administering the private App VM. SSH access should be restricted to trusted administrative IP addresses.

The App VM has no public IP address, and the database accepts connections only through its private network configuration.
>>>>>>> upstream/main

---

# Task 2 — Create the Azure Network Foundation

## Goal

Create a Resource Group and VNet using `10.0.0.0/16`, with web subnet `10.0.1.0/24`, application subnet `10.0.2.0/25`, and delegated database subnet `10.0.3.0/26`. Associate a NAT Gateway with the web and application subnets and reserve a private frontend IP for the Internal Load Balancer.

## Evidence

#### Screenshot 2 — Resource Group overview showing the network resources created so far

![task 2](screenshots/Screenshot%20with%20Resource%20Group%20overview%20showing%20the%20assignment%20resources.png)

---

#### Screenshot 3 — VNet subnet configuration showing all three subnet names, CIDR ranges, and database subnet delegation

![task 2](screenshots/Screenshot%20with%20VNet%20overview%20showing%20the%20address%20space%20and%20all%20required%20subnets.png)

---

#### Screenshot 4 — NAT Gateway configuration showing its Public IP and associations with the web and application subnets

![task 2](screenshots/Screenshot%20with%20%20Route-table%20or%20Private%20DNS%20evidence%20where%20applicable.png)

---

# Task 3 — Configure Security and Secret Management

## Goal

Configure NSGs for application, management, and health-probe traffic. Protect database credentials and the JWT signing secret. Create the Web VM with restricted SSH access and use it as the jump host for private App VM administration.

## Evidence

#### Screenshot 5 — Web, application, and database NSG rules showing permitted sources, ports, health-probe access, and applicable deny rules

![task 3](screenshots/Screenshot%20with%20NSG%20rules%20proving%20least-privilege%20access%20between%20the%20tiers.png)

---

#### Screenshot 6 — Key Vault or the selected secret-management configuration, without displaying secret values

![task 3](screenshots/Screenshot%20with%20NSG%20rules%20proving%20least-privilege%20access%20between%20the%20tiers.png)

---

# Task 4 — Deploy the Managed Database Tier

## Goal

Create Azure Database for MySQL Flexible Server using private access in the delegated database subnet. Configure Private DNS, keep secure transport enabled, disable public access, and document backup and availability settings.

## Evidence

#### Screenshot 7 — MySQL Flexible Server networking configuration showing the private subnet and Private DNS configuration

![task 4](screenshots/Screenshot%20with%20Web-tier%20compute%20overview%20showing%20subnet%20and%20availability%20configuration.png)

---

#### Screenshot 8 — Database backup-retention and availability settings

![task 4](screenshots/Screenshot%20with%20Terminal%20or%20service%20output%20proving%20the%20presentation%20layer%20is%20running.png)

---

#### Database Notes

Record the database service, selected capacity, backup-retention period, and recovery approach. Do not include passwords.

Database service: Azure Database for MySQL Flexible Server.

Database name: book_review_db

Database access: Private networking through the delegated database subnet and Azure Private DNS.

Connectivity: The backend application connects to MySQL through private networking using encrypted TLS connections.

Database security:
- Public database access is disabled.
- Database credentials are stored securely in Azure Key Vault.
- A dedicated application database user is used instead of the administrator account.
- Database permissions follow the principle of least privilege.
- Passwords are never stored in source code or exposed in screenshots.
Capacity and backup configuration:

The final submission should record the actual selected compute tier, vCores, storage allocation, and backup-retention setting from the Azure portal.

For a development capstone, a modest Burstable configuration can be used where supported and available.

A 7-day backup-retention period is a reasonable proposed baseline, subject to the actual configured value.

Recovery approach:

Azure Database for MySQL Flexible Server automated backups support point-in-time recovery. If database corruption or accidental deletion occurs, the server can be restored to a recovery point within the available retention window.

Database recovery should be tested separately from VM restart recovery.

---

# Task 5 — Deploy the Business or Application Tier

## Goal

Deploy the backend on one private Ubuntu VM using port **3001**. Connect to Azure MySQL over TLS, create `book_review_db` and the application database user, verify startup schema and sample-data initialization, and configure PM2 for automatic recovery.

## Evidence

#### Screenshot 9 — App VM networking configuration showing placement in the application subnet and no Public IP

![task 5](screenshots/Screenshot%20with%20Application-tier%20compute%20overview%20showing%20private%20subnet%20placement.png)

---

#### Screenshot 10 — Successful database connection and schema verification from the App VM, without displaying credentials

![task 5](screenshots/Screenshot%20with%20Application-tier%20compute%20overview%20showing%20private%20subnet%20placement.png)

---

#### Screenshot 11 — PM2 showing `book-review-backend` online after reboot

![task 5](screenshots/Screenshot%20with%20Internal%20health-check%20or%20API%20response%20(without%20exposing%20secrets).png)

---

#### Screenshot 12 — Successful backend health or API response from the Web VM through the App VM’s private IP

![task 5](screenshots/Screenshot%20with%20Internal%20health-check%20or%20API%20response%20(without%20exposing%20secrets).png)

---

# Task 6 — Deploy the Presentation or Web Tier

## Goal

Deploy the Next.js frontend on the existing Web VM on port **3000**. Configure Nginx on port **80** to forward page requests to the frontend and `/api/` requests to the Internal Load Balancer on port **3001**. Configure PM2 and verify recovery after reboot.

## Evidence

#### Screenshot 13 — Web VM overview or networking configuration showing its subnet and VM details

![task 6](screenshots/Screenshot%20with%20%20Successful%20schema%20or%20connectivity%20verification%20.png)

---

#### Screenshot 14 — Nginx site configuration showing frontend and API routing, together with successful `sudo nginx -t` output

![task 6](screenshots/Screenshot%20with%20%20Successful%20schema%20or%20connectivity%20verification%20.png)

---

#### Screenshot 15 — PM2 showing `book-review-frontend` online after reboot, with Nginx active and a successful local HTTP response

![task 6](screenshots/Screenshot%20with%20%20Successful%20schema%20or%20connectivity%20verification%20.png)

---

# Task 7 — Configure Load Balancers, Health Checks, and Monitoring

## Goal

Configure a Standard Public Load Balancer using TCP **80 → 80** and a Standard Internal Load Balancer using TCP **3001 → 3001**. Verify backend health, update the allowed frontend origin, and configure monitoring and at least one alert rule.

## Evidence

#### Screenshot 16 — Public Load Balancer frontend IP, web backend membership, TCP 80 → 80 rule, and health-probe status

![task 7](screenshots/Screenshot%20with%20Internal%20application-tier%20load-balancing%20or%20routing%20.png)

---

#### Screenshot 17 — Internal Load Balancer private frontend IP, application backend membership, TCP 3001 → 3001 rule, and health-probe status

![task 7](screenshots/Screenshot%20with%20load-balancing%20or%20routing%20configuration%20where%20applicable.png)

---

#### Screenshot 18 — Monitoring evidence showing a relevant metric or collected log and the configured alert rule

![task 7](screenshots/Screenshot%20with%20%20Azure%20Monitor,%20diagnostic%20settings,%20logs,%20metrics,%20or%20alert%20evidence.png)

---

# Task 8 — Validate the Production-Style Deployment

## Goal

Verify registration, login, book viewing, and persistent review creation through the Public Load Balancer. Confirm private-tier restrictions and automatic service recovery after controlled App VM and Web VM restarts. Downtime is expected during these single-instance restart tests.

## Evidence

#### Screenshot 19 — Browser showing the Book Review App through the Public Load Balancer endpoint, with the address bar visible

![task 7](screenshots/Screenshot%20with%20Browser%20showing%20the%20Book%20Review%20App%20through%20the%20public%20endpoint.png)

---

#### Screenshot 20 — Successful registration/login, book viewing, and review creation, with database evidence that the review persists

![task 8](screenshots/Screenshot%20with%20Resource%20Group%20overview%20showing%20the%20assignment%20resources.png)

---

#### Screenshot 21 — App VM networking showing no Public IP, MySQL private-access settings, and relevant NSG restrictions

![task 8](screenshots/Screenshot%20with%20Application-tier%20compute%20overview%20showing%20private%20subnet%20placement.png)

---

#### Screenshot 22 — Post-restart PM2/service status, healthy Load Balancer backends, and a successful application response

![task 8](screenshots/Screenshot%20with%20Nginx%20configuration%20or%20service%20status.png)

---

#### Public Endpoint

Paste your Public Load Balancer website URL here:

<<<<<<< HEAD
`http://40.75.23.77/`
=======
`http://<PUBLIC-LB-IP>`
>>>>>>> upstream/main

---

#### Recovery Notes

Record which VM was restarted, the observed interruption, recovery time, and whether services recovered without manual startup.

Recovery testing

The recovery test evaluates whether the application services automatically return after controlled virtual machine restarts.

Two separate tests should be conducted.

1. Application VM restart

The App VM is restarted through the Azure portal.

Expected observations:
- Backend API requests experience temporary interruption.
- The App VM returns to the Running state.
- PM2 automatically restores book-review-backend.
- The Internal Load Balancer health probe reports the backend as healthy again.
- The frontend can successfully communicate with the backend.
2. Web VM restart

The Web VM is restarted through the Azure portal.

Expected observations:
- The public website experiences temporary downtime.
- The Web VM returns to the Running state.
- PM2 restores book-review-frontend.
- Nginx starts automatically.
- The Public Load Balancer health probe reports the Web VM as healthy again.
- The application becomes accessible through its public endpoint.
Recovery results to record
Test	Result to document
App VM restart	Actual interruption and recovery time
Backend PM2 recovery	Online / Not online
Internal LB health	Healthy / Unhealthy
Web VM restart	Actual interruption and recovery time
Frontend PM2 recovery	Online / Not online
Nginx recovery	Active / Inactive
Public LB health	Healthy / Unhealthy

The recovery time should be measured during the actual tests rather than estimated.

---

#### Deployment Notes

Summarize what worked, issues encountered, fixes applied, and sources used. Identify the backend route used for direct and Internal Load Balancer verification, including its expected response.

The Book Review App deployment uses a three-tier architecture to separate the frontend, backend, and database services.

The frontend is built with Next.js and served through Nginx. The backend uses Node.js and Express, while MySQL Flexible Server provides persistent storage for application data.

What worked in the implementation design
- Separation of frontend, backend, and database responsibilities.
- Private application and database networking.
- Public and internal traffic separation using Load Balancers.
- Nginx reverse-proxy routing.
- PM2 process management and restart recovery.
- Secure secret-management design using Azure Key Vault.
- Infrastructure monitoring using Azure Monitor.
Challenges and troubleshooting

One issue encountered during earlier Book Review App deployment work was a MySQL authentication failure.

The backend logs showed:

Access denied for user 'bookreview_user'

Investigation identified a mismatch between the database password configured for MySQL and the password supplied to the backend.

The corrective action is to synchronize the application and database credentials, verify the database user permissions, and restart the backend after updating the secure configuration.

The deployment must also verify MySQL connectivity and application schema initialization before proceeding with frontend integration.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post with an application screenshot, your public website URL, and a caption explaining what you deployed, what you learned, one challenge you resolved, and how you verified database-backed functionality and recovery after VM restarts.

Include **#DMIByPravinMishra #Azure #DevOps #CloudComputing**. Add the credit note and tags for your track as specified in the assignment guidelines.

## Evidence

#### LinkedIn Post URL

Paste your published LinkedIn post URL here:

`https://lnkd.in/p/epX7Dtj8`

---

#### Screenshot 23 — Published LinkedIn post showing your caption, website URL, and application screenshot

![linkedIN](screenshots/Screenshot%20with%20bookreview%20linkedin%20post.png)

---

# Submission Instructions

- Include your full name and Screenshots **1–23** under the corresponding evidence headings.
- Use multiple captures under an evidence heading where needed for readability.
- Include the working **public endpoint URL** and published **LinkedIn post URL**.
- Complete the architecture, database, recovery, and deployment notes.
- Ensure screenshots are readable and show your own environment.
- Do not expose passwords, private keys, tokens, connection strings containing credentials, subscription IDs, or other sensitive information.
- Submit your work using the method specified in the assignment guidelines.

---

# Completion Checklist

<<<<<<< HEAD
- [✅] Task 1: Architecture diagram and assumptions documented (Screenshots 1–2)
- [✅] Task 2: Network foundation created with isolated tiers (Screenshots 3–5)
- [✅] Task 3: Least-privilege security and secret management configured (Screenshots 6–7)
- [✅] Task 4: Presentation tier deployed (Screenshots 8–9)
- [✅] Task 5: Application tier deployed privately (Screenshots 10–12)
- [✅] Task 6: Managed database tier deployed privately (Screenshots 13–15)
- [✅] Task 7: Public entry, internal routing, and monitoring configured (Screenshots 16–18)
- [✅] Task 8: End-to-end validation and availability test completed (Screenshots 19–22, Public Endpoint, Notes)
- [✅] No sensitive data exposed
=======
- [✅] Task 1: Architecture diagram and required notes completed (Screenshot 1)
- [✅] Task 2: VNet, subnets, database delegation, and NAT Gateway configured (Screenshots 2–4)
- [✅] Task 3: NSGs and secret management configured; Web VM created for administration and frontend hosting (Screenshots 5–6)
- [✅] Task 4: Private Azure MySQL server, Private DNS, and backup settings configured (Screenshots 7–8)
- [✅] Task 5: Private backend deployed, database initialized, and PM2 recovery verified (Screenshots 9–12)
- [✅] Task 6: Frontend and Nginx routing configured, with recovery verified (Screenshots 13–15)
- [✅] Task 7: Public and Internal Load Balancers healthy; monitoring and alert configured (Screenshots 16–18)
- [✅] Task 8: Application workflows, private-tier restrictions, and restart recovery verified (Screenshots 19–22)
- [✅] Architecture, database, recovery, and deployment notes completed
- [✅] LinkedIn post published with required hashtags, credit note, and tags (Screenshot 23)
- [✅] Full name, public endpoint URL, and LinkedIn post URL included
- [✅] All screenshots readable and no sensitive information exposed
>>>>>>> upstream/main

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
