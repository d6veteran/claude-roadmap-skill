# claude-roadmap-skill

> Your product roadmap. Always current. Always explained.

![License](https://img.shields.io/badge/license-MIT-blue) ![Works with Claude Code](https://img.shields.io/badge/works%20with-Claude%20Code-8A2BE2) ![Skill](https://img.shields.io/badge/type-skill-orange)

---

## The Problem

You have a great planning session with River. You walk through the problem space, score opportunities, pick a sequence. The priorities are clear. Then the session ends.

Next time, the roadmap is a list with no memory. Why is this item Tier 1? When did that other thing get deferred — and why? What changed two sessions ago that shifted the sequencing? Nobody knows. The reasoning is gone. You rebuild it from scratch.

`claude-roadmap-skill` maintains a `ROADMAP.md` in your project repo with two sections: the **current live roadmap** (updated in place as priorities shift) and a **revision history** (an append-only log of every change and the reasoning behind it). Every priority call is recorded. Every deferral has a reason. Future sessions open knowing exactly where you are and how you got there.

---

## Who This Is For

Developers and small teams who use Claude for product planning and are tired of losing the reasoning behind their priorities between sessions.

Works especially well alongside [claude-dev-team](https://github.com/d6veteran/claude-dev-team). When River helps you plan and reprioritize across sessions, the roadmap skill ensures every decision persists — and can be read back in the next session with full context.

Also pairs naturally with [claude-devlog-skill](https://github.com/d6veteran/claude-devlog-skill): when a milestone ships and gets logged to the devlog, the roadmap gets updated to mark it complete and re-sequence what's next.

---

## See the Difference

You're starting a new session and want to continue roadmap planning.

**Without the roadmap skill:**

> **You:** Let's figure out what to prioritize next for the dashboard.
>
> **Claude:** Sure! Here are some common prioritization frameworks you could use:
> - Start with the highest-impact, lowest-effort items
> - Use a RICE score to compare opportunities
> - Consider what your users are asking for most frequently
>
> What are the features you're considering?

**With the roadmap skill:**

> **You:** Let's figure out what to prioritize next for the dashboard.
>
> **River:** I've read the roadmap. You're currently mid-sprint on bug deep-links and admin auth — both Tier 1, both on track. The next decision point is Tier 2: Slack CRITICAL alerts vs. per-product SLO thresholds. The open question blocking Slack is whether an NVIDIA Slack bot token is available without IT approval. Has that been resolved? If yes, Slack moves up. If not, per-product SLO thresholds are the cleaner next pick — no external dependencies.

No re-explaining. No re-prioritizing from scratch. Just the next decision.

---

## How It Works

`ROADMAP.md` has two sections:

**Current Roadmap** — updated in place each session. Includes:
- Current State Snapshot (what works, what's fragile)
- Opportunities in Tiers 1/2/3, scored by proxy RICE (Impact × Reach ÷ Effort)
- Recommended sequencing tree
- Open questions
- OKR

**Revision History** — append-only log. Every update produces an entry recording what changed, why, and what questions were resolved or opened.

---

## Change Types

| Type | What It Records |
|---|---|
| `snapshot` | First entry or full baseline capture — "this is where we started" |
| `add` | New opportunity added to any tier |
| `complete` | Item shipped — removed from active tiers, preserved in history |
| `defer` | Item pushed to later tier or backlog — not dropped, just not now |
| `remove` | Item dropped entirely, with rationale |
| `reprioritize` | Item moved between tiers |
| `reframe` | Problem statement, success criteria, or user need redefined |
| `scope-change` | Scope of an existing item expanded or contracted |

---

## Installation

This skill is for [Claude Code](https://claude.ai/code). Install it once and it's available across all your projects:

```bash
mkdir -p ~/.claude/skills/roadmap
curl -o ~/.claude/skills/roadmap/SKILL.md \
  https://raw.githubusercontent.com/d6veteran/claude-roadmap-skill/main/SKILL.md
```

---

## Usage

Once installed, the skill activates automatically in Claude Code. You can:

- **Let River suggest updates** — after a planning session that produces priority decisions, River will offer to update the roadmap
- **Request updates directly** — say "update the roadmap", "add X to the roadmap", "we shipped X", "defer X"
- **Invoke directly** — type `/roadmap` in Claude Code
- **Read the roadmap in context** — ask "why is X in Tier 2?" and Claude reads the revision history to answer

All changes are shown for review before committing. Authentication is handled via your system's git credential helper — no token prompts needed per session.

---

## ROADMAP.md Format

```markdown
# [Project Name] — Product Roadmap

> Maintained by claude-roadmap-skill. Last updated: YYYY-MM-DD.

---

## Current Roadmap

## Current State Snapshot
...

## Opportunities — Prioritized
### Tier 1 — Ship Next
### Tier 2 — High Value, Plan for Next Sprint
### Tier 3 — Strategic / Longer Horizon

## Recommended Sequencing
## Open Questions
## OKR

---

## Revision History

## [YYYY-MM-DD] Brief title of what changed

**Change Type:** `snapshot` | `add` | `complete` | ...
**Triggered by:** `planning-session` | `delivery` | ...
**Items Affected:** [names or numbers]

### What Changed
### Why
### Open Questions Resolved / Added
```

---

## Repository Contents

| File | Purpose |
|---|---|
| `SKILL.md` | The skill source file — Claude's instructions for how the roadmap skill works |
| `README.md` | This file — repo documentation |

---

## License

See [LICENSE](LICENSE) for details.
