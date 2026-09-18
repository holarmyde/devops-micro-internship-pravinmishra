# Assignment 2 — Ad-Hoc Automation on Azure: 4 VMs, Inventory & Passwordless SSH

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will provision four Azure Linux VMs with Terraform, configure passwordless SSH, build a custom Ansible inventory with web/app/db groups, and run ad-hoc commands across individual hosts and groups.

---

# Task 1 — Provision 4 Azure VMs (Terraform)

## Goal

Provision four Ubuntu 22.04 VMs (`web1`, `web2`, `app1`, `db1`, Standard_B1s) with SSH key authentication and public IPs, in a VNet with an NSG allowing SSH (22) and HTTP (80), and output all four public IPs.

### Evidence

#### Screenshot 1 — Terminal showing successful `terraform apply` output and `terraform output public_ips`

![task 1](screenshots/Screenshot%20with%20terraform%20apply%20output%20and%20terraform%20output%20public_ips.png)

---

#### Screenshot 2 — Azure Portal showing all four running Ubuntu VMs

![task 1](screenshots/Screenshot%20with%20the%20four%20runnung%20ubuntus%20VM.png)

---

#### Screenshot 3 — Network Security Group inbound rules showing SSH 22 and HTTP 80

![task 1](screenshots/Screenshot%20with%20Network%20Security%20Group%20inbound%20rules%20showing%20SSH%2022%20and%20HTTP%2080.png)

---

# Task 2 — Configure Passwordless SSH

## Goal

Connect to each of the four VMs as `azureuser` and run `hostname` remotely without a password prompt.

### Evidence

#### Screenshot 4 — Terminal showing successful `hostname` output from all four passwordless SSH tests

![task 2](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20hostname%20output%20.png)

---

# Task 3 — Create a Custom Ansible Inventory

## Goal

Create `inventory.ini` mapping VM indices 0–1 to `[web]`, index 2 to `[app]`, and index 3 to `[db]`, with `ansible_user` and `ansible_ssh_private_key_file` set under `[all:vars]`.

### Evidence

#### Screenshot 5 — Editor or terminal showing `inventory.ini` with the web, app, db, and all:vars sections

![task 3](screenshots/Screenshot%20with%20Editor%20or%20terminal%20showing%20inventory.ini.png)

---

# Task 4 — Run Your First Ansible Ad-Hoc Commands

## Goal

Run `ping`, `whoami`, and `uptime` against all hosts; install and start Nginx on the `web` group with `--become`; install `htop` on all hosts; and run `df -h` on `db` and `free -m` on all hosts.

### Evidence

#### Screenshot 6 — Terminal showing `ansible ping` SUCCESS for all four hosts

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20ansible%20ping%20SUCCESS%20for%20all%20four%20hosts.png)

---

#### Screenshot 7 — Terminal showing `uptime` output for all four hosts

![task 4](screenshots/Screenshot%20with%20%20Terminal%20showing%20uptime%20output%20for%20all%20four%20hosts.png)

---

#### Screenshot 8 — Terminal showing Nginx installation and service start on the web group

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20Nginx%20installation%20and%20service.png)

---

#### Screenshot 9 — Terminal showing `htop` installation on all hosts and group-targeted command output

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20htop%20installation.png)

---

### Notes

Describe an issue you faced and how you fixed it, what you learned, when you'd use an ad-hoc command instead of a playbook, and one challenge you faced during SSH or inventory setup.

### Issue I Faced and How I Fixed It

One issue I faced was SSH authentication between WSL and the Azure VMs. Terraform had used the Windows SSH public key, while Ansible in WSL was initially using a different WSL SSH key. Because the keys did not match, SSH authentication failed. I fixed this by copying the Windows private key into WSL as `~/.ssh/azure-ansible-key`, setting the correct permissions, and updating `inventory.ini` to use that key. After that, Ansible successfully connected to all three VMs.

### What I Learned

I learned how Terraform can create multiple Azure VMs using `for_each` instead of repeating resource blocks, and how Ansible inventory groups can be used to target specific servers. I also learned the importance of using the correct SSH key between the Terraform-created VMs and the Ansible controller.

### Ad-Hoc Commands vs. Playbooks

I would use an Ansible ad-hoc command for quick, one-time tasks such as checking connectivity with `ping`, installing a package such as `htop`, checking a service, or gathering basic system information. I would use a playbook when I need repeatable, organized, multi-step configuration or deployment across multiple hosts.

### SSH and Inventory Challenge

Another challenge was host key verification when Ansible first connected to the `app1` and `db1` servers. Ansible stopped with a host key verification error. I addressed this for the lab by setting `ansible_host_key_checking=False` in the inventory. I also verified that the inventory contained the correct public IP addresses, usernames, and SSH private-key path for all three hosts.


---

# Submission Instructions

- Add all required screenshots in your submission
- Public IP addresses may be redacted
- Do not expose, upload, or commit the SSH private key

---

# Completion Checklist

- [✅] Task 1: Four Azure VMs provisioned with Terraform (Screenshots 1–3)
- [✅] Task 2: Passwordless SSH verified on all four VMs (Screenshot 4)
- [✅] Task 3: `inventory.ini` created with web/app/db groups (Screenshot 5)
- [✅] Task 4: Ad-hoc ping, uptime, Nginx, and htop commands run successfully (Screenshots 6–9)
- [✅] Reflection notes written (Notes)
- [✅] No private key material exposed

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
