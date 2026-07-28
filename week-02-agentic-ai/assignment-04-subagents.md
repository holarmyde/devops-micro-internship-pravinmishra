# Assignment 4 — Building Your AI Team

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build and configure a set of specialized AI subagents inside your project. You will learn how different models and tool permissions define agent behavior, and you will trigger two real agent delegations to analyze security and cost aspects of your Terraform infrastructure.

---

# Task 1 — Create the Agents Folder and Add Files

## Goal

Create the `.claude/agents/` directory and add all required agent files.

### Evidence

#### Screenshot 1 — VS Code sidebar showing `.claude/agents/` with all 3 files

![task 1](screenshots/Screenshot%20with%20claude_agents.png)

--- 

# Task 2 — Compare the Agent Configurations

## Goal

Analyze the configuration differences between the three agents and demonstrate understanding of model and tool selection.

### Written Answers

#### 1. Why does the cost optimizer use Haiku instead of Sonnet?

The cost optimizer file use the Haiku Cos the cost doesnt need deep reasoning you scan the resources check price classes and compare storage tiers and you pay less. Haiku is good for this as against Sonnet its lightweith. Sonnet cost more and its used for security auditor file cos the file needs careful analysis

--- 

#### 2. Why does the security auditor NOT have Write in its tools list?

It doesnt have Write cos its the auditor file hence it doesnt need to be modified thats why only read, grep and glob are available access. It only looks into (reads) a file and reports it back

---

#### 3. Why does the tf-writer use `inherit` instead of a specific model?

The goal is that the sub agent should use whatever mode1 our main session is runnin on hence if you use opus in the main session tf-writer would use opus. If you use sonnet in the main session tf-writer would sonnet. Why is this? cos code quality generation depends on the model. This means if am using opus for the big changes I want my tf-writer to also be on opus. That would inherit hence inherit gives us that flexibility

---

### Evidence

#### Screenshot 2 — `security-auditor.md` frontmatter showing model and tools configuration

![task 2](screenshots/Screenshot%20with%20agent_security_auditor.png)

--- 

#### Screenshot 3 — `cost-optimizer.md` frontmatter showing the model and tools configuration

 ![task 2](screenshots/Screenshot%20with%20agent_cost%20optimizer.png)

---

# Task 3 — Run the Security Auditor

## Goal

Trigger the security auditor agent and analyze the generated security report for your Terraform infrastructure.

### Evidence

#### Screenshot 4 — The delegation message showing Claude launched the security-auditor

![task 3](screenshots/Screenshot%20with%20agent_security_auditor_%20launch_AUDITING%202.png)

--- 

#### Screenshot 5 — Security audit report output

![task 3](screenshots/Screenshot%20with%20agent_security_auditor_AUDITING%202.png)

--- 

# Task 4 — Run the Cost Optimizer

## Goal

Trigger the cost optimizer agent and review the generated cost optimization report.

### Evidence

#### Screenshot 6 — The full cost optimization report

![task 4](screenshots/Screenshot%20with%20agent_cost_optimizer_FULL_launch.png)

--- 

# Submission Instructions

- Ensure all agent files are committed in `.claude/agents/`
- Complete all written answers in your GitHub Repo
- Push final changes to your forked GitHub repository

---

## GitHub Repository URL

Paste your forked repository URL here:

<<<<<<< HEAD
`__https://github.com/holarmyde/Ultimate-Agentic-DevOps-with-Claude-Code___`
=======
`https://github.com/holarmyde/Ultimate-Agentic-DevOps-with-Claude-Code_`
>>>>>>> upstream/main

---

# Completion Checklist

- [✅ ] `.claude/agents/` folder contains all 3 agent files
- [✅ ] Screenshot 2 shows correct `security-auditor.md` configuration
- [✅ ] Screenshot 3 shows correct `cost-optimizer.md` configuration
- [✅ ] All 3 written answers completed 
- [✅ ] Security auditor executed successfully
- [✅ ] Cost optimizer executed successfully
- [✅ ] Security report is visible with findings
- [✅ ] Cost report is visible with recommendations
- [✅ ] All required screenshots added
- [✅ ] GitHub repo updated with agents

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