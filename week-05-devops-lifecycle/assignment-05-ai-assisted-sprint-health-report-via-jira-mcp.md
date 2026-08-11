# Assignment 5 — AI-Assisted Sprint Health Report via Jira MCP

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will connect Claude Code to your Jira board through an MCP server, the same way you connected it to GitHub in Week 2, and build a read-only `/sprint-health` skill. The skill reads your current sprint through Jira's API and reports sprint velocity, stories at risk of missing the sprint, and items missing an estimate — but it must never create, edit, comment on, or transition a single ticket itself. You will prove that boundary holds by making a real change on the board yourself and confirming the skill only ever reports, never acts.

---

# Task 1 — Create a Jira API Token

## Goal

Generate an API token from your Atlassian account that the MCP server will use to authenticate with your Jira site. Do not screenshot the token value itself.

### Evidence

#### Screenshot 1 — Jira API token creation confirmation page showing the token name, with the token value not visible

![task 1](screenshots/Screenshot%20with%20Jira%20API%20token%20creation%20confirmation%20page%20showing%20the%20token%20name.png)

### Notes You Must Write (Very Important):

Why does the MCP server need your site URL and account email in addition to the token?

An MCP server needs your site URL and account email alongside a token to route requests to the correct self-hosted or multi-tenant instance, bind the session to your exact user identity for audit logs, and verify permissions against upstream services without a browser login

Jiras component as a REST API usually uses Auth. Auth would need  the API token which is usually a sercet and a username (your email). the token alone doesn't identify which Atlassian account it belongs to or which Jira site to connect to, since Atlassian hosts many separate instances (your-site.atlassian.net is yours specifically, distinct from anyone else's). The token proves "this really is you," the email says "on behalf of this specific account," and the URL says "talk to this specific Jira instance, not some other one."


---

# Task 2 — Create .mcp.json at the Project Root

## Goal

Create or update `.mcp.json` at your project root with a Jira MCP server block, following the same shape as the GitHub MCP server you configured in Week 2.

### Evidence

#### Screenshot 2 — `.mcp.json` open in VS Code showing the Jira server configuration

![task 2](screenshots/Screenshot%20with%20mcp_json%20open%20in%20VS%20Code%20showing%20the%20Jira%20server%20configuration.png)

### Notes You Must Write (Very Important):

Compare this jira block to the github block from Week 2 Assignment 5. The GitHub server ran via npx (a Node.js package); this one runs via uvx (a Python package) — what stays exactly the same shape despite that difference, and why doesn't Claude Code care which language a given MCP server is written in?

Despite the change in the underlying runtime launcher from Node's npx to Python's uvx, the JSON configuration block structure (the outer shape, keys like command, args, and env) stays identical, and Claude Code remains agnostic to the host language because it communicates via a language-neutral Model Context Protocol (MCP) abstraction layer using standard input/output (stdio) streams.

What Stays the Same Shape
•	The JSON Block Keys: Both configurations use the exact same parent-child keys (mcpServers -> server_name -> command, args, and env) inside configuration files like ~/.claude.json.
•	The Transport Mechanism: Both spawn as local child processes communicating over standard I/O (stdio), where JSON-RPC messages flow back and forth identically regardless of whether the executable running at the other end interprets JavaScript or Python.

Why Claude Code Doesn’t Care About the Language
•	Protocol Standardization: Claude Code interacts with an MCP Client that speaks a strict JSON-RPC schema. It only cares about the manifest—the declared tool names, input schemas, and returned payloads. 
•	Process Decoupling: The server runs as a completely isolated black-box process on the machine. 
•	Universal Stdio Bridge: Once npx or uvx boots the package, the execution layer is hidden; Claude Code only reads the standardized tool outputs piped through stdout. 



---

# Task 3 — Add Your Credentials to settings.local.json

## Goal

Add your Jira site URL, account email, and API token to `.claude/settings.local.json`, and confirm that file is listed in `.gitignore` so it is never committed.

### Evidence

#### Screenshot 3 — `settings.local.json` open in VS Code showing the `env` section, with the actual token value blurred or covered

![task 3](screenshots/Screenshot%20with%20env%20section%20with%20the%20actual%20token%20value%20blurred%20or%20covered.png)

### Notes You Must Write (Very Important):

Why must JIRA_API_TOKEN live in settings.local.json and never in .mcp.json?

This is to prevent accidental credential leaks. Files named .mcp.json are typically tracked in version control (git), which risks exposing sensitive secrets publicly. Local settings files are ignored by git to keep tokens safe on my computer

---

# Task 4 — Verify the Connection with /mcp

## Goal

Restart Claude Code and confirm the Jira MCP server shows as connected.

### Evidence

#### Screenshot 4 — `/mcp` output showing `jira: connected`

![task 4](screenshots/Screenshot%20with%20mcp%20output%20showing%20jira%20connected.png)

---

# Task 5 — Run a Live Query to Prove Real Board Data

## Goal

Ask Claude to list the issues in your current active sprint through the Jira MCP connection, and confirm the result matches what you see on your live board in the browser.

### Evidence

#### Screenshot 5 — Claude's response showing the live sprint issue list retrieved via Jira MCP

![task 5](screenshots/Screenshot%20with%20Claude's%20response%20showing%20the%20live%20sprint.png)

### Notes You Must Write (Very Important):

How did you confirm this was real board data and not something Claude guessed?

Visual & Technical Indicators
•	Tool Call Blocks: The chat interface displays an explicit, collapsible "Tool Use" or "Executing MCP Call" notification within the response, showing the actual request sent to Jira.
•	Exact Identifiers: The output contains precise, non-generic ticket keys, exact user handles, and timestamps that cannot be reverse-engineered or guessed from general training data.
•	Fresh State Validation: The data reflects real-time status attributes, recent comments, or freshly updated assignees that occurred minutes prior. 

Procedural Verification Steps
•	Cross-Check the UI: Opening your actual Jira agile board in the browser tab I can verify that the issue keys, summaries, and sprint markers match the list Claude returned item-for-item. 
•	Inspect Client Logs: my local client configuration logs (such as Claude Desktop logs or claude.json) is confirmed to see the successful HTTP/SSE handshake and payload responses returned from my authorized Jira Cloud domain.



---

# Task 6 — Build the /sprint-health Skill

## Goal

Create a `/sprint-health` skill restricted to read-only Jira tools plus `Read`, with no issue-mutating tools and no `Write`. Run it and confirm it produces a report covering sprint velocity, at-risk stories, and items missing an estimate.

### Evidence

#### Screenshot 6 — `SKILL.md` frontmatter showing `allowed-tools` limited to read-only Jira tools plus `Read`, with `disable-model-invocation: true`

![task 6](screenshots/Screenshot%20with%20SKILL.md%20frontmatter%20showing%20allowed-tools%20limited%20to%20read-only.png)

#### Screenshot 7 — `/sprint-health` output showing the full triage report against your real sprint

![task 6](screenshots/Screenshot%20with%20sprint-health%20output%20showing%20the%20full%20triage%20report%20.png)

### Notes You Must Write (Very Important):

1. Which Jira MCP tools does this skill's allowed-tools list include, and which mutating tools (create issue, update issue, transition issue, add comment) does it deliberately exclude?

The skill includes query and retrieval actions in its allowed-tools list (such as getJiraIssue or JQL searches) while deliberately omitting mutating tools like createJiraIssue, updateJiraIssue, transitionJiraIssue, and addJiraComment to enforce a strict read-only security boundary. 

Included Read/Search Tools
•	mcp__atlassian__getJiraIssue or equivalent retrieval commands.
•	mcp__atlassian__searchJiraUsingJql / getIssuesByJQL.
•	General metadata lookup functions (e.g., user info, accessible resources).

Deliberately Excluded Mutating Tools
•	createJiraIssue / create_issue
•	updateJiraIssue / update_issue
•	transitionJiraIssue / status workflow transitions
•	addJiraComment / comment additions 


2. Why does a Scrum Master need this restriction more than almost any other role in this course?

a Scrum Master need this restriction because of a fundamental agile principle: the Scrum Master does not own, assign, or manage the data inside the sprint backlog. 

A Scrum Master agent must be restricted to read-only capabilities due to the following specific reasons:
1. Enforcing Team Self-Management
•	The Anti-Pattern: If an SM agent has access to updateJiraIssue or transitionJiraIssue, the AI will inevitably begin moving tickets across the board based on chat conversations or standup logs.
•	The Agile Reality: The Development Team owns the Sprint Backlog. Forcing an AI to automatically transition tickets on their behalf strips developers of autonomy, turns the daily standup into a "status reporting theater," and models the Scrum Master as an administrative micromanager. 
2. Preventing AI "Proxy Ownership" of Impediments
•	The Anti-Pattern: An SM agent with addJiraComment or createJiraIssue access might try to fix blockers by instantly spamming other teams' tickets with automated messages or creating dozens of sub-tasks. 
•	The Agile Reality: A core teaching in Scrum is that the Scrum Master should not remove impediments without involving the team. The role is meant to coach humans to communicate, resolve dependencies face-to-face, and self-organize—not to deploy a robotic proxy that silently alters Jira state. 
3. Protecting Psychological Safety and Team Metrics
•	The Anti-Pattern: An agent executing a JQL query to find bottlenecks might use a mutating tool to add a warning comment to a developer's ticket, e.g., "This ticket has been in In Progress for 4 days, reducing team velocity."
•	The Agile Reality: Velocity and cycle times are ecosystem diagnostic metrics, never weapons to shame individuals. Automated, non-human reprimands or system-generated "nudges" completely destroy the psychological safety required for high-performing agile environments


---

# Task 7 — Prove the Skill Never Mutates the Board

## Goal

Manually update one ticket on your board in the browser (for example, move a story to "Done" or add a missing estimate), then run `/sprint-health` again and confirm the new report reflects your change — proving the skill only ever reads live state and never wrote to the board itself.

### Evidence

#### Screenshot 8 — Second `/sprint-health` run showing the report now reflects your manual board change

![task 7](screenshots/Screenshot%20with%20Second%20sprint-health%20run%20showing%20the%20report%20.png)

### Notes You Must Write (Very Important):

Map this assignment to Gather → Analyze → Human Act → Verify from Week 3 Assignment 6. Which step did you perform manually in the browser, and why must that step stay human?

 Gather: Claude uses the Jira MCP server tool to fetch raw data (e.g., executing a JQL query for open sprints or calling search_issues).
 Analyze: Claude processes the JSON output payload, structuring the raw issue data into a human-readable sprint table or list.
  Human Act: I manually cross-reference the ticket keys, labels, or active statuses in your browser.
Verify: I confirm the interface's data matches reality, providing feedback or changing a state to close the validation loop. 


---

# Submission Instructions

Complete all tasks in sequence.

Your submission must include:
- All 8 required screenshots
- All the required notes

---

# Completion Checklist

- [✅ ] Task 1: Jira API token created, value never screenshotted (Screenshot 1)
- [✅ ] Task 2: `.mcp.json` has the Jira server block (Screenshot 2)
- [✅ ] Task 3: Credentials stored in `settings.local.json`, token blurred, file gitignored (Screenshot 3)
- [✅ ] Task 4: `/mcp` shows the Jira server connected (Screenshot 4)
- [✅ ] Task 5: Live query returned real sprint data, verified against the browser (Screenshot 5)
- [✅ ] Task 6: `/sprint-health` skill created with correct read-only `allowed-tools`, and produced a full report (Screenshots 6–7)
- [✅ ] Task 7: A manual board change was reflected in a second `/sprint-health` run (Screenshot 8)
- [✅ ] Skill never created, edited, transitioned, or commented on any issue
- [✅ ] Reflection answered (Notes)
- [✅ ] No API token value exposed

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
