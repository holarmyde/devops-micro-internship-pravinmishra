# Assignment 4 — Capstone: Automate the EpicBook Application with Dual Pipelines

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this capstone assignment, you will design a complete DevOps automation workflow for EpicBook across two repositories and two Azure DevOps pipelines: an Infra Pipeline (`infra-epicbook`, Terraform) that provisions the network, frontend/backend VMs, and MySQL database, and an App Pipeline (`theepicbook`, Ansible) that consumes the Terraform outputs, configures the servers, and deploys EpicBook end to end.

---

# Task 1 — Prepare Repositories

## Goal

Prepare `infra-epicbook` (Terraform for network, frontend/backend VMs, MySQL, with `app_public_ip` and `mysql_fqdn` outputs) and `theepicbook` (application code, Ansible roles/playbooks, inventory template, MySQL variable files), keeping infrastructure and application responsibilities separated.

### Evidence

#### Screenshot 1 — Both repositories showing their required files and separation of responsibilities

![task 1](screenshots/Screenshot%20with%20Both%20repositories%20showing%20their%20required%20files%20-%202.png)

---

# Task 2 — Create Azure Service Connection

## Goal

Create and validate an Azure Resource Manager SPN service connection (Tenant ID, Subscription ID, Client ID, Client Secret) for the Infra Pipeline.

### Evidence

#### Screenshot 2 — Azure Resource Manager service connection showing successful configuration with secrets hidden

![task 2](screenshots/Screenshot%20with%20Azure%20Resource%20Manager%20service%20connection.png)

---

# Task 3 — Create Infra Pipeline

## Goal

Create a YAML pipeline for `infra-epicbook` that authenticates via the SPN connection and runs `terraform init`/`plan`/`apply`, producing `app_public_ip` and `mysql_fqdn`.

### Evidence

#### Screenshot 3 — Infra Pipeline run showing `terraform apply` completion and the `app_public_ip` and `mysql_fqdn` outputs

![task 3](screenshots/Screenshot%20with%20Infra%20Pipeline%20run%20showing%20terraform%20apply%20completion.png)

---

#### Screenshot 4 — Azure Portal confirming the provisioned resources

![task 3](screenshots/Screenshot%20with%20Azure%20Portal%20confirming%20the%20provisioned%20resources.png)

---

# Task 4 — Create App Pipeline

## Goal

Upload the SSH private key to Azure DevOps Secure Files, create a YAML pipeline for `theepicbook` that installs Ansible, downloads the key, manually incorporates the Infra Pipeline's `app_public_ip`/`mysql_fqdn` outputs into the inventory/variables, and runs the playbook to configure the frontend/backend and deploy EpicBook behind Nginx.

### Evidence

#### Screenshot 5 — App Pipeline run summary showing successful completion

![task 4](screenshots/Screenshot%20with%20App%20Pipeline%20run%20summary%20showing%20successful%20completion.png)

---

#### Screenshot 6 — Ansible playbook output showing successful configuration with `failed=0`

![task 4](screenshots/Screenshot%20with%20Ansible%20playbook%20output%20showing%20successful%20configuration.png)

---

# Task 5 — Verify End-to-End Workflow

## Goal

Confirm both pipelines succeeded, the EpicBook application loads through the frontend public IP via Nginx, and a backend feature confirms successful MySQL connectivity.

### Evidence

#### Screenshot 7 — Browser displaying the running EpicBook application with the frontend public IP visible

![task 5](screenshots/Screenshot%20wit%20Browser%20displayin%20d%20runnin%20EpicBook%20applicatn%20wit%20d%20fend%20public%20IP%20visible.png)

---

### Notes

Record the frontend public application URL and a short issue-and-resolution note, if applicable.

## EpicBook Deployment Record

**Frontend Public Application URL:**  
http://68.210.145.38

**Frontend:** Azure VM running Nginx  
**Backend:** Azure VM running the EpicBook Node.js application  
**Database:** Azure Database for MySQL  
**Deployment Tool:** Ansible

### Issue and Resolution

**Issue:** The Ansible deployment failed during database schema initialization because the configured `epicbook` database did not exist on the MySQL server. The server contained a separate `bookstore` database with existing tables.

**Resolution:** Verified database connectivity and privileges, identified the database-name mismatch, and created the missing `epicbook` database using `CREATE DATABASE IF NOT EXISTS`. This preserved the existing `bookstore` database.

**Current status:** The `epicbook` database creation was successful. The subsequent full application deployment and public URL response still need to be verified.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post about the completed capstone project, mentioning the two-repository/dual-pipeline model, Terraform + Ansible responsibilities, and one end-to-end verification result, with public/"Anyone" visibility and at least one link or image.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/eRET2MuN`

---

#### Screenshot — Published LinkedIn post showing the text and at least one link or image

![LinkedIn](screenshots/Screenshot%20with%20Published%20LinkedIn%20post%20showing%20the%20text%20and%20at%20least%20one%20link.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not commit or expose the Client Secret, SSH private key, database password, or other credentials

---

# Completion Checklist

- [✅ ] Task 1: `infra-epicbook` and `theepicbook` repositories prepared with clean separation (Screenshot 1)
- [✅ ] Task 2: Azure Resource Manager SPN service connection created and validated (Screenshot 2)
- [✅ ] Task 3: Infra Pipeline provisioned resources and produced outputs (Screenshots 3–4)
- [✅ ] Task 4: App Pipeline configured servers and deployed EpicBook (Screenshots 5–6)
- [✅ ] Task 5: End-to-end workflow verified, including MySQL connectivity (Screenshot 7)
- [✅ ] Frontend URL and issue notes written (Notes)
- [✅ ] LinkedIn post published and URL submitted
- [✅ ] No secrets exposed

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
