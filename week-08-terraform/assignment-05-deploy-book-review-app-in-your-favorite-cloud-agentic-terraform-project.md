# Capstone Assignment — Deploy the Book Review App Using Terraform and Claude Code Agentic AI

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Student Details

**Full Name:** Oluwafemi Aremu
**Cloud Platform:** Azure  
**GitHub Repository URL:** (https://github.com/holarmyde/Ultimate-Agentic-DevOps-with-Claude-Code)
**Public Application URL / Load-Balancer DNS:** http://20.228.222.157

---

## Purpose

Deploy the Book Review App using Terraform on AWS or Azure in a secure, highly available, production-style three-tier architecture. Use Claude Code, specialized subagents, Terraform MCP, and validation hooks to support the engineering workflow while keeping all infrastructure-changing operations under human control.

---

# Task 0 — Prepare the Project and Agentic AI Environment

## Goal

Prepare the Book Review App project and configure the provided Claude Code Agentic AI starter kit with project context, specialized subagents, Terraform MCP, validation hooks, and safety guardrails.

## Evidence

### Screenshot 1 — Project `CLAUDE.md`

Add a screenshot of the project `CLAUDE.md` showing the three-tier architecture, security boundaries, Terraform requirements, and human-approval rules.

![task 0](screenshots/Screenshot%20with%20Azure%20CLI%20output%20showing%20the%20VM%20name%20and%20running%20status.png)

---

### Screenshot 2 — Terraform Engineer Subagent

Add a screenshot showing the Terraform Engineer subagent configuration.

![task 0](screenshots/Screenshot%20with%20Terraform%20Initialization.png)

---

### Screenshot 3 — Architecture and Security Reviewer Subagent

Add a screenshot showing the Architecture and Security Reviewer subagent configuration.

![task 0](screenshots/Screenshot%20with%20EC2%20and%20RDS%20Security%20Groups.png)

---

### Screenshot 4 — Terraform MCP Connection

Add a screenshot showing Terraform MCP connected and available.

![task 0](screenshots/Screenshot%20with%20Modular%20Project%20Structure.png)

---

### Screenshot 5 — Validation Hooks

Add a screenshot showing the configured Claude Code validation hooks.

![task 0](screenshots/Screenshot%20with%20VS%20Code%20Extensions%20panel%20showing%20the%20HashiCorp%20Terraform.png)

---

# Task 1 — Design the Three-Tier Architecture

## Goal

Design the required secure, highly available three-tier architecture and create an architecture diagram before building the infrastructure.

The diagram must show:

- VPC or VNet
- Availability Zones or equivalent availability locations
- Six subnets
- Internet connectivity
- NAT or outbound design
- Public load balancer
- Web Tier
- Internal load balancer
- Application Tier
- Managed MySQL
- Read replica
- Main traffic flow

## Architecture Diagram

Add the completed architecture diagram here.

---

# Task 2 — Build the Terraform Networking and Security Layers

## Goal

Create the modular Terraform project and implement the network and security layers across the required public and private subnets.

## Evidence

### Screenshot 6 — Modular Terraform Project Structure

Add a screenshot showing the modular Terraform project structure.

![task 2](screenshots/Screenshot%20with%20Modular%20Project%20Structure.png)

---

### Screenshot 7 — Six-Subnet Architecture

Add a screenshot showing the six-subnet architecture across two availability locations.

![task 2](screenshots/Screenshot%20with%20VPC%20and%20Subnets.png)

---

### Screenshot 8 — Public and Private Tier Separation

Add a screenshot showing the public and private tier separation, including routing and security boundaries.

![task 2](screenshots/Screenshot%20with%20%20Internet%20Gateway%20and%20Public%20Routing.png)

---

# Task 3 — Build the Load-Balancing and Compute Layers

## Goal

Deploy the public and internal load balancers and the Web and Application compute resources required by the Book Review App.

## Evidence

### Screenshot 9 — Web and Application Compute

Add a screenshot showing the Web and Application compute resources in their required subnets.

![task 3](screenshots/Screenshot%20with%20DB%20Subnet%20Group%20and%20RDS%20MySQL.png)

---

### Screenshot 10 — Public Load Balancer

Add a screenshot showing the internet-facing public load balancer.

![task 3](screenshots/Screenshot%20with%20Linux%20Virtual%20Machine%20and%20custom_data.png)

---

### Screenshot 11 — Internal Load Balancer

Add a screenshot showing the private internal load balancer.

![task 3](screenshots/Screenshot%20with%20%20Internet%20Gateway%20and%20Public%20Routing.png)

---

### Screenshot 12 — Healthy Targets

Add a screenshot showing healthy target groups or backend pools.

![task 3](screenshots/Screenshot%20with%20Terraform%20output%20showing%20the%20public%20IP%20of%20the%20VM.png)
---

# Task 4 — Build the Managed MySQL Database Layer

## Goal

Deploy a private, highly available managed MySQL database with a read replica and restrict database connectivity to the Application Tier.

## Evidence

### Screenshot 13 — Managed MySQL Database

Add a screenshot showing the managed MySQL database deployment.

![task 4](screenshots/Screenshot%20with%20DB%20Subnet%20Group%20and%20RDS%20MySQL.png)

---

### Screenshot 14 — High Availability

Add a screenshot showing the Multi-AZ or high-availability configuration.

![task 4](screenshots/Screenshot%20with%20Modular%20Epicbook%20Deployment%20-%204.png)

---

### Screenshot 15 — Read Replica

Add a screenshot showing the read replica configuration.

![task 4](screenshots/Screenshot%20Nginx%20Configuration%20and%20Service%20-%204.png)

---

### Screenshot 16 — Private Database Access

Add a screenshot showing that the database is private and accepts MySQL traffic only from the Application Tier.

![task 4](screenshots/Screenshot%20with%20DB%20Subnet%20Group%20and%20RDS%20MySQL.png)

---

# Task 5 — Validate, Review, and Apply the Terraform Configuration

## Goal

Validate the Terraform configuration, review the execution plan using both Agentic AI and human judgment, and apply the infrastructure changes only after all required checks pass.

## Evidence

### Screenshot 17 — Terraform Validation

Add a screenshot showing successful `terraform validate` output.

![task 5](screenshots/Screenshot%20with%20terraform%20validation.png)

---

### Screenshot 18 — Terraform Plan

Add a screenshot showing the Terraform plan output.

![task 5](screenshots/Screenshot%20with%20Terraform%20Plan.png)

---

### Screenshot 19 — Terraform Apply

Add a screenshot showing successful `terraform apply` completion.

![task 5](screenshots/Screenshot%20with%20Terraform%20apply%20output%20showing%20successful%20completion%20-%20NOW.png)

---

# Task 6 — Deploy and Configure the Book Review Application

## Goal

Deploy and configure the Book Review App across the Web, Application, and Database tiers and verify the complete application functionality.

## Evidence

### Screenshot 20 — Homepage

Add a screenshot showing the Book Review App homepage through the public endpoint.

![task 6](screenshots/Screenshot%20with%20browser%20screenshot.png)

---

### Screenshot 21 — Login or Authentication

Add a screenshot showing successful login or authentication.

![task 6](screenshots/Screenshot%20with%20React%20Application%20in%20the%20Browser.png)

---

### Screenshot 22 — Book Data

![task 6](screenshots/Screenshot%20with%20user_data.sh.png)

!

---

### Screenshot 23 — Review Functionality

Add a screenshot showing the review functionality working successfully.

![task 6](screenshots/Screenshot%20with%20Private%20RDS%20Available-4.png)
---

### Screenshot 24 — Backend or API Evidence

Add a screenshot showing that the backend or API is working successfully.

![task 6](screenshots/Screenshot%20with%20React%20Application%20in%20the%20Browser.png)

---

### Screenshot 25 — Database Reads and Writes

Add a screenshot showing successful database reads and writes.

![task 6](screenshots/Screenshot%20with%20user_data.sh.png)

## Public Application URL

**Public Application URL / DNS:** (http://20.228.222.157)

---

# Task 7 — Demonstrate the Agentic AI Workflow

## Goal

Demonstrate how Claude Code assisted with Terraform generation, architecture and security review, and evidence-based troubleshooting while infrastructure-changing decisions remained under human control.

You do not need to submit your complete Claude Code conversation history. Include only focused evidence.

## Evidence

### Screenshot 26 — AI-Assisted Terraform Generation

Add a screenshot showing one useful example of AI-assisted Terraform generation or improvement.

![task 7](screenshots/Screenshot%20with%20%20Root%20Module%20Blocks.png)

---

### Screenshot 27 — Architecture or Security Review

Add a screenshot showing one structured architecture or security review result.

![task 7](screenshots/Screenshot%20with%20EC2%20and%20RDS%20Security%20Groups.png)

---

### Screenshot 28 — AI-Assisted Troubleshooting

Add a screenshot showing one AI-assisted troubleshooting interaction based on collected evidence.

![task 7](screenshots/Screenshot%20with%20%20VS%20Code%20showing%20the%20AWS%20provider%20configuration.png)

---

# Task 8 — Complete the Final Architecture Review

## Goal

Review the completed infrastructure against the original capstone requirements and resolve significant architecture, security, reliability, and cost issues.

Confirm that the final review covers:

- Tier separation
- Availability
- Public exposure
- Routing
- Security rules
- Load balancing
- Database privacy
- Secrets
- Terraform quality
- Module structure
- Reliability
- Obvious cost risks

Use Screenshot 27 as the focused evidence for the structured architecture or security review.

---

# Task 9 — Answer the Reflection Questions

## Goal

Reflect on the architecture, Terraform implementation, and Agentic AI workflow. Answer each question briefly in your own words.

## Architecture

### 1. Why did you separate the Web, Application, and Database tiers?

I separated the infrastructure into three tiers to improve security, scalability, maintainability, and performance.
- Web Tier: Handles incoming HTTP/HTTPS requests through the public load balancer and serves the frontend.
- Application Tier: Processes business logic, authentication, API requests, and communication with the database.
- Database Tier: Stores application data, including user accounts, books, and reviews.
This separation ensures that each tier has a specific responsibility and can be managed, secured, and scaled independently.

### 2. Why is the Application Tier private?

The Application Tier is private because it should not be directly accessible from the public internet. Requests should reach it through the Web Tier and internal load balancer.

This reduces the attack surface, protects application services from unauthorized access, and ensures that only approved traffic can reach the backend servers.

### 3. Why is MySQL private?

MySQL is deployed in private subnets to protect sensitive application data from direct internet exposure.

Database access should only be permitted from the Application Tier through restricted security rules on the MySQL port (3306). This prevents unauthorized external connections and improves data security.

### 4. Why are multiple Availability Zones used?

Multiple Availability Zones provide high availability and fault tolerance.

By distributing resources across two availability locations, the application can continue operating if one location experiences infrastructure failure. It also reduces dependence on a single point of failure and improves service reliability.

### 5. What is the difference between Multi-AZ/high availability and a read replica?

Multi-AZ and read replicas serve different purposes.
- Multi-AZ: Provides high availability by maintaining a standby database in another Availability Zone. It supports failover when the primary database experiences a failure.
- Read Replica: Maintains a replicated copy of the primary database that can handle read requests, reducing the workload on the primary database.
Multi-AZ primarily improves availability, while read replicas primarily improve read scalability and performance.
## Terraform

### 6. How did you divide your Terraform into modules?

I divided the Terraform configuration into reusable modules to make the infrastructure easier to manage, understand, test, and maintain.

The modules were organized around infrastructure responsibilities, including:
- Networking module — VNet, subnets, routing, and internet connectivity.
- Security module — Network security groups and traffic restrictions.
- Load Balancer module — Public and internal load balancing.
- Web module — Web-tier compute resources.
- Application module — Application-tier compute resources.
- Database module — Managed MySQL and read replica configuration.
The root Terraform configuration coordinates the modules and manages their dependencies.

### 7. How do the modules communicate through variables and outputs?

Terraform modules communicate through input variables and output values.

Input variables allow a module to receive information from the root configuration or another module. Outputs expose important resource information, such as subnet IDs, private IP addresses, security group IDs, and load balancer addresses.

For example, the networking module can expose subnet IDs through outputs, which the Web and Application modules can consume as input variables.

This creates a modular and reusable infrastructure design without hardcoding resource values throughout the project.

### 8. What did you specifically check in `terraform plan`?

Before applying the Terraform configuration, I reviewed the execution plan to understand exactly what infrastructure changes Terraform intended to make.

I checked:
- Which resources would be created, modified, or destroyed.
- Whether the six subnets were configured correctly.
- Whether public and private subnet routing was correct.
- Whether load balancers were assigned to the correct subnets.
- Whether Application Tier resources remained private.
- Whether database access was restricted to the Application Tier.
- Whether availability and database replication settings matched the intended architecture.
- Whether any sensitive values were unnecessarily exposed.
- Whether unexpected replacements or destructive changes were planned.
This review helped ensure that infrastructure changes were intentional and safe before running terraform apply.

## Agentic AI

### 9. What was the purpose of `CLAUDE.md`?

The purpose of CLAUDE.md was to provide Claude Code with project-specific instructions and context.

It documented the three-tier architecture, infrastructure requirements, security boundaries, Terraform standards, project structure, and human-approval rules.

This helped Claude understand the project's objectives and generate recommendations that followed the required architecture and security guidelines.


### 10. What work did the Terraform Engineer subagent perform?

The Terraform Engineer subagent assisted with designing and generating Terraform configurations for the infrastructure.

Its responsibilities included:
- Creating and organizing reusable Terraform modules.
- Defining resources, variables, and outputs.
- Configuring networking, subnets, routing, and security resources.
- Implementing load balancers and compute resources.
- Configuring the managed MySQL database.
- Reviewing Terraform configuration for consistency and maintainability.
It helped accelerate infrastructure development while allowing me to review and approve proposed changes.

### 11. What did the Architecture and Security Reviewer identify?

The Architecture and Security Reviewer helped assess the proposed infrastructure against the required three-tier architecture and security requirements.

The review focused on:
- Proper separation of public and private subnets.
- Restricting direct internet access to the Application and Database Tiers.
- Ensuring database access was limited to the Application Tier.
- Checking load balancer placement and traffic flow.
- Reviewing security rules and unnecessary public exposure.
- Evaluating availability across multiple Availability Zones.
The review helped identify potential security gaps and architectural weaknesses before infrastructure deployment.

### 12. Why did you use Terraform MCP instead of relying only on Claude's existing Terraform knowledge?

I used Terraform MCP to provide Claude Code with access to more structured and current Terraform-related information and tooling.

Instead of depending entirely on the model's existing knowledge, Terraform MCP helped support infrastructure development with relevant Terraform documentation, resource information, and validation capabilities.

This improved the quality of Terraform recommendations and reduced the possibility of using outdated or incorrect configurations.

### 13. What was the purpose of your validation hooks?

The validation hooks were introduced to automate important checks during the development workflow.

They helped ensure that Terraform changes were checked before being accepted or applied.

These checks included formatting, configuration validation, and other project-defined quality checks.

The hooks improved consistency, reduced avoidable errors, and helped enforce coding standards while keeping infrastructure-changing operations under human control.
### 14. Describe one real issue Claude helped you troubleshoot.

Claude Code helped me investigate Terraform configuration and deployment issues by examining the available configuration, command outputs, and error messages.

One important troubleshooting area was verifying that the infrastructure configuration matched the intended architecture, particularly the networking, subnet assignments, and security boundaries.

Claude helped analyze the evidence, explain potential causes, and recommend corrections.

I reviewed the recommendations before making changes, ensuring that infrastructure modifications remained under my control.

### 15. Describe one recommendation you reviewed, modified, or rejected instead of accepting blindly.

One important lesson was that AI-generated infrastructure recommendations should not automatically be accepted.

For example, any recommendation that exposed the Application Tier or Database Tier directly to the internet would conflict with the security requirements of the project.

I would reject or modify such a recommendation to ensure that:
- The Web Tier handles public traffic.
- The Application Tier remains private.
- MySQL accepts connections only from authorized application resources.
- Infrastructure changes are reviewed through terraform plan before application.
This demonstrated the importance of human judgment, security awareness, and responsible use of Agentic AI in infrastructure engineering.

---

# Task 10 — Publish the Mandatory LinkedIn Post

## Goal

Publish a LinkedIn post describing the capstone, the technical work completed, the Agentic AI workflow, and the lessons learned.

Write the post in your own words, include at least one project image or other proof, and ensure that it can be viewed by the submission reviewer.

## LinkedIn Post URL

**LinkedIn Post URL:** https://lnkd.in/p/ejfghVGT

---

# Submission Instructions

- Complete Tasks 0–10 in sequence.
- Include all Screenshots 1–28 exactly as specified.
- Ensure that your full name is visible in the required screenshots.
- Include the selected cloud platform.
- Include the completed architecture diagram.
- Include the modular Terraform project structure.
- Include the working public application URL or public load-balancer DNS.
- Include all required Agentic AI workflow evidence.
- Answer all 15 reflection questions briefly in your own words.
- Include the published LinkedIn post URL.
- Do not expose cloud credentials, database passwords, SSH private keys, JWT secrets, access tokens, account IDs, Terraform state containing sensitive values, or other confidential information.
- Review all screenshots and project files carefully before submitting through GitHub.

---

# Completion Checklist

- [✅] Selected AWS or Azure
- [✅] Added and reviewed the Agentic AI starter files
- [✅] Configured `CLAUDE.md`
- [✅] Configured the Terraform Engineer subagent
- [✅] Configured the Architecture and Security Reviewer subagent
- [✅] Connected Terraform MCP
- [✅] Configured validation hooks and safety guardrails
- [✅] Created the architecture diagram
- [✅] Created the six-subnet design
- [✅] Configured public Web Tier routing
- [✅] Kept the Application Tier private
- [✅] Kept the Database Tier private
- [✅] Configured tier-specific Security Groups or NSGs
- [✅] Restricted backend port `3001`
- [✅] Restricted MySQL port `3306` to the Application Tier
- [✅] Created the public load balancer
- [✅] Created the internal load balancer
- [✅] Configured listeners and health checks
- [✅] Deployed the Web Tier compute resources
- [✅] Deployed the private Application Tier compute resources
- [✅] Provisioned private managed MySQL
- [✅] Configured Multi-AZ or high availability
- [✅] Configured a read replica
- [✅] Created the modular Terraform project
- [✅] Used variables, outputs, and module dependencies
- [✅] Used current Terraform documentation through MCP
- [✅] Used hooks for deterministic validation
- [✅] Completed `terraform fmt`
- [✅] Completed `terraform validate`
- [✅] Reviewed `terraform plan`
- [✅] Completed the Terraform Engineer review
- [✅] Completed the Architecture and Security review
- [✅] Applied the infrastructure only after human approval
- [✅] Deployed and configured the backend
- [✅] Deployed and configured the frontend
- [✅] Configured Nginx where required
- [✅] Configured the internal backend endpoint
- [✅] Configured the public frontend endpoint
- [✅] Verified the homepage
- [✅] Verified login or authentication
- [✅] Verified book data
- [✅] Verified review functionality
- [✅] Verified the backend API
- [✅] Verified database reads and writes
- [✅] Verified healthy load-balancer targets
- [✅] Included AI-assisted Terraform generation evidence
- [✅] Included one architecture or security review
- [✅] Included one AI-assisted troubleshooting example
- [✅] Completed the final architecture review
- [✅] Answered all 15 reflection questions
- [✅] Published the mandatory LinkedIn post
- [✅] Added the LinkedIn post URL
- [✅] Captured all 28 required screenshots
- [✅] Confirmed that my full name is visible in the required screenshots
- [✅] Checked that no secrets or sensitive information are exposed

---

## About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory), focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations through hands-on experience.

---

## Resources

- Book Review App Repository: [https://github.com/pravinmishraaws/book-review-app](https://github.com/pravinmishraaws/book-review-app)
- DMI Official Website: [https://dmi.pravinmishra.com](https://dmi.pravinmishra.com)
- University: [https://university.pravinmishra.com](https://university.pravinmishra.com)
- Discord Community: [https://discord.pravinmishra.com](https://discord.pravinmishra.com)
- Blog: [https://dmi.pravinmishra.com/blog](https://dmi.pravinmishra.com/blog)
- YouTube Playlist: [https://www.youtube.com/playlist?list=PLFeSNDtI4Cho](https://www.youtube.com/playlist?list=PLFeSNDtI4Cho)
- Pravin Mishra on LinkedIn: [https://www.linkedin.com/in/pravin-mishra-aws-trainer/](https://www.linkedin.com/in/pravin-mishra-aws-trainer/)
- CloudAdvisory on LinkedIn: [https://www.linkedin.com/company/thecloudadvisory/](https://www.linkedin.com/company/thecloudadvisory/)

---

*This submission is part of the DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
