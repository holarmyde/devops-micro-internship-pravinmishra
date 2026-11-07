# Assignment 11 — AI-Assisted Kubernetes Incident Triage

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

Configure a read-only Bash triage script for your Book Review App, connect it to Claude Code through `/k8s-triage`, simulate one controlled failure, recover the application manually, and verify recovery.

**Workflow: Gather → Analyze → Human Act → Verify**

---

## Submission Details

**Full Name:** OLUWAFEMI AREMU

**Kubernetes Platform:** Docker-Desktop

**Selected Workload:** Add the Deployment name here.

**Namespace:**book-review.

**GitHub Repository URL:** https://github.com/holarmyde?tab=repositories.

**Simulated Incident:** broken readiness probe.

**Baseline Status:** HEALTHY.

**Recovery Status:** HEALTHY.

---

# Task 1 — Discover the Application and Capture a Healthy Baseline

## Goal

Identify the actual application configuration and prove that the selected workload is healthy before incident simulation.

### Evidence

#### Screenshot 1 — Healthy Pods and ready Service backends

![task 1](screenshots/Screenshot%20with%20Healthy%20Pods%20and%20ready%20Service%20backends.png)

---

#### Screenshot 2 — Workspace directory structure

![task 1](screenshots/Screenshot%20with%20Workspace%20directory%20structure.png)

### Notes

**1. What proves the selected Pods are ready, not merely running?**

The Kubernetes READY column shows 1/1, meaning the container has passed its readiness check. Running alone only indicates that the container is executing; it does not guarantee that it can receive traffic.

**2. What proves the Service has ready backends?**

The EndpointSlice resource identifies the backend Pod IP addresses registered with the Service. Its conditions.ready: true field confirms that the endpoint is ready to receive traffic.

**3. Why must the original image be recorded before incident simulation?**

Recording the original working image provides a reliable recovery reference. If an invalid image is introduced during the incident simulation, the original image can be restored manually to return the application to its previous working state.

---

# Task 2 — Configure CLAUDE.md

## Goal

Define the project configuration, incident workflow, and safety boundary for Claude Code.

### Evidence

#### Screenshot 3 — CLAUDE.md showing application configuration, workflow, safety rules, and output rules

![task 2](screenshots/Screenshot%20with%20CLAUDE.md%20showing%20application%20configuration.png)

### Notes

**1. Why does Claude need project-specific cluster rules?**

Claude needs project-specific cluster rules so it understands the correct Kubernetes context, namespace, Deployment, Pod selector, and safety restrictions. These rules prevent it from investigating the wrong environment or recommending commands that could affect unrelated resources.

**2. Why must a human execute recovery commands?**

A human must execute recovery commands to maintain control, accountability, and safety. Recovery actions can affect service availability or application stability, so a human should review the evidence, approve the recommendation, and decide when to make changes.

**3. Which rule prevents an unsupported diagnosis?**

The evidence-based diagnosis rule prevents unsupported conclusions. Claude must base its diagnosis on actual Kubernetes evidence, such as Pod status, container events, logs, and EndpointSlice information, rather than guessing or making assumptions.

---

# Task 3 — Plan the Read-Only Triage Workflow

## Goal

Ask Claude Code to explain the five health checks before running the supplied script.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check read-only plan

![task 3](screenshots/Screenshot%20with%20Claude%20Code%20showing%20the%20five-check%20read-only%20plan.png)

### Notes

**1. Which part represents the Gather phase?**

The Gather phase involved collecting Kubernetes evidence before making any changes. This included checking Pod status and readiness, container restart counts, Warning events, and Service EndpointSlice information using kubectl commands and the triage script.

**2. How did you verify that Claude did not change anything?**

I verified that Claude did not change anything by reviewing the commands it suggested and manually executing the recovery command myself. I also checked the Kubernetes Deployment and Pod status to confirm that the configuration changed only when I authorized and executed the recovery action.
**3. Why is planning before coding useful for incident triage?**

Planning before coding helps define the investigation steps, required evidence, safety checks, and recovery procedures before taking action. It reduces mistakes, prevents unnecessary changes, improves consistency, and ensures that AI recommendations remain within human-approved boundaries.

---

# Task 4 — Configure and Validate the Bash Script

## Goal

Configure the supplied triage script for the selected workload and validate its syntax.

### Evidence

#### Screenshot 5 — Script configuration variables and checks array

![task 4](screenshots/Screenshot%20with%20Script%20configuration%20variables%20and%20checks%20array.png)

---

#### Screenshot 6 — Functions for status, readiness, restarts, events, and Service backends

![task 4](screenshots/Screenshot%20with%20Functions%20for%20status%20readiness%20restarts.png)

---

#### Screenshot 7 — Preflight, summary, and exit-code logic

![task 4](screenshots/Screenshot%20with%20Preflight,%20summary,%20and%20exit-code%20logic.png)

---

#### Screenshot 8 — Successful syntax validation and executable permission

![task 4](screenshots/Screenshot%20with%20Successful%20syntax%20validation%20and%20executable%20permission.png)

### Notes

**1. Why must the script fail when no Pods match the configured selector?**

If no Pods match, the script has no valid workload evidence to analyze. Failing prevents it from reporting a false healthy status.

**2. How does the readiness check detect a running but unready Pod?**

It checks the Pod's Ready condition separately from its Running phase. A Pod can be Running while its Ready condition is False.

**3. Why are Service backends checked separately from Pod health?**

A healthy Pod does not automatically prove the Service can route traffic to it. Checking EndpointSlices confirms whether the Service has ready and serving endpoints.

**4. What do exit codes 0, 1, and 2 represent?**

0: No failed checks; healthy result.



1: One or more health checks failed.



2: Script could not complete a required check or encountered a critical execution/configuration error.

---

# Task 5 — Run the Healthy Baseline Report

## Goal

Run the script on the healthy application and confirm that it contains no failed checks.

### Evidence

#### Screenshot 9 — Baseline report showing your full name and all checks

![task 5](screenshots/Screenshot%20with%20Baseline%20report%20showing%20your%20full%20name.png)

---

#### Screenshot 10 — Captured exit code and final summary

![task 5](screenshots/Screenshot%20with%20Captured%20exit%20code%20and%20final%20summar.png)

### Notes

**1. What is the baseline status?**

The backend Deployment was 1/1 available, and its Pod was Running and Ready. The Pod had 3 previous restarts, so that historical condition should be recorded as a warning if the script reports it. The final baseline classification must come from the actual script report.

**2. Which evidence proves the Service has a ready backend?**

The backend EndpointSlice showed an endpoint with ready: true and serving: true.

**3. What is the difference between a warning and a failure?**

A warning identifies something worth monitoring that does not necessarily mean the workload is currently unhealthy. A failure means required health evidence shows a current problem or a required check could not be completed.
---

# Task 6 — Configure and Run the Claude Code Skill

## Goal

Connect the Bash script to a manually invoked Claude Code evidence-analysis workflow.

### Evidence

#### Screenshot 11 — SKILL.md showing frontmatter and safety rules

![task 6](screenshots/Screenshot%20with%20SKILL.md%20showing%20frontmatter%20and%20safety%20rules.png)

---

#### Screenshot 12 — Baseline `/k8s-triage` analysis

![task 6](screenshots/Screenshot%20with%20Baseline%20`k8s-triage`%20analysis.png)

### Notes

**1. What does Bash do in this workflow?**

Bash runs the read-only Kubernetes commands, collects the health evidence, evaluates the checks, and saves a report.

**2. What does Claude do in this workflow?**

Claude analyzes the collected report, explains the evidence, identifies likely causes when supported, and recommends recovery steps for a human to review.

**3. Why are instructions alone insufficient to guarantee read-only access?**

Instructions guide Claude's behavior, but they do not technically prevent commands from changing resources. Commands and tool requests must still be inspected, and permissions should be restricted where possible.

---

# Task 7 — Simulate One Controlled Incident

## Goal

Create one controlled failure, save its evidence, and let Claude analyze it without executing recovery.

### Evidence

#### Screenshot 13 — Failed or unready Pod evidence and current Service backends

![task 7](screenshots/Screenshot%20with%20Failed%20or%20unready%20Pod%20evidence%20and%20current%20Service%20backends.png)

---

#### Screenshot 14 — Failed `/k8s-triage` analysis with evidence and a suggested manual recovery command

![task 7](screenshots/Screenshot%20with%20Failed%20`k8s-triage`%20analysis%20with%20evidence.png)

---

#### Screenshot 15 — Saved `incident-failure-report.txt`

![task 7](screenshots/Screenshot%20with%20Saved%20`incident-failure-report.txt`.png)

### Notes

**1. Which failure did you choose?**

I simulated an image-pull failure by changing the backend Deployment to use a deliberately invalid container image.

**2. Which checks failed, and what evidence proved it?**

The Pod status and readiness checks should identify the unavailable or unready backend Pod. The restart and event checks may provide additional evidence, such as image-pull errors. Record the actual report results.

**3. Did Claude execute recovery? Why is that important?**

No. Claude only analyzed the evidence and recommended a recovery command. This preserves human approval and control over changes to the Kubernetes cluster.

**4. Why might the Service retain ready backends during the incident?**

During a rolling update, the old healthy Pod may remain available while Kubernetes attempts to start the replacement Pod.

---

# Task 8 — Recover Manually and Verify

## Goal

Restore the healthy configuration yourself and verify recovery using a second triage report.

### Evidence

#### Screenshot 16 — Recovered Pods and ready Service backends

![task 8](screenshots/Screenshot%20with%20Recovered%20Pods%20and%20ready%20Service%20backends.png)

---

#### Screenshot 17 — Recovered `/k8s-triage` analysis

![task 8](screenshots/Screenshot%20with%20Recovered%20`k8s-triage`%20analysis.png)

---

#### Screenshot 18 — Reports directory showing failure and recovery reports

![task 8](screenshots/Screenshot%20with%20Reports%20directory%20showing%20failure.png)

---

#### Screenshot 19 — `incident-summary.md` with all required sections

![yask 8](screenshots/Screenshot%20with%20`incident-summary.md`%20with%20all%20required%20sections.png)

### Notes

**1. What manual recovery action did you execute?**

I manually restored the backend Deployment to its original working image, book-review-backend:1.0.

**2. What proves the workload recovered?**

The backend Pod returned to Running and Ready, the Deployment became available, and the backend Service EndpointSlice showed a ready and serving endpoint.

**3. Why is the second triage run necessary?**

It independently collects fresh evidence after recovery and confirms the workload's current condition rather than relying on assumptions.

**4. Why should AI not automatically restart every failing production workload?**

Restarting without understanding the cause could interrupt healthy traffic, destroy useful diagnostic evidence, or make the incident worse. Human review is essential.

### Remaining WARN Results

Explain any WARN results remaining after recovery, or write “None.”

WARN results remaining after recovery: None.

The recovery report confirms that the backend Deployment successfully rolled out, the Pod was Running and Ready (1/1), and the Service EndpointSlice showed ready: true and serving: true. Any Warning events from the earlier failed-image simulation were historical and did not indicate an active failure after recovery.

---

# Task 9 — Publish Your Kubernetes Incident-Triage Workflow on LinkedIn

## Goal

Share the incident-triage workflow you completed and explain the role of human control.

### Evidence

#### Screenshot 20 — Published LinkedIn post about your Kubernetes incident-triage workflow

![task 9](screenshots/Screenshot%20with%20kubernates%20linkedIn%20post_FInal.png)

### LinkedIn Post URL

`https://lnkd.in/p/eS_bhVy4`

---

# Required Project Files

Include these files in your project repository:

- `CLAUDE.md`
- `lab-config.md`
- `scripts/k8s-triage.sh`
- `.claude/skills/k8s-triage/SKILL.md`
- `reports/incident-failure-report.txt`
- `reports/recovery-report.txt`
- `incident-summary.md`

Your `incident-summary.md` must contain:

1. Reported Symptom
2. Evidence Collected
3. Most Likely Cause
4. Human-Approved Recovery Action
5. Verification
6. Safety Decision
7. Agentic Loop Mapping

---

# Final Summary

## Evidence and Cause

Summarize the evidence and the supported cause of the incident.

The incident was caused by a deliberately configured invalid container image, book-review-backend:does-not-exist, in the backend Deployment. Kubernetes events reported ErrImagePull and ImagePullBackOff. The replacement Pod was not ready (0/1), while the original backend Pod remained Running and Ready (1/1). The EndpointSlice confirmed that the original Pod was still ready and serving traffic, but the failed replacement was not. This evidence supports an invalid or unavailable container image as the root cause.
## Manual Recovery

Describe the command or configuration change you performed.

After reviewing the incident evidence and Claude's recommendation, I manually restored the known-good backend image using:

kubectl set image deployment/backend backend=book-review-backend:1.0 -n book-review

I then monitored the Deployment rollout using:

kubectl rollout status deployment/backend -n book-review --timeout=180s

Kubernetes confirmed that the Deployment successfully rolled out.

## Verification

Explain how the recovery report and Kubernetes outputs confirmed recovery.

I generated and saved a post-recovery health report and checked the Kubernetes outputs. The rollout completed successfully, the backend Pod returned to Running with readiness 1/1, and the failed replacement Pod was no longer present in the active Pod listing. The EndpointSlice showed ready: true, serving: true, and terminating: false, confirming that the backend Service had a healthy endpoint. These checks demonstrated that the recovery was successful.

## Safety Decision

Explain how you kept Claude’s role limited to evidence gathering and analysis.

I kept Claude's role limited to gathering and interpreting Kubernetes evidence, identifying the likely root cause, and recommending a recovery action. I did not authorize Claude to make changes directly to the cluster. As the human operator, I reviewed the findings, assessed the recommendation, and manually executed the recovery command. I then independently verified the result. This human-in-the-loop approach ensured that infrastructure changes remained under human supervision and accountability.

---

# Submission Instructions

- Include all 20 required screenshots under their corresponding Evidence sections.
- Complete all Notes questions in your own words.
- Include the required project files in your linked repository.
- Include your published LinkedIn post URL.
- Use clear, readable screenshots showing relevant commands, configuration, and outputs.
- Display your full name before taking required terminal screenshots.
- Preserve the failure report before recovery.
- Ensure the baseline and recovery reports contain no FAIL results.
- Explain any WARN results remaining after recovery.
- Review reports, logs, configuration records, and screenshots before publishing.
- Do not expose credentials, kubeconfig contents, tokens, certificates, JWT secrets, or confidential account information.

---

# Completion Checklist

- [✅ ] Confirmed the personal lab context
- [✅ ] Discovered and recorded application configuration
- [✅ ] Recorded the original working image
- [✅ ] Captured healthy Pods and ready Service backends (Screenshot 1)
- [✅ ] Created the workspace (Screenshot 2)
- [✅ ] Configured CLAUDE.md (Screenshot 3)
- [✅] Reviewed the five-check triage plan (Screenshot 4)
- [✅] Configured and reviewed the supplied script (Screenshots 5–7)
- [✅ ] Passed Bash syntax validation and set executable permission (Screenshot 8)
- [✅ ] Captured a baseline report with no FAIL (Screenshot 9)
- [✅ ] Captured the correct exit code (Screenshot 10)
- [✅ ] Configured and invoked `/k8s-triage` (Screenshots 11–12)
- [✅ ] Simulated one controlled incident manually (Screenshot 13)
- [✅ ] Reviewed Claude’s failed-state analysis (Screenshot 14)
- [✅] Saved failure evidence before recovery (Screenshot 15)
- [✅] Executed recovery manually and waited for rollout completion
- [✅] Confirmed recovered Pods and ready Service backends (Screenshot 16)
- [✅] Captured recovery analysis with no FAIL (Screenshot 17)
- [✅] Explained any remaining warnings
- [✅] Saved failure and recovery reports (Screenshot 18)
- [✅] Completed the seven-section incident summary (Screenshot 19)
- [✅] Answered all Notes questions
- [✅] Published the LinkedIn post (Screenshot 20)
- [✅] Included the LinkedIn post URL
- [✅] Included all required project files
- [✅] Checked that no sensitive information is exposed

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
