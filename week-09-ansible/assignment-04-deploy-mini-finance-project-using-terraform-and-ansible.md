# Assignment 4 — Deploy Mini Finance Project Using Terraform and Ansible

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will provision an Azure VM with Terraform and use Ansible to automate the install, deploy, and verify workflow for the Mini Finance static website — a clean separation between infrastructure and configuration management.

---

# Task 1 — Set Up Folder Layout

## Goal

Create the `mini-finance` project with separate `terraform/` and `ansible/` subdirectories.

### Evidence

#### Screenshot 1 — Terminal or editor showing the complete `mini-finance` project tree

![task 1](screenshots/Screenshot%20with%20Terminal%20or%20editor%20showing%20the%20complete%20mini-finance%20project%20tree.png)

---

# Task 2 — Terraform — Azure VM + NSG (Ports 22/80)

## Goal

Provision an Ubuntu 22.04 Standard_B1s VM with a public IP, SSH key authentication, and an NSG allowing SSH (22) and HTTP (80), and output the public IP.

### Evidence

#### Screenshot 2 — Terminal showing the end of a successful `terraform apply`

![task 1](screenshots/Screenshot%20with%20Terminal%20showing%20the%20end%20of%20a%20successful%20terraform%20apply.png)

---

#### Screenshot 3 — Terminal showing `terraform output public_ip`

![task 2](screenshots/Screenshot%20with%20Terminal%20showing%20terraform%20output%20public_ip.png)

---

#### Screenshot 4 — Terraform code or Azure Portal showing NSG inbound rules for ports 22 and 80

![task 2](screenshots/Screenshot%20with%20Azure%20Portal%20showing%20NSG%20rules%20for%20ports%2022%20and%2080.png)

---

# Task 3 — Configure Passwordless SSH

## Goal

Connect to the VM with SSH using the injected key and run `hostname` remotely without a password prompt.

### Evidence

#### Screenshot 5 — Terminal showing the successful passwordless SSH hostname check

![task 3](screenshots/Screenshot%20with%20Terminal%20showing%20the%20successful%20passwordless%20SSH%20hostname%20.png)

---

# Task 4 — Ansible — Multi-Play: Install → Deploy → Verify

## Goal

Create `ansible/inventory.ini` and a three-play `site.yml` that installs Nginx and Git, clones and deploys the Mini Finance repository to `/var/www/html/` with a reload handler, and verifies HTTP 200 from `localhost`.

### Evidence

#### Screenshot 6 — Editor showing `inventory.ini` and the three plays in `site.yml`

![task 4](screenshots/Screenshot%20with%20Editor%20showing%20inventory.ini%20and%20the%20three%20plays.png)

---

#### Screenshot 7 — Terminal showing `ansible-playbook -i inventory.ini site.yml` with HTTP 200, assertion OK, and no failures

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20ansible-playbook%20-i%20inventory.ini%20site_yml.png)

---

# Task 5 — Test End-to-End Functionality

## Goal

Confirm the Mini Finance site is publicly accessible and correctly served by Nginx.

### Evidence

#### Screenshot 8 — Browser showing the Mini Finance site loaded from `http://<public_ip>` with the URL visible

![task 5](screenshots/Screenshot%20with%20Browser%20showing%20the%20Mini%20Finance%20site%20loaded.png)

---

### Notes

Describe an issue you faced and how you fixed it, and what you learned.

One issue I faced during the Mini Finance deployment was that the Ansible playbook kept hanging when it tried to clone the GitHub repository. I first tested the connection from the Azure VM and confirmed that DNS and HTTPS access to GitHub were working. I then discovered that the repository URL provided in my playbook was not accessible, while the correct repository was https://github.com/pravinmishraaws/mini_finance. I updated the repository URL in site.yml and reran the playbook. The deployment then completed successfully, including synchronizing the application to /var/www/html/, reloading Nginx, and passing the HTTP 200 validation.

This taught me the importance of troubleshooting each layer separately instead of assuming that a deployment failure is caused by Ansible itself. I learned to verify DNS, network connectivity, Git access, and the repository URL independently before changing the playbook. I also learned that small configuration details, such as an incorrect repository URL, can stop an otherwise correctly configured automation workflow

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post about the Terraform + Ansible deployment, mentioning the Azure VM, secure networking, passwordless SSH, Nginx deployment, and HTTP verification, with one challenge you faced and how you fixed it, and one real-world example of this workflow.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/gzGZV9Tb`

---

#### Screenshot — Published LinkedIn post showing the text and at least one image or proof

![LinkedIn](screenshots/Screenshot%20with%20linkedIn%20post.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- The `mini-finance` project tree and `inventory.ini` may have the final IP octet masked
- Do not expose private keys or other secrets

---

# Completion Checklist

- [✅] Task 1: `mini-finance` project structure created (Screenshot 1)
- [✅] Task 2: Azure VM and NSG provisioned with Terraform (Screenshots 2–4)
- [✅] Task 3: Passwordless SSH verified (Screenshot 5)
- [✅] Task 4: Ansible install/deploy/verify plays run successfully (Screenshots 6–7)
- [✅] Task 5: Site verified in the browser (Screenshot 8)
- [✅] Reflection notes written (Notes)
- [✅] LinkedIn post published and URL submitted
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
