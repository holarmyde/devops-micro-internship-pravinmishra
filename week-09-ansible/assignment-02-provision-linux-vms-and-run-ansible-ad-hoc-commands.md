# Assignment 02 — Provision Linux VMs with Terraform and Run Ansible Ad-Hoc Commands

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will use Terraform to provision three or four Ubuntu Linux Virtual Machines on either Microsoft Azure or Amazon Web Services.

You will configure SSH key-based authentication, organize the servers using a custom Ansible inventory, and run Ansible ad-hoc commands across individual hosts and inventory groups.

---

# Task 1 — Create the Multi-Host Lab Structure

## Goal

Create a separate project directory for the multi-host lab and prepare the Terraform, Ansible, and documentation files.

This project will use the Git repository and Ansible controller prepared in Assignment 01.

### Evidence

#### Screenshot 1 — Terminal showing the complete `ansible-adhoc-lab` project structure

![task 1](screenshots/Screenshot%20with%20terraform%20apply%20output%20and%20terraform%20output%20public_ips.png)

---

#### Screenshot 2 — Terminal showing `git status --short` with the new project files and updated `.gitignore`

![task 1](screenshots/Screenshot%20with%20the%20four%20runnung%20ubuntus%20VM.png)

---

### Notes

The first task involved creating a dedicated project directory named ansible-adhoc-lab to organize the Terraform and Ansible configuration files.

I structured the project to separate infrastructure provisioning, Ansible inventory management, and documentation. I also prepared a .gitignore file to prevent sensitive information, such as private SSH keys, Terraform state files, and local configuration files, from being committed to Git.

This structure makes the project easier to maintain, troubleshoot, and share.

 project structure:
ansible-adhoc-lab/
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── terraform.tfvars
├── ansible/
│   └── inventory.ini
├── README.md
└── .gitignore

---

# Task 2 — Create the Terraform Configuration

## Goal

Create the Terraform configuration required to provision three or four Ubuntu Linux VMs on your selected cloud platform.

Complete only one option:

- Option A — Microsoft Azure
- Option B — Amazon Web Services

Do not configure both providers for this assignment.

### Evidence

#### Screenshot 3 — Terraform configuration showing the three or four server roles and the `for_each` or `count` implementation

![task 1](screenshots/Screenshot%20with%20Network%20Security%20Group%20inbound%20rules%20showing%20SSH%2022%20and%20HTTP%2080.png)

---

#### Screenshot 4 — Terraform configuration showing SSH restricted to the controller IP and HTTP allowed only for web hosts

![task 2](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20hostname%20output%20.png)

---

#### Screenshot 5 — Terraform output configuration showing how public IP addresses are associated with the server roles

![task 3](screenshots/Screenshot%20with%20Editor%20or%20terminal%20showing%20inventory.ini.png)

---

### Notes

In this task, I created Terraform configuration files using the Azure provider to define the cloud infrastructure.

I designed the configuration to provision four Ubuntu Linux virtual machines representing different infrastructure roles: two web servers, one application server, and one database server.

I used Terraform's for_each functionality to avoid repeating the same VM configuration. Each VM receives a role-based name and its own public IP address.

I also configured network security rules to restrict SSH access to the Ansible controller's public IP address and allow HTTP traffic only to the web servers.

This approach demonstrates infrastructure automation, reusable Terraform configurations, and basic cloud security practices.

Four-server design:
Server	Role	Purpose
web-1	Web	Hosts web applications
web-2	Web	Hosts web applications
app-1	App	Handles application processing
db-1	DB	Database services

---

# Task 3 — Provision the Infrastructure with Terraform

## Goal

Initialize and validate the Terraform configuration, review the execution plan, provision the selected three or four VMs, and retrieve their public IP addresses.

### Evidence

#### Screenshot 6 — Final `terraform apply` output showing `Apply complete`

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20ansible%20ping%20SUCCESS%20for%20all%20four%20hosts.png)


---

#### Screenshot 7 — `terraform output public_ips` showing the role-to-IP mapping for all three or four VMs

![task 4](screenshots/Screenshot%20with%20%20Terminal%20showing%20uptime%20output%20for%20all%20four%20hosts.png)

---

#### Screenshot 8 — Azure Portal or AWS Management Console showing all three or four VMs in the `Running` state, with their role-based names visible

![task 3](screenshots/Screenshot%20with%20Terminal%20showing%20Nginx%20installation%20and%20service.png)

---

### Notes

After completing the Terraform configuration, I initialized the Terraform working directory and validated the configuration to identify any syntax or configuration errors.

I used terraform plan to review the infrastructure changes before deployment. After confirming the proposed resources, I executed terraform apply to provision the four Ubuntu virtual machines on Microsoft Azure.

Terraform successfully created the infrastructure resources and generated public IP address outputs associated with the different server roles.

This task demonstrated how Infrastructure as Code can automate the deployment of multiple cloud servers consistently and efficiently.

Commands to execute:
cd ~/ansible-adhoc-lab/terraform

terraform init

terraform fmt

terraform validate

terraform plan

terraform apply

---

# Task 4 — Verify SSH Key-Based Access

## Goal

Verify that each managed VM can be accessed from the Ansible controller using SSH key-based authentication.

### Evidence

#### Screenshot 9 — Terminal showing successful SSH hostname output from all VMs

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20htop%20installation.png)

---

### Notes

In this task, I verified SSH connectivity between my Ansible controller and the four provisioned Ubuntu virtual machines.

I configured SSH key-based authentication so that the controller could securely connect to the managed servers without requiring password authentication for every connection.

I tested each server individually using the SSH command and the hostname utility.

Successful hostname responses confirmed that the virtual machines were reachable and that SSH authentication was working correctly.

Commands:

First, retrieve the IP addresses from Terraform.

Then test each VM:
ssh -i ~/.ssh/your_private_key \
azureadmin@WEB1_PUBLIC_IP hostname


Repeat for the other three VMs, replacing the IP address.

For convenience, you can also test all four in one loop:
for ip in WEB1_IP WEB2_IP APP1_IP DB1_IP
do
  ssh -i ~/.ssh/your_private_key \
  -o StrictHostKeyChecking=accept-new \
  azureadmin@$ip hostname
done

---

# Task 5 — Create the Custom Ansible Inventory

## Goal

Create an Ansible inventory file that groups the managed VMs by role.

The inventory allows Ansible to run commands against all servers, or only specific groups such as `web`, `app`, or `db`.

### Evidence

#### Screenshot 10 — `inventory.ini` showing the `web`, `app`, and `db` groups

![task 5](screenshots/Screenshot%20with%20Editor%20showing%20inventory.ini%20and%20the%20three%20plays.png)

---

#### Screenshot 11 — Output of `ansible-inventory -i inventory.ini --graph`

![task 5](screenshots/Screenshot%20with%20Terminal%20showing%20ansible-playbook%20-i%20inventory.ini%20site_yml.png)
---

### Notes

In this task, I created a custom Ansible inventory file named inventory.ini to organize the managed virtual machines according to their infrastructure roles.

I created three inventory groups: web, app, and db.

Each group contains the public IP addresses of the corresponding virtual machines. I also configured the SSH connection details, including the remote username and private key location.

The inventory allows Ansible to target individual servers, specific groups, or all managed servers without manually specifying their IP addresses every time.

This improves infrastructure organization and makes configuration management more efficient.

Example inventory.ini:
[web]
web-1 ansible_host=WEB1_PUBLIC_IP
web-2 ansible_host=WEB2_PUBLIC_IP

[app]
app-1 ansible_host=APP1_PUBLIC_IP

[db]
db-1 ansible_host=DB1_PUBLIC_IP

[all:vars]
ansible_user=azureadmin
ansible_ssh_private_key_file=~/.ssh/your_private_key
ansible_python_interpreter=/usr/bin/python3


Replace the placeholders with your actual Terraform outputs.

---

# Task 6 — Run Ansible Ad-Hoc Commands

## Goal

Run Ansible ad-hoc commands from the controller to verify connectivity, check server information, and manage packages and services across inventory groups.

This task proves that the inventory is working and that Ansible can control multiple managed VMs without writing a playbook.

### Evidence

#### Screenshot 12 — Output of `ansible all -i inventory.ini -m ping`

![task 6](screenshots/Screenshot%20with%20Terminal%20showing%20ansible%20all%20-m%20ping%20with%20every%20host%20reachable.png)

---

#### Screenshot 13 — Output of `ansible all -i inventory.ini -m command -a "uptime"`

![task 6](screenshots/Screenshot%20with%20Terminal%20showing%20ansible%20all%20-m%20ping%20with%20every%20host%20reachable.png)

---

#### Screenshot 14 — Output of `ansible web -i inventory.ini -m apt -a "name=nginx state=present update_cache=yes" --become`

![task 6](screenshots/Screenshot%20with%20Terminal%20showing%20ansible%20all%20-m%20ping%20with%20every%20host%20reachable.png)

---

#### Screenshot 15 — Output of `ansible web -i inventory.ini -m service -a "name=nginx state=started enabled=yes" --become`

![task 6](screenshots/Screenshot%20with%20Terminal%20showing%20ansible%20all%20-m%20ping%20with%20every%20host%20reachable.png)

---

#### Screenshot 16 — Output of `ansible all -i inventory.ini -m apt -a "name=htop state=present update_cache=yes" --become`

![task 6](screenshots/Screenshot%20with%20Terminal%20showing%20ansible%20all%20-m%20ping%20with%20every%20host%20reachable.png)

---

#### Screenshot 17 — Output of `ansible web -i inventory.ini -m command -a "systemctl is-active nginx"`

![task 6](screenshots/Screenshot%20with%20Terminal%20showing%20ansible-playbook%20-i%20inventory.ini%20site_yml.png)

---

### Notes

In this task, I used Ansible ad-hoc commands to execute administrative operations across the managed Ubuntu virtual machines.

First, I used the Ansible ping module to verify connectivity between the controller and all four managed servers.

I then executed the uptime command to retrieve server availability information.

Next, I installed and started the NGINX web server on the web server group using Ansible's apt and service modules.

Finally, I installed the htop monitoring utility across all managed servers and checked the NGINX service status.

These exercises demonstrated how Ansible can perform remote administration, package management, and service management across multiple servers without requiring a playbook.

Commands and their purposes:

1. Verify connectivity across all servers
ansible all -i inventory.ini -m ping

---

# LinkedIn Post Required

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/eXHYQvAG`

---

#### Screenshot — Published LinkedIn post

![linkedIn](screenshots/Screenshot%20with%20linkedIn%20post_terraform.png)

---

# Assignment Questions

Answer the following in your own words:

**1. What is the purpose of an Ansible inventory file?**

An Ansible inventory file identifies and organizes the servers that Ansible manages. It contains information such as server IP addresses, hostnames, connection usernames, and SSH key locations.

It also allows servers to be grouped according to their roles, making it easier to execute commands against individual servers, specific groups, or all servers at once.

---

**2. What is the difference between the `web`, `app`, and `db` groups in your inventory?**

The three groups represent different server responsibilities within the infrastructure.
- Web: Contains web servers responsible for hosting websites and handling HTTP requests. In this assignment, these servers run NGINX.
- App: Contains application servers responsible for running application logic and processing requests from the web layer.
- DB: Contains database servers responsible for storing, managing, and retrieving application data.
Separating servers into groups makes infrastructure management easier and allows different configurations to be applied to each server role.

---

**3. What does the Ansible `ping` module verify?**

The Ansible ping module checks whether Ansible can successfully communicate with a managed server.

It verifies that the server is reachable, SSH authentication works, and Python is available to execute the module on the remote machine.

A successful response returns pong, confirming that Ansible can communicate with the managed host.

---

**4. Why do package installation commands require `--become`?**

Package installation commands require --become because installing software or modifying system packages requires administrative privileges.

The --become option allows Ansible to execute commands with elevated privileges, usually through sudo, instead of requiring the user to log in directly as root.

For example, installing NGINX or htop requires administrative access.

---

**5. When would you use an ad-hoc command instead of a playbook?**

I would use an Ansible ad-hoc command when I need to perform a quick, one-time administrative operation without creating a complete playbook.

For example, I can use an ad-hoc command to check server uptime, test connectivity, install a package, restart a service, or retrieve system information across multiple servers.

For more complex, repetitive, and multi-step configuration tasks, I would use an Ansible playbook because it provides a reusable and organized way to automate operations.

---

**6. What is one challenge you faced while setting up SSH or inventory, and how did you fix it?**

One challenge I encountered was ensuring that the Ansible controller could connect to all managed virtual machines using SSH key-based authentication.

Incorrect SSH key paths, file permissions, usernames, or inventory IP addresses can prevent successful connections.

I addressed this by checking the SSH private key location, ensuring the correct remote username and public IP addresses were configured in the inventory, and verifying that the SSH key had the appropriate permissions.

After correcting the configuration, I tested the connections using SSH and the Ansible ping module until all managed servers returned successful responses.

---

# Required Files

Confirm that the following files are included in your assignment workspace:

- [✅] `ansible-adhoc-lab/README.md`
- [✅] `ansible-adhoc-lab/terraform/providers.tf`
- [✅] `ansible-adhoc-lab/terraform/main.tf`
- [✅] `ansible-adhoc-lab/terraform/variables.tf`
- [✅] `ansible-adhoc-lab/terraform/outputs.tf`
- [✅] `ansible-adhoc-lab/ansible/inventory.ini`
- [✅] Updated `.gitignore`

---

# Submission Instructions

- Add all required screenshots from the tasks.
- Full Name must be visible in required screenshots.
- Mention whether you used Azure or AWS.
- Mention whether you used the three-VM option or four-VM option.
- Add the public IP addresses of the VMs, redacted if preferred.
- Add your `inventory.ini` proof.
- Add a short explanation of what you learned.
- Answer all assignment questions clearly in your own words.
- Add your LinkedIn post URL.
- Do not expose SSH private keys, Terraform state files, cloud credentials, passwords, access keys, secret keys, account IDs, or subscription IDs.

---

# Completion Checklist

- [✅] Task 1: `ansible-adhoc-lab` project structure created
- [✅] Task 1: `.gitignore` updated for Terraform files
- [✅] Task 2: Terraform configuration created
- [✅] Task 2: Server roles defined for either three or four VMs
- [✅] Task 2: `count` or `for_each` used
- [✅] Task 2: SSH restricted to the controller public IP
- [✅] Task 2: HTTP allowed only for web hosts
- [✅] Task 2: Terraform output maps roles to public IPs
- [✅] Task 3: Terraform initialized successfully
- [✅] Task 3: Terraform configuration validated
- [✅] Task 3: Terraform apply completed successfully
- [✅] Task 3: All selected VMs are running
- [✅] Task 4: SSH key-based access works for every VM
- [✅] Task 5: `inventory.ini` contains `web`, `app`, and `db` groups
- [✅] Task 5: `ansible-inventory -i inventory.ini --graph` shows the correct groups
- [✅] Task 6: `ansible all -i inventory.ini -m ping` returns `SUCCESS`
- [✅] Task 6: Ad-hoc commands run successfully
- [✅] Task 6: `--become` was used for package and service tasks
- [✅] Task 6: Nginx is active on the `web` group
- [✅] Screenshots 1–17 are included
- [✅] Assignment questions are answered
- [✅] LinkedIn post published
- [✅] LinkedIn post URL added
- [✅] No sensitive information is exposed

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