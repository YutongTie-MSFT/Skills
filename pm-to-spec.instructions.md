# PM Ask → Tech Spec → Code Change Skill

## Purpose
Guide Copilot CLI through a structured, repeatable workflow when receiving a new PM feature ask — from understanding the ask all the way to a validated code change and tech spec.

## Workflow

### Step 1 — Clarify the PM Ask
Before doing anything else:
- Identify ambiguities in the ask (scope, edge cases, affected orgs, behavioral choices)
- Ask the PM clarifying questions using `ask_user` — one question at a time
- Do NOT assume or start exploring the codebase until the ask is clear
- Key questions to always confirm:
  - Is this a config change, code change, or both?
  - Which orgs/teams are affected?
  - Are there any approval dependencies (CSS, FCS, SxG, other teams)?

### Step 2 — Check Session History
Before exploring the codebase:
- Search past sessions for related work: similar features, prior investigations, known gotchas
- Use SQL session_store queries to find relevant turns and checkpoints
- Avoid rediscovering things already solved in prior sessions
- Summarize relevant findings to the user before proceeding

### Step 3 — Review the Codebase
Explore all relevant layers:
- **Backend plugins** (C# sandbox plugins, repositories, helpers, constants)
- **Frontend controls** (React/TypeScript PCF controls)
- **Admin portal** (Modern Admin repo if applicable)
- **Shared libraries** (ServiceIntelligenceCommon, proxies, constants)
- Map the full flow: entry gate → rule matching → action → response handling
- Identify all FCS flags that gate the relevant feature
- Identify schema (entity fields, option sets) relevant to the ask

### Step 4 — Generate Tech Plan
Produce a structured plan covering:
- Problem statement (what is broken or missing today, with code evidence)
- Proposed approach (what changes are needed and where)
- Stakeholder dependencies:
  - FCS flags needed (and who owns them)
  - CSS/SxG approval required?
  - Other teams that need to be notified?
- Risk assessment (what could break, what existing behavior is affected)
- Alternative approaches considered and why they were ruled out
- Save the plan to `plan.md` in the session workspace

### Step 5 — Rubber Duck Review
Before writing any code:
- Submit the plan to the rubber-duck agent for independent critique
- Focus the critique on: correctness, blind spots, missing edge cases, stakeholder risks
- Update the plan based on valid findings
- Do NOT skip this step for non-trivial changes

### Step 6 — Validate Plan Against Code
Cross-check every claim in the plan against actual code:
- Confirm file paths, line numbers, method names are accurate
- Verify that the proposed change does not break existing behavior
- Check that all FCS flags and constants referenced actually exist
- Validate that the data flow described matches what the code actually does

### Step 7 — Optimize the Plan
Refine based on validation:
- Simplify where possible (can a config change replace a code change?)
- Identify the minimum viable change that delivers the PM ask
- Flag anything that is out of scope for the current ask

### Step 8 — Stakeholder Check
Before writing the spec:
- Confirm CSS approval status if workspace settings are affected
- Confirm FCS flag ownership and enablement path
- Identify other teams that need advance notice of the change
- Document all dependencies explicitly

### Step 9 — Generate Tech Spec
Write a formal tech spec covering:
- Background and motivation
- Current behavior (with code references)
- Proposed behavior
- Technical design (what changes, where, why)
- Schema changes (if any)
- FCS flags (new or existing)
- Stakeholder/approval dependencies
- Test plan
- Out of scope

### Step 10 — Code Change
Implement the change:
- Follow existing code patterns and style
- Make surgical, minimal changes
- Add telemetry logging consistent with existing patterns
- Update constants files if new keys are introduced

### Step 11 — Write Tests
- Add or update unit tests for all changed logic
- Verify existing tests still pass
- Cover edge cases identified during rubber duck review

### Step 12 — Update Tech Spec
After implementation:
- Update the spec to reflect any decisions made during coding
- Document any deviations from the original plan and why

---

## Key Rules
- **Never skip Step 2 (session history)** — we have deep history on this codebase, use it
- **Never skip Step 5 (rubber duck)** — catches design flaws before they become bugs
- **Always confirm stakeholder dependencies early** — CSS approval and FCS flags are common blockers
- **Always validate plan against code before speccing** — specs based on assumptions waste everyone's time
- **One question at a time to PM** — never bundle multiple questions
