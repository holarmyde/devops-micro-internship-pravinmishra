# Assignment 01 — Set Up a Team-Ready Ansible Development Workstation

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will prepare an isolated and reusable Ansible development workstation.

You will install Ansible and supporting tools inside a Python virtual environment, configure VS Code, prepare SSH access, configure Git and pre-commit hooks, and document the complete setup.

This workstation will be used as the Ansible controller in upcoming assignments.

---

# Task 1 — Create and Initialize the Ansible Workspace

## Goal

Create the assignment workspace, initialize a Git repository, prepare the required directories, and add Git ignore rules for local and sensitive files.

### Evidence

#### Screenshot 1 — Terminal showing the `ansible-onboarding` path, `ls -la` output, and `git status` confirming the Git repository is on the `main` branch

![task 1](screenshots/Screenshot%20Terminal%20showing%20the%20activated%20venv%20-1.png)

---

# Task 2 — Create the Virtual Environment and Install Ansible Tools

## Goal

Create an isolated Python virtual environment and install Ansible and the required validation tools without modifying the system Python environment.

### Evidence

#### Screenshot 2 — Terminal showing the active `(.venv)` environment, `which ansible`, `ansible --version`, `ansible-lint --version`, `yamllint --version`, and `pre-commit --version`

![task 1](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20ansible-lint%20--version.png)

---

# Task 3 — Configure VS Code for Ansible Development

## Goal

Configure Visual Studio Code to use the project’s Python virtual environment and provide validation support for Python, YAML, and Ansible files.

### Evidence

#### Screenshot 3 — VS Code Extensions panel showing the Ansible, YAML, and Python extensions installed

![task 3](screenshots/Screenshot%20with%20VS%20Code%20Extensions%20panel%20showing%20Ansible.png)

---

#### Screenshot 4 — VS Code showing `.vscode/settings.json` and `.editorconfig` open side by side, with the required settings clearly visible

![task 3](screenshots/Screenshot%20with%20VS%20Code%20showing%20vscodesettings.json.png)

---

# Task 4 — Create the Baseline Ansible Configuration

## Goal

Create a reusable `ansible.cfg` file containing the default settings that will be used in this workspace and upcoming Ansible assignments.

### Evidence

#### Screenshot 5 — `ansible.cfg` open in VS Code or another editor, showing the complete configuration

![task 4](screenshots/Screenshot%20with%20VS%20Code%20or%20terminal%20showing%20ansible_cfg.png)

---

#### Screenshot 6 — Terminal showing `ansible --version` with the `ansible.cfg` path and the output of `ansible-config dump --only-changed`

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20ssh-add%20-l%20with%20the%20key%20loaded.png)

---

# Task 5 — Configure SSH Readiness

## Goal

Prepare SSH key authentication, load the key into the SSH agent, configure reusable SSH client settings, and understand how trusted host fingerprints are stored.

### Evidence

#### Screenshot 7 — Terminal showing `ssh-add -l` with the ED25519 key loaded and the SSH configuration verification output

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20pre-commit%20install%20output.png)

---

# Task 6 — Configure Git Identity and Pre-commit Hooks

## Goal

Configure your Git identity and install pre-commit hooks that validate YAML and Ansible files before commits are created.

### Evidence

#### Screenshot 8 — Terminal showing your Git full name, Git email, default branch, successful `pre-commit install` output, and `.git/hooks/pre-commit`

![task 5](screenshots/Screenshot%20with%20Terminal%20showing%20pre-commit%20run%20--all-files%20passing.png)

---

# Task 7 — Test the Complete Workstation Setup

## Goal

Verify that Ansible, the linting tools, Git hooks, SSH agent, and Git ignore rules are working correctly.

### Evidence

#### Screenshot 9 — Terminal showing `pre-commit run --all-files` completing successfully

![task 7](screenshots/Screenshot%20with%20Repository%20tree%20showing%20the%20required%20files.png)

---

#### Screenshot 10 — Terminal showing `ansible --version` with the project configuration path and `ssh-add -l` with the ED25519 key loaded

![task 7](screenshots/Screenshot%20with%20Repository%20tree%20showing%20the%20required%20files.png)

---

# Task 8 — Create the README and Onboarding Checklist

## Goal

Document the completed Ansible workstation setup and create a reusable checklist for preparing another workstation in the future.

### Evidence

#### Screenshot 11 — Terminal showing the final `ansible-onboarding` project structure

![task 8](screenshots/Screenshot%20with%20Terminal%20or%20editor%20showing%20the%20complete%20mini-finance%20project%20tree.png)

---

#### Screenshot 12 — VS Code Markdown preview showing your full name, project summary, and part of the “New Machine? Do This” checklist

![task 8](screenshots/Screenshot%20with%20Terminal%20or%20editor%20showing%20the%20complete%20mini-finance%20project%20tree.png)

---

# Assignment Questions

Answer the following in your own words:

**1. What is one feature that makes your workstation setup team-friendly?**

The use of a Python virtual environment combined with a reusable project configuration makes the workstation team-friendly. Every team member can install the same Ansible tools, use the same configuration files, and follow the same validation procedures. This promotes consistency and makes onboarding new team members easier.

---

**2. What is one pitfall you avoided while completing the setup?**

One major pitfall I avoided was installing Ansible and its supporting Python packages directly into the system Python environment. Instead, I created a dedicated Python virtual environment. This prevents dependency conflicts, protects the operating system's Python installation, and keeps the project tools isolated.

---

**3. Why should Ansible be installed inside a Python virtual environment?**

Ansible should be installed inside a Python virtual environment because it isolates Ansible and its dependencies from the system-wide Python installation and other projects. It allows developers to manage package versions independently, avoid dependency conflicts, and reproduce the same development environment across different machines.

---

**4. Why must SSH private keys and `.venv/` remain outside version control?**

SSH private keys must remain outside version control because they provide access to remote servers. Exposing them in a Git repository could compromise server security and allow unauthorized access. The .venv/ directory should also be excluded because it contains locally installed packages, environment-specific files, and dependencies that can be recreated using installation commands. Keeping both outside version control improves security, reduces repository size, and makes the project easier to share.

---

# Required Files

Confirm that the following files are included in your assignment workspace:

- [✅] `README.md`
- [✅] `requirements.txt`
- [✅] `.gitignore`
- [✅] `.editorconfig`
- [✅] `.vscode/settings.json`
- [✅] `ansible.cfg`
- [✅] `.pre-commit-config.yaml`
- [✅] `inventories/`
- [✅] `roles/`

---

# Submission Instructions

- Add all required screenshots in your submission.
- Full Name must be visible in required screenshots.
- All screenshots must be readable.
- Answer all assignment questions clearly in your own words.
- Do not expose SSH private-key contents, passwords, access tokens, API keys, credentials, private certificates, or other sensitive information.

---

# Completion Checklist

- [✅] Task 1: `ansible-onboarding` workspace created
- [✅] Task 1: Git initialized on the `main` branch
- [✅] Task 1: `.gitignore` created
- [✅] Task 2: Python virtual environment created
- [✅] Task 2: Virtual environment activated
- [✅] Task 2: Ansible installed inside `.venv`
- [✅] Task 2: `ansible-lint`, `yamllint`, and `pre-commit` installed
- [✅] Task 2: `requirements.txt` created
- [✅] Task 3: Required VS Code extensions installed
- [✅] Task 3: VS Code uses the Python interpreter from `.venv`
- [✅] Task 3: `.vscode/settings.json` created
- [✅] Task 3: `.editorconfig` created
- [✅] Task 4: `ansible.cfg` created
- [✅] Task 4: Ansible loads `ansible.cfg` from the project directory
- [✅] Task 5: ED25519 SSH key exists
- [✅] Task 5: SSH private key has not been exposed
- [✅] Task 5: SSH key loaded into the SSH agent
- [✅] Task 5: `~/.ssh/config` contains the required settings
- [✅] Task 5: `~/.ssh/known_hosts` exists
- [✅] Task 6: Git identity configured correctly
- [✅] Task 6: Pre-commit hooks installed
- [✅] Task 7: `pre-commit run --all-files` completes successfully
- [✅] Task 8: `README.md` contains your full name and workstation details
- [✅] Task 8: “New Machine? Do This” checklist contains 10–12 items
- [✅] All 12 required screenshots are included
- [✅] Assignment questions are answered
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