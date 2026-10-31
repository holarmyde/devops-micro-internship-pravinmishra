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

Add your answer here.

---

**2. Why should you confirm playbook syntax before building a risk-review script?**

Add your answer here.

---

# Task 2 — Create Project Context and Safety Rules in CLAUDE.md

## Goal

Create a `CLAUDE.md` file that tells Claude Code how this project must behave.

### Evidence

#### Screenshot 4 — `CLAUDE.md` open in VS Code or terminal showing the safety rules

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Why should Claude Code have project-specific safety rules?**

Add your answer here.

---

**2. Why should the human run the real Ansible playbook manually?**

Add your answer here.

---

**3. Which rule prevents Claude Code from applying changes automatically?**

Add your answer here.

---

# Task 3 — Ask Claude Code to Plan the Risk Review

## Goal

Use Claude Code to produce a read-only plan before writing the Bash script.

### Evidence

#### Screenshot 5 — Claude Code showing the four-category risk-classification plan

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Which part of this task represents the Gather phase?**

Add your answer here.

---

**2. Which part represents the Analyze phase?**

Add your answer here.

---

**3. How did you verify Claude Code did not create or edit files?**

Add your answer here.

---

# Task 4 — Build the Ansible Risk Review Script

## Goal

Create a Bash script that runs an Ansible dry run and classifies risky changes.

### Evidence

#### Screenshot 6 — Top section of `ansible-check-review.sh` showing `full_name`, `playbook_path`, `inventory_path`, and the `checks` array

Add your screenshot here.

---

#### Screenshot 7 — Middle section showing `extract_changed_tasks` and `check_tasks_matching_pattern`

Add your screenshot here.

---

#### Screenshot 8 — Bottom section showing the loop, summary, and exit behavior

Add your screenshot here.

---

#### Screenshot 9 — Output of `bash -n ansible-check-review.sh` and `ls -l ansible-check-review.sh`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is stored in the `changed_tasks` array?**

Add your answer here.

---

**2. Which function finds changed tasks from the Ansible output?**

Add your answer here.

---

**3. Why does the script use `--check --diff`?**

Add your answer here.

---

**4. Why does the script use different exit codes for healthy, warning, and failed results?**

Add your answer here.

---

# Task 5 — Run the Baseline Dry-Run Review

## Goal

Run the script against your current EpicBook playbook and confirm the baseline risk status.

### Evidence

#### Screenshot 10 — Output of `./ansible-check-review.sh`

Add your screenshot here.

---

#### Screenshot 11 — Output of `echo "Captured Exit Code: $script_exit_code"` and `cat reports/ansible-risk-report.txt`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What was the overall status of your baseline run?**

Add your answer here.

---

**2. Did any tasks report `changed`?**

Add your answer here.

---

**3. Were any changed tasks flagged as risky?**

Add your answer here.

---

**4. What does the script exit code mean?**

Add your answer here.

---

# Task 6 — Create and Run the Claude Code Skill

## Goal

Turn the Bash script into a reusable Claude Code skill called `/ansible-risk-review`.

### Evidence

#### Screenshot 12 — `SKILL.md` showing the frontmatter, allowed tools, and safety rules

Add your screenshot here.

---

#### Screenshot 13 — Claude Code output after running `/ansible-risk-review`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Why does this skill allow `Bash`, `Read`, and `Grep`?**

Add your answer here.

---

**2. Why does this skill not allow file editing?**

Add your answer here.

---

**3. What part is handled by Bash?**

Add your answer here.

---

**4. What part is handled by Claude Code?**

Add your answer here.

---

**5. Why is this better than asking Claude Code if the playbook is safe without giving it evidence?**

Add your answer here.

---

# Task 7 — Introduce a Controlled Risky Change and Let the Skill Catch It

## Goal

Add a small controlled risky change in your lab playbook and confirm the script and Claude Code catch it before applying.

### Evidence

#### Screenshot 14 — The added risky task inside the role file

Add your screenshot here.

---

#### Screenshot 15 — Output of `./ansible-check-review.sh`

Add your screenshot here.

---

#### Screenshot 16 — Claude Code `/ansible-risk-review` output showing the risky finding

Add your screenshot here.

---

#### Screenshot 17 — Output of `cat reports/risky-change-report.txt`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Which risk category did the added task fall into?**

Add your answer here.

---

**2. What evidence proves the task would change something?**

Add your answer here.

---

**3. Did Claude Code apply the playbook?**

Add your answer here.

---

**4. Why is it important that Claude Code only analyzed the risk?**

Add your answer here.

---

**5. Which phase of the Agentic Loop is represented by the Bash report?**

Add your answer here.

---

# Task 8 — Apply as the Human, Verify, and Write the Change Summary

## Goal

Review the risky-change report, apply the playbook manually as the human operator, and verify the result.

### Evidence

#### Screenshot 18 — Output of the real playbook run showing the final recap with `failed=0`

Add your screenshot here.

---

#### Screenshot 19 — Output of `ansible web -i inventory.ini -m ping`

Add your screenshot here.

---

#### Screenshot 20 — Second `/ansible-risk-review` output after applying the change

Add your screenshot here.

---

#### Screenshot 21 — Output of `ls -lah reports`

Add your screenshot here.

---

#### Screenshot 22 — `change-summary.md` showing all required sections and your Full Name

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What command did you run to apply the change for real?**

Add your answer here.

---

**2. Who made the final decision to apply the playbook?**

Add your answer here.

---

**3. What evidence proves the VM is still reachable?**

Add your answer here.

---

**4. Why should the risk review be run again after applying?**

Add your answer here.

---

**5. What could go wrong if an AI agent applied Ansible changes automatically?**

Add your answer here.

---

# LinkedIn Post Required

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`Add your URL here`

---

#### Screenshot — Published LinkedIn post

Add your screenshot here.

---

# Required Files

Confirm that the following files are included in your GitHub repository or assignment folder:

- [ ] `CLAUDE.md`
- [ ] `ansible-check-review.sh`
- [ ] `.claude/skills/ansible-risk-review/SKILL.md`
- [ ] `reports/risky-change-report.txt`
- [ ] `reports/post-apply-report.txt`
- [ ] `change-summary.md`
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
- [ ] Task 1: EpicBook connectivity confirmed and workspace created
- [ ] Task 2: `CLAUDE.md` created with safety rules
- [ ] Task 3: Claude Code produced a read-only risk-review plan
- [ ] Task 4: `ansible-check-review.sh` created and syntax checked
- [ ] Task 5: Baseline dry-run review completed
- [ ] Task 6: Claude Code `/ansible-risk-review` skill created and tested
- [ ] Task 7: Controlled risky change introduced and detected
- [ ] Task 8: Human applied the change and verified the result
- [ ] Risky-change report saved
- [ ] Post-apply report saved
- [ ] Change summary completed
- [ ] All screenshots added
- [ ] All notes answered
- [ ] LinkedIn post published
- [ ] LinkedIn post URL added
- [ ] No sensitive information exposed
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