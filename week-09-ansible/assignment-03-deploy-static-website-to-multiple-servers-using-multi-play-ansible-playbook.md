# Assignment 03 — Deploy a Static Website to Multiple Servers Using a Multi-Play Ansible Playbook

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Student Details

**Full Name:** OLuwafemi Aremu 
**Cloud Platform Used:** Azure  
**Server 1 URL:** `http://68.210.145.38`  
**Server 2 URL:** `http://20.222.228.157`

---

## Purpose

In this assignment, you will create a multi-play Ansible playbook to install Nginx, deploy a static website to two Ubuntu servers, and verify that the website is accessible from both servers.

You may use either AWS EC2 instances or Azure Virtual Machines as your managed servers.

---

# Task 1 — Create the Project Structure

## Goal

Create the required folders and files for the Ansible project.

## Evidence

### Screenshot 1 — Terminal or VS Code showing the complete `static-web` project structure

![task 1](screenshots/Screenshot%20with%20Terminal%20or%20editor%20showing%20the%20complete%20static-web%20folder%20layout.png)

---

# Task 2 — Configure the Ansible Inventory

## Goal

Add both Ubuntu servers to the Ansible inventory.

## Evidence

### Screenshot 2 — Output of `ansible-inventory -i inventory.ini --graph` showing `web1` and `web2`

![task 2](screenshots/Screenshot%20with%20Editor%20or%20terminal%20showing%20files_index.html.png)

---

## Configuration File

Copy and paste the complete contents of your `inventory.ini` file below:

```ini
static-web/
├── inventory.ini
├── site.yml
├── README.md
└── files/
    └── index.html
```

---

# Task 3 — Verify Ansible Connectivity

## Goal

Confirm that the Ansible controller can connect to both servers.

## Evidence

### Screenshot 3 — Ansible ping output showing `SUCCESS` and `pong` for both servers

![task 3](screenshots/Screenshot%20with%20Editor%20showing%20the%20three%20plays%20in%20site.yml.png)

---

# Task 4 — Download and Personalize the Static Website

## Goal

Download `index.html` to the Ansible controller and personalize the website with your full name.

## Evidence

### Screenshot 4 — Edited `files/index.html` showing the footer line with your full name

![task 3](screenshots/Screenshot%20with%20%20Editor%20showing%20the%20copy%20task%20and%20HTTP%20200%20assertion.png)

---

# Task 5 — Create the Multi-Play Ansible Playbook

## Goal

Create a single Ansible playbook containing separate plays for installation, deployment, and verification.

## Configuration File

Copy and paste the complete contents of your `site.yml` file below:

```yaml
[webservers]
web1 ansible_host=68.210.145.38
web2 ansible_host=20.222.228.157

[webservers:vars]
ansible_user=azure_user
ansible_ssh_private_key_file=10.01.12.02
ansible_python_interpreter=/usr/bin/python3
```

---

# Task 6 — Validate the Playbook Syntax

## Goal

Check the playbook for YAML or Ansible syntax errors before running it.

## Evidence

### Screenshot 5 — Successful syntax-check output showing `playbook: site.yml`

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20the%20ansible-playbook%20run%20.png)

---

# Task 7 — Run the Multi-Play Playbook

## Goal

Install Nginx, deploy the website, and verify both servers in one playbook run.

## Evidence

### Screenshot 6 — Play 3 verification showing HTTP `200` for both servers

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20the%20ansible-playbook%20run%20and%20final-1.png)

---

### Screenshot 7 — Final play recap showing `unreachable=0` and `failed=0` for `web1`, `web2`, and `localhost`

![task 5](screenshots/Screenshot%20with%20Browser%20showing%20the%20static%20website%20loaded-1.png)

---

# Task 8 — Verify Idempotency

## Goal

Run the playbook again and confirm that it does not make unnecessary changes.

## Evidence

### Screenshot 8 — Second playbook run showing the play recap with `changed=0`, `unreachable=0`, and `failed=0` for both web servers

![task 7](screenshots/Screenshot%20with%20Terminal%20showin%20d%20real%20ansible-playbook%20run%20applying%20successfully.png)

---

# Task 9 — Test Both Websites Manually

## Goal

Confirm that the static website is accessible from both public IP addresses.

## Evidence

### Screenshot 9 — `curl -I` output showing HTTP `200 OK` from both servers

![task 8](screenshots/Screenshot%20with%20Terminal%20showing%20HTTP%20200%20and%20the%20Nginx%20site-file%20snippet.png)

---

### Screenshot 10 — Browser showing the website from Server 1 with the public IP and your full name visible

![task 9](screenshots/Screenshot%20with%20Browser%20showing%20the%20EpicBook%20site%20with%20the%20public%20IP%20visible.png)

---

### Screenshot 11 — Browser showing the website from Server 2 with the public IP and your full name visible

![task 9](screenshots/Screenshot%20with%20Browser%20showing%20the%20EpicBook%20site%20with%20the%20public%20IP%20visible.png)

---

## Website URLs

Add both deployed website URLs below:

```text
Server 1: `http://68.210.145.38` 
Server 2: `http://20.222.228.157`
```

---

# Task 10 — Complete the Project README

## Goal

Document how the project works and record what you learned.

## README Content

Copy and paste the complete contents of your `README.md` file below:

```markdown
# Deploy a Static Website to Multiple Servers Using Ansible

## Project Overview

This project demonstrates how to use Ansible to automate the deployment of a static website to two Ubuntu servers using a multi-play Ansible playbook.

The deployment process installs Nginx, copies website files to both servers, and verifies website availability using HTTP requests.

## Student Details

- **Name:** OLUWAFEMI AREMU
- **Cloud Platform:** Azure (confirm)
- **Server 1:** http://68.210.145.38
- **Server 2:** http://20.222.228.157

## Technologies Used

- Ansible
- Ubuntu Linux
- Nginx
- YAML
- SSH
- HTML
- HTTP

## Project Structure

```text
static-web/
├── inventory.ini
├── site.yml
├── README.md
└── files/
    └── index.html
```

## Deployment Architecture

The Ansible controller connects to two remote Ubuntu servers through SSH.

The multi-play playbook performs three operations:

1. **Installation:** Installs and starts Nginx on both servers.
2. **Deployment:** Copies the personalized static website to both servers.
3. **Verification:** Uses the Ansible URI module to verify HTTP 200 responses.

## How to Run the Project

### 1. Verify server connectivity

```bash
ansible webservers -i inventory.ini -m ping
```

### 2. Validate playbook syntax

```bash
ansible-playbook -i inventory.ini site.yml --syntax-check
```

### 3. Execute the playbook

```bash
ansible-playbook -i inventory.ini site.yml
```

### 4. Verify website availability

```bash
curl -I http://68.210.145.38
curl -I http://20.222.228.157
```

### 5. Test idempotency

Run the playbook again and check that no unnecessary changes are made.

```bash
ansible-playbook -i inventory.ini site.yml
```

## Expected Results

- Nginx is installed and running on both servers.
- The same personalized static website is deployed to both servers.
- Both websites return HTTP 200.
- The playbook executes successfully.
- A second execution demonstrates idempotency.

## What I Learned

Through this project, I gained practical experience in Ansible inventory management, SSH connectivity, multi-play playbook development, automated Nginx installation, static website deployment, HTTP verification, and idempotent infrastructure automation.

## Conclusion

This project demonstrates how Ansible simplifies repetitive infrastructure management by allowing administrators to configure and deploy applications across multiple servers from a centralized controller.

It also highlights the importance of automation, consistency, verification, and repeatability in DevOps practices.

**Author:** OLUWAFEMI AREMU
```

---

# LinkedIn Post Required

## Evidence

### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/eCr994dH`

---

### Screenshot — Published LinkedIn post

![task 10](screenshots/Screenshot%20with%20Multi_server%20LinkedIn%20post.png)

---

# Assignment Questions

Answer the following in your own words:

**1. What issue did you face while completing this assignment, and how did you fix it?**

One of the main challenges was configuring Ansible to communicate successfully with both remote Ubuntu servers. I addressed this by checking the SSH configuration, verifying the server IP addresses, ensuring the correct SSH credentials were being used, and confirming that the required network access was available.

After resolving the connectivity requirements, I used the Ansible ping module to confirm that both servers responded successfully with pong.

---

**2. What did you learn from this assignment?**

I learned how to use Ansible to automate the deployment of a static website across multiple Ubuntu servers using a single multi-play playbook.

I gained practical experience with:
- Creating and configuring an Ansible project structure.
- Defining multiple managed servers in an inventory file.
- Establishing SSH connectivity between the controller and managed servers.
- Installing and configuring Nginx using Ansible.
- Deploying website files using the Ansible copy module.
- Verifying website availability using the Ansible URI module.
- Validating playbook syntax and running Ansible playbooks.
- Understanding idempotency and repeatable automation.

---

**3. Why is it useful to split installation, deployment, and verification into separate plays?**

Splitting installation, deployment, and verification into separate plays makes the playbook easier to understand, manage, troubleshoot, and maintain.

Each play has a specific responsibility:
1. Installation: Installs and starts Nginx on both servers.
2. Deployment: Copies the static website files to the appropriate web directory on both servers.
3. Verification: Checks that both websites are accessible and return HTTP 200.
This separation makes troubleshooting easier because problems can be identified at the installation, deployment, or verification stage independently.

---

**4. What is one benefit of using the Ansible `copy` module instead of cloning the website directly from Git on every managed server?**

One major benefit is centralized deployment.

The Ansible controller maintains the website files and distributes them to all managed servers using the copy module.

This ensures that both servers receive the same version of the website. It also reduces the need to install Git or configure repository access on every managed server.

---

**5. What does idempotency mean in this assignment?**

Idempotency means that running the same Ansible playbook multiple times produces the desired configuration without making unnecessary changes.

For example, after Nginx has been installed and the website files have been deployed, running the playbook again should not reinstall or unnecessarily modify resources.

A successful second run should ideally show:
- changed=0
- unreachable=0
- failed=0
This demonstrates that the deployment is repeatable and that the servers are already in the desired state.

---

**6. What does the Ansible `uri` module verify in Play 3?**

The Ansible uri module sends HTTP requests to the deployed websites to check whether they are accessible.

In Play 3, it verifies that both servers respond with HTTP status code 200.

This confirms that Nginx is serving the website successfully and that the deployment is accessible through both public IP addresses.

---

# Required Files

Confirm that the following files are included in your assignment folder:

- [✅] `inventory.ini`
- [✅] `site.yml`
- [✅] `files/index.html`
- [✅] `README.md`

---

# Submission Instructions

- Add all required screenshots in the correct order.
- Full Name must be visible in required screenshots.
- Include both deployed website URLs.
- Paste `inventory.ini`, `site.yml`, and `README.md` as editable text.
- Answer all assignment questions clearly in your own words.
- Add your LinkedIn post URL.
- Do not expose SSH private keys, passwords, cloud account IDs, or other sensitive information.

---

# Completion Checklist

- [✅] Task 1: `static-web` folder structure is complete
- [✅] Task 2: Both servers are listed under the `[web]` group in `inventory.ini`
- [✅] Task 2: Inventory graph shows `web1` and `web2`
- [✅] Task 3: Ansible ping returns `SUCCESS` and `pong` for both servers
- [✅] Task 4: `files/index.html` contains your full name
- [✅] Task 5: `site.yml` contains three separate plays
- [✅] Task 5: Play 1 installs, starts, and enables Nginx
- [✅] Task 5: Play 2 deploys `index.html` using the `copy` module
- [✅] Task 5: Nginx reload handler is included
- [✅] Task 5: Play 3 verifies both web servers from the controller
- [✅] Task 6: Playbook syntax check passes
- [✅] Task 7: First playbook run completes with `unreachable=0` and `failed=0`
- [✅] Task 7: URI verification returns HTTP `200` for both servers
- [✅] Task 8: Second playbook run demonstrates idempotency
- [✅] Task 8: Second run shows `changed=0` for both web servers
- [✅] Task 9: Both `curl -I` commands return HTTP `200 OK`
- [✅] Task 9: Website loads from Server 1
- [✅] Task 9: Website loads from Server 2
- [✅] Task 9: Full name is visible on both deployed websites
- [✅] Task 10: `README.md` contains all required explanations
- [✅] Screenshots 1–11 are included
- [✅] `inventory.ini`, `site.yml`, and `README.md` are pasted as editable text
- [✅] Both website URLs are included
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