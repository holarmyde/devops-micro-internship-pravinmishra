# Assignment 5 — Production-Grade EpicBook: Terraform + Ansible Roles (Azure or AWS)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will deploy the EpicBook web application on a cloud VM provisioned with Terraform (Azure or AWS — pick one) and configured through reusable Ansible roles (`common`, `nginx`, `epicbook`) orchestrated by one playbook, using group variables, templates, and handlers, with a verified idempotent second run.

---

# Task 1 — Set Up Folder Layout

## Goal

Create the `epicbook-prod` project with `terraform/azure` or `terraform/aws`, `ansible/inventory.ini`, `ansible/site.yml`, `ansible/group_vars/web.yml`, and the `common`, `nginx`, and `epicbook` role directories.

### Evidence

#### Screenshot 1 — Terminal or editor showing the complete `epicbook-prod` project tree

![task 1](screenshots/Screenshot%20with%20Terminal%20or%20editor%20showing%20the%20complete%20epicbook-prod.png)

---

# Task 2 — Terraform (Pick One: Azure or AWS)

## Goal

Provision one secure Ubuntu 22.04 VM with SSH key authentication, inbound SSH (22) and HTTP (80), and `public_ip`/`admin_user` outputs, on your chosen cloud.

### Evidence

#### Screenshot 2 — Terminal showing successful `terraform apply` and `terraform output` with `public_ip` and `admin_user`

![task 2](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20terraform%20apply%20and%20terraform%20output.png)

---

#### Screenshot 3 — Terraform code or cloud console showing inbound rules for ports 22 and 80

![task 2](screenshots/Screenshot%20with%20Terraform%20code%20or%20cloud%20console%20showing%20inbound%20rules.png)

---

# Task 3 — Ansible Inventory

## Goal

Create the `[web]` inventory using the Terraform `public_ip` and `admin_user` outputs, and verify passwordless SSH and `ansible ping`.

### Evidence

#### Screenshot 4 — Terminal showing the successful passwordless SSH hostname check

![task 3](screenshots/Screenshot%20with%20Terminal%20showing%20the%20successful%20passwordless%20SSH%20hostname%20check.png)

---

#### Screenshot 5 — Editor or terminal showing `inventory.ini` and a successful Ansible ping

![task 3](screenshots/Screenshot%20with%20Editor%20or%20terminal%20showing%20a%20successful%20Ansible%20ping.png)

---

# Task 4 — Create site.yml (Role Orchestration)

## Goal

Create `site.yml` invoking the `common`, `nginx`, and `epicbook` roles in that exact order.

### Evidence

#### Screenshot 6 — Editor showing `ansible/site.yml` with the three roles in the required order

![task 4](screenshots/Screenshot%20with%20Editor%20showing%20ansible_site.yml%20with%20the%20three%20roles.png)

---

# Task 5 — Role: common

## Goal

Create `roles/common/tasks/main.yml` to update apt, upgrade packages, install baseline packages (`git`, `curl`, `unzip`, `software-properties-common`), with optional SSH hardening applied only after key-based access is confirmed.

### Evidence

#### Screenshot 7 — Editor showing `roles/common/tasks/main.yml`

![task 4](screenshots/Screenshot%20with%20Editor%20showing%20ansible_site.yml%20with%20the%20three%20roles.png)

---

# Task 6 — Role: nginx

## Goal

Create the `nginx` role to install Nginx, deploy the `epicbook.conf.j2` template to `/etc/nginx/sites-available/epicbook`, enable the site, remove the default site, and reload via handler.

### Evidence

#### Screenshot 8 — Editor showing the Nginx role tasks, handler, and `epicbook.conf.j2` template

![task 6](screenshots/Screenshot%20with%20Editor%20showing%20the%20Nginx%20role%20tasks%20handler.png)

---

#### Screenshot 9 — Terminal showing `/etc/nginx/sites-available/epicbook` and a successful Nginx configuration test

![task 6](screenshots/Screenshot%20with%20Terminal%20showing%20_etc_nginx_sites-available_epicbook.png)

---

# Task 7 — Role: epicbook

## Goal

Create the `epicbook` role to clone the repository to `{{ app_dest }}`, set ownership/permissions using group variables, and notify the Nginx reload handler on change.

### Evidence

#### Screenshot 10 — Editor showing `roles/epicbook/tasks/main.yml`

![task 7](screenshots/Screenshot%20with%20Editor%20showing%20roles_epicbook_tasks_main.yml.png)

---

# Task 8 — Group Variables

## Goal

Define `app_repo`, `app_dest`, `app_user`, and `app_group` in `ansible/group_vars/web.yml`.

### Evidence

#### Screenshot 11 — Editor showing `ansible/group_vars/web.yml`

![task 8](screenshots/Screenshot%20with%20Editor%20showing%20ansible_site.yml%20with%20the%20three%20roles.png)

---

# Task 9 — Run the Playbook

## Goal

Run `ansible-playbook -i inventory.ini site.yml` and confirm `common` → `nginx` → `epicbook` all complete with `failed=0`.

### Evidence

#### Screenshot 12 — Terminal showing the role-based Ansible run and final recap with `failed=0`

![task 9](screenshots/Screenshot%20with%20Terminal%20showing%20the%20ansible-playbook%20run%20.png)

---

# Task 10 — Verify

## Goal

Confirm the EpicBook site loads with HTTP 200, inspect the Nginx configuration, and rerun the playbook to confirm the second run is mostly OK/UNCHANGED with `failed=0`.

### Evidence

#### Screenshot 13 — Browser showing the EpicBook site with the public IP visible

![task 10](screenshots/Screenshot%20with%20Browser%20showing%20the%20EpicBook%20site%20with%20the%20public%20IP%20visible.png)

---

#### Screenshot 14 — Terminal showing HTTP 200 and the Nginx site-file snippet

![task 10](screenshots/Screenshot%20with%20Terminal%20showing%20HTTP%20200%20and%20the%20Nginx%20site-file%20snippet.png)

---

#### Screenshot 15 — Terminal showing the idempotent second Ansible run with mostly OK/UNCHANGED and `failed=0`

![task 10](screenshots/Screenshot%20with%20Terminal%20showing%20the%20ansible-playbook%20run%20.png)

---

### Notes

Describe an issue you faced and how you fixed it, what you learned, any security issues you identified, and your production remediation plan.

During the EpicBook deployment, one of the main issues I encountered was a connection failure between the Node.js application and Azure Database for MySQL. The database required secure transport, but Sequelize was not initially using SSL when connecting to the database. This caused the application to repeatedly restart under PM2 with the error: “Connections using insecure transport are prohibited.” I resolved the issue by explicitly configuring Sequelize to use SSL and redeploying the application through Ansible. After the fix, PM2 remained online, the application successfully performed database operations, and Nginx successfully forwarded requests to the Node.js application.

This experience taught me that production environments require more than simply getting an application to run. Security settings must be correctly implemented across every layer of the application. I also learned the importance of automation, logging, and testing each layer independently. Ansible allowed me to consistently configure the server, Nginx, Node.js, PM2, and database connection. I verified the deployment by checking the PM2 process, Nginx service, application response, and database connectivity.

I identified several security issues that should be addressed before considering the system fully production-ready. Database credentials must remain protected and should never be stored in source code. Ansible Vault currently protects the database password, but production should use Azure Key Vault. The database should only accept connections from the application tier, and the Node.js port should remain private behind Nginx. SSH access should also be restricted and use secure key-based authentication.

My production remediation plan includes enabling HTTPS, enforcing TLS certificate validation, using private networking and firewall rules, implementing Azure Key Vault, restricting SSH access, and adding Azure Monitor and Log Analytics for monitoring and alerts. Finally, I would use multiple application instances or a managed service to eliminate the single-server dependency and improve availability.


---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post describing the Terraform + Ansible roles deployment (cloud chosen, role structure, Nginx deployment, idempotency result), and add a 4–6 line video reflection covering one challenge/fix, security issues observed, and your production remediation plan.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/gtTgpBbK`

---

#### Screenshot — Published LinkedIn post

![linkedlIn](Screenshots/Screenshot%20linkedIn-5.png)

---

#### Video reflection screenshot

![Video](Screenshots/Screenshot%20linkedIn-5.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not expose private keys, credentials, tokens, or unrestricted management access

---

# Completion Checklist

- [✅] Task 1: `epicbook-prod` project and role structure created (Screenshot 1)
- [✅] Task 2: Cloud VM provisioned with Terraform (Screenshots 2–3)
- [✅] Task 3: Passwordless SSH and Ansible ping verified (Screenshots 4–5)
- [✅] Task 4: `site.yml` orchestrates roles in common → nginx → epicbook order (Screenshot 6)
- [✅] Task 5: `common` role created (Screenshot 7)
- [✅] Task 6: `nginx` role, template, and handler created (Screenshots 8–9)
- [✅] Task 7: `epicbook` role created (Screenshot 10)
- [✅] Task 8: Group variables defined (Screenshot 11)
- [✅] Task 9: Playbook run successfully with `failed=0` (Screenshot 12)
- [✅] Task 10: Site verified and idempotent rerun confirmed (Screenshots 13–15)
- [✅] Reflection and security remediation notes written (Notes)
- [✅] LinkedIn post and video reflection submitted
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

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
