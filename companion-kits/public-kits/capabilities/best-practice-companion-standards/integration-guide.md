# Best Practice Companion Standards: Integration Guide

How to bring a companion up to the ecosystem baseline — companion guide, universal skills, and fitted skills.

---

## Quick Start

For an existing companion that has none of the standard infrastructure:

1. **Copy the universal rules** into `.claude/rules/`
2. **Copy the three universal skills** from the reference implementation
3. **Create a fitted `/brief`** using the template below
4. **Create a fitted `/checkpoint`** using the template below
5. **Create `companion-guide.md`** following the companion-guide capability
6. **Update CLAUDE.md** to list the new skills

This gets the companion connected to the ecosystem. Refinement happens through use.

---

## Step 1: Copy Universal Rules

Universal rules are identical across all companions. Create them in the companion's `.claude/rules/` directory.

**Create `.claude/rules/knowledge-domains.md`:**

```markdown
When the user references a location you don't recognize (e.g., "client files", "knowledge", "look in X for..."),
read the `.knowledge/config.json` file at the project-creator root to resolve it.

That file maps domain labels to filesystem paths. Match the user's reference against domain labels
and use the corresponding path for your search.

Project-creator root: find it by traversing parent directories from the companion until you find CLAUDE.md
with "# Project Creator" at the top, or use the path from .workbench/config.json's pcDirectoryPath.
```

**No customization needed.** The rule works in any companion without modification.

---

## Step 2: Copy Universal Skills

The three universal skills are identical across all companions. Copy them from the Knowledge Explorer companion (the open-source reference implementation).

**Source:** `companions/common/knowledge-explorer/.claude/skills/`

> **Don't have the Knowledge Explorer?** Clone it from the consortium.team org into your `companions/common/` directory.

**Copy these directories as-is:**

```
.claude/skills/capture-capability/SKILL.md
.claude/skills/apply-capability/SKILL.md
.claude/skills/ask/SKILL.md
```

**No customization needed.** These skills resolve paths dynamically (companion discovery via Glob, capability resolution via search). They work in any companion without modification.

**Verification:** After copying, confirm each skill loads by running `/capture-capability`, `/apply-capability`, and `/ask` — each should display its argument hint without errors.

---

## Step 3: Create Fitted /brief

The `/brief` skill reads current state, synthesizes, and recommends what to work on. The structure is standard; the content is fitted to the companion's domain.

### Template

```yaml
---
name: brief
description: Session orientation — read current state, synthesize what's happening, and recommend what to work on this session. Use at the start of every working session.
disable-model-invocation: true
---
```

```markdown
# /brief — Session Orientation

Read the companion's current state and recommend what to work on.

## Step 1: Read Current State

Read the following files:
- [List the companion's tracking files — session log, progress, next-actions]
- [List the companion's primary context files — the ones that change between sessions]
- [List any external state — success maps, ticket boards, build progress]

If any file is missing or empty, note the gap.

## Step 2: Synthesize

Summarize in 3-5 sentences:
- Current phase of work (what's in progress, what's blocked, what's complete)
- What happened in the most recent session
- What needs attention (stale context, unresolved decisions, upcoming deadlines)
- Any flags (contradictions, accumulating debt, overdue activities)

## Step 3: Recommend

Based on the current state, recommend what to work on this session:
- [Primary recommendation with rationale]
- [1-2 alternatives if the primary doesn't fit]

## Step 4: Ask

"What would you like to work on?"

If the user agrees with the recommendation, proceed.
If the user has a different priority, follow their lead.
```

### Fitting Examples

**PM Companion:**
- Step 1 reads: hypothesis.md, success map registry, opportunity-tree.md, insights-log.md, session-log.md, next-actions.md, assumption-register.md
- Step 2 synthesizes: success map proximity, Torres risk signals, pillar sequencing, assumption health
- Step 3 recommends: based on success map commitments and queued actions
- May use three-layer architecture (orchestrator → agent ← strategic-analysis reference skill) if 10+ files

**SE Companion:**
- Step 1 reads: build-progress.md, current ticket status, blocked items
- Step 2 synthesizes: tickets remaining, test health, blocking dependencies
- Step 3 recommends: next ticket by priority, or unblock if blocked
- Single skill — no agent dispatch needed

**Architect Companion:**
- Step 1 reads: active specs, design-decisions.md, build-progress.md, floor plans
- Step 2 synthesizes: spec-reality alignment, pending product decisions, architecture coherence
- Step 3 recommends: next design decision, spec update, or review readiness

**Game Designer:**
- Step 1 reads: design pillars, mechanics catalog, playtest-log.md, session-log.md
- Step 2 synthesizes: design coherence, untested mechanics, player experience gaps
- Step 3 recommends: priority playtest, mechanic to design, or framework to apply

**Client Tracker:**
- Step 1 reads: client profiles (recent activity), success map statuses, follow-up queue, engagement patterns
- Step 2 synthesizes: overdue follow-ups, engagement health across clients, upcoming milestones
- Step 3 recommends: next follow-up, success map to update, or pattern to surface

---

## Step 4: Create Fitted /checkpoint

The `/checkpoint` skill captures session state before leaving. The structure is standard; the tracking files and insight format are fitted.

### Template

```yaml
---
name: checkpoint
description: Capture session state before leaving — externalize what happened, what was decided, and what comes next. Use at the end of every session to ensure continuity.
disable-model-invocation: true
---
```

```markdown
# /checkpoint — Session Capture

Externalize the current session's work so the next session can pick up where this one left off.

## Step 1: Review Session Activity

Scan the conversation for:
- What was discussed (topics, questions explored)
- What was created or modified (files written, decisions made)
- What was decided (and the rationale)
- What was discovered (insights, patterns, connections)

## Step 2: Capture Insights

[Fitted to companion's insight mechanism]

For each insight worth persisting:
- Write to [insight file] with date and source
- [Tag/categorize per companion's convention]
- [Connect to companion's strategic anchor if applicable]

If no insights emerged, that's fine — not every session produces insights.

## Step 3: Update Tracking

Update the following files:
- [session-log.md or equivalent] — add session entry with date, topic, key outcomes
- [next-actions.md or equivalent] — queue recommended next session activities
- [progress file if applicable] — update status of in-progress work
- [any companion-specific tracking files]

## Step 4: Flag Cross-Companion Signals

Check: did this session produce anything another companion needs to know?

If yes, suggest: "This session produced [X] that [companion name] should know about. Want to run `/ask [companion] [topic]`?"

If no, skip this step.

## Step 5: Confirm

Report what was captured:
- Session log entry: [summary]
- Insights captured: [count and IDs if applicable]
- Next actions queued: [list]
- Cross-companion signals: [list or "none"]
```

### Fitting Examples

**PM Companion:**
- Step 2: insights-log.md with sequential IDs (I-NNN), pillar tags, hypothesis connection
- Step 3: session-log.md, next-actions.md, opportunity-tree.md, assumption-register.md, patterns-discovered.md
- Step 4: specs for architect, tickets for dev, success maps for client tracker

**SE Companion:**
- Step 2: No formal insight log — insights are code and test results
- Step 3: build-progress.md (ticket status), note any blocks encountered
- Step 4: completed tickets for architect review, blocks needing product decisions

**Architect Companion:**
- Step 2: design-decisions.md (new decisions), patterns-discovered.md (architectural patterns)
- Step 3: session-log.md, build-progress.md
- Step 4: product decisions needing PM, implementation guidance for SE

---

## Step 5: Create companion-guide.md

Follow the **companion-guide capability** (`companion-kits/public-kits/capabilities/companion-guide/`) for structure and standards. The integration guide there provides:

- The definitive section order (command quick reference first)
- Templates for each section
- Quality checklists for new and updated guides

The companion guide must include an **Ecosystem Maturation** section in the Command Quick Reference listing the three universal skills:

```
### Ecosystem Maturation

/capture-capability public|client name    Capture a reusable pattern as a capability kit
/apply-capability name target             Apply a capability's standards to a target artifact

### Cross-Companion Communication

/ask companion-name [topic]               Compose a structured message to another companion
```

---

## Step 6: Update CLAUDE.md

Add the new skills to the companion's CLAUDE.md. Follow the existing structure — most companions have a phases table or skills list.

Add an **Ecosystem Maturation** phase or section:

```markdown
| **Ecosystem Maturation** | Capture and apply reusable patterns across companions | `/capture-capability`, `/apply-capability` |
```

Add `/ask` to the cross-cutting line:

```markdown
**Cross-cutting:** ..., `/ask` (cross-companion communication)
```

Update the skills count if the CLAUDE.md tracks it.

---

## Configuration Surfaces

| Surface | What Gets Added | Notes |
|---------|----------------|-------|
| `.claude/rules/` | 1 universal rule (knowledge-domains.md) | Copied from capability prototype |
| `.claude/skills/` | 5 skill directories (3 universal + 2 fitted) | Universal skills are copied; fitted skills are created |
| `companion-guide.md` | New file at project root | Follows companion-guide capability standards |
| `CLAUDE.md` | Updated phases/skills table | Ecosystem Maturation phase + cross-cutting /ask |
| `tracking/` | May need session-log.md, next-actions.md if missing | /brief reads these; /checkpoint writes them |

---

## Examples

### Reference Implementation: Knowledge Explorer Companion

The open-source reference implementation. Clone from the consortium.team org into `companions/common/`. Contains all universal skills as the canonical source for ecosystem propagation.

### Example: Full Implementation

A fully implemented companion has 16+ skills including all 5 standard skills. Companion guide follows the capability standard with command quick reference first, Mermaid lifecycle diagram, situation-driven tables, three-layer agent architecture section.

- `/brief` dispatches a Sonnet agent (session-analyst) with strategic-analysis reference skill — reads 10+ files, produces three-layer synthesis
- `/checkpoint` writes to 6 tracking files with sequential insight IDs and pillar tags
- Universal skills match the Knowledge Explorer reference

### Example: Partial Implementation

Has `/brief` and `/checkpoint` (fitted to its domain) but lacks the three universal skills. Has a companion guide. To complete:

1. Copy `/capture-capability`, `/apply-capability`, `/ask` from Knowledge Explorer
2. Add Ecosystem Maturation section to companion-guide.md
3. Update CLAUDE.md

### Missing Implementation: ImagineSports Game Hub PM

Has 17 domain skills but no companion guide, no `/brief`, no `/checkpoint`, no universal skills. Full implementation needed:

1. Copy 3 universal skills
2. Create fitted `/brief` (reads: design pillars, sprint status, PM methodology files)
3. Create fitted `/checkpoint` (writes: session-log, design-insights, next-actions)
4. Create companion-guide.md from scratch per companion-guide capability
5. Update CLAUDE.md

---

## Checklist: Applying to an Existing Companion

Use this when running `/apply-capability best-practice-companion-standards` against a companion:

- [ ] `knowledge-domains.md` **rule exists** in `.claude/rules/` — if not, create per Step 1
- [ ] `knowledge-domains.md` **matches capability prototype** — no unauthorized customization
- [ ] **companion-guide.md exists** — if not, create per companion-guide capability
- [ ] **companion-guide.md follows standard section order** — command quick reference first
- [ ] **companion-guide.md includes Ecosystem Maturation commands** in quick reference
- [ ] `/brief` **skill exists** — if not, create fitted version
- [ ] `/brief` **reads current state, synthesizes, recommends** — all three steps present
- [ ] `/brief` **reads the right files** for this companion type
- [ ] `/checkpoint` **skill exists** — if not, create fitted version
- [ ] `/checkpoint` **captures insights, updates tracking, queues next actions** — all steps present
- [ ] `/checkpoint` **flags cross-companion signals** — Step 4 present
- [ ] `/capture-capability` **skill exists** — if not, copy from reference
- [ ] `/capture-capability` **matches reference implementation** — no unauthorized customization
- [ ] `/apply-capability` **skill exists** — if not, copy from reference
- [ ] `/apply-capability` **matches reference implementation** — no unauthorized customization
- [ ] `/ask` **skill exists** — if not, copy from reference
- [ ] `/ask` **matches reference implementation** — no unauthorized customization
- [ ] **CLAUDE.md lists all 5 skills** — in appropriate phases/sections
- [ ] **Tracking files exist** for /brief to read and /checkpoint to write

---

## Customization Points

| Element | Customizable? | How |
|---------|--------------|-----|
| Universal skills (capture, apply, ask) | **No** | Copy exactly from reference implementation |
| /brief file list | **Yes** | Choose files relevant to companion's domain |
| /brief synthesis depth | **Yes** | Single skill vs. orchestrator+agent based on file count |
| /brief recommendation basis | **Yes** | PM uses success map; SE uses ticket priority; etc. |
| /checkpoint insight format | **Yes** | Sequential IDs with tags (PM) vs. none (SE) vs. design journal (game designer) |
| /checkpoint tracking files | **Yes** | Write to the companion's own tracking structure |
| companion-guide.md content | **Yes** | All content is companion-specific; structure follows the standard |
| companion-guide.md optional sections | **Yes** | Include only sections warranted by the companion's domain |
