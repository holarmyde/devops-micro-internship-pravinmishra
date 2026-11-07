# Assignment 5 — AI-Assisted Azure DevOps Dual-Pipeline Failure Triage

Part of the DevOps Micro Internship (DMI) — Agentic AI Track

---

## Student Information

**Full Name:** [OLUWAFEMI AREMU]

**GitHub Repository or Fork URL:** [Paste your repository URL]

**Public LinkedIn Post URL:** [Paste your LinkedIn post URL]

---

## Purpose

In this assignment, I configured an AI-assisted, read-only failure-triage workflow for the EpicBook Infrastructure and Application Pipelines. The workflow uses Bash to gather Azure DevOps pipeline evidence and Claude Code to analyze the evidence and recommend a recovery action while keeping all changes under human control.

---

# Task 0 — Verify Tools, Authentication, and Pipeline Details

## Goal

Verify the required tools, Azure DevOps authentication, organization and project details, and numeric pipeline IDs.

No screenshot is required for this task.

---

# Task 1 — Capture the Healthy Baseline and Prepare the Supplied Files

## Goal

Confirm that both EpicBook pipelines are healthy and place the supplied assignment files in the correct repository locations.

## Evidence

### Screenshot 1 — Healthy Baseline for Both Pipelines

Terminal output showing the latest completed Infrastructure and Application Pipeline runs with successful results.

![task 1](screenshots/Screenshot%20with%20App%20Pipeline%20run%20summary%20showing%20successful%20completion.png)

## Notes

### 1. What proves that both pipelines were healthy before the drill?

THe pipelines agents show they are online

### 2. Why is a healthy baseline necessary before introducing a controlled failure?

So you dont lose track of the requirements that are essential for a PASS

---

# Task 2 — Configure and Review the Supplied CLAUDE.md

## Goal

Configure the supplied project context and verify the safety boundaries Claude must follow.

## Evidence

### Screenshot 2 — CLAUDE.md Context and Safety Rules

`CLAUDE.md` open in the editor with the Project Overview, Incident Workflow, Safety Rules, and Output Rules visible.

![task 2](screenshots/Screenshot%20with%20CLAUDE.md%20showing%20the%20workflow%20and%20safety%20rules.png)

## Notes

### 1. Why does Claude need project-specific operational context?

Claude needs project-specific operational context to understand the cluster environment, Kubernetes context, namespace, Deployments, Services, and applicable safety restrictions. This helps it investigate the correct resources, provide relevant recommendations, and avoid unintended changes to unrelated workloads.

### 2. Which rules keep the human responsible for the recovery action?

The human-approval and no-autonomous-execution rules keep the operator responsible. Claude can gather evidence, analyze problems, and recommend recovery commands, but it must not execute changes independently. The human reviews, approves, and manually executes recovery actions before verifying the outcome.

### 3. Which rules protect pipeline credentials and application secrets?

The credential and secret protection rules prevent Claude from exposing, printing, copying, or modifying sensitive information such as API keys, passwords, access tokens, Kubernetes Secrets, and pipeline credentials. Sensitive values must remain private and should never be included in reports, logs, or shared outputs.

---

# Task 3 — Configure and Validate the Supplied Pipeline Triage Script

## Goal

Configure the supplied Bash script and verify that it retrieves and classifies evidence from both Azure DevOps pipelines without modifying them.

## Evidence

### Screenshot 3 — Pipeline Triage Script Configuration

Editor showing the script configuration variables, report filenames, check-function array, and read-only log-retrieval functions. Ensure that no token is visible.

![task 3](screenshots/Screenshot%20with%20pipeline-triage.sh%20showing%20the%20check%20functions.png)

---

### Screenshot 4 — Script Validation

Terminal showing successful Bash syntax validation and executable file permission.

![task 4](screenshots/Screenshot%20with%20Script%20output%20and%20report%20showing%20a%20healthy%20result.png)

## Notes

### 1. Why are pipeline metadata and step console logs handled separately?

Pipeline metadata and step console logs provide different types of information. Metadata shows the overall run status, workflow details, timestamps, and job or step results. Console logs provide detailed execution output, including errors, warnings, and failure messages. Handling them separately allows the script to combine both sources for a more accurate diagnosis.

### 2. How does the script obtain the actual console logs?

The script uses the pipeline platform's API or command-line interface to retrieve the logs associated with individual pipeline steps. It then processes the returned log content to identify relevant errors, warnings, and diagnostic messages rather than relying only on the overall pipeline status.

### 3. How does the check-function array control the classification loop?

The check-function array stores the diagnostic functions that the script must execute. The classification loop iterates through the array, runs each function, collects its result, and uses those results to determine the overall pipeline health classification.

### 4. What prevents a failed but unmatched run from being reported as healthy?

The script must explicitly check the overall pipeline result and use a safe default classification. A failed run must not be classified as healthy simply because no specific error pattern was matched. Unmatched failures should remain classified as failures or require further investigation.

### 5. Why are different exit codes useful to another automation tool?

Different exit codes allow another automation tool to understand the script's outcome without interpreting the entire report. For example, 0 can indicate HEALTHY, 1 can indicate WARN, and 2 can indicate FAIL. This allows automated workflows to make appropriate decisions, such as continuing, raising an alert, or stopping further execution.

---

# Task 4 — Run and Understand the Healthy-State Report

## Goal

Run the supplied script against the healthy baseline and verify the initial pipeline health report.

## Evidence

### Screenshot 5 — Healthy Pipeline Report

Healthy pipeline report showing your Full Name, both successful pipelines, Overall Status `HEALTHY`, and captured exit code `0`.

![task 5](screenshots/Screenshot%20with%20SKILL.md%20frontmatter%20showing%20the%20tool%20restrictions.png)

## Notes

### 1. What evidence proves that both pipelines are healthy?

Both pipelines are confirmed healthy when their latest runs complete successfully, all required steps finish without errors, and the diagnostic reports classify them as HEALTHY. Their metadata, step results, and console logs should contain no unresolved failures or critical warnings. Successful exit codes provide additional confirmation.

### 2. Why must the baseline exit code be verified before the incident drill?

The baseline exit code must be verified to establish that the pipeline is healthy before introducing a controlled failure. This provides a reliable comparison point, confirms that the monitoring script works correctly, and helps ensure that any failure detected during the incident drill is caused by the simulated incident rather than a pre-existing problem.

---

# Task 5 — Configure and Test the Supplied /pipeline-triage Skill

## Goal

Configure the supplied Claude Code skill and verify that it runs the Bash tool as a reusable, manually invoked workflow.

## Evidence

### Screenshot 6 — Pipeline-Triage Skill Definition

`SKILL.md` showing the frontmatter, manual-invocation setting, narrowly scoped tools, safety rules, and required output structure.

![task 5](screenshots/Screenshot%20with%20SKILL.md%20frontmatter%20showing%20the%20tool%20restrictions.png)

---

### Screenshot 7 — Healthy Skill Result

Healthy `/pipeline-triage` result showing that both pipelines are healthy and no fix is required.

![task 6](screenshots/Screenshot%20with%20The%20failed%20pipeline%20run%20showing%20the%20red_failed%20status.png)

## Notes

### 1. Why is `disable-model-invocation: true` appropriate for this skill?

It prevents Claude from automatically invoking the skill without the user's explicit request. This is appropriate because incident-triage operations may involve sensitive infrastructure information and should only run when intentionally requested by the human operator.

### 2. Why should the skill avoid broad Bash approval?

Broad Bash approval could allow commands to execute without proper review, potentially causing unintended changes to the cluster, pipelines, or application resources. Restricting approval to specific, safe commands helps reduce operational risks and maintains human oversight.

### 3. What work is performed by Bash, and what work is performed by Claude?

Bash performs the actual technical operations, such as running Kubernetes commands, retrieving pipeline information, collecting logs, executing diagnostic scripts, and generating reports. Claude interprets the collected evidence, identifies possible causes, explains findings, and recommends appropriate next steps. The human operator reviews and approves consequential actions.

### 4. Why are permission rules required in addition to written safety instructions?

Written instructions explain what Claude should and should not do, but permission rules enforce those boundaries at the tool level. They help prevent unauthorized command execution, restrict access to sensitive operations, and ensure that potentially destructive actions require human approval.

---

# Task 6 — Introduce a Safe Failure in the Application Pipeline

## Goal

Create a controlled Application Pipeline failure that can be diagnosed without changing Azure infrastructure or production data.

## Evidence

### Screenshot 8 — Controlled Application Pipeline Failure

Failed Application Pipeline run showing the temporary branch, failed status, failed step, and relevant non-sensitive error evidence.

![task 6](screenshots/Screenshot%20with%20The%20failed%20pipeline%20run%20showing%20the%20red_failed%20status.png)

## Notes

### 1. What exact failure did you introduce?

I deliberately changed the backend Deployment image from the working image book-review-backend:1.0 to the invalid image book-review-backend:does-not-exist. This caused the replacement Pod to enter ErrImagePull and ImagePullBackOff, preventing it from becoming ready.

### 2. Which category should detect it?

The Kubernetes deployment health / container image validation category should detect this failure. It should identify the image-pull errors and the replacement Pod's failure to become ready.

### 3. Why is the failure safe and easily reversible?

The failure was controlled and limited to the test environment and book-review namespace. It did not require deleting application data or making destructive changes. Recovery was straightforward: restore the known-good image, book-review-backend:1.0, and verify that the Deployment rollout completed successfully.

### 4. How did you prevent the deliberate failure from reaching `main` or changing the deployed application?

I kept the deliberate failure within the local Kubernetes test environment and did not commit or merge the invalid image change into the main branch. I also did not modify the production deployment. The failure was introduced only for testing, then manually reversed by restoring the known-good image and verifying the backend was healthy.

---

# Task 7 — Diagnose and Save the Incident Evidence

## Goal

Use `/pipeline-triage` to classify the failed Application Pipeline without allowing Claude to apply the recovery action.

## Evidence

### Screenshot 9 — Failed-State Diagnosis and Incident Report

`/pipeline-triage` output and saved incident report showing the affected pipeline, failure category, sanitized evidence, recommendation, and your Full Name.

![task 7](screenshots/Screenshot%20with%20The%20pipeline%20run%20succeeding%20after%20your%20fix.png)

## Notes

### 1. Which failure category was identified?

The Kubernetes deployment health / container image failure category was identified. The backend container could not pull the specified image because the image name or tag did not exist.

### 2. What exact evidence supported the diagnosis?

The evidence included:

The backend replacement Pod showed ErrImagePull and ImagePullBackOff.

The replacement Pod was not ready (0/1).

Kubernetes events reported image-pull failures.

The original backend Pod remained Running and Ready (1/1).

The EndpointSlice showed that the original Pod was ready and serving, while the replacement Pod was not ready to receive traffic.


Together, this evidence pointed to an invalid or unavailable container image.

### 3. Did Claude apply the fix or rerun the pipeline? Why is that important?

No. Claude analyzed the evidence and recommended a recovery action, but it did not independently apply the fix or rerun the pipeline. I reviewed and manually executed the recovery command. This is important because it keeps control and accountability with a human, prevents unauthorized changes, and ensures that potentially disruptive actions are reviewed before execution.

### 4. Which part represents Gather, and which part represents Analyze?

Gather: Collecting Kubernetes evidence using commands and diagnostic scripts, including Pod status, container states, events, logs, and EndpointSlice information.

Analyze: Claude reviewing the collected evidence, identifying the image-pull failure as the likely cause, explaining its impact, and recommending a safe recovery action.

---

# Task 8 — Apply the Human-Reviewed Fix and Verify Recovery

## Goal

Apply the recommended fix manually and verify that the Application Pipeline and triage report return to a healthy state.

## Evidence

### Screenshot 10 — Corrected Application Pipeline Run

Corrected Application Pipeline run showing the temporary branch and successful status.

![task 8](screenshots/Screenshot%20with%20The%20failed%20pipeline%20run%20showing%20the%20red_failed%20status.png)

---

### Screenshot 11 — Recovery Triage Result

Recovery `/pipeline-triage` output showing Overall Status `HEALTHY`, exit code `0`, your Full Name, and both saved report filenames.

![task 8](screenshots/Screenshot%20with%20The%20failed%20pipeline%20run%20showing%20the%20red_failed%20status.png)

## Notes

### 1. What exact fix did you apply?

I restored the backend Deployment to its known-good container image, book-review-backend:1.0, by running:

kubectl set image deployment/backend backend=book-review-backend:1.0 -n book-review

I then checked the rollout using:

kubectl rollout status deployment/backend -n book-review --timeout=180s

### 2. Did the fix match Claude’s recommendation? Explain briefly.

Yes. The recovery action addressed the identified cause by replacing the invalid image, book-review-backend:does-not-exist, with the previously working image, book-review-backend:1.0. This restored the backend without requiring unnecessary changes to the application.

### 3. What evidence proves that the pipeline recovered?

The recovery evidence showed that:

The Deployment rollout completed successfully.

The backend Pod returned to Running and Ready status (1/1).

The EndpointSlice reported ready: true and serving: true.

The failed replacement Pod was no longer present in the active Pod listing.

These results confirmed that the backend had recovered and was available to serve traffic.

### 4. Why is a second triage run required after the pipeline becomes green?

A second triage run verifies that recovery is complete and that no underlying failure or warning remains. It provides fresh evidence that the system is healthy, confirms that the original issue has been resolved, and helps prevent a false-positive recovery assessment.

### 5. What risk would be created if Claude could automatically edit, push, approve, and rerun the pipeline?

Giving Claude unrestricted permission to edit, push, approve, and rerun the pipeline could introduce unreviewed code changes, bypass required approvals, expose sensitive information, or deploy faulty changes. It could also cause service disruptions or make unauthorized production changes. Human review and approval are therefore essential safeguards.

---

# LinkedIn Post — Mandatory

## LinkedIn Post URL

`https://lnkd.in/p/e2Wut5QD`

## Evidence

### Screenshot 12 — Published LinkedIn Post

Published LinkedIn post showing its text and at least one image or link.

![task 8](screenshots/Screenshot%20with%20claude%20LinkedIn%20assig%2010.png)

---

# Required Repository Files

Confirm that the following files are available in your repository:

* [✅] `CLAUDE.md`
* [✅] `pipeline-triage.sh`
* [✅] `.claude/skills/pipeline-triage/SKILL.md`
* [✅] `reports/incident-failure-report.txt`
* [✅] `reports/recovery-report.txt`

---

# Submission Instructions

* Complete all tasks in sequence.
* Include all 12 required screenshots.
* Answer every Notes question in your own words.
* Include your GitHub repository or fork URL.
* Include your public LinkedIn post URL.
* Ensure your Full Name appears in the required reports.
* Do not include raw logs containing sensitive information.
* Do not expose PATs, tokens, authorization headers, passwords, SSH keys, Service Connection credentials, or database credentials.

---

# Completion Checklist

* [✅] Both Azure DevOps pipelines were healthy before the drill.
* [✅ ] The supplied files were copied to the correct repository locations.
* [✅ ] Only the required student-specific placeholders were updated.
* [✅ ] `CLAUDE.md` contains the required context and safety rules.
* [✅ ] `pipeline-triage.sh` passed Bash syntax validation.
* [✅ ] The script has executable permission.
* [✅ ] The script uses read-only Azure DevOps operations.
* [✅ ] The script retrieves pipeline metadata and console logs.
* [✅ ] No token or password is stored in the script.
* [✅ ] The healthy baseline reported `HEALTHY` with exit code `0`.
* [✅ ] `/pipeline-triage` was invoked manually.
* [✅ ] The skill does not have broad Bash approval.
* [✅ ] The controlled failure affected only the Application Pipeline.
* [✅ ] The failure occurred before deployment changes were applied.
* [✅ ] The deliberate failure was not merged into `main`.
* [✅ ] The failed-state report was saved before applying the fix.
* [✅ ] Claude diagnosed the failure but did not apply the fix.
* [✅ ] The fix was reviewed and applied manually.
* [✅ ] The corrected Application Pipeline completed successfully.
* [✅ ] The recovery triage reported `HEALTHY` with exit code `0`.
* [✅ ] `incident-failure-report.txt` exists.
* [✅ ] `recovery-report.txt` exists.
* [✅ ] All Notes questions have been answered.
* [✅ ] All 12 screenshots have been added.
* [✅ ] The GitHub repository or fork URL has been included.
* [✅ ] The LinkedIn post is public.
* [✅ ] The LinkedIn post URL has been included.
* [✅ ] The LinkedIn post URL has been included.
* [✅ ] No sensitive information is exposed.

---

# Final Submission

**Full Name:** [Enter your full name]

**GitHub Repository or Fork URL:** [Paste your repository URL]

**LinkedIn Post URL:** [Paste your public LinkedIn post URL]

---

*This submission is part of the DevOps Micro Internship (DMI) — Agentic AI Track.*
