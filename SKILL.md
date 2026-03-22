---
name: roadmap
description: Maintain a living product roadmap (ROADMAP.md) that tracks current priorities and the history of how they evolved. Use this skill whenever the user says "update the roadmap", "roadmap update", "/roadmap", "add [X] to the roadmap", "we shipped [X]", "mark [X] complete", "defer [X]", or "deprioritize [X]". Also trigger proactively after River planning sessions that produce tier or priority decisions. Works across any project — not tied to a specific repo or product.
---

# Product Roadmap Skill

This skill maintains a living `ROADMAP.md` in the root of a project's Git repository. It has two sections:

1. **Current Roadmap** — the live, up-to-date view of priorities. Updated in place each session.
2. **Revision History** — an append-only log of what changed and why. Every update to the roadmap produces a history entry.

This skill is **project-agnostic** — it works with any repository. River is the natural owner of this skill, but it can be used with any team member active.

## Why This Matters

Roadmap decisions get made in planning conversations, then evaporate. The next session starts from scratch: why is this item Tier 1? When did we defer that one? What changed last week? Without a revision history, the roadmap is just a list — with no memory of the reasoning behind it.

`claude-roadmap-skill` makes the roadmap a durable, explainable artifact. Every priority call is recorded. Every deferral has a reason. Future sessions open with full context.

## Project Context (First Use Per Session)

The first time this skill triggers in a session, establish the project context by checking for clues in this order:

1. **Memory/conversation context** — Does Claude already know what project the user is working on? If so, confirm: "I'll update the roadmap for [Project Name] — correct?"
2. **Project knowledge** — Are there project files or documents that identify the project name and repo?
3. **Ask the user** — If neither source provides clarity, ask: "Which project's roadmap should I update? I'll need the project name and Git repo URL."

Capture and remember for the session:
- **Project name** — Used in the ROADMAP.md header and commit messages
- **Git repo URL** — Where to push
- **Authentication** — If git is configured with a credential helper (e.g., macOS Keychain), no token is needed. Only ask for a Personal Access Token if a push fails with an auth error.
- **Default branch** — Usually `main`, but confirm if unsure

---

## ROADMAP.md Structure

### Section 1: Current Roadmap

Updated in place each session. Follows this structure:

```markdown
## Current State Snapshot

**What works well:**
- [Working capabilities]

**What's missing or fragile:**
- [Gaps, risks, known issues]

---

## Opportunities — Prioritized

Scored by **Impact × Reach ÷ Effort** (proxy RICE).

### Tier 1 — Ship Next

| # | Opportunity | Why Now | Success Signal |
|---|---|---|---|
| 1 | **[Item]** | [Rationale] | [Measurable outcome] |

---

### Tier 2 — High Value, Plan for Next Sprint

| # | Opportunity | User Need | Assumptions to Validate |
|---|---|---|---|

---

### Tier 3 — Strategic / Longer Horizon

| # | Opportunity | Strategic Value | Why Not Now |
|---|---|---|---|

---

## Recommended Sequencing

[ASCII tree of sprint/phase sequencing]

---

## Open Questions

[Numbered list of decisions or unknowns blocking Tier 2+]

---

## OKR

**Objective:** [One sentence]

| Key Result | Target | Current |
|---|---|---|
```

### Section 2: Revision History

Append-only. New entries inserted at the top, below the `---` separator. Never delete or modify existing entries.

---

## Revision History Entry Format

```markdown
## [YYYY-MM-DD] Brief title of what changed

**Change Type:** `snapshot` | `add` | `complete` | `defer` | `remove` | `reprioritize` | `reframe` | `scope-change`
**Triggered by:** `planning-session` | `stakeholder-feedback` | `delivery` | `strategy-shift` | `discovery`
**Items Affected:** [opportunity names or numbers]

### What Changed
Concise description of the delta — what moved, what was added, what shipped.

### Why
Rationale. What new information or decision drove this update?

### Open Questions Resolved / Added
- Resolved: [question] → [answer]
- Added: [new question]
```

### Change Types

- **`snapshot`** — First entry or full baseline capture after a major rewrite. The "this is where we started" entry.
- **`add`** — New opportunity added to any tier.
- **`complete`** — Item shipped or delivered. Removed from active tiers; recorded here.
- **`defer`** — Item pushed to a later tier or backlog. Not dropped — just not now.
- **`remove`** — Item dropped entirely. Record why so it's not re-proposed without context.
- **`reprioritize`** — Item moved between tiers (up or down).
- **`reframe`** — Problem statement, success criteria, or user need redefined.
- **`scope-change`** — Scope of an existing item expanded or contracted.

### Triggered By

- **`planning-session`** — Came out of a structured planning conversation with River or another team member.
- **`stakeholder-feedback`** — A stakeholder, customer, or user provided input that changed priorities.
- **`delivery`** — An item shipped, freeing capacity or changing sequencing.
- **`strategy-shift`** — A business or product strategy decision changed what matters.
- **`discovery`** — New user research, data, or technical discovery changed the picture.

---

## Trigger Conditions

This skill activates when the user says:

- "update the roadmap" / "roadmap update" / `/roadmap`
- "River, update the roadmap"
- "add [X] to the roadmap"
- "we shipped [X]" / "[X] is done" / "mark [X] complete"
- "defer [X]" / "deprioritize [X]" / "move [X] to backlog"
- "remove [X] from the roadmap"
- "reprioritize [X]"

**Proactive trigger:** After a River planning session that produces a tier decision, priority call, or scope change — suggest a roadmap update: "This changes the roadmap — want me to update it and log the decision?"

---

## Workflow: Updating the Roadmap

### Step 1: Read the Current ROADMAP.md
Read the existing file to understand current state before making changes.

### Step 2: Update Section 1
Modify the Current Roadmap section to reflect the new state. Be precise — move items between tiers, add new items, mark completed items as removed (with a note), update the sequencing tree and open questions as needed.

### Step 3: Draft a Revision History Entry
Write a new entry for Section 2 documenting the delta: what changed, why, and any questions resolved or opened. Insert it at the top of the Revision History section.

### Step 4: Show the User
Present both changes (Section 1 diff summary + new history entry) for approval before committing. Never push without the user seeing the changes.

### Step 5: Commit and Push

```bash
cd /path/to/repo
git add ROADMAP.md
git commit -m "roadmap: [brief title from entry]"
git push origin main
```

Use a commit message prefixed with `roadmap:` followed by a short version of the entry title.

---

## Creating a New ROADMAP.md

If no ROADMAP.md exists, create it using this template:

```markdown
# [Project Name] — Product Roadmap

> Maintained by [claude-roadmap-skill](https://github.com/code-katz/claude-roadmap-skill). Last updated: YYYY-MM-DD.

---

## Current Roadmap

## Current State Snapshot

**What works well:**
- [Add working capabilities]

**What's missing or fragile:**
- [Add known gaps and risks]

---

## Opportunities — Prioritized

Scored by **Impact × Reach ÷ Effort** (proxy RICE).

### Tier 1 — Ship Next

| # | Opportunity | Why Now | Success Signal |
|---|---|---|---|

### Tier 2 — High Value, Plan for Next Sprint

| # | Opportunity | User Need | Assumptions to Validate |
|---|---|---|---|

### Tier 3 — Strategic / Longer Horizon

| # | Opportunity | Strategic Value | Why Not Now |
|---|---|---|---|

---

## Recommended Sequencing

[To be defined]

---

## Open Questions

[To be defined]

---

## OKR

**Objective:** [To be defined]

| Key Result | Target | Current |
|---|---|---|

---

## Revision History

```

Then immediately create a `snapshot` entry in the Revision History capturing the initial state.

---

## Git Setup

Same as claude-devlog-skill. If Claude Code's primary working directory is already inside the project repo, no setup is needed. ROADMAP.md belongs in the repo root.

---

## Reading the Roadmap

When the user asks about priorities, sequencing, or "why is X in Tier 2":

1. Read ROADMAP.md from the repo
2. Check both the Current Roadmap (current state) and Revision History (past reasoning)
3. Summarize the relevant context in the conversation

The revision history is the most valuable part for answering "why" questions.

---

## Style Guidelines

- Keep Tier 1 short — three to five items maximum. If everything is Tier 1, nothing is.
- Every Tier 1 item must have a **Success Signal** — a concrete, observable outcome.
- Every Tier 2 item must have **Assumptions to Validate** — what must be true before building this.
- Tier 3 items must have a **Why Not Now** — explicitly stating what's blocking or deprioritizing them.
- The sequencing tree should show realistic sprint/phase groupings, not a wishlist.
- Open Questions should be numbered and actionable — "Who owns the Slack bot token decision?" not "Figure out Slack."
- Update the `Last updated:` date in the header every time the roadmap changes.
