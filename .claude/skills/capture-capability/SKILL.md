---
name: capture-capability
description: >
  Use when a reusable capability pattern has been identified during companion building and
  needs to be extracted into the companion-kits system as a shareable CAPABILITY.md.
  Triggers on requests to capture, extract, create, or document a capability.
disable-model-invocation: true
argument-hint: "[public|client] [capability-name] [description]"
---

# /capture-capability -- Extract a Reusable Capability

Capture a capability pattern into the companion-kits system as a shareable CAPABILITY.md following the standard template.

**Usage:**
- `/capture-capability public session-hygiene A pattern for...`
- `/capture-capability client workflow-decomposition A client-specific...`

---

## Phase 1: Parse & Resolve

### Step 1: Parse Arguments

Extract from `$ARGUMENTS`:
1. **Scope**: First word — must be `public` or `client`
2. **Capability name**: Second word — kebab-case (e.g., `session-hygiene`, `workflow-decomposition`)
3. **Description**: Everything after the second word — the seed description

If scope is missing or not `public`/`client`, STOP:
```
Usage: /capture-capability [public|client] [capability-name] [description]
Scope must be "public" (shared across all clients) or "client" (org-specific).
```

If capability name is missing, STOP with usage message.

If description is missing, STOP:
```
Provide a brief description of the capability to seed the definition.
```

### Step 2: Resolve Target Directory

**If scope is `public`:**
- Target: `companion-kits/public-kits/capabilities/[capability-name]/`

**If scope is `client`:**
1. Read `tracking/current-companion.md` to get the current companion path
2. Extract the client segment (text before the `/`)
3. Target: `companion-kits/private-kits/[client]-companion-kit/capabilities/[capability-name]/`
4. If no current companion is set, ask: "Which client is this capability for?"

### Step 3: Check for Conflicts

1. Check if the target directory already exists
2. If it does, read the existing `CAPABILITY.md` and ask:
   ```
   A capability named [name] already exists at [path].
   Options: (1) Update existing, (2) Choose a different name, (3) Cancel
   ```

**Output collected values before proceeding:**

```
Phase 1 Complete:
- Scope: [public/client]
- Name: [capability-name]
- Description: [seed description]
- Target: [full path]
- Conflict: [none / existing found]
```

---

## Phase 2: Define the Capability

Use the seed description and reverse prompting to build the capability definition. Ask ONE question at a time.

### Step 1: Core Definition

Present this framing and ask the first gap question:

```
Building capability: [name]
Seed: [description]

To write a strong CAPABILITY.md, I need to understand:
```

Work through these dimensions, skipping any the description already answers:

1. **What problem does this solve?** What goes wrong without this capability? What's the "standard approach" that fails?
2. **What's the mechanism?** Not just "do X" — how does it work and why does the mechanism produce better results?
3. **Which companion types benefit?** PM, SE, game design, writing mentor, strategic? Weight: mandatory / strong / moderate / light / not applicable.
4. **What's the key principle?** The single foundational insight — one sentence. Name it if a name emerges naturally.
5. **What are the anti-patterns?** 2-4 common mistakes and why they fail.

Stop prompting when you have enough to write a complete CAPABILITY.md. You do NOT need exhaustive answers — a capability can start as a proto-capability and mature through use.

### Step 2: Determine Maturity

Based on the evidence behind the capability:

| Status | Criteria |
|--------|----------|
| **PROTO-CAPABILITY** | Derived from source material or single engagement, not yet validated across clients |
| **EMERGING** | Validated in 1-2 companion projects, patterns are forming |
| **ESTABLISHED** | Validated in 3+ projects, patterns are stable and documented |

Client-scoped capabilities start as proto-capabilities by default.

### Step 3: Integration Guide Decision

Ask: "Does this capability need an integration guide (implementation templates, code examples, step-by-step setup)?"

- If yes: will generate both CAPABILITY.md and integration-guide.md
- If no: CAPABILITY.md only (most capabilities start here)

---

## Phase 3: Generate & Write

### Step 1: Generate CAPABILITY.md

Follow this template structure exactly. Adapt section content to the capability:

```markdown
# Capability: [Display Name]

[One-liner: what this capability is in one sentence.]

---

## What It Is

[2-3 paragraphs:
- What problem it solves (the "without this" scenario)
- How it works (the mechanism)
- Standard approach vs this approach (the contrast)]

---

## When to Use

| Companion Type | Usage | Weight |
|---------------|-------|--------|
| [Type] | [How this type uses it] | [Mandatory/Strong/Moderate/Light] |

---

## [Core Content Sections]

[Varies by capability. Use the most natural structure:
- For frameworks: Steps or phases
- For patterns: The pattern + examples
- For standards: Standards + good/bad contrasts
- For processes: The process + decision points]

---

## Key Principle: [Named Principle]

[The foundational insight in one paragraph. Everything else derives from this.]

---

## Relationship to Other Capabilities

[How this connects to existing capabilities. Reference by name:
- Context Ecosystem, Session Hygiene, Insight Feedback Loop
- Reverse Prompting, Strategic Planning, Knowledge Zones
- Claude Code Configuration, Companion Guide
- Meeting Processing, Mentor Framework, Process Evolution, Craft Assessment]

---

## Anti-Patterns

| Anti-Pattern | Problem | Better Approach |
|--------------|---------|-----------------|
| [Mistake 1] | [Why it fails] | [What to do instead] |
```

**For proto-capabilities**, add a status header after the title:

```markdown
**Status**: PROTO-CAPABILITY (untested — [source context])
**Created**: [today's date]
**Validation target**: [what needs to happen to promote]
```

### Step 2: Generate Integration Guide (if applicable)

Follow this template:

```markdown
# [Capability Name]: Integration Guide

How to wire [capability name] into a companion project.

---

## Quick Start: Minimum Viable [Capability]

[3-5 steps to get the minimum working version]

---

## [Implementation sections with templates and examples]

---

## Common Pitfalls

| Pitfall | Symptom | Fix |
|---------|---------|-----|
```

### Step 3: Present for Review

Display the generated CAPABILITY.md (and integration-guide.md if applicable) in full. Ask:

```
Review the capability definition above.
Options: (1) Write as-is, (2) Edit specific sections, (3) Cancel
```

### Step 4: Write Files

1. Create the target directory if it does not exist
2. Write `CAPABILITY.md`
3. Write `integration-guide.md` if generated
4. Report:

```
Capability captured:
- [path]/CAPABILITY.md
- [path]/integration-guide.md (if applicable)
- Status: [PROTO-CAPABILITY/EMERGING/ESTABLISHED]
- Next: Validate through use in companion projects
```
