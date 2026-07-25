# Assignment 6 — Building an AI-Assisted Git Safety Net (PR Ready Check)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In Week 2 you built Claude Code hooks that block a dangerous action *before* it happens (`PreToolUse`), and a restricted skill that could look but not touch (`allowed-tools` without `Write`). In this assignment you will discover that Git has the exact same idea, decades older: a **pre-commit hook** that blocks a commit before it's created.

You will build both halves of a real "PR Ready" workflow:

1. A **Git hook that follows fixed rules** — scans staged changes for hardcoded secrets and oversized files and refuses the commit. No AI involved, no guessing, just a rule that gives the same answer every time.
2. A **restricted Claude Code skill** (`/pr-ready`) that reads your staged diff and drafts a Pull Request title, description, and a short list of things worth a second look — the kind of judgment a fixed rule can't make (mixed changes, missing context, unclear intent). The skill never commits, pushes, or opens the PR. You do that yourself, using its draft as a starting point.

This mirrors the Agentic Loop from Week 3's Linux triage assignment: **Gather → Analyze → Human Act → Verify**. The hook and the skill both gather and analyze; only you act.

---

# Task 0 — Confirm Your Fork and Create a Feature Branch

## Goal

Confirm you are working in your own fork, then create a dedicated branch for this assignment.

### Evidence

#### Screenshot 1 — Output of git remote -v and git branch showing the new branch

![task 0](screenshots/Screenshot%20with%20Output%20of%20git%20remote%20-v%202.png)

---

### Notes

**1. Why create a dedicated branch instead of doing this work on main?**

creating a dedicated branch instead of doing this work on main is important for collaboration(Helping track what changes belong to which specific task), stability and safety Protecting the main code from bugs and broken builds

---

# Task 1 — Stage a Change With Realistic Risk

## Goal

On your own fork of this repository (the one you've been submitting your DMI work in since onboarding), create a new branch and stage a change that a real reviewer should catch: a hardcoded-looking secret and a leftover debug statement.

### Evidence

#### Screenshot 1 — Output of  `git status` showing the staged file on feature/ai-pr-ready

![task 1](screenshots/Screenshot%20with%20Output%20of%20%20git%20status%20showing%20the%20staged%20file%20on%20feature_ai-pr-ready.png)

---

### Notes

**1. Why does this assignment use an obviously fake key instead of a real one?**

This assignment use fake API keys to prevent security risks, unauthorized access, and accidental credential leaks if the code is published or shared publicly.

---

# Task 2 — Write a Real Git Pre-Commit Hook

## Goal

Create a tracked, shareable pre-commit hook that blocks a commit containing secret-like patterns or files over 1MB.

### Evidence

#### Screenshot 2 — `hooks/pre-commit` open in VS Code showing the full script

![task 2](screenshots/Screenshot%20_with%20_hooks_pre-commit%20open%20in%20VS%20Code%20showing%20the%20full%20script.png)

---

#### Screenshot 3 — Output of `git config core.hooksPath` confirming it points to `hooks`

![task 2](screenshots/Screenshot_with%20_git%20config%20core.hooksPath%20confirming%20it%20points%20to%20hooks.png)

---

### Notes

**1. Why is `hooks/pre-commit` tracked in the repo instead of living only in `.git/hooks/`?**

Tracking hooks/pre-commit  in the repository (inside  folder hooks/) instead of keeping it only in .git/hooks/ allows the team collaboratively to share, version control, and enforce the same code quality checks across all developers. Tracking it is beneficial cos of the version control i.e. Putting the script in a tracked folder lets you treat hooks like any other code. You can review changes, track history, and fix bugs in your validation scripts through regular pull requests.

---

**2. Compare this to `PreToolUse` from Week 2 Assignment 6. What does each one intercept, and what do they have in common?**

In Comparsion A git repository's hooks/pre-commit intercepts a local Git commit action, while PreToolUse  a hook from the AI agent framework Claude Code intercepts an AI tool call (such as a Bash or file-edit command) before it executes

---

# Task 3 — Prove the Hook Blocks the Risky Commit

## Goal

Attempt to commit the staged file from Task 1 and show the hook rejecting it.

### Evidence

#### Screenshot 4 — Terminal showing `git commit` rejected with the hook's "BLOCKED" message naming the exact file

![task 3](screenshots/Screenshot%20with%20git_commit_rejected%20with%20the%20hook's%20BLOCKED%20message%20name.png)

---

### Notes

**1. Which line in `hooks/pre-commit` matched your fake key, and why did it match?**

In line 7 it did match cos there was a comparsion of patterns of the regex string  AKIA[0-9A-Z]{16} matches AWS access key IDs

---

**2. Could this hook have caught a poorly-named variable that stores a secret without the `AKIA` prefix? What does that tell you about the limits of a fixed rule like this?**

they always start with AKIA followed by 16 uppercase letters/numbers

---

# Task 4 — Build the `/pr-ready` Skill

## Goal

Create a manually invoked Claude Code skill that reads your staged changes and produces a PR-readiness report and a draft PR description — without writing, committing, or pushing anything itself.

### Evidence

#### Screenshot 5 — `SKILL.md` frontmatter showing `allowed-tools: Bash, Read, Grep` (no `Write`) and `disable-model-invocation: true`

![taks 4](screenshots/Screenshot%20with%20frontmatter%20showing%20allowed_tools%20Bash%20Read%20Grep%20no%20Write.png)

---

#### Screenshot 6 — `/pr-ready` output while the risky file is still staged, showing it flagged the secret and/or debug statement

![task 4](screenshots/Screenshot%20with%20output%20while%20the%20risky%20file%20is%20still%20staged,%20showing%20it%20flagged%20the%20secret.png)

---

### Notes

**1. Why does `/pr-ready` have `Bash` and `Read` but not `Write`?**

/pr-readys sole purpose is strictly to read(inspect), test, and validate your repository state. Its doesn’t write code cos its purpose is not to modify code. This means The command /pr-ready has Bash and Read because it is an analysis and review step that should not create new files or modify source hence only needs to inspect files, run tests, and execute git commands, but it does not have Write 

---

**2. The pre-commit hook and `/pr-ready` both looked at the same staged diff. Did they flag the same things? What did one catch that the other didn't?**

 /pr-ready skill works differently. it inspects and reviews the code changes just the same way an human would review it. The   /pr-ready identified issues and explained why any thing was a problem. For instance the fact that The AWS key which is fake could still trigger scans automatically, although it was being used for testing.

 the pre-commit hook simply blocks the commit Instead after searching for fixed patterns and doesn’t find a match.


---

# Task 5 — Fix the Issues and Re-Verify

## Goal

Remove the secret and debug statement, then prove both gates now pass clean.

### Evidence

#### Screenshot 7 — `git commit` succeeding after the fix (no BLOCKED message)

![task 5](screenshots/Screenshot%20with%20git%20commit%20succeeding%20after%20the%20fix%20no%20BLOCKED%20message.png)

---

#### Screenshot 8 — Second `/pr-ready` run showing a clean risk report and a drafted PR title + description

![task 5](screenshots/Screenshot%20with%20pr-ready%20run%20showing%20a%20clean%20risk%20report%20and%20a%20drafted%20PR%20title_description.png)

---

### Notes

**1. What exactly did you change to satisfy the pre-commit hook?**

 To satisfy the pre-commit hook I had to remove the security issues in the notify.sh file that were previously detected:
the hardcoded AWS-style key
the debug echo statement that printed the key

Then ran the command git add scripts/notify.sh command that Staged the updated version of the file so Git will use the corrected version during the next commit.

After which I ran the git commit -m "add notification script" command that Created a new commit containing the corrected file. Before the commit is completed, Git automatically runs the pre-commit hook. Since the secret and debug statement have been removed, the hook finds no problems and allows the commit to proceed successfully.



---

# Task 6 — Push and Open a Pull Request Using the AI Draft

## Goal

Push your branch and open a real Pull Request, using `/pr-ready`'s drafted title and description as your starting point — read it critically and edit before you use it.

**Important:** Open this Pull Request with base repository set to **your own fork** — not the shared upstream `pravinmishraaws/devops-micro-internship-pravinmishra` repository. This assignment's hook and skill files are your own practice work, not a change meant for the shared class repo.

### Evidence

#### Screenshot 9 — Your Pull Request showing the base repository is your own fork, plus the title and description, with the `/pr-ready` draft visible for comparison (paste it in the PR conversation or your notes below)

![task 6](screenshots/Screenshot%20with%20Pull%20Request%20showing%20the%20base%20repository.png)

---

#### PR Link

![PR link](https://github.com/pravinmishraaws/devops-micro-internship-interviews/pull/410)

---

### Notes

**1. What, if anything, did you edit in the AI's drafted PR description before using it? Why?**

No I didnt edit cos it gave human recommendations such as:
being responsible for Reviewing the warnings, for Fixing the code, for Staging the updated file and Running the checks again.


---

**2. If you had blindly copy-pasted the AI's draft without reading it, what could go wrong?**

Blindly copy-pasting the AI draft without reading it can introduce broken formats from hidden text, messy tags or wrong placegolders getting published factual errors, and misconstrued tone that could destroy the credibility or violate policies.

---

**3. Why does this PR need to target your own fork instead of the shared upstream repository?**

because I dont have direct write access to push feature branches directly to the main project repository.

---

# Task 7 — Map the Workflow to the Agentic Loop

## Goal

Explain this assignment's workflow using the same Gather → Analyze → Human Act → Verify structure from Week 3.

### Notes

**1. Which step(s) represent Gather?**

	Gather – Collect information as pertaining the changes utilizing tools like git diff and git status.

---

**2. Which step(s) represent Analyze?**

	Analyze – Review the changes using both the rule-based pre-commit hook and also of the /pr-ready AI review 

---

**3. Which step is Human Act, and why must a human — not Claude — run `git commit`, `git push`, and open the PR?**

Human Action – I as the developer makes the decison on how to fix the identified issues. Only me as a human, with the judgment to read, verify, and correct an AI's draft — should be the one to actually execute these steps. Opening the PR against your own fork (not upstream) is also a deliberate boundary: this assignment's files are demonstration/practice artifacts, not a contribution intended for the shared original repository.


---

**4. Which step is Verify?**

  Verify – Run the checks again to confirm that the problems have been resolved before committing.

---

**5. In one or two sentences: why do you need *both* the fixed-rule pre-commit hook and the AI skill? Isn't one enough?**

I need both the fixed-rule pre-commit hook and the AI skill because fixed-rule pre-commit hooks instantly catch fast, deterministic errors like syntax and formatting taking actions like blocking commits, while AI skills can comprehend advance context to catch subtle logical flaws and design issues that static rules miss.

---

# Task 8 — LinkedIn Post

## Goal

Publish a LinkedIn post summarizing what you built and what you learned about combining fixed-rule safety checks with AI-assisted review.

### Evidence

#### LinkedIn Post URL

[linkedIn](https://www.linkedin.com/posts/oluwafemi-aremu-a85ab4197_devops-git-github-share-7486620863618949120-Wrk6/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAC5CRAUBiD9hmJ-VjKBPuiy5dtrdA6AJQDk)

---

## Key Learnings

Add 3-5 bullet points on what you learned this week.

- the pre-commit hook instantly catches and  blocks the commit due to varying patterns, mismatches or any form of security issues
- /pr-ready purpose to read(inspect), test, and validate your repository state.
- Tracking hooks/pre-commit  in the repository  instead of keeping it only in .git/hooks/ allows the team collaboratively to share, version control, and enforce the same code quality checks across all developers.

---

# Submission Instructions

- Ensure `hooks/pre-commit` and `.claude/skills/pr-ready/SKILL.md` are committed to your GitHub repository
- Add all required screenshots to your submission
- All written answers must be in your own words
- Do not use a real secret or credential anywhere in your submission — the fake key in Task 1 is intentional and must stay clearly fake
- Open your Pull Request against your own fork, not the shared upstream repository
- Push your final changes to your forked repository
- Include your PR link and LinkedIn post URL

---

## GitHub Repository URL

Paste your forked repository URL here:

`https://github.com/holarmyde/devops-micro-internship-interviews`

---

# Completion Checklist

- [✅ ] Branch `feature/ai-pr-ready` created with a staged file containing a fake secret and a debug statement
- [✅ ] `hooks/pre-commit` created and tracked in the repo (not only in `.git/hooks/`)
- [✅ ] `core.hooksPath` configured to point at `hooks/`
- [✅ ] Pre-commit hook shown blocking the risky commit
- [✅ ] `.claude/skills/pr-ready/SKILL.md` created with correct `allowed-tools` (no `Write`) and `disable-model-invocation: true`
- [✅ ] `/pr-ready` run against the risky diff and shown flagging issues
- [✅ ] Risky file fixed; `git commit` succeeds cleanly
- [✅ ] `/pr-ready` re-run showing a clean report and drafted PR title/description
- [✅ ] Pull Request opened using the AI draft as a starting point, with your own fork as the base repository (not upstream), PR link included
- [✅ ] Agentic Loop mapping (Task 7) completed in your own words
- [✅ ] LinkedIn post published and URL submitted
- [✅ ] All required screenshots added
- [✅ ] GitHub repository URL provided

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
