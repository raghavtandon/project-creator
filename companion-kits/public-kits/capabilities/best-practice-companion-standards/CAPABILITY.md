# Capability: Best Practice Companion Standards

Every companion in the ecosystem must have a baseline set of infrastructure: a companion guide that follows the companion-guide capability, orientation and capture skills fitted to its domain, and universal ecosystem skills that are identical across all companions. Without this baseline, companions are isolated — they can't communicate with each other, can't capture reusable patterns, and can't have standards applied to them. The baseline is what makes a collection of companions into an ecosystem.

---

## What It Is

Companions are built organically. A PM companion gets 19 skills over months of iteration. An SE companion gets 7. A game designer gets 11. Each evolves to serve its domain — but the ecosystem infrastructure (communication, pattern capture, quality enforcement) gets built only where the pain is felt first, then never propagated.

**The result:** 10 companions, 1 with full ecosystem connectivity. The other 9 can't send structured handoffs, can't capture capabilities they discover, can't have standards applied to their artifacts. The practice's compound intelligence loop — where every engagement makes the next one smarter — requires that every companion can participate in the loop, not just the PM.

**This capability defines the minimum standard.** Two categories:

1. **Universal skills** — identical across all companions, never customized. These are the ecosystem's connective tissue.
2. **Fitted skills** — present in every companion but adapted to its domain. The structure is standard; the content reflects what each companion needs.

Plus the **companion guide** — the operational playbook that makes the companion usable.

---

## When to Use

| Companion Type | Usage | Notes |
|---------------|-------|-------|
| All companions | **Mandatory** | This is the minimum viable companion standard |
| New companions | At creation | Wire in during `/build` or initial setup |
| Existing companions | Via `/apply-capability` | Audit against standards, gap analysis, remediation |

---

## The Standard

### Required Artifact: companion-guide.md

Every companion must have a `companion-guide.md` at its project root following the structure defined in the **companion-guide** capability. This is the operational playbook — what to do, when, and which commands to use. It renders in the workbench right pane during active sessions.

The companion-guide capability defines the section order, formatting standards, and quality checklists. This capability does not redefine those standards — it requires their application.

### Universal Skills (Identical Across Companions)

These skills do not vary between companions. They serve the ecosystem, not the individual companion's domain. Copy them as-is; do not customize.

| Skill | Purpose | Why Universal |
|-------|---------|---------------|
| `/capture-capability` | Capture a reusable pattern as a capability kit (CAPABILITY.md + integration-guide.md) | Pattern capture follows the same structure regardless of where the pattern was discovered |
| `/apply-capability` | Apply a capability's standards to a target artifact — gap analysis + remediation | The audit-and-fix loop is the same mechanism everywhere |
| `/ask` | Compose a structured handoff message to another companion | Cross-companion communication format is standardized so any companion can receive and process it |

**Source of truth:** The CAIO BaaS companion (`companions/consortium.team/caio-baas-companion/.claude/skills/`) is the reference implementation. When these skills are updated, propagate changes to all companions.

### Fitted Skills (Adapted Per Companion)

These skills must exist in every companion but are adapted to the companion's domain. The structure follows established patterns; the content reflects what each companion reads, tracks, and produces.

| Skill | Purpose | What Gets Fitted |
|-------|---------|-----------------|
| `/brief` | Session orientation — read current state, synthesize, recommend what to work on | Which files to read, what "current state" means, how to prioritize recommendations. A PM reads hypothesis + success map + opportunity tree. An SE reads tickets + build progress. An architect reads specs + design decisions. |
| `/checkpoint` | Capture session state before leaving — externalize what happened, what was decided, what comes next | Which tracking files to update, what constitutes a "session insight" in this domain, where next-actions are queued. |

**The invariant:** Every `/brief` reads state, synthesizes, and recommends. Every `/checkpoint` captures insights, updates tracking, and queues next actions. The mechanism is the same; the files and domain language differ.

---

## Fitting /brief to a Companion

The `/brief` skill follows the session-hygiene capability's session start protocol, enhanced with domain-specific context.

### The Standard Brief Structure

1. **Read current state** — tracking files, context files, domain-specific state
2. **Synthesize** — what phase is the work in, what happened recently, what needs attention
3. **Recommend** — based on current state, what should this session focus on
4. **Ask** — "What would you like to work on?"

### What Changes Per Companion Type

| Companion Type | Primary State Files | Synthesis Focus | Recommendation Basis |
|---------------|--------------------|-----------------|--------------------|
| PM / Strategic | hypothesis, success map, opportunity tree, insights log | Strategic position, assumption health, pillar sequencing | Success map commitments, Torres risk signals, queued actions |
| Architect | specs, design decisions, build progress, floor plans | Architecture coherence, spec-reality gap, blocked decisions | Current floor status, pending product decisions, spec freshness |
| SE / Developer | tickets, build progress, blocked items | Implementation status, test health, blocking dependencies | Next ticket, unresolved blocks, review readiness |
| Game Designer | design pillars, mechanics catalog, playtest results | Design coherence, untested mechanics, player experience gaps | Priority playtest, design decision backlog |
| Tracker / Operational | client profiles, activity logs, success maps | Client engagement health, upcoming deadlines, pattern signals | Next follow-up, overdue activities, cross-client insights |
| Writer / Creative | craft assessment, mentor insights, writing samples | Craft development trajectory, skill gaps, session continuity | Next craft exercise, mentor focus area |

### The Three-Layer Decision

Does `/brief` need an agent? Apply the test: **does it need to read many files and produce substantial synthesis?**

- **PM companions** → Yes. 10+ tracking files, Torres diagnostics, compound loop analysis. Use orchestrator → agent ← reference skill.
- **SE companions** → Usually no. 3-5 files (tickets, progress, blocks). A single skill suffices.
- **Architect companions** → Borderline. If the spec ecosystem is large, agent dispatch helps. Otherwise single skill.

---

## Fitting /checkpoint to a Companion

The `/checkpoint` skill follows the session-hygiene capability's session finish protocol.

### The Standard Checkpoint Structure

1. **Review session activity** — what was discussed, created, modified, decided
2. **Capture insights** — observations worth persisting (to the companion's insight mechanism)
3. **Update tracking** — session log, next-actions, progress files
4. **Flag cross-companion signals** — anything another companion needs to know (suggest `/ask`)

### What Changes Per Companion Type

| Companion Type | Insight Mechanism | Tracking Files | Cross-Companion Signals |
|---------------|-------------------|----------------|------------------------|
| PM / Strategic | insights-log.md with insight IDs, pillar tags, hypothesis connection | session-log.md, next-actions.md, opportunity-tree.md, assumption-register.md | Specs ready for architect, tickets for dev, success maps for client tracker |
| Architect | design-decisions.md, patterns-discovered.md | session-log.md, build-progress.md | Product decisions needing PM, implementation tickets for SE |
| SE / Developer | None (insights are commits and test results) | build-progress.md, ticket status | Completed tickets for architect review, blocks needing product decisions |
| Game Designer | design-insights.md | session-log.md, playtest-log.md | Balance parameters for implementation, UX patterns for architect |
| Tracker / Operational | client-insights.md | activity logs, follow-up queue | Engagement patterns for PM, success map signals |

---

## Applying This Capability

When `/apply-capability best-practice-companion-standards` is run against a companion:

### If Skills Don't Exist

1. Copy universal skills from the reference implementation (CAIO BaaS companion)
2. Create fitted skills using the templates and fitting guidance above
3. Create companion-guide.md following the companion-guide capability
4. Update CLAUDE.md to reference new skills

### If Skills Already Exist

1. Audit each universal skill against the reference implementation — flag divergences
2. Audit each fitted skill against the structural standards — flag missing elements
3. Audit companion-guide.md against the companion-guide capability standards
4. Produce gap analysis with recommended fixes
5. Execute fixes with approval

---

## Anti-Patterns

| Anti-Pattern | Problem | Better Approach |
|--------------|---------|-----------------|
| Customizing universal skills per companion | Ecosystem connectivity breaks — `/ask` output from one companion can't be parsed by another if formats diverge | Copy universal skills exactly; customize only fitted skills |
| Skipping /brief because "the companion is simple" | Sessions start without orientation; work drifts; state gets lost | Every companion benefits from orientation, even if the brief is 5 lines |
| Skipping /checkpoint because "I'll remember" | Knowledge trapped in conversation; next session starts from scratch | Session-hygiene is non-negotiable; the checkpoint is how companions learn |
| Building /brief as a heavyweight orchestrator for every companion | SE and game designer companions don't need agent dispatch for 3-5 files | Match brief complexity to state complexity; single skill is fine for most |
| Building cross-companion skills from scratch in each companion | Divergent formats, duplicated maintenance, ecosystem fragmentation | Universal skills are copied from reference; one update propagates to all |

---

## Relationship to Other Capabilities

| Capability | Relationship |
|-----------|-------------|
| **companion-guide** | Depends on. This capability requires a companion guide; the companion-guide capability defines what a good guide looks like. |
| **session-hygiene** | Implements. The fitted skills (/brief, /checkpoint) are the concrete implementations of session-hygiene's start/finish protocols. |
| **claude-code-configuration** | Built on. All skills follow the reliability hierarchy and skill architecture patterns from claude-code-configuration. |
| **context-ecosystem** | Complements. /checkpoint writes to context files; /brief reads them. The context ecosystem defines the files; this capability ensures every companion has skills that read and write them. |
| **reverse-prompting** | Supports. /brief and /checkpoint frame sessions; reverse prompting fills them with content. |
