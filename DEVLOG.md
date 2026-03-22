# claude-roadmap-skill — Development Log

A living record of architectural decisions, milestones, key insights, and strategic direction.
Auto-maintained via [claude-devlog-skill](https://github.com/code-katz/claude-devlog-skill). Entries are reverse-chronological.

---

## [2026-03-10] First production use validated — nvoss-dashboard

**Category:** `milestone`
**Tags:** `validation`, `production`, `nvoss-dashboard`
**Risk Level:** `low`
**Breaking Change:** `no`

### Summary
Skill validated in a real project session — used on `nvoss-dashboard` to create and populate a ROADMAP.md. Skill triggered correctly, generated the two-section structure, and committed cleanly.

### Detail
- First real-world invocation confirmed the trigger conditions, ROADMAP.md template, and git workflow all work as designed
- `/roadmap` slash command added to `claude-team-cli` and installed — available in all Claude Code sessions alongside `/devlog`
- Both companion skills (`claude-devlog-skill`, `claude-roadmap-skill`) now listed in the `claude-dev-team` README under "Companion Skills" with a combined install block

### Related
- [claude-team-cli](https://github.com/code-katz/claude-dev-team) — `/roadmap` command added in this session

---

## [2026-03-10] v1 shipped — SKILL.md, README, project structure

**Category:** `milestone`
**Tags:** `v1`, `init`, `skill`, `roadmap`
**Risk Level:** `low`
**Breaking Change:** `no`

### Summary
Initial release of `claude-roadmap-skill` — a Claude Code skill that maintains a living `ROADMAP.md` with a current roadmap section and an append-only revision history.

### Detail
- **SKILL.md** — Full skill definition including trigger conditions, ROADMAP.md structure, revision history entry format, 8 change types, workflow, and style guidelines
- **README.md** — Same marketing treatment as `claude-devlog-skill`: problem statement, ICP, before/after exchange, change types table, installation and usage
- **Two-section design**: Current Roadmap (updated in place) + Revision History (append-only) — this is the key structural decision that distinguishes it from the devlog

### Decisions Made
- Chose a two-section document over a purely append-only format because roadmaps need a "current state" view — you shouldn't have to read history entries to find the active Tier 1 list
- Modeled revision history entry format on devlog entries but replaced `Category`/`Risk Level`/`Breaking Change` with `Change Type`/`Triggered by`/`Items Affected` — more relevant fields for roadmap changes
- Kept 8 change types (`snapshot`, `add`, `complete`, `defer`, `remove`, `reprioritize`, `reframe`, `scope-change`) to cover the full lifecycle of a roadmap item without over-engineering
- River is the natural persona owner but the skill is persona-agnostic — any team member can trigger it

### Related
- Modeled on [claude-devlog-skill](https://github.com/code-katz/claude-devlog-skill)
- Reference format from `engops-dashboard/ROADMAP.md`
