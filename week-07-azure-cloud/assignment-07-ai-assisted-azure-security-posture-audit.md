# Assignment 7 — AI-Assisted Azure Security Posture Audit

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

Build a read-only Bash script that audits Azure resources for common security misconfigurations. Create a reusable Claude Code `/azure-audit` Skill that explains findings and recommends remediation without executing it. Fix one finding yourself and rerun the audit to verify the result.

You may create the required resources specifically for this assignment or reuse suitable resources in your own subscription. A Load Balancer and a deployed application are not required.

---

# Task 1 — Confirm Your Azure Resources and Create the Workspace

## Goal

Confirm Azure CLI authentication and the availability of a VM, an associated NSG, a Storage Account, and a MySQL Flexible Server. Create the workspace with `scripts`, `reports`, and `.claude/skills/azure-audit` directories.

## Evidence

#### Screenshot 1 — Azure CLI showing the subscription name, VM, Storage Account, and MySQL Flexible Server without exposing subscription or tenant IDs

![task 1](screenshots/Screenshot%20with%20%20confirming%20your%20subscription%20and%20running%20VM%20.png)

---

#### Screenshot 2 — Workspace location and directory structure shown using `pwd` and `find . -maxdepth 4 -type d | sort`

![task 1](screenshots/Screenshot%20with%20MySQL%20Flexible%20Server%20overview%20showing%20Private%20access%20.png)

---

## Notes You Must Write

1. Which Azure resources will be inspected by this audit, and why is each one required?

The audit will inspect four main Azure resource areas:
- Azure Virtual Machine (VM): To identify potential security weaknesses associated with the VM's network access and managed disk encryption.
- Network Security Group (NSG): To identify unrestricted inbound SSH (port 22) and RDP (port 3389) access that could expose virtual machines to unauthorized connections.
- Azure Storage Account: To check whether public blob access is enabled, which could expose stored data to unauthorized users.
- Azure Database for MySQL Flexible Server: To verify whether public network access is enabled and determine whether the database is protected from direct public exposure.
These resources represent important parts of an Azure environment's compute, networking, storage, and database security.

2. Why should you confirm that the resources exist before writing an audit script against them?

Confirming that the resources exist ensures that the audit script targets actual Azure resources rather than nonexistent or incorrectly named resources.

It also helps identify the correct resource groups, resource names, subscription context, and associated configurations.

This prevents unnecessary script failures, improves the accuracy of audit results, and ensures that security findings are based on real infrastructure evidence.

---

# Task 2 — Create Project Context and Safety Rules in CLAUDE.md

## Goal

Create `CLAUDE.md` defining the project overview, audit workflow, safety rules, and output rules. Require evidence-backed findings and keep all Azure remediation under human control.

## Evidence

#### Screenshot 3 — `CLAUDE.md` showing the Project Overview, Audit Workflow, Safety Rules, and Output Rules sections

![task 1](screenshots/Screenshot%20with%20open%20in%20your%20editor%20showing%20the%20project%20overview.png)

---

## Notes You Must Write

1. Why should Claude never be allowed to execute the remediation command itself?

Claude should not execute remediation commands because changing Azure configurations can affect production services, expose resources to unintended risks, or cause service interruptions.

Remediation actions such as modifying NSG rules, disabling public access, or changing encryption configurations can have significant operational consequences.

Keeping remediation under human control ensures that every proposed change is reviewed, authorized, and executed deliberately by the infrastructure owner.

2. Which rule prevents Claude from reporting a security problem without evidence?

The evidence-backed findings rule prevents Claude from reporting unsupported security problems.

Every finding must include actual Azure CLI output or other verifiable resource configuration evidence.

Claude must distinguish between confirmed security issues, healthy configurations, and conditions that cannot be verified. It must never invent findings or assume a resource is insecure without supporting evidence.

---

# Task 3 — Use Agentic AI to Plan the Audit Before Writing the Script

## Goal

Ask Claude Code to read `CLAUDE.md` and propose exactly four read-only checks: unrestricted inbound SSH/RDP rules, Storage Account public blob access, managed disk encryption-at-rest type, and MySQL Flexible Server public network access. Do not allow file creation or modification during planning.

## Evidence

#### Screenshot 4 — Claude Code showing the four-check audit plan, including evidence-gathering commands, healthy and risky results, and why each check matters

![task 3](screenshots/Screenshot%20with%20showing%20the%20four-check%20plan,%20with%20no%20files%20created%20or%20modified.png)

---

## Notes You Must Write

1. Which part of the Agentic Loop is being designed in this task?

The Planning stage of the Agentic AI Loop is being designed.

During this stage, Claude examines the project instructions, understands the audit objectives, identifies the required security checks, and determines which Azure CLI commands will gather the necessary evidence.

The planning stage establishes what the system needs to inspect before any script is created or executed.

2. Why is it useful to identify the exact evidence-gathering commands before writing the Bash script?

Identifying the exact commands beforehand ensures that the audit script collects accurate and relevant information from Azure.

It also helps confirm that the commands are read-only, reduces scripting errors, and establishes clear criteria for identifying healthy and risky configurations.

This makes the script more reliable, repeatable, transparent, and easier to troubleshoot.

The four planned checks are:
1. Unrestricted inbound SSH/RDP access through NSG rules.
2. Public blob access on Azure Storage Accounts.
3. Managed disk encryption-at-rest type.
4. Public network access on MySQL Flexible Server.

---

# Task 4 — Build the Azure Audit Bash Script

## Goal

Create `scripts/azure-audit.sh` using read-only Azure CLI commands. Generate a report containing your Full Name, evidence, PASS/WARN/FAIL results, and a summary. Use exit codes **0 for HEALTHY**, **1 for WARN**, and **2 for FAIL**. Validate the script and make it executable.

## Evidence

#### Screenshot 5 — Top of `azure-audit.sh` showing the variables and checks array

![task 4](screenshots/Screenshot%20with%20Your%20script%20open%20in%20your%20editor,%20showing%20the%20check%20functions.png)

---

#### Screenshot 6 — NSG and Storage Account check functions

![task 4](screenshots/Screenshot%20with%20NSG%20rules%20proving%20least-privilege%20access%20between%20the%20tiers.png)

---

#### Screenshot 7 — Managed disk encryption and MySQL check functions

![task 4](screenshots/Screenshot%20with%20MySQL%20Flexible%20Server%20overview%20showing%20Private%20access%20.png)

---

#### Screenshot 8 — `print_summary` function and script exit behavior

![task 4](screenshots/Screenshot%20with%20Your%20script%20open%20in%20your%20editor,%20showing%20the%20check%20functions.png)

---

#### Screenshot 9 — Successful `bash -n` validation and `ls -l` output showing executable permission

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20ls%20-la%20build%20output.png)

---

## Notes You Must Write

1. What is stored inside the `checks` array?

The checks array stores the names of the audit functions that the Bash script must execute.

These functions represent the four security checks:
- NSG inbound SSH/RDP security.
- Storage Account public blob access.
- Managed disk encryption-at-rest configuration.
- MySQL Flexible Server public network access.
The array provides a centralized and reusable way to organize the audit operations.

2. How does the `for` loop execute those checks?

The for loop iterates through each function name stored in the checks array.

During each iteration, it executes the corresponding function, collects the security configuration evidence, evaluates the result, and records the appropriate PASS, WARN, or FAIL status.

This approach avoids duplicating execution logic and makes it easier to maintain or extend the audit script.

3. Why does the NSG check inspect both single-value and multi-value address/port fields?

Azure NSG rules support different ways of specifying network addresses and ports.

Single-value fields include:
- sourceAddressPrefix
- destinationPortRange
Multi-value fields include:
- sourceAddressPrefixes
- destinationPortRanges
Checking both formats ensures that the audit detects unrestricted access regardless of how the NSG rule was configured.

For example, an SSH rule might use * in a single-value field or include 0.0.0.0/0 in a multi-value field.

Inspecting both prevents potential security misconfigurations from being overlooked.

4. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?

Different exit codes allow users and automation tools to understand the overall audit outcome programmatically.
- Exit code 0 — HEALTHY: All checks passed without identified security warnings or failures.
- Exit code 1 — WARN: One or more checks identified security concerns that require review.
- Exit code 2 — FAIL: A critical failure or audit execution problem occurred, according to the script's defined failure criteria.
These codes make it easier to integrate the audit into CI/CD pipelines, monitoring workflows, and automated reporting systems.

5. What does the disk encryption check verify?

The disk encryption check verifies the encryption-at-rest configuration of Azure managed disks attached to virtual machines.

It examines the disk's encryption properties, including its encryption type, to determine whether the disk uses an approved encryption configuration.

This helps establish whether stored operating system and application data receive protection against unauthorized access to underlying storage.

---

# Task 5 — Run the Baseline Audit

## Goal

Run the script, capture its exit code immediately, review all four results, and save `reports/baseline-report.txt` before making any remediation.

## Evidence

#### Screenshot 10 — Baseline audit output showing your Full Name and all four check results

![task 5](screenshots/Screenshot%20with%20Script%20output%20showing%20your%20Full%20Name.png)

---

#### Screenshot 11 — Captured exit code and final report summary

![task 5](screenshots/Screenshot%20with%20Script%20output%20showing%20your%20Full%20Name.png)

---

## Notes You Must Write

1. What is the Overall Status of your baseline audit?

Use the result from your actual reports/baseline-report.txt.

Suggested answer:

The overall status of my baseline audit was [HEALTHY/WARN/FAIL]. The audit evaluated four Azure security areas and generated a report containing the configuration evidence and results for each check.

2. Did any check return WARN or FAIL?

The audit identified [number] WARN findings and [number] FAIL findings.

The checks were evaluated individually, and each result was recorded in the baseline report for further investigation and remediation.

If all four checks passed, write:

No checks returned WARN or FAIL. All four security checks passed based on the configurations inspected during the baseline audit.

3. What evidence supports each finding?

The findings are supported by Azure CLI configuration outputs collected during the audit.

The evidence includes:
- NSG: Inbound security rule properties, including source address prefixes, destination ports, and access settings.
- Storage Account: The allowBlobPublicAccess configuration value.
- Managed Disk: The disk encryption-at-rest type reported by Azure.
- MySQL Flexible Server: The publicNetworkAccess configuration value.
These configuration details provide verifiable evidence for determining whether each resource meets the audit's security requirements.

4. Which finding represents the highest security risk in your environment? If none were found, state that.

Use your actual findings.

Suggested answer if a risky finding exists:

The highest security risk identified was [insert actual finding] because it could potentially expose sensitive resources or data to unauthorized access. This finding required priority attention because of its potential impact on the security of the Azure environment.

If all checks passed:

No high-risk security findings were identified during the baseline audit. All four checks passed the defined security criteria based on the evidence collected.

---

# Task 6 — Create and Run the /azure-audit Skill

## Goal

Create `.claude/skills/azure-audit/SKILL.md` with Bash, Read, and Grep permissions, without Write permission, and with manual invocation enabled. The Skill must run the audit, read the report, explain findings and their impact, and recommend remediation and verification commands without changing Azure resources.

## Evidence

#### Screenshot 12 — `SKILL.md` showing frontmatter, tool restrictions, and safety rules

![task 5](screenshots/Screenshot%20with%20Script%20output%20showing%20your%20Full%20Name.png)

---

#### Screenshot 13 — `/azure-audit` output showing baseline findings, supporting evidence, risk explanations, recommended remediation, and verification commands

![task 5](screenshots/Screenshot%20with%20Script%20output%20showing%20your%20Full%20Name.png)

---

## Notes You Must Write

1. Why does the Skill include Bash, Read, and Grep, but not Write?

The Skill includes Bash, Read, and Grep because these tools provide everything necessary to execute and investigate a read-only security audit.
- Bash: Runs the audit script and executes read-only Azure CLI commands.
- Read: Reads the generated audit reports and project documentation.
- Grep: Searches reports and configuration files for relevant security findings.
Write permission is intentionally excluded to prevent Claude from modifying project files or making unauthorized changes to the audit environment.

2. What part of the workflow is performed by Bash?

Bash performs the technical execution of the audit.

It runs Azure CLI commands, retrieves resource configuration details, evaluates the four security checks, assigns PASS/WARN/FAIL results, and generates the audit report.

It provides the actual configuration evidence used to assess the Azure environment.

3. What part is performed by Claude?

Claude performs the AI-assisted analysis and interpretation of the audit results.

It reads the generated report, explains identified security issues, describes their potential impact, prioritizes risks, and recommends appropriate remediation and verification commands.

Claude acts as an intelligent security analysis assistant rather than an autonomous infrastructure administrator.

4. What additional value does Claude add beyond the script’s PASS/WARN/FAIL result?

Claude provides context and actionable explanations that go beyond basic audit classifications.

It explains why a configuration is risky, what could happen if the issue remains unresolved, how the problem should be addressed, and which commands can verify the remediation.

This transforms raw technical audit output into understandable security recommendations that support informed human decision-making.

---

# Task 7 — Fix a Real Finding and Reverify

## Goal

Save the before-fix report, review a recommended remediation, execute the fix yourself, and rerun `/azure-audit` to confirm that the finding is resolved. Save the after-fix report.

If the baseline is clean, use only the controlled temporary SSH-rule exercise permitted in the assignment guidelines. Do not deliberately expose the database or Storage Account.

## Evidence

#### Screenshot 14 — `before-fix-report.txt` showing the original finding

![task 5](screenshots/Screenshot%20with%20Script%20output%20showing%20your%20Full%20Name.png)

---

#### Screenshot 15 — Terminal showing the remediation command executed manually by you

![task 5](screenshots/Screenshot%20with%20Script%20output%20showing%20your%20Full%20Name.png)

---

#### Screenshot 16 — `/azure-audit` output or `after-fix-report.txt` showing the finding resolved

![task 5](screenshots/Screenshot%20with%20Script%20output%20showing%20your%20Full%20Name.png)

---

## Notes You Must Write

1. Which exact finding did you fix?

I fixed the [insert actual finding, e.g., unrestricted inbound SSH access from the internet through an Azure NSG rule] identified during the baseline security audit.

The remediation focused on reducing unnecessary exposure and strengthening the security posture of the Azure environment.

2. What evidence proved that it existed?

The baseline audit report contained Azure CLI configuration evidence showing that the affected resource had an insecure configuration.

For example, if the issue was unrestricted SSH access, the evidence would show an inbound NSG security rule allowing TCP port 22 from * or 0.0.0.0/0.

This confirmed that the issue was based on the actual Azure configuration rather than an assumption.

3. What command did you run?

Insert the exact command you executed manually.

For example, if you restricted SSH access to your trusted public IP address:
az network nsg rule update \
  --resource-group <RESOURCE_GROUP> \
  --nsg-name <NSG_NAME> \
  --name <RULE_NAME> \
  --source-address-prefixes <YOUR_TRUSTED_PUBLIC_IP>/32


This is an example only. Use the command corresponding to your actual finding and remediation.

4. Why did you choose that remediation?

I selected this remediation because it addressed the identified security weakness while following the principle of least privilege.

Rather than allowing unrestricted access, the configuration was changed to permit only the required and trusted network source.

This reduces the potential attack surface while maintaining necessary administrative access.

5. Did Claude execute the command, or did you?

I executed the remediation command manually.

Claude was used to analyze the audit evidence, explain the security risk, and recommend the appropriate remediation and verification commands.

The actual Azure configuration change remained under my control, following the assignment's human-approval and safety requirements.

6. Why does keeping remediation under human control matter?

Keeping remediation under human control prevents unauthorized or unintended infrastructure changes.

Azure security changes can affect application availability, network connectivity, database access, and business operations.

Human approval ensures that proposed changes are reviewed against operational requirements before execution.

It also establishes accountability and prevents AI from independently modifying critical cloud infrastructure.

7. Map your actions to **Gather → Analyze → Human Act → Verify**.

Agentic AI stage	What happened
Gather	Bash executed read-only Azure CLI commands and collected security configuration evidence.
Analyze	Claude reviewed the audit report, identified risks, explained their impact, and recommended remediation.
Human Act	I reviewed the recommendation and manually executed the approved Azure remediation command.
Verify	I reran the audit and compared the after-fix report with the original baseline to confirm that the finding was resolved.

The workflow demonstrates how Agentic AI can support cloud security without removing human responsibility for infrastructure changes.

---

# LinkedIn Post — Required

## Goal

Publish a LinkedIn post describing the read-only audit script, the `/azure-audit` Skill, one finding you fixed, and the evidence → AI analysis → human remediation → reverification workflow. Include before-and-after evidence and one lesson about applying the same audit methodology across AWS and Azure.

Write **4–6 lines in your own words**, followed by the credit note and tags specified for your track in the assignment guidelines.

Suggested hashtags: **#DMIByPravinMishra #Azure #AgenticAI #ClaudeCode #DevOps**

## Evidence

#### LinkedIn Post URL

Paste your published LinkedIn post URL here:

`https://lnkd.in/p/eZkt33r7`

---

#### Screenshot 17 — Published LinkedIn post showing your caption and before-and-after audit evidence

![linkedIN](screenshots/Screenshot%20with%20azure%20devops%20linkedIN.png)

---

# Assignment Files

## Evidence

#### GitHub Repository or Assignment Folder URL

Paste the link containing these files:

- `CLAUDE.md`
- `scripts/azure-audit.sh`
- `.claude/skills/azure-audit/SKILL.md`
- `reports/baseline-report.txt`
- `reports/before-fix-report.txt`
- `reports/after-fix-report.txt`

`https://github.com/holarmyde/book-review-app.git`

---

# Submission Instructions

- Include your Full Name and Screenshots **1–17** under the corresponding evidence headings.
- Answer every **Notes You Must Write** question.
- Include the required source files and saved reports through your GitHub repository/folder link.
- Include your published LinkedIn post URL.
- Ensure required outputs show your Full Name.
- Use multiple captures under an evidence heading where needed for readability.
- Do not expose subscription IDs, tenant IDs, client secrets, passwords, tokens, connection strings, private keys, or other credentials.
- After saving all evidence, remove temporary resources created specifically for this assignment as directed in the guidelines.
- Submit your work using the method specified in the assignment guidelines.

---

# Completion Checklist

<<<<<<< HEAD
- [✅] Task 1: Azure resources confirmed and workspace created (Screenshot 1)
- [✅] Task 2: `CLAUDE.md` created with project context and safety rules (Screenshot 2)
- [✅] Task 3: Claude produced a read-only four-check plan before any script existed (Screenshot 3)
- [✅] Task 4: Audit script built, syntax-checked, and executable (Screenshots 4–5)
- [✅] Task 5: Baseline audit run and reviewed honestly (Screenshot 6)
- [✅] Task 6: `/azure-audit` skill created with no `Write` permission and run successfully (Screenshots 7–8)
- [✅] Task 7: A real finding fixed by you (not Claude) and re-verified as resolved (Screenshots 9–11)
- [✅] Notes comparing this to the Week 6 AWS audit completed
- [✅] No subscription IDs, tenant IDs, or credentials exposed
=======
- [✅] Task 1: Required resources confirmed and workspace created (Screenshots 1–2)
- [✅] Task 2: `CLAUDE.md` contains project context, workflow, safety rules, and output rules (Screenshot 3)
- [✅] Task 3: Four-check read-only plan completed before scripting (Screenshot 4)
- [✅] Task 4: Script created, syntax-checked, and executable (Screenshots 5–9)
- [✅] Task 5: Baseline report saved and exit code verified (Screenshots 10–11)
- [✅] Task 6: `/azure-audit` Skill created and run without executing remediation (Screenshots 12–13)
- [✅] Task 7: Finding documented, fixed manually, and verified as resolved (Screenshots 14–16)
- [✅] Baseline, before-fix, and after-fix reports saved
- [✅] All required written answers completed in my own words
- [✅] Required files and GitHub repository/folder URL included
- [✅] LinkedIn post published with the applicable credit note and tags (Screenshot 17)
- [✅] LinkedIn post URL included
- [✅] Full Name visible in required outputs
- [✅] No sensitive information exposed
>>>>>>> upstream/main

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

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
