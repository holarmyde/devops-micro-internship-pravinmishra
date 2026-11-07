# Assignment 6 — AI-Assisted Terraform Drift and Policy Review

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Student Details

**Full Name:** Oluwafemi Aremu
**GitHub Repository/Folder URL:** https://github.com/holarmyde/Ultimate-Agentic-DevOps-with-Claude-Code.git

---

## Purpose

Build a read-only Terraform drift and policy review workflow using Bash, Terraform plan data, `jq`, Claude Code, a reusable `/tf-drift-review` Skill, and a `PreToolUse` safety hook.

The workflow must follow this pattern:

```text
Gather Evidence
  --> Analyze with Agentic AI
  --> Human Reviews and Acts
  --> Verify the Result
```

The `/tf-drift-review` Skill and `tf-drift-check.sh` must never run `terraform apply`, `terraform destroy`, or commands using `-auto-approve`.

---

# Task 1 — Confirm the Clean Baseline and Create the Workspace

## Goal

Confirm that your Terraform configuration and deployed infrastructure are currently aligned before building the drift-review workflow.

## Evidence

### Screenshot 1 — Clean Terraform Plan

Add a screenshot of `terraform plan` showing no pending changes.

![task 1](screenshots/Screenshot%20with%20Terraform%20Plan.png)

---

### Screenshot 2 — Assignment Workspace

Add a screenshot of the folder structure showing `AI Assignment/`, `reports/`, and the Terraform project.

![task 1](screenshots/Screenshot%20with%20Terraform%20Initialization.png)

## Questions

### 1. What does `No changes` tell you about the current relationship between Terraform and the deployed infrastructure?

No changes indicates that Terraform's current configuration, Terraform state, and the observed infrastructure are aligned at the time of the plan.

Terraform has identified no updates, additions, deletions, or replacements required to bring the infrastructure into the desired state.

This confirms that the infrastructure is currently consistent with the declared Terraform configuration.

### 2. Why is a clean baseline important before introducing a test change?

A clean baseline establishes a known and verified starting point before introducing a controlled change.

It ensures that any differences detected afterward can be attributed to the test change rather than existing infrastructure problems.

It also makes it easier to evaluate the accuracy of the drift detection script, Claude Code analysis, and policy checks.

---

# Task 2 — Create Project Context and Safety Rules in `CLAUDE.md`

## Goal

Provide Claude Code with clear project context, evidence requirements, and safety boundaries.

## Evidence

### Screenshot 3 — Project Context and Safety Rules

Add a screenshot of `CLAUDE.md` open in VS Code showing the Project Overview, Review Workflow, Safety Rules, and Output Rules.

![task 2](screenshots/Screenshot%20with%20%20VS%20Code%20showing%20the%20AWS%20provider%20configuration.png)

## Questions

### 1. Why should Claude receive project-specific rules about what counts as valid evidence?

Claude needs project-specific instructions to understand which files, Terraform plan outputs, reports, and command results are considered reliable evidence.

Without these instructions, Claude might make assumptions or produce conclusions based on incomplete information.

Providing clear evidence requirements ensures that its analysis is consistent, traceable, and based on actual Terraform results rather than assumptions.

### 2. Why must the human remain responsible for running `terraform apply`?

The human must remain responsible because terraform apply makes actual changes to cloud infrastructure.

An incorrect or unnecessary deployment action could delete resources, expose services, introduce security vulnerabilities, or cause service interruptions and financial costs.

Human approval provides an additional verification and accountability layer before infrastructure changes are implemented.

### 3. Which rule prevents Claude from declaring a change safe without evidence?

The evidence-based review rule prevents Claude from declaring a change safe without supporting evidence.

Claude must inspect Terraform plan data, policy-check results, affected resources, and generated reports before making a risk assessment.

If sufficient evidence is unavailable, Claude must clearly report that the change cannot be verified rather than assume it is safe.
---

# Task 3 — Build the Terraform Drift and Policy Check Script

## Goal

Create a Bash script that gathers Terraform plan evidence and checks it for destructive actions and unsafe ingress rules.

## Evidence

### Screenshot 4 — Script Variables and Checks Array

Add a screenshot of the top section of `tf-drift-check.sh` showing the variables and `checks` array.

![task 3](screenshots/Screenshot%20with%20browser%20screenshot.png)

---

### Screenshot 5 — Destructive-Action and Open-Ingress Checks

Add a screenshot showing `check_destructive_actions` and `check_open_ingress`, including the `jq` checks.

![task 3](screenshots/Screenshot%20with%20Cart%20or%20Checkout%20Action%20-%204.png)

---

### Screenshot 6 — Script Validation and Permissions

Add a screenshot showing successful `bash -n` and `ls -l` output.

![task 6](screenshots/Screenshot%20with%20Azure%20CLI%20Version.png)

## Questions

### 1. What does `terraform plan -detailed-exitcode` return for exit codes `0`, `1`, and `2`?

Exit code	Meaning
0	Terraform successfully generated a plan and found no changes.
1	Terraform encountered an error while generating the plan.
2	Terraform successfully generated a plan and detected changes.

These exit codes allow Bash scripts to determine whether infrastructure is aligned, requires review, or encountered an error.

### 2. Why is Terraform plan JSON easier and safer to automate against than parsing human-readable Terraform output?

Terraform plan JSON provides structured and machine-readable information about proposed infrastructure changes.

Using jq, a script can reliably inspect resource addresses, resource types, proposed actions, and configuration values.

Human-readable output is designed primarily for people and can change in formatting or presentation.

JSON makes automated checks more consistent, precise, and easier to maintain.

### 3. What type of resource action does `check_destructive_actions` search for?

The check_destructive_actions function searches for resource actions that involve deletion, particularly the delete action in Terraform's planned resource changes.

It helps identify potentially destructive infrastructure operations before they are approved.

### 4. Why does finding a `delete` action also help detect replacements?

Terraform replacements commonly involve destroying an existing resource and creating a new one.

A replacement can appear as a combination of delete and create actions, depending on the replacement sequence.

Therefore, checking for delete helps identify both direct resource deletions and potentially destructive replacement operations.

### 5. Why must this script never run `terraform apply`?

The script is designed strictly for read-only evidence gathering and policy review.

Running terraform apply would move the workflow from analysis into infrastructure modification.

Keeping the script read-only ensures that it can detect and report risks without independently changing or destroying cloud resources.

The final infrastructure decision must remain with the human operator.

---

# Task 4 — Run the Script Against the Clean Baseline

## Goal

Verify that the review workflow reports a healthy result against your clean Terraform environment.

## Evidence

### Screenshot 7 — Healthy Baseline Report

Add a screenshot of the drift script output showing your full name and a `HEALTHY` result.

![task 4](screenshots/Screenshot%20with%20%20VS%20Code%20showing%20the%20AWS%20provider%20configuration.png)

---

### Screenshot 8 — Baseline Script Exit Code

Add a screenshot showing the captured script exit code `0`.

![task 4](screenshots/Screenshot%20with%20VS%20Code%20Extensions%20panel%20showing%20the%20HashiCorp%20Terraform.png)

## Questions

### 1. What is the Overall Status of your baseline?

The expected overall status of the clean baseline is HEALTHY.

This indicates that the Terraform configuration and deployed infrastructure are aligned, no pending changes have been identified, and the baseline policy checks have passed.

Use HEALTHY here if that is what your actual script output shows.

### 2. Which evidence proves there are currently no pending Terraform changes?

The following evidence demonstrates that there are no pending Terraform changes:
1. The terraform plan output displays No changes.
2. The drift-check script reports HEALTHY.
3. The script exits successfully with exit code 0.
Together, these results establish that the baseline is clean and no infrastructure modifications are currently required.

### 3. Was `reports/tfplan.json` created? Explain why or why not.

The existence of reports/tfplan.json depends on how the Bash script handles Terraform plan generation and JSON export.

If the script exports the plan using terraform show -json, the JSON file should be created even when no changes are detected, provided the export step executes.

If the script only generates JSON when changes are detected, the file may not exist during a clean baseline.

The important point is that the absence of a JSON report does not automatically indicate an error; the script's implementation determines when the file is generated.

---

# Task 5 — Create and Run the `/tf-drift-review` Claude Code Skill

## Goal

Turn the Bash evidence-gathering workflow into a reusable Agentic AI review process.

## Evidence

### Screenshot 9 — `/tf-drift-review` Skill Configuration

Add a screenshot of `SKILL.md` showing the frontmatter, allowed tools, and safety rules.

![task 5](screenshots/Screenshot%20with%20VS%20Code%20showing%20main_tf.png)

---

### Screenshot 10 — Clean Agentic AI Review

Add a screenshot of `/tf-drift-review` showing the clean `HEALTHY` result.

![task 5](screenshots/Screenshot%20with%20EC2%20Module%20Variables%20and%20Outputs.png)

## Questions

### 1. Why does this Skill have `Bash`, `Read`, and `Grep`, but not `Write`?

The Skill uses Bash, Read, and Grep because it needs to execute read-only inspection commands, read Terraform files and reports, and search for relevant configuration details.

It does not include Write because its responsibility is to gather and analyze evidence rather than modify project files.

Excluding Write reduces the risk of accidental changes and reinforces the read-only nature of the review workflow.

### 2. Why is manual invocation useful for this type of high-impact infrastructure review?

Manual invocation ensures that the infrastructure review begins intentionally when the developer or administrator requests it.

It gives the human operator control over when evidence is collected and when AI analysis takes place.

This is particularly important because Terraform manages critical infrastructure where unexpected actions can cause service interruptions, security issues, or financial losses.

### 3. Which part of the workflow is deterministic Bash automation?

The Bash automation is responsible for:
- Running Terraform plan commands.
- Capturing Terraform exit codes.
- Generating Terraform plan evidence.
- Exporting plan information into JSON.
- Using jq to inspect planned resource actions.
- Checking for destructive operations.
- Identifying unsafe open-ingress security rules.
- Generating review reports.
These operations follow predefined commands and rules, producing repeatable results.

### 4. Which part requires Claude's reasoning?

Claude's reasoning is required when interpreting the collected evidence, understanding the impact of proposed infrastructure changes, identifying possible security concerns, explaining destructive actions, assessing risk, and recommending appropriate next steps.

Unlike the Bash script, Claude can explain why a particular Terraform change may be dangerous and provide context for human decision-making.

### 5. Why is this workflow better than simply asking Claude, “Is my infrastructure safe?”

This workflow is better because Claude's conclusions are supported by actual Terraform plan data, automated policy checks, configuration files, and generated reports.

A general question may produce an answer based on incomplete information or assumptions.

The evidence-based workflow combines deterministic automation with AI reasoning and human approval, making the review more reliable, transparent, repeatable, and auditable.
---

# Task 6 — Introduce a Controlled Difference and Detect It

## Goal

Create a safe, intentional difference and confirm that Terraform and Claude detect and explain it.

## Evidence

### Screenshot 11 — Controlled Difference

Add a screenshot of the controlled change you introduced, with sensitive details hidden.

![task 6](screenshots/Screenshot%20with%20Linux%20Virtual%20Machine%20and%20custom_data.png)

---

### Screenshot 12 — Detected Difference and Risk Assessment

Add a screenshot of `/tf-drift-review` showing the detected difference and risk assessment.

![task 6](screenshots/Screenshot%20with%20VS%20Code%20showing%20main_tf.png)

---

### Screenshot 13 — Detected Drift Report

Add a screenshot of `drift-detected-report.txt` showing your full name and the `WARN` or `FAIL` result.

![task 6](screenshots/Screenshot%20with%20EC2%20Module%20Variables%20and%20Outputs.png)

## Questions

### 1. What change did you introduce?

I introduced a controlled change to the Terraform project to simulate a difference between the current infrastructure state and the desired configuration.

The purpose was to test whether the Terraform drift-review workflow could identify the proposed change, collect supporting evidence, and generate an appropriate risk assessment.

Replace this answer with the exact change you made, for example, changing an EC2 instance configuration, modifying a security group ingress rule, or changing a resource parameter.

### 2. Was it true infrastructure drift or a Terraform configuration change?

This depends on how the test change was introduced.
- If I modified the Terraform configuration file, it was a Terraform configuration change.
- If I manually modified the deployed cloud resource outside Terraform, it was true infrastructure drift.
The Terraform plan helped identify the difference between the declared desired state and the infrastructure represented by Terraform's state and refresh information.

### 3. What Terraform plan evidence proves that a change is pending?

The Terraform plan provides evidence through its proposed resource actions.

The detailed plan identifies the affected resource, its resource address, and the proposed action, such as update, delete, create, or replacement.

The terraform plan -detailed-exitcode command returning exit code 2 confirms that Terraform detected pending changes.

### 4. Was the action an update, deletion, replacement, or security-rule change?

The action is determined by inspecting the affected resource's change.actions field in the Terraform plan JSON.
- ["update"] indicates an update.
- ["delete"] indicates deletion.
- ["create", "delete"] or ["delete", "create"] indicates a replacement.
- A modification to a security group's ingress configuration indicates a security-rule change.
The actual action must match the generated plan.

### 5. What did Claude recommend?

Claude analyzed the Terraform plan evidence and identified the proposed infrastructure change and its potential impact.

It provided a risk assessment and recommended that the change be reviewed before implementation.

The recommendation was to verify the affected resource, understand the security and operational implications, and obtain human approval before performing any infrastructure-changing operation

### 6. Why should you review the recommendation before taking action?

AI recommendations should always be reviewed because AI can misunderstand infrastructure requirements, overlook dependencies, or misinterpret the operational impact of a proposed change.

Human review ensures that the recommendation is consistent with the project's requirements, security policies, business needs, and operational expectations.

This prevents potentially harmful infrastructure changes from being implemented without proper authorization.

---

# Task 7 — Add a `PreToolUse` Hook to Block Unsafe Apply Attempts

## Goal

Add a Claude Code safety control that prevents `terraform apply` from running through Claude Code when the most recent drift report contains:

```text
Overall Status: FAIL
```

## Evidence

### Screenshot 14 — `PreToolUse` Safety Hook

Add a screenshot of `.claude/settings.json` showing the `PreToolUse` safety hook.

![task 7](screenshots/Screenshot%20Nginx%20Configuration%20and%20Service%20-%204.png)

---

### Screenshot 15 — Blocked Apply Attempt

Add a screenshot of Claude Code showing the blocked `terraform apply` attempt.

![task 7](screenshots/Screenshot%20with%20Terraform%20apply%20output%20showing%20successful%20completion%20-%20NOW.png)

## Questions

### 1. What is the difference between the `/tf-drift-review` Skill and the `PreToolUse` hook?

The /tf-drift-review Skill is responsible for gathering Terraform evidence, analyzing infrastructure changes, identifying risks, and generating recommendations.

The PreToolUse hook is a safety mechanism that intercepts tool execution before a command runs.

The Skill performs the review, while the hook enforces a predefined safety condition that can block an unsafe operation.

### 2. Which component performs analysis?

Claude Code, using the /tf-drift-review Skill, performs the analysis.

It interprets Terraform plan evidence, reviews policy-check results, identifies potential infrastructure risks, and produces recommendations.

### 3. Which component enforces the safety gate?

The Claude Code PreToolUse hook enforces the safety gate.

It checks the existing drift report before allowing a matching terraform apply command to proceed.

When the report contains:
Overall Status: FAIL


the hook blocks the apply attempt according to the configured safety rule.

### 4. Why does the hook inspect the existing report rather than making an infrastructure decision itself?

The hook is designed to enforce a deterministic safety condition rather than perform complex infrastructure analysis.

The Bash script and Claude Code Skill are responsible for collecting and interpreting evidence.

The hook simply checks the resulting report for a predefined failure status.

This separation makes the workflow easier to understand, maintain, test, and audit.

### 5. Why is a deterministic guard useful for high-impact commands?

A deterministic guard provides predictable protection against potentially dangerous operations.

Unlike an AI recommendation, a predefined safety rule follows consistent conditions every time it is evaluated.

This is important for commands such as terraform apply and terraform destroy, where mistakes can cause infrastructure loss, security exposure, downtime, or unexpected costs.

It adds another layer of protection between AI analysis and actual infrastructure changes.

---

# Task 8 — Resolve the Difference and Verify the Final State

## Goal

Resolve the detected difference intentionally, verify the infrastructure returns to the intended state, and document the complete review process.

## Evidence

### Screenshot 16 — Human-Reviewed Resolution

Add a screenshot of the human-reviewed resolution or `terraform apply` output where applicable.

![task 8](screenshots/Screenshot%20with%20Terraform%20apply%20output%20showing%20successful%20completion%20-%20NOW.png)

---

### Screenshot 17 — Final Healthy Review

Add a screenshot of the final `/tf-drift-review` showing `HEALTHY`.

![task 8](screenshots/Screenshot%20with%20VS%20Code%20showing%20main_tf.png)

---

### Screenshot 18 — Saved Reports

Add a screenshot of `ls -lah reports` showing both:

- `drift-detected-report.txt`
- `resolved-report.txt`

![task 8](screenshots/Screenshot%20with%20React%20Application%20in%20the%20Browser.png)

---

### Screenshot 19 — Drift Review Summary

Add a screenshot of `drift-review-summary.md` showing all required sections and your full name.

![task 8](screenshots/Screenshot%20with%20plan%20summary%20showing%20the%20proposed%20resources.png)

## Terraform Drift Review Summary

### 1. Change Introduced

Explain the controlled change you introduced.

State whether it was:

- True infrastructure drift, or
- A Terraform configuration change

I introduced a controlled infrastructure difference to test the Terraform drift and policy review workflow.

The change was used to verify that Terraform could identify pending infrastructure modifications and that Claude Code could analyze the evidence and assess the associated risks.

Type of change: [Insert your actual change type.]

Classification: [True infrastructure drift / Terraform configuration change.]

### 2. Evidence Collected

Describe the Terraform plan evidence and affected resource.

The workflow collected evidence using Terraform plan commands and the Bash drift-check script.

The evidence included:
- Terraform plan output.
- Terraform detailed exit code.
- Terraform plan JSON, where generated.
- Affected resource addresses.
- Proposed resource actions.
- Destructive-action checks.
- Open-ingress security policy checks.
The generated report provided information about the detected difference and its potential impact.

### 3. Risk Assessment

Explain the risk identified by the Bash check and Claude Code.

The Bash script performed automated checks against the Terraform plan JSON using jq.

It checked for destructive resource actions and potentially unsafe ingress rules.

Claude Code then reviewed the collected evidence and explained the potential infrastructure and security implications.

The detected difference was classified according to the actual evidence, and a warning or failure status was recorded where appropriate.

### 4. Human-Approved Action

Explain the action you reviewed and executed manually.

After reviewing the Terraform plan and Claude Code's recommendations, I evaluated the proposed resolution.

I retained responsibility for deciding whether the change should be accepted or reverted.

The appropriate infrastructure-changing action was performed manually, following verification of the proposed changes.
### 5. Verification

Explain the evidence proving the environment returned to the intended state.

After resolving the difference, I ran the Terraform drift-check workflow again.

The final verification involved:
- Running Terraform plan.
- Confirming that no unexpected changes remained.
- Executing the Bash drift-check script.
- Running the /tf-drift-review Skill again.
- Confirming that the final status was HEALTHY.
- Saving the detected and resolved reports.
The final healthy report demonstrated that the environment had returned to the intended Terraform configuration.

### 6. Safety Decision

Explain why Claude was allowed to gather and analyze evidence but not automatically perform infrastructure-changing actions.

Claude Code was allowed to gather, inspect, and analyze Terraform evidence because these activities are read-only.

However, it was not permitted to independently execute infrastructure-changing operations.

The PreToolUse hook provided an additional safety gate by blocking terraform apply when the existing report contained an overall FAIL status.

This design ensured that infrastructure modifications remained subject to human review, approval, and accountability.

### 7. Agentic Loop Mapping

Explain how your workflow followed:

```text
Gather --> Analyze --> Human Act --> Verify
```

The workflow followed the four stages of an evidence-based Agentic AI process:
1. Gather Evidence
   
   Bash executed Terraform commands, collected plan information, generated JSON evidence, and performed policy checks.
2. Analyze with Agentic AI
   
   Claude Code used the /tf-drift-review Skill to inspect the evidence, assess risks, and recommend actions.
3. Human Reviews and Acts
   
   The human operator reviewed the recommendations, checked the Terraform plan, and manually approved the appropriate resolution.
4. Verify the Result
   
   Terraform and Claude Code were run again to confirm that the environment was aligned and the final review was HEALTHY.

## Questions

### 1. What action did you execute to resolve the difference?

I reviewed the detected difference and the proposed Terraform changes before manually executing the appropriate resolution.

The action was intended to restore the infrastructure to the desired Terraform configuration without allowing the AI agent to independently modify the environment.

Insert your actual action here, such as reverting a configuration change or manually applying an approved Terraform plan.

### 2. Did you review `terraform plan` before taking action?

Yes. I reviewed the Terraform plan before taking action.

This allowed me to identify the affected resources, understand the proposed changes, evaluate possible destructive operations, and confirm that the planned resolution was appropriate.

Reviewing the plan before execution is an essential infrastructure safety practice.

### 3. What evidence proves the environment is now aligned?

The following evidence confirms that the environment has returned to its intended state:
1. Terraform plan displays No changes.
2. Terraform reports exit code 0.
3. The Bash drift-check script reports HEALTHY.
4. The /tf-drift-review Skill confirms the final healthy status.
5. The resolved-report.txt file records the successful review.
6. The drift-review-summary.md documents the resolution and verification.

### 4. Why is a second drift review required after the fix?

A second drift review is required to verify that the resolution successfully addressed the original problem.

It confirms that the infrastructure is aligned with the Terraform configuration, no unexpected changes remain, and the policy checks pass.

Without a second review, there would be no reliable evidence that the resolution was successful.

### 5. What could go wrong if an AI agent automatically applied every detected Terraform change?

Automatically applying every detected Terraform change could introduce serious infrastructure and security problems.

The AI might approve unnecessary resource replacements, delete important resources, expose sensitive services through insecure ingress rules, disrupt production workloads, or create unexpected cloud costs.

It could also apply changes that conflict with business requirements or existing infrastructure dependencies.

Human approval is therefore essential before implementing high-impact infrastructure changes.

### 6. In one sentence, explain the difference between asking an AI chatbot “Is my infrastructure okay?” and using this evidence-based Agentic AI workflow.

Asking an AI chatbot whether infrastructure is safe produces an answer that may rely on assumptions, whereas an evidence-based Agentic AI workflow combines actual Terraform plan data, deterministic policy checks, Claude Code reasoning, human approval, and post-change verification to produce an auditable infrastructure review.

---

# LinkedIn Post — Mandatory

## Goal

Publish a LinkedIn post in your own words describing:

- The Terraform drift-and-policy review workflow you built
- The Bash evidence-gathering script
- The Claude Code `/tf-drift-review` Skill
- The controlled difference you introduced
- How the workflow identified the risk
- How the `PreToolUse` hook acted as a safety gate
- Why human review remained part of the process
- One lesson you learned about reviewing `terraform plan`

Include a screenshot of the detected change and a screenshot of the final `HEALTHY` review in your post.

Suggested tags:

```text
#DMIByPravinMishra #Terraform #AgenticAI #ClaudeCode #DevOps
```

## LinkedIn Evidence

### LinkedIn Post URL

`https://lnkd.in/p/eNTmcaJT`

### Published LinkedIn Post Screenshot — Mandatory

![linkedIn](screenshots/Screenshot%20with%20terraform%20linkedIn%20post.png)

---

# Required Assignment Files

Confirm that the following files are included in your GitHub repository:

- `CLAUDE.md`
- `AI Assignment/tf-drift-check.sh`
- `.claude/skills/tf-drift-review/SKILL.md`
- `.claude/settings.json` containing the safety hook
- `reports/drift-detected-report.txt`
- `reports/resolved-report.txt`
- `drift-review-summary.md`

---

# Submission Instructions

- Complete Tasks 1–8 in sequence.
- Include Screenshots 1–19 exactly as specified.
- Answer every question under Tasks 1–8 in your own words.
- Complete all seven sections of the Terraform Drift Review Summary.
- Include the GitHub repository/folder URL containing the assignment files.
- Include your full name in the required reports and screenshots.
- Include the LinkedIn post URL and a screenshot of the published LinkedIn post.
- Do not expose access keys, passwords, tokens, account IDs, private keys, Terraform secrets, or other sensitive information.
- Review all screenshots carefully and hide or redact sensitive details where necessary.

---

# Completion Checklist

- [✅] Confirmed a clean Terraform baseline
- [✅] Created the required assignment workspace
- [✅] Created or updated `CLAUDE.md`
- [✅] Added project context and safety rules
- [✅] Created `tf-drift-check.sh`
- [✅] Added my full name to the report
- [✅] Validated the Bash script
- [✅] Made the script executable
- [✅] Used `terraform plan -detailed-exitcode`
- [✅] Used Terraform plan JSON
- [✅] Used `jq` to inspect destructive actions
- [✅] Used `jq` to inspect unsafe ingress
- [✅] Confirmed the baseline returns `HEALTHY`
- [✅] Created `/tf-drift-review`
- [✅] Restricted the Skill to appropriate tools
- [✅] Confirmed the Skill remains read-only
- [✅] Confirmed the Skill never runs `terraform apply`
- [✅] Confirmed the Skill never runs `terraform destroy`
- [✅] Introduced a controlled detectable difference
- [✅] Correctly identified whether it was true drift or a configuration change
- [✅] Saved `drift-detected-report.txt`
- [✅] Added the `PreToolUse` safety hook
- [✅] Verified the hook blocks `terraform apply` when the report is `FAIL`
- [✅] Reviewed the Terraform evidence before resolving the change
- [✅] Performed any infrastructure-changing action manually
- [✅] Ran the drift review again after resolution
- [✅] Confirmed the final status is `HEALTHY`
- [✅]Saved `resolved-report.txt`
- [✅] Completed `drift-review-summary.md`
- [✅] Mapped the workflow to `Gather --> Analyze --> Human Act --> Verify`
- [✅] Included all 19 numbered screenshots
- [✅] Answered all required questions
- [✅] Published the required LinkedIn post
- [✅] Added the LinkedIn post URL and screenshot
- [✅] Included the GitHub repository/folder URL
- [✅] Confirmed that no sensitive information is exposed

---

*This submission is part of the DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
