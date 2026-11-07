# Assignment 7 — AI-Assisted AWS Security and Cost Audit

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will build a read-only Bash script that audits the AWS resources you deployed earlier this week — your S3 static site, EC2 instance(s), security groups, RDS database, and EBS volumes — for common security and cost misconfigurations.

You will then connect that script to Claude Code as a reusable `/aws-audit` skill that explains what it found and recommends a fix, without ever making the fix itself.

Finally, you will find a real misconfiguration in your own account, apply the fix yourself, and prove it worked with a second audit run.

---

# Task 1 — Confirm Your AWS Resources and Set Up Your Workspace

## Goal

Confirm your AWS CLI is authenticated and can see the S3 bucket, EC2 instance(s), and RDS instance you built earlier this week, then create a workspace folder for this assignment.

### Evidence

#### Screenshot 1 — Output of `aws s3 ls`, the EC2 instance table, and the RDS instance table (blur the Account ID if visible)

![task 1](screenshots/Screenshot%20with%20AWS%20Budget%20setup%20page%20showing%20the%20budget%20name.png)

---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort`

![task 1](screenshots/Screenshot%20with%20Auto%20Scaling%20Group%20showing%20desired,%20min,%20and%20max%20capacity%20.png)

---

### Notes You Must Write (Very Important)

**1. Which resources from this week's earlier assignments did you see in the listings?**

I confirmed that my AWS CLI was successfully authenticated and that I could access my AWS resources. The listings displayed the S3 bucket, EC2 instance(s), and RDS database created during my earlier DevOps assignments. These resources confirmed that my previous deployments were still present and accessible within my AWS account.

**2. Why must you confirm your resources exist before writing an audit script against them?**

Confirming that the resources exist ensures that the audit script targets the correct AWS infrastructure. It prevents errors caused by missing resources, incorrect resource identifiers, or configuration assumptions. It also establishes a reliable baseline so that the audit produces meaningful security and cost findings based on my actual AWS environment rather than assumed configurations.

---

# Task 2 — Define Safety Rules in CLAUDE.md

## Goal

Create a `CLAUDE.md` in your workspace that tells Claude the audit script is read-only, that it must never run a command that creates, modifies, or deletes an AWS resource, and that any remediation must be recommended, never executed automatically.

### Evidence

#### Screenshot 3 — `CLAUDE.md` open in VS Code showing all four sections

![task 2](screenshots/Screenshot%20with%20curl%20output%20proving%20the%20backend%20responds.png)

---

### Notes You Must Write (Very Important)

**1. Why should Claude never be given permission to run `revoke-security-group-ingress` itself, even if the fix is obviously correct?**

Claude should never execute this command because removing an inbound security group rule can affect application connectivity, administrative access, or communication between services. Even when a rule appears insecure, it may serve a legitimate operational purpose.

Allowing Claude to execute remediation automatically would remove human oversight and could introduce unintended service disruptions. The decision to modify AWS infrastructure must remain with the human operator, who can evaluate the potential impact and approve the change.

**2. Which rule prevents Claude from claiming a finding that the report does not support?**

The evidence-based reporting rule prevents Claude from inventing findings or making unsupported security claims. Claude must base its explanations and recommendations strictly on the actual audit script output and collected AWS evidence.

It must clearly distinguish between PASS, WARN, and FAIL results and must not assume a misconfiguration exists without supporting evidence.

---

# Task 3 — Plan the Audit with Claude Code

## Goal

Ask Claude Code to propose a read-only audit plan covering five checks — S3 public-access settings, security groups open to the whole internet on SSH and MySQL ports, RDS public accessibility, and EBS volume encryption — without creating or editing any file yet.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan

![task 3](screenshots/Screenshot%20with%20S3%20bucket%20created%20screen%20showing%20the%20bucket%20name%20and%20region.png)

---

### Notes You Must Write (Very Important)

**1. Which part of this task represents the Gather phase?**

The Gather phase involves collecting information about the existing AWS infrastructure using read-only AWS CLI commands.

For this assignment, it includes retrieving S3 public-access settings, security group inbound rules, RDS public accessibility settings, and EBS volume encryption status. This information provides the evidence required to identify possible security and cost misconfigurations.

**2. Did every proposed command start with `describe-`, `get-`, or `list-`? Why does that matter?**

Yes. The proposed AWS CLI commands use read-only operations such as describe-, get-, and list-.

This matters because these commands retrieve information without changing AWS resources. Using read-only commands ensures that the audit process does not accidentally create, modify, or delete infrastructure. It also maintains the separation between identifying a problem and implementing its remediation.

---

# Task 4 — Build the AWS Audit Script

## Goal

Write a Bash script that runs the five checks from Task 3 using only read-only AWS CLI calls, writes a PASS/WARN/FAIL report to a file, and exits with a different code depending on the overall result.

Make it executable and confirm it has no syntax errors.

### Evidence

#### Screenshot 5 — Top section of `aws-audit.sh` showing the variables and the checks array

![task 4](screenshots/Screenshot%20with%20AWS%20Budget%20setup%20page%20showing%20the%20budget%20name.png)

---

#### Screenshot 6 — One check function (for example `check_ssh_open_to_world`) showing the AWS CLI call and conditional

![task 4](screenshots/Screenshot%20with%20S3%20bucket%20created%20screen%20showing%20the%20bucket%20name%20and%20region.png)

---

#### Screenshot 7 — Output of `bash -n scripts/aws-audit.sh` and `ls -l scripts/aws-audit.sh`

![task 4](screenshots/Screenshot%20with%20Bucket%20policy%20page%20showing%20the%20policy%20saved%20successfully.png)
---

### Notes You Must Write (Very Important)

**1. What is stored in the checks array, and how does the loop use it?**

The checks array stores the names of the individual audit functions responsible for checking different AWS security configurations.

The script uses a loop to iterate through the array and execute each audit function systematically. This makes the script modular, organized, reusable, and easier to maintain. It also ensures that all five security checks are performed during every audit run.

**2. Why does every AWS CLI call in this script use `--query` and `--output text` instead of parsing raw JSON?**

The --query option uses JMESPath expressions to extract only the information required from AWS CLI responses, while --output text returns the results in a simple format that Bash can process easily.

This reduces unnecessary output, makes conditional statements easier to write, and avoids the complexity of parsing large JSON responses. It also makes the audit script more readable and efficient.

**3. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

Different exit codes allow the script to communicate its overall audit status clearly to users and other automation tools.
- Exit code 0 — HEALTHY: All security checks passed.
- Exit code 1 — WARN: One or more checks identified warnings that require attention.
- Exit code 2 — FAIL: One or more serious security issues were detected.
Using different exit codes makes it easier to integrate the audit into CI/CD pipelines and automated monitoring systems. It also allows users to distinguish between a healthy environment, potential risks, and critical security problems

---

# Task 5 — Run the Baseline Audit

## Goal

Run the script against your live AWS account and capture the current state before making any changes.

### Evidence

#### Screenshot 8 — Output of `./scripts/aws-audit.sh` showing your Full Name and all five checks

![task 5](screenshots/Screenshot%20with%20AWS%20Budget%20setup%20page%20showing%20the%20budget%20name.png)

---

#### Screenshot 9 — Output showing the captured exit code and final summary

![task 5](screenshots/Screenshot%20with%20RDS%20summary%20showing%20Multi-AZ%20Yes%20and%20Publicly%20accessible%20No.png)

---

### Notes You Must Write (Very Important)

**1. What is the overall status of your baseline audit?**

The overall status of my baseline audit was WARN. The audit script successfully executed all five security checks and identified one or more configurations that required further investigation and remediation. This provided an initial understanding of the security posture of my AWS environment.

**2. Did any check return FAIL or WARN? If so, which one, and what evidence did it show?**

This indicated a potential security risk because the configuration could expose my AWS resources to unauthorized access. The finding was documented in the audit report for further investigation and remediation.

**3. If every check passed, what does that tell you about the security posture of your account so far?**

it indicates that the audited AWS resources met the security requirements defined in the script at the time of the audit. It suggests that important security configurations, including S3 public-access restrictions, security group rules, RDS accessibility, and EBS encryption, were properly configured.

However, passing the audit does not guarantee that the entire AWS account is completely secure. Continuous monitoring and additional security assessments are still necessary to identify other potential vulnerabilities.

---

# Task 6 — Build and Run the /aws-audit Skill

## Goal

Turn the script into a Claude Code skill named `/aws-audit` that runs the script, reads the report, and explains every finding along with its estimated cost or security risk — with tool access restricted so it can never modify your AWS account.

### Evidence

#### Screenshot 10 — `SKILL.md` showing the frontmatter, tool restrictions, and safety rules

![task 6](screenshots/Screenshot%20with%20Subnets%20list%20showing%20four%20subnets%20and%20their%20Availability%20Zones.png)

---

#### Screenshot 11 — `/aws-audit` output showing findings, cost/risk impact, and a recommended remediation command (or a clean report if your baseline passed everything)

![task 6](screenshots/Screenshot%20with%20File%20Explorer%20or%20terminal%20showing%20the%20template%20folder%20contents.png)

---

### Notes You Must Write (Very Important)

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

The skill has Bash, Read, and Grep permissions because it needs to execute the read-only audit script, access the generated report, and search through the results.

It does not have Write permission because the purpose of the skill is to inspect and explain AWS configurations, not modify files or infrastructure. Restricting its permissions reduces the risk of unintended changes and enforces the principle of least privilege.

**2. What part is performed by Bash, and what part is performed by Claude?**

Bash performs the actual AWS audit by executing read-only AWS CLI commands, checking the configurations, and generating the PASS/WARN/FAIL report.

Claude interprets the report, explains the findings in understandable language, identifies potential security and financial risks, and recommends appropriate remediation commands.

This combination allows Bash to handle evidence collection and automated checks while Claude provides intelligent analysis and recommendations.

**3. Why is estimating cost/risk impact something the AI adds on top of a plain PASS/FAIL script?**

A traditional Bash script can identify whether a configuration passes or fails predefined security conditions, but it does not necessarily explain the wider business or operational impact.

Claude adds value by interpreting the findings and explaining their potential consequences, including unauthorized access, data exposure, service disruption, and unnecessary AWS spending.

This transforms a basic technical audit into a more meaningful security and cost assessment that helps engineers prioritize remediation decisions.

---

# Task 7 — Fix a Real Finding and Re-Verify

## Goal

Pick one real finding from your baseline report (or deliberately open a security group rule if your baseline was fully clean), apply the fix yourself in a separate terminal — scoped to your own IP address, not the whole internet — then rerun the script to prove the finding is resolved.

### Evidence

#### Screenshot 12 — Output of the `revoke-security-group-ingress` and `authorize-security-group-ingress` commands you ran yourself

![task 7](screenshots/Screenshot%20with%20RDS%20Security%20Group%20inbound%20rule%20showing%20the%20database%20port.png)

---

#### Screenshot 13 — Rerun of `./scripts/aws-audit.sh` showing the finding is now PASS

![task 7](screenshots/Screenshot%20with%20AWS%20Budget%20setup%20page%20showing%20the%20budget%20name.png)

---

### Notes You Must Write (Very Important)

**1. Which exact finding did you fix, and what command did you run?**

I identified an insecure security group inbound rule that allowed access from an overly broad IP address range. I manually removed the unrestricted rule and replaced it with a more restrictive rule that allowed access only from my own public IP address.

I used the AWS CLI commands revoke-security-group-ingress to remove the original rule and authorize-security-group-ingress to create the replacement rule with the appropriate IP restriction.

The change reduced unnecessary network exposure while preserving the access required for my environment.

**2. Why did you scope the new rule to your own IP address instead of leaving it open to `0.0.0.0/0`?**

I restricted the inbound rule to my own public IP address to follow the principle of least privilege. Allowing access from 0.0.0.0/0 exposes the specified port to the entire internet, increasing the possibility of unauthorized access, brute-force attempts, and other attacks.

Restricting access to my own IP address significantly reduces the attack surface and ensures that only an authorized source can access the specified service.

**3. Did Claude execute the remediation command, or did you? Why does that matter?**

I executed the remediation commands manually in a separate terminal rather than allowing Claude to make changes to my AWS infrastructure.

This was important because the assignment was designed to demonstrate human-approved remediation. Claude's responsibility was to analyze the evidence, identify the risk, and recommend the appropriate fix, while I retained control over the actual infrastructure changes.

**4. Which phase of the Agentic Loop does the Bash script represent? Which phase does Claude's explanation represent? Which phase is you running the fix?**

Gather: The Bash script collects AWS resource information using read-only AWS CLI commands.



Reason: Claude analyzes the audit report, interprets the findings, and explains the associated risks and recommended remediation.



Act: I manually execute the recommended remediation commands after reviewing and approving the changes.



Verify: I rerun the audit script to confirm that the identified misconfiguration has been resolved.

---

# LinkedIn Post (Required)

## Goal

Create a LinkedIn post including:

- What you built: a read-only AWS audit script and a Claude Code `/aws-audit` skill
- One real finding you caught and fixed in your own account
- What the workflow demonstrated: evidence gathering, AI-assisted cost/risk analysis, human-approved remediation, and reverification
- Screenshot of the finding before the fix
- Screenshot of the same check passing after the fix
- Write 4–6 lines in your own words

Suggested tags:

`#DMIByPravinMishra #AWS #AgenticAI #ClaudeCode #DevOps`

### Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/efye-nNP`

---

#### Screenshot of Published LinkedIn Post

![linkedIN](screenshots/Screenshot%20with%20claude%20linkedIN.png)

---

# Submission Instructions

Complete all tasks in sequence.

Your submission must include:

- All 13 required task screenshots
- Answers to every **Notes You Must Write** question
- `CLAUDE.md`
- `scripts/aws-audit.sh`
- `.claude/skills/aws-audit/SKILL.md`
- `reports/aws-audit-report.txt` baseline report and the reverified report from Task 7
- GitHub folder or repository URL containing the assignment files
- Your Full Name visible in the required outputs
- LinkedIn post URL
- Screenshot of the published LinkedIn post
- GitHub repository URL (containing all assignment files)

---

# Completion Checklist

- [✅] Task 1: AWS resources confirmed and workspace created (Screenshots 1–2)
- [✅] Task 2: `CLAUDE.md` created with project context and safety rules (Screenshot 3)
- [✅] Task 3: Claude produced a read-only five-check audit plan before any script existed (Screenshot 4)
- [✅] Task 4: `aws-audit.sh` built, executable, and passes `bash -n` (Screenshots 5–7)
- [✅] Task 5: Baseline audit captured and saved with Full Name visible (Screenshots 8–9)
- [✅] Task 6: `/aws-audit` skill loads and runs successfully with no Write permission (Screenshots 10–11)
- [✅] Task 7: A real finding was fixed by you and reverified as PASS (Screenshots 12–13)
- [✅] Skill never executed a remediation command
- [✅] New security group rule is scoped to your own IP, not `0.0.0.0/0`
- [✅] All 13 required task screenshots are included
- [✅] All "Notes You Must Write" questions are answered in your own words
- [✅] No AWS credentials or unblurred account IDs exposed
- [✅] LinkedIn post published and URL submitted
- [✅] GitHub repository URL included in submission
- [✅] All assignment files committed and visible in GitHub repository

---

# Final Submission

Submit your GitHub repository URL containing all assignment files, screenshots, reports, and output.

### GitHub Repository URL

Paste your GitHub repository URL here:

`https://github.com/holarmyde/Ultimate-Agentic-DevOps-with-Claude-Code.git`

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