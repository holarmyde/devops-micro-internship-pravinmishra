# Assignment — Deploy EpicBook with Terraform and Ansible Roles

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will deploy the EpicBook web application using Terraform and Ansible roles.

Terraform provisions the cloud infrastructure, including one Ubuntu VM and one managed MySQL database. Ansible roles configure the VM, install required software, deploy the EpicBook application, configure Nginx, connect the app to the managed MySQL database, and verify the deployment.

---

# Task 1 — Set Up the Project Folder Layout

## Goal

Create the project folder structure for Terraform and Ansible roles.

Terraform will be used to provision the cloud infrastructure. Ansible roles will be used to configure the VM and deploy the EpicBook application.

### Evidence

<<<<<<< HEAD
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
=======
#### Screenshot 1 — Terminal showing the completed `epicbook-prod` project structure
>>>>>>> upstream/main

![task 10](screenshots/Screenshot%20with%20Terminal%20showing%20the%20ansible-playbook%20run%20.png)

---

### Notes

Answer the following in your own words:

<<<<<<< HEAD
During the EpicBook deployment, one of the main issues I encountered was a connection failure between the Node.js application and Azure Database for MySQL. The database required secure transport, but Sequelize was not initially using SSL when connecting to the database. This caused the application to repeatedly restart under PM2 with the error: “Connections using insecure transport are prohibited.” I resolved the issue by explicitly configuring Sequelize to use SSL and redeploying the application through Ansible. After the fix, PM2 remained online, the application successfully performed database operations, and Nginx successfully forwarded requests to the Node.js application.

This experience taught me that production environments require more than simply getting an application to run. Security settings must be correctly implemented across every layer of the application. I also learned the importance of automation, logging, and testing each layer independently. Ansible allowed me to consistently configure the server, Nginx, Node.js, PM2, and database connection. I verified the deployment by checking the PM2 process, Nginx service, application response, and database connectivity.

I identified several security issues that should be addressed before considering the system fully production-ready. Database credentials must remain protected and should never be stored in source code. Ansible Vault currently protects the database password, but production should use Azure Key Vault. The database should only accept connections from the application tier, and the Node.js port should remain private behind Nginx. SSH access should also be restricted and use secure key-based authentication.

My production remediation plan includes enabling HTTPS, enforcing TLS certificate validation, using private networking and firewall rules, implementing Azure Key Vault, restricting SSH access, and adding Azure Monitor and Log Analytics for monitoring and alerts. Finally, I would use multiple application instances or a managed service to eliminate the single-server dependency and improve availability.

=======
**1. Which cloud provider did you choose for this assignment?**

Add your answer here.
>>>>>>> upstream/main

---

**2. Why is it useful to keep Terraform files and Ansible files in separate folders?**

Add your answer here.

---

**3. What is the purpose of the `roles` directory in Ansible?**

Add your answer here.

---

# Task 2 — Provision the Infrastructure with Terraform

## Goal

Run Terraform to provision the cloud infrastructure for the EpicBook deployment.

Terraform will create the VM, managed MySQL database, networking, security rules, and required outputs.

### Evidence

#### Screenshot 2 — `terraform apply` completed successfully

Add your screenshot here.

---

#### Screenshot 3 — Output of `terraform output`

Add your screenshot here.

---

#### Screenshot 4 — Azure Portal or AWS Console showing the VM running

Add your screenshot here.

---

#### Screenshot 5 — Azure Portal or AWS Console showing the managed MySQL database created

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What resources did Terraform create for this assignment?**

Add your answer here.

---

**2. Why should you review `terraform plan` before running `terraform apply`?**

Add your answer here.

---

**3. Why should database passwords not be shown in Terraform output?**

Add your answer here.

---

# Task 3 — Verify SSH Key-Based Access

## Goal

Verify that the cloud VM can be accessed from the Ansible controller using SSH key-based authentication.

### Evidence

#### Screenshot 6 — Successful SSH hostname check from the Ansible controller

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What command did you use to verify SSH access?**

Add your answer here.

---

**2. What proves that SSH key-based access worked successfully?**

Add your answer here.

---

**3. What would you check if SSH returned `Permission denied (publickey)`?**

Add your answer here.

---

# Task 4 — Create the Ansible Inventory and Configuration

## Goal

Create the Ansible inventory file and local Ansible configuration for the EpicBook VM.

The inventory tells Ansible which VM to manage and which SSH user to use.

### Evidence

#### Screenshot 7 — `inventory.ini` showing the VM under the `web` group

Add your screenshot here.

---

#### Screenshot 8 — Output of `ansible-inventory -i inventory.ini --graph`

Add your screenshot here.

---

#### Screenshot 9 — Output of `ansible web -i inventory.ini -m ping`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is the purpose of `inventory.ini`?**

Add your answer here.

---

**2. What does `ansible_host` store?**

Add your answer here.

---

**3. What does `ansible_ssh_private_key_file` tell Ansible?**

Add your answer here.

---

**4. Why is `host_key_checking = False` used only for this temporary lab?**

Add your answer here.

---

# Task 5 — Create the Main Ansible Playbook

## Goal

Create the main Ansible playbook that runs the required roles in the correct order.

The `site.yml` file will call the `common`, `nginx`, and `epicbook` roles.

### Evidence

#### Screenshot 10 — `site.yml` showing the roles in the correct order

Add your screenshot here.

---

#### Screenshot 11 — Output of `ansible-playbook -i inventory.ini site.yml --syntax-check`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is the purpose of `site.yml`?**

Add your answer here.

---

**2. Why should the roles run in the order `common`, `nginx`, and `epicbook`?**

Add your answer here.

---

**3. What does `become: true` allow Ansible to do?**

Add your answer here.

---

# Task 6 — Create the `common` Role

## Goal

Create the `common` role to prepare the Ubuntu VM with the basic packages required for the EpicBook deployment.

This role handles the common server setup before Nginx and the application are configured.

### Evidence

#### Screenshot 12 — `roles/common/tasks/main.yml` showing the common setup tasks

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is the responsibility of the `common` role?**

Add your answer here.

---

**2. Why should Nginx installation not be placed inside the `common` role?**

Add your answer here.

---

**3. Why is `mysql-client` useful in this deployment?**

Add your answer here.

---

# Task 7 — Create the `nginx` Role

## Goal

Create the `nginx` role to install Nginx and configure it as a reverse proxy for the EpicBook application.

Nginx will receive browser traffic on port `80` and forward it to the EpicBook Node.js application running on the VM.

### Evidence

#### Screenshot 13 — `roles/nginx/tasks/main.yml` showing Nginx installation and site configuration tasks

Add your screenshot here.

---

#### Screenshot 14 — `roles/nginx/templates/epicbook.conf.j2` showing the reverse proxy configuration

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is the responsibility of the `nginx` role?**

Add your answer here.

---

**2. Why is Nginx configured as a reverse proxy in this deployment?**

Add your answer here.

---

**3. Why should the application port come from `group_vars/web.yml` instead of being hard-coded?**

Add your answer here.

---

# Task 8 — Create the `epicbook` Role

## Goal

Create the `epicbook` role to deploy the EpicBook application, connect it to the managed MySQL database, and run the application on port `8080` using PM2.

### Evidence

#### Screenshot 15 — `roles/epicbook/tasks/main.yml` showing application deployment tasks

Add your screenshot here.

---

#### Screenshot 16 — Task or file showing how the database connection is configured, with secrets hidden

Add your screenshot here.

---

#### Screenshot 17 — Task or output showing the EpicBook application managed by PM2

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is the responsibility of the `epicbook` role?**

Add your answer here.

---

**2. Why is PM2 used for the EpicBook Node.js application?**

Add your answer here.

---

**3. Why should database passwords not be hard-coded in public files?**

Add your answer here.

---

**4. What does it mean for the application to run on port `8080` while Nginx listens on port `80`?**

Add your answer here.

---

# Task 9 — Create Group Variables

## Goal

Create reusable variables for the EpicBook deployment.

The `group_vars/web.yml` file stores values that can be reused across the Ansible roles.

### Evidence

#### Screenshot 18 — `group_vars/web.yml` showing the application, PM2, and database variables, with passwords hidden or masked

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is the purpose of `group_vars/web.yml`?**

Add your answer here.

---

**2. Which values did you store in `group_vars/web.yml`?**

Add your answer here.

---

**3. How did you handle the database password securely?**

Add your answer here.

---

# Task 10 — Run the Ansible Playbook

## Goal

Run the Ansible playbook to configure the VM and deploy the EpicBook application.

The playbook should run the roles in this order:

1. `common`
2. `nginx`
3. `epicbook`

### Evidence

#### Screenshot 19 — Ansible playbook output showing the roles running

Add your screenshot here.

---

#### Screenshot 20 — Final Ansible recap showing `failed=0`

Add your screenshot here.

---

#### Screenshot 21 — Output of `ansible web -i inventory.ini -m command -a "systemctl is-active nginx" --become`

Add your screenshot here.

---

#### Screenshot 22 — Output of `ansible web -i inventory.ini -m command -a "pm2 status"`

Add your screenshot here.

---

#### Screenshot 23 — Output of `ansible web -i inventory.ini -m command -a "curl -I http://localhost:8080"`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What command did you run to execute the Ansible playbook?**

Add your answer here.

---

**2. How do you know all roles completed successfully?**

Add your answer here.

---

**3. What proves that Nginx is active?**

Add your answer here.

---

**4. What proves that PM2 is managing the EpicBook application?**

Add your answer here.

---

**5. What proves that the EpicBook application responds on port `8080`?**

Add your answer here.

---

# Task 11 — Verify the EpicBook Deployment

## Goal

Verify that the EpicBook application is running, accessible in the browser, and connected to the managed MySQL database.

### Evidence

#### Screenshot 24 — Output of `curl -I http://<public_ip>`

Add your screenshot here.

---

#### Screenshot 25 — Output of the cart API test command

Add your screenshot here.

---

#### Screenshot 26 — Output of the `/cart` HTTP status check

Add your screenshot here.

---

#### Screenshot 27 — Browser showing the EpicBook application loaded from `http://<public_ip>`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What HTTP response did you receive from the public application URL?**

Add your answer here.

---

**2. What did the cart API test prove?**

Add your answer here.

---

**3. What did the `/cart` status check return?**

Add your answer here.

---

**4. What issue did you face during verification, and how did you fix it?**

Add your answer here.

---

# LinkedIn Post Required

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/gtTgpBbK`

---

#### Screenshot — Published LinkedIn post

![linkedlIn](Screenshots/Screenshot%20linkedIn-5.png)

---

# Assignment Questions

<<<<<<< HEAD
![Video](Screenshots/Screenshot%20linkedIn-5.png)
=======
Answer the following in your own words:

**1. Why is Terraform used for infrastructure provisioning?**

Add your answer here.

---

**2. Why are Ansible roles useful for production-style deployments?**

Add your answer here.

---

**3. What is the purpose of `group_vars/web.yml`?**

Add your answer here.

---

**4. Why should database passwords not be committed to GitHub?**

Add your answer here.

---

**5. What is the purpose of Nginx in this deployment?**

Add your answer here.

---

**6. Why should the managed MySQL database not be publicly accessible?**

Add your answer here.

---

**7. Why is PM2 used for the EpicBook Node.js application?**

Add your answer here.

---

**8. What does idempotency mean in Ansible?**

Add your answer here.

---

**9. What issue did you face during the deployment, and how did you fix it?**

Add your answer here.

---

**10. What security improvement would you make before using this setup in production?**

Add your answer here.

---

# Required Files

Confirm that the following files are included in your GitHub repository or assignment folder:

- [ ] `README.md`
- [ ] Terraform files under either `terraform/azure/` or `terraform/aws/`
- [ ] `ansible/ansible.cfg`
- [ ] `ansible/inventory.ini`
- [ ] `ansible/site.yml`
- [ ] `ansible/group_vars/web.yml`
- [ ] `ansible/roles/common/tasks/main.yml`
- [ ] `ansible/roles/nginx/tasks/main.yml`
- [ ] `ansible/roles/nginx/templates/epicbook.conf.j2`
- [ ] `ansible/roles/epicbook/tasks/main.yml`
>>>>>>> upstream/main

---

# Submission Instructions

- Add all required screenshots in your submission.
- Full Name must be visible in required screenshots.
- Mention the cloud provider used: Azure or AWS.
- Add the VM public IP address.
- Add the final application URL.
- Add Terraform output proof.
- Add Ansible role tree proof.
- Add all required notes and assignment question answers.
- Add your LinkedIn post URL.
- Do not expose SSH private keys, passwords, cloud credentials, database credentials, Terraform state files, subscription IDs, or account IDs.

---

# Completion Checklist

<<<<<<< HEAD
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
=======
- [ ] Task 1: Project folder layout created
- [ ] Task 2: Terraform infrastructure provisioned
- [ ] Task 3: SSH key-based access verified
- [ ] Task 4: Ansible inventory and configuration created
- [ ] Task 5: Main Ansible playbook created
- [ ] Task 6: `common` role created
- [ ] Task 7: `nginx` role created
- [ ] Task 8: `epicbook` role created
- [ ] Task 9: Group variables created
- [ ] Task 10: Ansible playbook run completed
- [ ] Task 11: EpicBook deployment verified
- [ ] Terraform files created under only one cloud provider folder
- [ ] One Ubuntu VM was created
- [ ] One managed MySQL database was created
- [ ] SSH port `22` is restricted to the controller public IP
- [ ] HTTP port `80` is accessible
- [ ] MySQL port `3306` is not publicly open
- [ ] `ansible web -i inventory.ini -m ping` returns `SUCCESS`
- [ ] `site.yml` calls the roles in the correct order
- [ ] Database secrets are hidden or handled securely
- [ ] Nginx is active
- [ ] PM2 shows the EpicBook application running
- [ ] EpicBook responds on port `8080`
- [ ] Public URL loads in the browser
- [ ] Cart API verification works
- [ ] Playbook completes with `failed=0`
- [ ] Screenshots 1–27 are included
- [ ] Assignment questions are answered
- [ ] LinkedIn post published
- [ ] LinkedIn post URL added
- [ ] No sensitive information is exposed
>>>>>>> upstream/main

---

## About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra and The CloudAdvisory, focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## Resources

- DMI Official Website: [https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme](https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme)
- University: [https://university.pravinmishra.com?utm_source=github&utm_medium=readme](https://university.pravinmishra.com?utm_source=github&utm_medium=readme)
- Discord Community: [https://discord.pravinmishra.com?utm_source=github&utm_medium=readme](https://discord.pravinmishra.com?utm_source=github&utm_medium=readme)
- Blog: [https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme](https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme)
- YouTube Playlist: [https://www.youtube.com/playlist?list=PLFeSNDtI4Cho](https://www.youtube.com/playlist?list=PLFeSNDtI4Cho)
- Pravin Mishra LinkedIn: [https://www.linkedin.com/in/pravin-mishra-aws-trainer/](https://www.linkedin.com/in/pravin-mishra-aws-trainer/)
- CloudAdvisory LinkedIn: [https://www.linkedin.com/company/thecloudadvisory/](https://www.linkedin.com/company/thecloudadvisory/)

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*