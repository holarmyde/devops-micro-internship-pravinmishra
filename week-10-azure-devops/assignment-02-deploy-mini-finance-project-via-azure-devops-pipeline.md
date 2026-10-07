# Assignment 2 — Deploy Mini Finance Project via Azure DevOps Pipeline

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build an Azure DevOps CI/CD pipeline that deploys the Mini Finance static website to an Ubuntu VM running Nginx: importing the repo into Azure Repos, provisioning the VM with Terraform and Ansible, connecting via an SSH Service Connection, and deploying on every commit to `main`.

---

# Task 1 — Import the Repository

## Goal

Import `https://github.com/pravinmishraaws/Azure-Static-Website` into Azure Repos and confirm `index.html` is present.

### Evidence

#### Screenshot 1 — Azure Repos showing the imported repository files with `index.html` visible

![task 1](screenshots/Screenshot%20with%20Azure%20Repos%20showing%20the%20imported%20repository%20files%20with%20index.html.png)

---

# Task 2 — Prepare the Target VM

## Goal

Provision a Linux VM with Terraform (ports 22/80 open), then use Ansible to install and start Nginx and prepare `/var/www/html`.

### Evidence

#### Screenshot 2 — Terraform output or cloud console showing the running VM and public IP

![task 2](screenshots/Screenshot%20with%20Terraform%20output%20or%20cloud%20console%20showing%20the%20running%20VM%20.png )

---

#### Screenshot 3 — Terminal showing Ansible completed successfully and Nginx is active

![task 2](screenshots/Screenshot%20with%20Terminal%20showing%20Ansible%20completed%20successfully%20and%20Nginx%20is%20active.png)

---

# Task 3 — Create an SSH Service Connection

## Goal

Create the password-based SSH Service Connection `ubuntu-nginx-ssh` pointing to the VM, and validate it.

### Evidence

#### Screenshot 4 — SSH Service Connection configuration page showing the connection details and successful validation, with the password hidden

![task 3](screenshots/Screenshot%20with%20SSH%20Service%20Connection%20configuration%20page%20showing.png)

---

# Task 4 — Author the YAML Pipeline

## Goal

Write a pipeline triggered on `main` that checks out the repo, copies files to `/var/www/html` via `CopyFilesOverSSH@0`, and verifies the deployment directory via an `SSH@0` task, using `ubuntu-nginx-ssh` and the self-hosted (or available Microsoft-hosted) pool.

### Evidence

#### Screenshot 5 — Pipeline YAML definition open in the Azure DevOps editor

![task 4](screenshots/Screenshot%20with%20Pipeline%20YAML%20definition%20open%20in%20the%20Azure%20DevOps%20editor.png)

---

# Task 5 — Verify Deployment

## Goal

Confirm the pipeline run succeeded (checkout, SSH connection, file transfer, remote verification) and the Mini Finance website is live at the VM's public IP.

### Evidence

#### Screenshot 6 — Successful Azure DevOps pipeline run log summary

![task 5](screenshots/Screenshot%20with%20Successful%20Azure%20DevOps%20pipeline%20run%20log%20summary.png)


---

#### Screenshot 7 — Browser showing the deployed website with the VM public IP visible

![task 5](screenshots/Screenshot%20with%20Browser%20showin%20the%20deployed%20website%20wit%20d%20VM%20public%20IP%20visible.png)

---

### Notes

Include the VM public URL. Describe any issue you faced and how you fixed it (e.g. parallelism/agent-pool issues).

VM Public URL

http://3.145.52.251

Issue Faced and Resolution

I initially encountered issues with the Azure DevOps pipeline connecting to the AWS EC2 instance. The EC2 public IP address changed, which caused the SSH connection to time out. I also had an issue with the self-hosted Azure DevOps agent being offline, which prevented the pipeline from running.

I resolved the issues by bringing the self-hosted agent service back online and verifying that it was active on the EC2 instance. I then updated the Azure DevOps SSH service connection to use the EC2 instance's private IP address (10.0.1.211) because the self-hosted agent and deployment target were on the same VM. I created a dedicated SSH deployment key and configured it on the VM. Finally, Nginx was installed and started on the EC2 instance.

After these fixes, the pipeline successfully copied the website, deployed it to /var/www/html, and verified the site through Nginx. The final pipeline output confirmed:

DEPLOYMENT VERIFICATION PASSED
OLUWAFEMI AREMU is present on the website

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not commit the VM password to the repository or write it directly in YAML

---

# Completion Checklist

- [✅ ] Task 1: Repository imported into Azure Repos (Screenshot 1)
- [✅ ] Task 2: VM provisioned and Nginx configured (Screenshots 2–3)
- [✅ ] Task 3: SSH Service Connection created and validated (Screenshot 4)
- [✅ ] Task 4: YAML pipeline authored (Screenshot 5)
- [✅ ] Task 5: Pipeline run succeeded and site verified (Screenshots 6–7)
- [✅ ] VM URL and issue notes written (Notes)
- [✅ ] No passwords, tokens, or credentials exposed

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
