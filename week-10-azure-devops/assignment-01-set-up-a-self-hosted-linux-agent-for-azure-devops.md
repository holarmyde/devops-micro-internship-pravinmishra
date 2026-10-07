# Assignment 1 — Set Up a Self-Hosted Linux Agent for Azure DevOps (Ubuntu + PAT)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will set up a self-hosted Azure DevOps build agent on an Ubuntu VM (AWS or Azure), registering it with a Personal Access Token in a dedicated agent pool, running it as a system service, and verifying it by running a test pipeline.

---

# Task 1 — Create a Personal Access Token (PAT)

## Goal

Create a PAT with Agent Pools (Read & Manage) and Build (Read & Execute) scopes, and store it securely.

> No PAT value screenshot required. If you capture the token settings page, ensure the secret value is not visible.

---

# Task 2 — Create an Agent Pool

## Goal

Create a self-hosted agent pool (e.g. `SelfHostedPool`) in Azure DevOps Organization Settings.

### Evidence

#### Screenshot 1 — Azure DevOps Agent Pools page showing the newly created pool

![task 2](screenshots/Screenshot%20with%20Azure%20DevOps%20Agent%20Pools%20page%20showing%20the%20newly%20created%20pool.png)
---

# Task 3 — Provision the Ubuntu VM

## Goal

Create an Ubuntu 22.04 (or latest) VM in AWS or Azure with SSH access, and confirm connectivity.

### Evidence

#### Screenshot 2 — Cloud console showing the running Ubuntu VM and its public IP or DNS name

![task 3](screenshots/Screenshot%20with%20cloud%20console%20and%20its%20public%20IP%20or%20DNS%20name.png)

---

#### Screenshot 3 — Terminal showing a successful SSH login and Ubuntu version details

![task 3](screenshots/Screenshot%20with%20Terminal%20showing%20a%20successful%20SSH%20login%20and%20Ubuntu%20version%20details.png)

---

# Task 4 — Install and Configure the Agent

## Goal

Download the Linux agent package, register it with your organization/pool/PAT via `config.sh`, and install/start it as a system service.

### Evidence

#### Screenshot 4 — Terminal showing successful agent configuration without exposing the PAT

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20agent%20configuration%20without%20exposing%20the%20PAT.png)

---

#### Screenshot 5 — Terminal showing the agent service running successfully

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20the%20agent%20service%20running%20successfully.png)

---

# Task 5 — Verify the Setup

## Goal

Confirm the agent service is running and the agent shows as Online in the Azure DevOps agent pool.

### Evidence

#### Screenshot 6 — Agent Pool listing showing the registered agent online

![task 5](screenshots/Screenshot%20with%20Pipeline%20online.png)

---

# Task 6 — Run a Test Pipeline

## Goal

Create and run a YAML pipeline targeting the self-hosted pool, running `uname -a`, `whoami`, and `df -h` on the VM.

### Evidence

#### Screenshot 7 — Successful test pipeline run output in Azure DevOps showing the Linux commands

![task 6](screenshots/Screenshot%20with%20verify%20test.png)

---

### Notes

Note the cloud platform used, your Azure DevOps organization/project name, and the agent pool name. Describe any issue you faced and how you resolved it.

Cloud Platform and Azure DevOps Details
Cloud platform: AWS EC2
Azure DevOps organization: bebotchild
Azure DevOps project: SelfHostedAgentProject
Agent pool: Default
Self-hosted agent: SelfHostedAgentProject
Issue Faced and Resolution

One issue I faced was getting SSH access to the Ubuntu EC2 instance. The correct AWS private key initially returned Permission denied (publickey) because the corresponding public key was not present in the Ubuntu user's authorized_keys file. I used AWS EC2 Instance Connect to temporarily access the VM, added the correct public key to ~/.ssh/authorized_keys, corrected the SSH file permissions, and then successfully connected using the original key.

I also encountered an authentication mistake while configuring the Azure DevOps agent, where a PAT was entered at the wrong prompt. I revoked that exposed PAT, created a replacement PAT, and completed the agent configuration successfully.

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not display the PAT, SSH private key, or other secrets in screenshots or logs

---

# Completion Checklist

- [✅] Task 1: PAT created with required scopes and stored securely
- [✅] Task 2: Self-hosted agent pool created (Screenshot 1)
- [✅] Task 3: Ubuntu VM provisioned and SSH verified (Screenshots 2–3)
- [✅] Task 4: Agent installed, registered, and running as a service (Screenshots 4–5)
- [✅] Task 5: Agent verified Online (Screenshot 6)
- [✅] Task 6: Test pipeline run successfully (Screenshot 7)
- [✅] Platform/org/pool details and issue notes written (Notes)
- [✅] No secrets exposed

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
