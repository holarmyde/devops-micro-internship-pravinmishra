# Assignment 6 — Build an AI-Assisted Linux Health Check (AI-Assisted Linux Incident Triage)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build a read-only Bash triage script that checks the health of your Ubuntu server and Nginx application, connect it to Claude Code as a reusable `/linux-triage` skill, simulate a controlled Nginx incident, use the skill to gather and analyze evidence, recover the service manually, and verify recovery. The workflow follows the Agentic Loop: Gather → Analyze → Human Act → Verify.

---

# Task 1 — Confirm the Healthy Baseline and Create the Workspace

## Goal

Confirm that Nginx and the React application are healthy before building the automation.

### Evidence

#### Screenshot 1 — Output of `systemctl is-active nginx`, `ss -ltn | grep ':80'`, and `curl -I http://localhost`

Add your screenshot here.

--- [task 1](screenshots/Screenshot%20with%20system_active%206.png)
    [task 1](screenshots/Screenshot%20with%20system%20grep%2080.png)
    [task 1](screenshots/Screenshot%20with%20curl%20localhost%206.png)

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort` showing the workspace folder structure

Add your screenshot here.

--- [task 1](screenshots/Screenshot%20with%20pwd%20no%206.png)
    [task 1](screenshots/Screenshot%20with%20maxdepth%206.png)


### Notes

Answer the following in your own words:

**1. What proves that Nginx is running?**

Add your answer here.

---What proves that Nginx is running is when systemctl is-active nginx, it returns active. 


**2. What proves that the server is listening for HTTP traffic?**

Add your answer here.

--- To prove a server is actively listening for HTTP traffic, you can verify it by checking for open network ports,
    The result of ss -ltn | grep ':80' output verifies that port 80 is listening. This means the server is ready to receive HTTP requests.


**3. Why must you capture a healthy baseline before simulating an incident?**

Add your answer here.

--- It helps to put into stability normal operation envronment. This  is essential for accurately measuring how systems respond. After simulations comparison is made to see what went for the failed state as against the healthy state. Then normalcy is hit again

# Task 2 — Create Project Context and Safety Rules in CLAUDE.md

## Goal

Tell Claude exactly what this project does and what it is not allowed to do.

### Evidence

#### Screenshot 3 — CLAUDE.md open in VS Code showing all four sections (Project Overview, Incident Workflow, Safety Rules, Output Rules)

Add your screenshot here.

--- [task 2](screenshots/Screenshot%20with%20claude_vs.png)

### Notes

Answer the following in your own words:

**1. Why should Claude receive project-specific operational rules?**

Add your answer here.

--- Because it ensures the AI by default would align with your workflows and honor necessary guardrails. Defining these rules reduces consistent repetition and maintains high quality across iterations

**2. Why is the human required to execute the recovery command?**

Add your answer here.

--- A human is required to execute recovery commands so as to provide essential context, oversight, and liability protection. Its important to know if the recovery command is safe before running it. e.g.  gemini can recommend a command, but it should not make changes to the server by itself.


**3. Which rule prevents Claude from making an unsupported diagnosis?**

Add your answer here.

--- the Professional Consultation Rule.

# Task 3 — Use Agentic AI to Plan Before Writing the Script

## Goal

Use Claude Code to inspect the environment and produce a read-only plan before creating any Bash code.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan and read-only inspection results

Add your screenshot here.

--- [task 3](screenshots/Screenshot%20with%20filecheck_1.png)
    [task 3](screenshots/Screenshot%20with%20filecheck_2.png)
    [task 3](screenshots/Screenshot%20with%20filecheck_3.png)
    [task 3](screenshots/Screenshot%20with%20file_inspection_result1.png)
    [task 3](screenshots/Screenshot%20with%20file_inspection_result2.png)

### Notes

Answer the following in your own words:

**1. Which part of this task represents the Gather phase?**

Add your answer here.

--- The read-only part server represents the Gather phase. Claude uses commands to collect information about Nginx, port 80, the HTTP response, disk usage

**2. Did Claude follow the instruction not to create files? How did you verify this?**

Add your answer here. 

--- Sure, Claude followed the instruction and performed read-only checks. I verified this by the listing of the files.


**3. Why is planning before coding useful in DevOps automation?**

Add your answer here.

---  planning before coding useful in DevOps automation cause it helps to envisage each output before writing the code by making the script know what to look out for.

# Task 4 — Build the Linux Triage Bash Script

## Goal

Create one Bash script that gathers consistent Linux and Nginx health evidence.

### Evidence

#### Screenshot 5 — Top section of `linux-triage.sh` showing variables, thresholds, and the checks array

Add your screenshot here.

--- [task 4](screenshots/Screenshot%20with%20linetriage%206.png)

#### Screenshot 6 — Middle section showing check functions and conditionals

Add your screenshot here.

--- [task 4](screenshots/Screenshot%20with%20if_conditions%201.png)

#### Screenshot 7 — Bottom section showing the loop, summary function, and exit behavior

Add your screenshot here.

---[task 4](screenshots/Screenshot%20with%20if_conditions%201.png)

#### Screenshot 8 — Output of `bash -n scripts/linux-triage.sh` (no syntax errors) and `ls -l scripts/linux-triage.sh` showing executable permission

Add your screenshot here.

--- [task 4](screenshots/Screenshot%20with%20bash%20no%20error.png)
    [task 4](screenshots/Screenshot%20with%20ls_linux_triage.png)
   
### Notes

Answer the following in your own words:

**1. What is stored in the checks array?**

Add your answer here.

--- The checks array stores the collection of 5 functions which check the HTTP, Nginx service, port 80, disk usage, and available memory.

**2. How does the `for` loop use that array?**

Add your answer here.

--- The for loop iterates over the array and reads each name from the array in the order its given.

**3. Why are the health checks separated into functions?**

Add your answer here.

--- the health checks separated into functions to perform targeted checks without overwhelming system resources or triggering false alarms this helps to remove system failure and helps with system recovery

**4. What is the purpose of `$(...)` in this script?**

Add your answer here.

--- $(...) runs a command and stores its output.

**5. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

Add your answer here.

---  cos these standardized codes allow automated tools to instantly categorize the severity of an issue and trigger appropriate alerts or actions without human review. HEALTHY means all checks is passed, WARN means the script found warnings and FAIL means at least one check failed.
 


# Task 5 — Run and Understand the Healthy-State Report

## Goal

Run the Bash script against the healthy server and verify that it creates a report.

### Evidence

#### Screenshot 9 — Output of `./scripts/linux-triage.sh` showing your Full Name and all five check results

Add your screenshot here.

--- ![task 5](screenshots/Screenshot%20with%20ls_linux_triage_output.png)

#### Screenshot 10 — Output showing the captured exit code and final summary

Add your screenshot here.

--- ![task 5](screenshots/Screenshot%20with%20executive%20summary.png)

### Notes

Answer the following in your own words:

**1. What is the overall status of your healthy baseline?**

Add your answer here.

--- The overall status of my healthy baseline being secured is HEALTHY hence the incident simulation cab be continued.


**2. Which exact Linux evidence proves the application is serving traffic?**

Add your answer here.

--- [PASS] Port 80 is listening. This shows  the application is serving traffic 

**3. Did your script return exit code 0 or 1? Explain why.**

Add your answer here.

--- The script I ran returned exit code 0. This occured cos the 5 health checks passed.i.e.
Nginx showed active, 
port 80 was listening, 
the application returned HTTP 200, 


**4. What is the difference between a warning and a failure in this script?**

Add your answer here.

--- a warning alerts me that there an unexpected issue such as excessive memory being used but thats not enough to stop the script hence the script continue running . 
A failure (or error) stops the operation because a critical task could not be completed, and returns a non-zero exit code to halt execution. This means the 5 health check didnt pass i.e. port 80 not listening, the application retuens internal error HTTP 500...

# Task 6 — Create and Run the /linux-triage Skill

## Goal

Turn the Bash script into a reusable, manually invoked Agentic AI workflow.

### Evidence

#### Screenshot 11 — `SKILL.md` showing the frontmatter, allowed tool restrictions, and safety rules

Add your screenshot here.

--- ![task 6](screenshots/Screenshot%20with%20skills_md%206.png)

#### Screenshot 12 — `/linux-triage` output for the healthy server

Add your screenshot here.

--- ![task 6](screenshots/Screenshot%20with%20skills_md_claude_output.png)
 
### Notes

Answer the following in your own words:

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

Add your answer here.

--- Read is needed to open the generated report, 
    Grep  is needed to locate the right output(PASS, WARN OR FAIL) .
    Bash is needed to run the commands and the script, 
    Write isnt needed because Claude should not edit project files during the triage process.


**2. Why is `disable-model-invocation: true` useful for this skill?**

Add your answer here.

---The setting `disable-model-invocation: true`  prevents Claude from executing the skill automatically so the process in under human control

**3. What part is performed by Bash, and what part is performed by Claude?**

Add your answer here.

---  Bash does the checks for example for the 5 health checks Nginx, port 80, the HTTP response, disk usage, available memory, is logged in recent logs.
     Claude does the reading of the report, analyzes the outputs, locates warnings, suggest the safe next move. 


**4. Why is this better than asking Claude "Is my server healthy?" without giving it evidence?**

Add your answer here.

--- A prompt not specific enough doesnt give claude adequate information to work with connecting the server.  
/linux-triage skill collects real time evidence using the Bash script while 
Claude reads, analyzes the result, locates warnings and recommends next move instead of guessing.


# Task 7 — Simulate an Nginx Incident and Let the Skill Diagnose It

## Goal

Create a controlled service failure, gather evidence through Bash, and let Claude analyze the evidence without taking recovery action.

### Evidence

#### Screenshot 13 — Output showing Nginx is inactive and the HTTP request fails

Add your screenshot here.

--- ![task 7](screenshots/Screenshot%20with%20nginx_inactive_http_fail.png)

#### Screenshot 14 — `/linux-triage` output showing failed evidence, most likely cause, and a suggested recovery command

Add your screenshot here.

--- ![task 7](screenshots/Screenshot%20with%20linux_failed_evidence.png)

#### Screenshot 15 — `incident-failure-report.txt` showing the failed checks and your Full Name

Add your screenshot here.

--- ![task 7](screenshots/Screenshot%20with%20incident_failure_report.png)

### Notes

Answer the following in your own words:

**1. Which three checks failed?**

Add your answer here.

--- The checks for Nginx, port 80, and local HTTP failed the health check. 

**2. What evidence supports the conclusion that Nginx is unavailable?**

Add your answer here.

--- Apparently The report revealed  Nginx not active, port 80 not listening. All these show that Nginx is unavailable.


**3. Did Claude execute the recovery command? Why is that important?**

Add your answer here.

---No, Claude only advised we run the recovery command. 

   This is important because It tends to hinders an AI tool from switching service effectively during an incident. Also the evidence must be reviewed and the action also approved before making a change to the server. 

**4. Which phase of the Agentic Loop is represented by the Bash report?**

Add your answer here.

--- the Gather phase where real-time evidence is collected

**5. Which phase is represented by Claude's explanation?**

Add your answer here.

--- the Analyze phase where  evidence is read, checks identified and a recovery commandnis advised for my take.


# Task 8 — Recover Manually, Verify Again, and Write the Incident Summary

## Goal

Recover the service as the human operator and prove that the system is healthy again.

### Evidence

#### Screenshot 16 — Output showing Nginx is active and `curl -I http://localhost` returns 200 OK

Add your screenshot here.

--- ![task 8](screenshots/Screenshot%20with%20nginx_active_again.png)

#### Screenshot 17 — Second `/linux-triage` output showing successful recovery with no FAIL results

Add your screenshot here.

--- ![task 8](screenshots/Screenshot%20with%20output_report_successful_6.png)

#### Screenshot 18 — Output of `ls -lah reports` showing both `incident-failure-report.txt` and `recovery-report.txt`

Add your screenshot here.

--- ![task 8](screenshots/Screenshot%20with%20reports%20of%20info%20and%20incident.png)

#### Screenshot 19 — `incident-summary.md` showing all required sections and your Full Name

Add your screenshot here.

---  ![task 8](screenshots/Screenshot%20with%20summary%20summary3_%206_8.png)

### Notes   

Answer the following in your own words:

**1. What action did you execute manually?**

Add your answer here.

--- sudo systemctl start nginx

**2. What evidence proves that the service recovered?**

Add your answer here.

--- The local HTTP request showed 200 OK.
The systemctl is-active nginx command showed active and the HTTP checks passed

**3. Why is the second triage run necessary?**

Add your answer here.

--- It is to ensure that the server gets to a healthy state.  Nginx working does not indicate that the complete application is ok. The second triage run checks the service, port, HTTP response, disk, and memory again

**4. What could go wrong if an AI agent automatically restarted every failed service?**

Add your answer here.

---Automatically restarting every service would hide the real issue cos a failed service could have resource problems, dependency issues, config probelems> Its good to create a restart loop or you would make the incident worse. 


**5. In one sentence, explain the difference between using AI as a chatbot and using AI in this agentic workflow.**

Add your answer here.

--- A chatbot only responds to my prompts, however in this agentic workflow, Claude gathers, analyze reports, identifies issues and even recommends next steps while I take the responsibility for approving and performing any action.


# Incident Summary

Fill in all seven sections below in your own words.

**Full Name:** Oluwafemi Aremu

**Date:** 17/07/2026

---

**1. Reported Symptom**

Add your answer here.

--- The React application was not opening. Also the HTTP request wasnt connecting to port

**2. Evidence Collected**

Add your answer here.

--- There were fail checks [FAIL] with Nginx inactive, Post 80 not listening and HTTP check returning status 000

The eventual logs showed that service deactivated 
The resouce checks passed with adequate disk deployment and sufficient memory available 

**3. Most Likely Cause**

Add your answer here.

--- The result showed a stoppage on Nginx service

**4. Human-Approved Recovery Action**

Add your answer here.

--- Claude suggests nginx with the command 'sudo systemact1 start nginx' but it did not run the command

**5. Verification**

Add your answer here.

--- Nginx started 'systemact1 is-active nginx' output showed active
    After running 'curl -I http://localhost' the application gave 'Http/1.1 200K'
    Went on claude and ran '/linux-triage' this time the recovery command run revealed HEALTHY with all 5 passed checks, no warnings, no failures



**6. Safety Decision**

Add your answer here.

--- Enabled the AI skill.md to run the bash script, read, analyze and descibe the outcomes

**7. Agentic Loop Mapping**

Add your answer here.

--- **Gather** :   The bash script collected outcomes on Nginx, port 80, HTTP response, disk usage.
                   No assumptions. No blind automation. Just evidence.
    **Analyze**:  Claude read the report, found 3 checks to have failed and descibed the outcome. I 
                  diagnosized issues. identify root causes then recommend recovery steps  
    **Human Act**: read Claudes reports, reviewd the analyses, and recommendation and also ran recovery commands
                   Hence I position myself for control, I decide the fix to make, I execute the fix (e.g., restarting Nginx). Am accountable for the system. as a Human I decide.
    **Verify** :   I attest of the evidence that I received HTTP 200 response from the application,   
                   also Nginx is active and disk usage was adequately managed.

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`__https://www.linkedin.com/feed/update/urn:li:activity:7483995724192395266/__`

---

#### Screenshot — Published LinkedIn post

Add your screenshot here.

--- [LinkedIn](screenshots/Screenshot%20with%20nginx%20linkedIn.png)

# GitHub Repository URL

Paste the URL of your GitHub folder or repository containing the assignment files here:

`__https://github.com/pravinmishraaws/devops-micro-internship-pravinmishra/tree/main/week-03-linux-and-bash-for-devops__`

---

# Submission Instructions

- Add all required screenshots in your submission
- Full Name must be visible in required screenshots and the Bash report
- All written answers must be in your own words
- Do not expose sensitive information (keys, passwords, AWS account IDs, tokens)
- GitHub URL must be included in this document

---

# Completion Checklist

- [✅ ] Task 1: Healthy baseline confirmed, workspace created (Screenshots 1–2, Notes answered)
- [✅ ] Task 2: CLAUDE.md created with all four sections (Screenshot 3, Notes answered)
- [✅ ] Task 3: Five-check plan produced by Claude using read-only tools (Screenshot 4, Notes answered)
- [✅ ] Task 4: `linux-triage.sh` created, syntax validated, executable permission set (Screenshots 5–8, Notes answered)
- [✅ ] Task 5: Healthy-state report generated with no FAIL result (Screenshots 9–10, Notes answered)
- [✅ ] Task 6: `/linux-triage` skill created and run successfully on healthy server (Screenshots 11–12, Notes answered)
- [✅ ] Task 7: Nginx incident simulated, failed evidence captured, Claude did not execute recovery (Screenshots 13–15, Notes answered)
- [✅ ] Task 8: Nginx recovered manually, recovery verified, reports saved, incident summary complete (Screenshots 16–19, Notes answered)
- [✅ ] Incident summary contains all seven required sections
- [✅ ] LinkedIn post published and URL submitted
- [✅ ] Full Name visible in all required screenshots and the Bash report
- [✅ ] Skill does not have Write permission
- [✅ ] Skill did not execute any recovery commands
- [✅ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://pravinmishra.com/dmi  
- 🎓 DevOps for Beginners (Udemy): https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/  
- 🎓 Agentic AI DevOps with Claude Code: https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/  
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*