# Assignment 4 — Gotto Job: Backlog Refinement & Sprint 1 in Jira

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this 90-minute, time-boxed exercise, you will act as a Scrum team — or run in Solo Mode, playing every role yourself — to turn the Gotto Job template into a value-ordered backlog, estimate the work in story points, plan Sprint 1, open the burndown chart, and ship one small UI-only increment (text, color, spacing, a label, or a CTA — no backend changes).

---

# Task 1 — Roles & Mode Setup (Team vs Solo)

## Goal

Choose Team Mode or Solo Mode, and document how each Scrum role (Product Owner, Scrum Master, Dev Lead, DevOps Lead) was handled.

### Evidence

#### Screenshot 1 — Jira "Create project" screen, or the project sidebar after creation

![task 1](screenshots/Screenshot%20with%20number%201%20wk%205.png)
![task 1](screenshots/Screenshot%20with%20number%201%20wk%205_2.png)

---

### Notes

Write one line for each role: PO (what you prioritized), SM (how you ensured process), Dev Lead (what you built), DevOps Lead (how you shipped).

PO: Prioritized the high-value user authentication flow and payment gateway integration for the core checkout release.
SM: Facilitated daily standups and removed cross-team blockers to maintain team velocity and agile flow.
Dev Lead: Built the secure OAuth2 login microservice and optimized backend API endpoints for fast data retrieval.
DevOps Lead: Shipped the automated CI/CD pipeline using blue-green deployments on Kubernetes for zero-downtime releases.

---

# Task 2 — Create the Jira Project (Team-managed → Scrum)

## Goal

Create a Team-managed Scrum project named `Gotto Job – Team <#>` (Team Mode) or `Gotto Job – <YourName>` (Solo Mode).

### Evidence

#### Screenshot 2 — Project created page showing the project name and key

![task 2](screenshots/Screenshot%20with%20Jira%20Create%20project%20screen%20or%20the%20project%20sidebar%20after%20creation.png)

---

# Task 3 — Create the Epic

## Goal

Create the Epic `Improve Gotto Job UI discoverability & trust` to group the UI improvement initiative.

### Evidence

#### Screenshot 3 — Backlog showing the Epic panel with the Epic visible

![task 3](screenshots/Screenshot%20with%20Backlog%20showing%20the%20Epic%20panel%20with%20the%20Epic%20visible.png)

---

# Task 4 — Seed the Product Backlog (6–8 Stories + Fibonacci Points + Ranking)

## Goal

Create at least six Stories under the Epic, estimate each with 1, 2, or 3 story points, and rank them by value.

### Evidence

#### Screenshot 4 — Backlog showing the Epic and at least six Stories under it

![task 4](screenshots/Screenshot%20with%20Backlog%20showing%20the%20Epic%20and%20at%20least%20six%20Stories%20under%20it.png)

---

#### Screenshot 5 — One Story opened showing its Story Points and acceptance criteria filled in

![task 4](screenshots/Screenshot%20with%20One%20Story%20opened%20showing%20its%20Story%20Points%20and%20acceptance%20.png)

---

# Task 5 — Planning Poker (Estimate + Debate Notes)

## Goal

Confirm the Story Points (1, 2, or 3) for each Story and record brief reasoning for each estimate.

### Evidence

#### Screenshot 6 — Backlog showing Story Points visible, or two or three Stories opened showing their points

![task 5](screenshots/Screenshot%20with%20Backlog%20showin%20Story%20Points%20visible,%202or%203%20Stories%20opened%20.png)

---

### Notes

For each story, explain in one or two lines why it is a 1, 2, or 3 (mention any debate, even in Solo Mode).

Story 1 – Hero tagline (1 point): This is a small task no priroty only requires changing heading which is usally a line,
Story 2 – Button colour (1 point): Only the line of the the button colour in the CSS stylesheet need be corrected/ updated hence 1 point is ok.
Story 3 – Job card typography (2 points): Here we change the font size and weight, and ensuring that the layout fits on several screen sizes.
Story 4 – REMOTE badge (2 points): The goal is to add a new badge and display it only for remote jobs, making it slightly more complex than a simple text change.
Story 5 – Posted on date (1 point): This is bascially going to be dating date numbers which is integers.
Story 6 – Search labels (2 points):  labels would be updated and tested same also for placeholders. so it requires more work than a single text change.
Story 7 – Job Detail Apply Now CTA (1 Point): It is a basic change, so it is estimated as 1 point.The goal is to add a single "Apply Now" button that links to an email address having a placeholder link. 
Story 8 – Footer Trust Links (1 Point): The udpate done here line of HTML , so it is estimated as 1 point. .The goal is to add two footer links ("About" and "Contact"). T


---

# Task 6 — Sprint Planning: Create Sprint 1 + Sprint Goal + Scope

## Goal

Create Sprint 1, move three or four Stories into it (approximately 3–6 points), set the Sprint Goal, and break each selected Story into Build, Verify, Deploy, and Screenshot Sub-tasks.

### Evidence

#### Screenshot 7 — Sprint 1 with the selected Stories inside it

![task 6](screenshots/Screenshot%20Sprint%201%20created%20with%20the%20Story%20inside%20it.png)

---

#### Screenshot 8 — One Story showing the Sub-tasks created

![task 6](screenshots/Screenshot%20with%20One%20Story%20showing%20the%20Sub-tasks%20created.png)

---

# Task 7 — Reports: Open Burndown Chart

## Goal

Open the Burndown Chart and confirm it exists for Sprint 1. It is acceptable if the chart is not yet populated.

### Evidence

#### Screenshot 9 — Burndown Chart page opened, even if empty

![task 7](screenshots/Screenshot%20with%20Burndown%20Chart%20page%20opened,%20even%20if%20empty.png)

---

# Task 8 — Ship One Small Increment (Build + Deploy + Proof)

## Goal

Implement one small UI-only Story from Sprint 1, commit it, deploy it live, and move the Story and its Sub-tasks to Done in Jira.

### Evidence

#### Screenshot 10 — Jira board showing the Story moved to Done

![yask 8](screenshots/Screenshot%20with%20Jira%20board%20showing%20the%20Story%20moved%20to%20Done.png)

---

#### Screenshot 11 — Git commit output

![task 8](screenshots/Screenshot%20with%20Git%20commit%20output.png)

---

#### Screenshot 12 — Live URL in the browser showing the UI change, with the URL visible

![task 8](screenshots/Screenshot%20with%20Live%20URL%20in%20the%20browser%20showing%20the%20UI%20change.png)
![task 8](screenshots/Screenshot%20with%20Live%20URL%20in%20the%20browser%20showing%20the%20UI%20change_2.png)

---

# Task 9 — Retro Notes (Scrum Pillar + Value)

## Goal

Add a retro comment covering what went well, what to improve, one Scrum pillar observed (Transparency, Inspection, or Adaptation), and one Scrum value (Openness, Focus, Commitment, Courage, or Respect).

### Evidence

#### Screenshot 13 — Jira retro comment visible

![task 9](screenshots/Screenshot%20with%20Jira%20retro%20comment%20visible.png)

---

# Task 10 — LinkedIn Post (Mandatory)

## Goal

Publish a LinkedIn post about what you delivered, including your live URL, three to five lines on what you did and learned, and one screenshot (Burndown Chart, Sprint board, or the live UI change).

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/eJv7jCj4`

---

#### Screenshot 14 — Published LinkedIn post

![linkedIn](screenshots/Screenshot%20with%20published%20linkedIn%20post%20number%204.png)

---

# Submission Instructions

- Add all 14 required screenshots
- Full name must be visible in required screenshots
- Do not expose sensitive information (keys, passwords, account IDs)

---

# Completion Checklist

<<<<<<< HEAD
- [✅] Task 1: Team Mode or Solo Mode selected and all four roles documented (Screenshot 1 & Notes)
- [✅] Task 2: Team-managed Scrum project created with the required name (Screenshot 2)
- [✅] Task 3: UI improvement Epic created (Screenshot 3)
- [✅] Task 4: 6–8 Stories added under the Epic and ranked by value (Screenshots 4 & 5)
- [✅] Task 5: Story Points set (1, 2, or 3) with reasoning recorded (Screenshot 6 & Notes)
- [✅] Task 6: Sprint 1 created with Sprint Goal, 3–4 Stories, and Sub-tasks (Screenshots 7 & 8)
- [✅] Task 7: Burndown Chart opened (Screenshot 9)
- [✅] Task 8: One UI-only increment implemented, committed, deployed, and verified (Screenshots 10–12)
- [✅] Task 9: Retro comment with one Scrum pillar and one Scrum value (Screenshot 13)
- [✅] LinkedIn post published and URL submitted (Screenshot 14)
- [✅] Full Name visible in required screenshots
- [✅] No sensitive data exposed
=======
- [✅] Task 1: Team Mode or Solo Mode selected and all four roles documented (Screenshot 1 & Notes)
- [✅] Task 2: Team-managed Scrum project created with the required name (Screenshot 2)
- [✅] Task 3: UI improvement Epic created (Screenshot 3)
- [✅] Task 4: 6–8 Stories added under the Epic and ranked by value (Screenshots 4 & 5)
- [✅] Task 5: Story Points set (1, 2, or 3) with reasoning recorded (Screenshot 6 & Notes)
- [✅] Task 6: Sprint 1 created with Sprint Goal, 3–4 Stories, and Sub-tasks (Screenshots 7 & 8)
- [✅] Task 7: Burndown Chart opened (Screenshot 9)
- [✅] Task 8: One UI-only increment implemented, committed, deployed, and verified (Screenshots 10–12)
- [✅] Task 9: Retro comment with one Scrum pillar and one Scrum value (Screenshot 13)
- [✅] Task 10: Mandatory LinkedIn post published with the live URL, backlog refinement, Sprint planning, one shipped increment, proof, and Screenshot 14
- [✅ ] Full Name visible in required screenshots
- [✅ ] No sensitive data exposed
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

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
