# Assignment 6 — AI-Assisted Ansible Change Risk Review

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will build an AI-assisted Ansible risk-review workflow using `ansible-playbook --check --diff`, Bash scripting, and Claude Code.

You will review possible server changes before applying them, classify risky tasks, and keep the final apply decision under human control.

---

# Task 1 — Confirm EpicBook Connectivity and Create the Workspace

## Goal

Confirm that your previous EpicBook Ansible project is working before creating the risk-review automation.

### Evidence

#### Screenshot 1 — Output of `ansible web -i inventory.ini -m ping`

![task 1](screenshots/Screenshot%20with%20Terminal%20showing%20ansible%20all%20-m%20ping%20with%20every%20host%20reachable.png)

---

#### Screenshot 2 — Output of `ansible-playbook -i inventory.ini site.yml --syntax-check`

![task 2](screenshots/Screenshot%20with%20CLAUDE_md%20open%20showing%20the%20workflow%20and%20safety%20rules.png)

---

<<<<<<< HEAD
# Task 3 — Use Agentic AI to Plan the Risk Categories

## Goal

Ask Claude Code to read `CLAUDE.md` and propose a risk-classification plan with exactly four categories — service restarts/handlers, firewall rule changes, user/sudo changes, and package or file removal — before any script exists.

### Evidence

#### Screenshot 3 — Claude Code showing the four-category risk-classification plan

![task 3](screenshots/Screenshot%20with%20Claude%20Code%20showing%20the%204%20category%20risk%20classification%20plan.png)

---

# Task 4 — Build the Ansible Risk Review Script

## Goal

Create `ansible-check-review.sh` that runs `ansible-playbook --check --diff`, parses the task and recap output, deduplicates changed tasks so a task that changes across multiple hosts in your inventory is counted once rather than once per host, and classifies each changed task into one of the four risk categories.

### Evidence

#### Screenshot 4 — Editor showing the script's task-classification functions

![task 4](screenshots/Screenshot%20with%20Editor%20showing%20the%20script's%20task-classification%20functions.png)
---

#### Screenshot 5 — Terminal showing `bash -n` passing with no syntax errors

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20`bash%20-n`%20passing%20with%20no%20syntax%20errors.png)

---

# Task 5 — Run the Dry-Run Review Against the Current Playbook

## Goal

Run the script against your unmodified playbook and confirm it reports low risk with no unreachable or failed hosts.

### Evidence

#### Screenshot 6 — Terminal output showing the risk report and its overall status

![task 4](screenshots/Screenshot%20%20with%20Terminal%20output%20showing%20the%20risk%20report%20and%20its%20overall%20status.png)

---

# Task 6 — Create and Run the /ansible-risk-review Skill

## Goal

Create a Claude Code skill restricted to read-only tools that runs the script, reads the risk report, and explains which changes are risky and why — without ever running the playbook itself.

### Evidence

#### Screenshot 7 — `SKILL.md` frontmatter showing `allowed-tools` with no `Write`

![task 6](screenshots/Screenshot%20with%20SKILL.md%20frontmatter%20showing%20`allowed-tools`%20with%20no%20Write.png)

---

#### Screenshot 8 — `/ansible-risk-review` output for the baseline playbook

![task 6](screenshots/Screenshot%20with%20ansible-risk-review`%20output%20for%20the%20baseline%20playbook.png)

---

# Task 7 — Introduce a Risky Change and Let the Skill Catch It

## Goal

Add one task to your playbook that touches a risk category — a service restart, a firewall rule change, or a file removal — then confirm the skill flags it and recommends holding for review before you apply anything for real.

### Evidence

#### Screenshot 9 — Raw dry-run output showing the new task reporting `changed`

![task 7](screenshots/Screenshot%20with%20Raw%20dry-run%20output%20showing%20the%20new%20task%20reporting%20changed.png)

---

#### Screenshot 10 — `/ansible-risk-review` output showing the risky finding and the recommendation to hold for review

![task 7](screenshots/Screenshot%20with%20output%20showing%20the%20risky%20finding%20and%20the%20recommendation%20.png)

---

# Task 8 — Apply as the Human, Verify, and Write a Change Summary

## Goal

Review the recommendation, run the playbook for real yourself (never Claude), confirm every host still responds to the ping module, run the skill again to confirm no further changes are pending, and write a short change summary.

### Evidence

#### Screenshot 11 — Terminal showing the real `ansible-playbook` run applying successfully

![task 8](screenshots/Screenshot%20with%20Terminal%20showin%20d%20real%20ansible-playbook%20run%20applying%20successfully.png)

---

#### Screenshot 12 — Second `/ansible-risk-review` output confirming no further changes are pending
=======
#### Screenshot 3 — Output of `pwd` and `find . -maxdepth 4 -type d | sort`
>>>>>>> upstream/main

![task 8](screenshots/Screenshot%20with%20Second%20output%20confirming%20no%20further%20changes%20are%20pending.png)

---

### Notes

Answer the following in your own words:

<<<<<<< HEAD
ansible-playbook --check --diff deserves the same respect as terraform plan because both reveal intended infrastructure changes before they are applied, helping identify unexpected or risky changes. The AI skill was allowed to analyze this read-only evidence, but not run the real playbook, because applying infrastructure changes must remain under explicit human control and approval.
=======
**1. What proves that Ansible can reach your EpicBook VM?**

Ansible successfully reached the EpicBook VM when I ran:
ansible web -i inventory.ini -m ping


The output returned "ping": "pong" and showed SUCCESS for the target host. This confirms that Ansible can connect to the VM, authenticate successfully, and execute remote modules.

2. Why should you confirm playbook syntax before building a risk-review script?

Checking playbook syntax helps identify YAML formatting errors, incorrect indentation, missing parameters, and invalid Ansible configurations before building the automation. This ensures that the risk-review script works with a valid playbook rather than reporting errors caused by an existing syntax problem.

---

**2. Why should you confirm playbook syntax before building a risk-review script?**

Checking playbook syntax helps identify YAML formatting errors, incorrect indentation, missing parameters, and invalid Ansible configurations before building the automation. This ensures that the risk-review script works with a valid playbook rather than reporting errors caused by an existing syntax problem.

---

# Task 2 — Create Project Context and Safety Rules in CLAUDE.md

## Goal

Create a `CLAUDE.md` file that tells Claude Code how this project must behave.

### Evidence

#### Screenshot 4 — `CLAUDE.md` open in VS Code or terminal showing the safety rules

![task 4](screenshots/Screenshot%20with%20Editor%20showing%20the%20script's%20task-classification%20functions.png)

---

### Notes

Answer the following in your own words:

**1. Why should Claude Code have project-specific safety rules?**

Project-specific safety rules guide Claude Code on how to behave within the Ansible project. They establish boundaries, prevent unauthorized infrastructure changes, and ensure that Claude Code follows the project's security and operational requirements.

---

**2. Why should the human run the real Ansible playbook manually?**

The human should run the real playbook because infrastructure changes can affect server availability, security, and application functionality. Manual execution ensures that a qualified human reviews the risk report, understands the proposed changes, and gives explicit approval before anything is applied.

---

**3. Which rule prevents Claude Code from applying changes automatically?**

The rule that prevents Claude Code from applying changes automatically is the instruction that it must never execute the real ansible-playbook command without --check. It is restricted to read-only analysis and must leave the final infrastructure-application decision to the human operator.

---

# Task 3 — Ask Claude Code to Plan the Risk Review

## Goal

Use Claude Code to produce a read-only plan before writing the Bash script.

### Evidence

#### Screenshot 5 — Claude Code showing the four-category risk-classification plan

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20`bash%20-n`%20passing%20with%20no%20syntax%20errors.png)

---

### Notes

Answer the following in your own words:

**1. Which part of this task represents the Gather phase?**

The Gather phase involves Claude Code reading the project's CLAUDE.md file and examining the existing Ansible project structure and instructions to understand the environment, requirements, and safety restrictions.

---

**2. Which part represents the Analyze phase?**

The Analyze phase involves Claude Code reviewing the project requirements and identifying the four risk categories:
- Service restarts and handlers.
- Firewall rule changes.
- User and sudo changes.
- Package or file removal.
It then proposes a classification plan for identifying potentially dangerous infrastructure changes.

---

**3. How did you verify Claude Code did not create or edit files?**

I verified this by reviewing Claude Code's actions and confirming that it only inspected project files and provided a risk-classification plan. I also checked that no new files were created or existing files modified during the planning stage.

---

# Task 4 — Build the Ansible Risk Review Script

## Goal

Create a Bash script that runs an Ansible dry run and classifies risky changes.

### Evidence

#### Screenshot 6 — Top section of `ansible-check-review.sh` showing `full_name`, `playbook_path`, `inventory_path`, and the `checks` array

![task 4](screenshots/Screenshot%20%20with%20Terminal%20output%20showing%20the%20risk%20report%20and%20its%20overall%20status.png)

---

#### Screenshot 7 — Middle section showing `extract_changed_tasks` and `check_tasks_matching_pattern`

![task 6](screenshots/Screenshot%20with%20SKILL.md%20frontmatter%20showing%20`allowed-tools`%20with%20no%20Write.png)

---

#### Screenshot 8 — Bottom section showing the loop, summary, and exit behavior

![task 6](screenshots/Screenshot%20with%20ansible-risk-review`%20output%20for%20the%20baseline%20playbook.png)

---

#### Screenshot 9 — Output of `bash -n ansible-check-review.sh` and `ls -l ansible-check-review.sh`

![task 7](screenshots/Screenshot%20with%20Raw%20dry-run%20output%20showing%20the%20new%20task%20reporting%20changed.png)

---

### Notes

Answer the following in your own words:

**1. What is stored in the `changed_tasks` array?**

The changed_tasks array stores the names of Ansible tasks that report changed during the dry-run execution. The script removes duplicate task names so that a task affecting multiple hosts is counted only once.

---

**2. Which function finds changed tasks from the Ansible output?**

The extract_changed_tasks function identifies and extracts changed task names from the Ansible dry-run output and stores them in the changed_tasks array.

---

**3. Why does the script use `--check --diff`?**

The --check option runs Ansible in simulation mode to show what changes would happen without applying them to the servers. The --diff option displays the differences between the current configuration and the proposed configuration.

Together, they provide visibility into intended infrastructure changes before actual execution, helping identify unexpected or risky modifications.

---

**4. Why does the script use different exit codes for healthy, warning, and failed results?**

Different exit codes allow users and automation tools to understand the result of the risk review programmatically.
- 0 — Healthy: No risky changes or execution failures were detected.
- 1 — Warning: Potentially risky changes were detected and require human review.
- 2 — Failed: The review could not complete successfully because of an execution or validation error.
This makes the script easier to integrate into automated workflows and helps prevent risky changes from being overlooked.

---

# Task 5 — Run the Baseline Dry-Run Review

## Goal

Run the script against your current EpicBook playbook and confirm the baseline risk status.

### Evidence

#### Screenshot 10 — Output of `./ansible-check-review.sh`

![task 7](screenshots/Screenshot%20with%20output%20showing%20the%20risky%20finding%20and%20the%20recommendation%20.png)

---

#### Screenshot 11 — Output of `echo "Captured Exit Code: $script_exit_code"` and `cat reports/ansible-risk-report.txt`

![task 8](screenshots/Screenshot%20with%20Terminal%20showin%20d%20real%20ansible-playbook%20run%20applying%20successfully.png)

---

### Notes

Answer the following in your own words:

**1. What was the overall status of your baseline run?**

The overall status of my baseline run was LOW RISK / HEALTHY. The Ansible risk-review script completed its dry-run assessment successfully without identifying any risky changes in the existing playbook.

---

**2. Did any tasks report `changed`?**

If your baseline report showed no changes, use:

No tasks reported changed during the baseline dry run. This indicates that Ansible did not identify any configuration changes that needed to be applied to the target VM.

If your report listed changed tasks, replace this answer with their actual names.

---

**3. Were any changed tasks flagged as risky?**

No changed tasks were flagged as risky during the baseline review. The script checked the task output against the four defined risk categories and found no potentially dangerous changes requiring additional review.

---

**4. What does the script exit code mean?**

The script exit code indicates the overall result of the risk-review process. Exit code 0 represents a healthy review, exit code 1 indicates that risky changes require human attention, and exit code 2 indicates that the review encountered an error.

The exit code allows the result to be interpreted by both users and automation tools.

---

# Task 6 — Create and Run the Claude Code Skill

## Goal

Turn the Bash script into a reusable Claude Code skill called `/ansible-risk-review`.

### Evidence

#### Screenshot 12 — `SKILL.md` showing the frontmatter, allowed tools, and safety rules

![task 6](screenshots/Screenshot%20with%20SKILL.md%20frontmatter%20showing%20`allowed-tools`%20with%20no%20Write.png)

---

#### Screenshot 13 — Claude Code output after running `/ansible-risk-review`

![task 6](screenshots/Screenshot%20with%20ansible-risk-review`%20output%20for%20the%20baseline%20playbook.png)

---

### Notes

Answer the following in your own words:

**1. Why does this skill allow `Bash`, `Read`, and `Grep`?**

These tools provide the skill with the ability to execute the risk-review script, read the generated reports, and search for relevant information within project files.
- Bash runs the review script and retrieves command output.
- Read examines the generated risk reports and project documentation.
- Grep searches for specific tasks, risk categories, and relevant configuration details.
Together, these tools allow Claude Code to investigate and explain potential risks without modifying the project.

---

**2. Why does this skill not allow file editing?**

The skill does not allow file editing because its responsibility is to review and explain potential infrastructure changes, not to modify project files or apply changes. Restricting editing permissions protects the existing configuration and maintains human control over infrastructure modifications.

---

**3. What part is handled by Bash?**

Bash handles the technical execution of the risk-review process. It runs ansible-playbook --check --diff, extracts changed tasks, removes duplicate entries, classifies tasks into risk categories, generates reports, and returns the appropriate exit code.

---

**4. What part is handled by Claude Code?**

Claude Code handles the AI-assisted analysis and explanation. It reads the Bash-generated reports, interprets the findings, identifies potentially dangerous changes, explains why they are risky, and recommends whether the changes should be reviewed before application.

---

**5. Why is this better than asking Claude Code if the playbook is safe without giving it evidence?**

This approach is better because Claude Code bases its analysis on actual Ansible dry-run results rather than assumptions or guesses. The Bash script provides structured evidence of proposed changes, while Claude Code interprets that evidence and explains the associated risks.

This makes the review more reliable, transparent, and evidence-based.

---

# Task 7 — Introduce a Controlled Risky Change and Let the Skill Catch It

## Goal

Add a small controlled risky change in your lab playbook and confirm the script and Claude Code catch it before applying.

### Evidence

#### Screenshot 14 — The added risky task inside the role file

![task 6](screenshots/Screenshot%20with%20output%20showing%20the%20risky%20finding%20and%20the%20recommendation%20.png)

---

#### Screenshot 15 — Output of `./ansible-check-review.sh`

![task 7](screenshots/Screenshot%20with%20Terminal%20showing%20ansible%20all%20-m%20ping%20with%20every%20host%20reachable.png)

---

#### Screenshot 16 — Claude Code `/ansible-risk-review` output showing the risky finding

![task 7](screenshots/Screenshot%20with%20ansible-risk-review`%20output%20for%20the%20baseline%20playbook.png)

---

#### Screenshot 17 — Output of `cat reports/risky-change-report.txt`

![task 7](screenshots/Screenshot%20with%20Raw%20dry-run%20output%20showing%20the%20new%20task%20reporting%20changed.png)

---

### Notes

Answer the following in your own words:

**1. Which risk category did the added task fall into?**

Use the answer that matches the task you actually added.

For example, if you added a task to restart NGINX:

The added task fell into the service restarts/handlers risk category because restarting a service can interrupt application availability or affect users connected to the server.

Other possible answers:
- Firewall modification: Firewall rule changes.
- User or sudo permission modification: User/sudo changes.
- File deletion or package removal: Package or file removal.

---

**2. What evidence proves the task would change something?**

The Ansible dry-run output showed that the newly added task reported changed. This confirms that Ansible detected a proposed modification during check mode. The --diff option also provides additional evidence of the intended configuration difference when the module supports diff reporting.

---

**3. Did Claude Code apply the playbook?**

No. Claude Code did not apply the playbook. It only executed the authorized read-only risk-review workflow, examined the generated report, and explained the potential risks. The actual playbook execution remained under human control.

---

**4. Why is it important that Claude Code only analyzed the risk?**

It is important because AI-generated decisions should not automatically authorize infrastructure changes. A potentially incorrect or misunderstood change could cause service downtime, security problems, or loss of important data. Keeping Claude Code in an analysis-only role ensures that a human reviews the findings and approves the actual execution.

---

**5. Which phase of the Agentic Loop is represented by the Bash report?**

The Bash report primarily represents the Observe/Gather phase because it collects and presents evidence about the proposed infrastructure changes. Claude Code then uses that evidence during the Analyze phase to interpret the risks and recommend an appropriate action.
---

# Task 8 — Apply as the Human, Verify, and Write the Change Summary

## Goal

Review the risky-change report, apply the playbook manually as the human operator, and verify the result.

### Evidence

#### Screenshot 18 — Output of the real playbook run showing the final recap with `failed=0`

![task 8](screenshots/Screenshot%20with%20Terminal%20showing%20%20Ansible%20run%20and%20final%20recap%20with%20failed=0.png)

---

#### Screenshot 19 — Output of `ansible web -i inventory.ini -m ping`

![task 8](screenshots/Screenshot%20with%20Editor%20showing%20inventory.ini%20and%20the%20three%20plays.png)

---

#### Screenshot 20 — Second `/ansible-risk-review` output after applying the change

![task 8](screenshots/Screenshot%20with%20ansible-risk-review`%20output%20for%20the%20baseline%20playbook.png)

---

#### Screenshot 21 — Output of `ls -lah reports`

![task 8](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20ansible-lint%20--version.png)

---

#### Screenshot 22 — `change-summary.md` showing all required sections and your Full Name

![task 8](screenshots/Screenshot%20with%20Terminal%20showing%20uptime%20output%20for%20all%20four%20hosts.png)

---

### Notes

Answer the following in your own words:

**1. What command did you run to apply the change for real?**

I manually executed the Ansible playbook using:
ansible-playbook -i inventory.ini site.yml


This command runs the playbook without check mode and applies the approved configuration changes to the target VM.

---

**2. Who made the final decision to apply the playbook?**

I, as the human operator, made the final decision to apply the playbook after reviewing the risk report and understanding the proposed changes. Claude Code provided recommendations but did not authorize or execute the actual infrastructure changes.


---

**3. What evidence proves the VM is still reachable?**

The successful output of the following command proves that the VM remained reachable:
ansible web -i inventory.ini -m ping


The target host returned SUCCESS with "ping": "pong", confirming that Ansible could still connect to the VM and execute remote modules after the changes were applied.

---

**4. Why should the risk review be run again after applying?**

The risk review should be run again to confirm that the intended changes have been applied and that no additional changes remain pending. A successful second review should report no unexpected changes, providing confidence that the infrastructure has reached the expected configuration state.

---

**5. What could go wrong if an AI agent applied Ansible changes automatically?**

If an AI agent applied Ansible changes automatically, it could introduce incorrect configurations, remove important files or packages, change firewall rules, modify user permissions, or restart critical services without proper approval.

These actions could cause service downtime, security vulnerabilities, data loss, or system failures. Keeping a human involved in the final approval process reduces these risks and ensures accountability for infrastructure changes.

---

# LinkedIn Post Required

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/egT2aNdb`

---

#### Screenshot — Published LinkedIn post

![linkedIn](screenshots/Screenshot%20ansible_linkedIn_09.png)

---

# Required Files

Confirm that the following files are included in your GitHub repository or assignment folder:

- [✅ ] `CLAUDE.md`
- [✅ ] `ansible-check-review.sh`
- [✅ ] `.claude/skills/ansible-risk-review/SKILL.md`
- [✅ ] `reports/risky-change-report.txt`
- [✅ ] `reports/post-apply-report.txt`
- [✅ ] `change-summary.md`
>>>>>>> upstream/main

---

# Submission Instructions

- Add all required screenshots in your submission.
- Full Name must be visible in required screenshots and reports.
- All required notes must be answered clearly.
- Do not expose SSH private keys, passwords, cloud credentials, database credentials, or secret environment variables.
- Add your GitHub repository or folder URL inside this document.

---

# Completion Checklist

<<<<<<< HEAD
- [✅ ] Task 1: Inventory connectivity confirmed (Screenshot 1)
- [✅ ] Task 2: `CLAUDE.md` created with workflow and safety rules (Screenshot 2)
- [✅ ] Task 3: Four-category risk-classification plan produced before scripting (Screenshot 3)
- [✅ ] Task 4: `ansible-check-review.sh` built and passes `bash -n` (Screenshots 4–5)
- [✅ ] Task 5: Dry-run review run against the current playbook (Screenshot 6)
- [✅ ] Task 6: `/ansible-risk-review` skill created and run (Screenshots 7–8)
- [✅ ] Task 7: Risky change introduced and correctly flagged (Screenshots 9–10)
- [✅ ] Task 8: Change applied by the human, verified, and summarized (Screenshots 11–12)
- [✅ ] Reflection written (Notes)
- [✅ ] No sensitive data exposed
=======
- [✅ ] Task 1: EpicBook connectivity confirmed and workspace created
- [✅ ] Task 2: `CLAUDE.md` created with safety rules
- [✅ ] Task 3: Claude Code produced a read-only risk-review plan
- [✅ ] Task 4: `ansible-check-review.sh` created and syntax checked
- [✅] Task 5: Baseline dry-run review completed
- [✅] Task 6: Claude Code `/ansible-risk-review` skill created and tested
- [✅] Task 7: Controlled risky change introduced and detected
- [✅] Task 8: Human applied the change and verified the result
- [✅] Risky-change report saved
- [✅] Post-apply report saved
- [✅] Change summary completed
- [✅] All screenshots added
- [✅] All notes answered
- [✅] LinkedIn post published
- [✅] LinkedIn post URL added
- [✅] No sensitive information exposed
>>>>>>> upstream/main

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