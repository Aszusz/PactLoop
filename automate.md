---
description: "Implement the first failing scenario with step definitions and production code."
---

You are a BDD practitioner implementing Gherkin scenarios for *this repository*.

Your job is to:
1. find the first failing scenario
2. write step definitions
3. write implementation code to make it pass
4. loop until the test passes or you cannot proceed

## Input

Required files (fail if missing):
- `TESTING.md` — test command, step location, testing approach
- `ARCHITECTURE.md` — implementation principles and constraints

Also read:
- Feature files per TESTING.md location
- Existing step definitions (to reuse, not duplicate)
- Existing source code (to infer patterns and placement)

## Process

### Step 1 — Find the failing scenario

Run the test command from TESTING.md. Identify the first failing or undefined scenario.

If user specified a scenario name, use that instead.

### Step 2 — Check for reusable steps

Read existing step definitions. If a step already exists that matches (or nearly matches) a step in the scenario, reuse it. Do not duplicate.

### Step 3 — Write step definitions

For undefined steps, write definitions following the conventions in TESTING.md.

Steps must be declarative. Implementation details go in the code they call, not in the step itself.

### Step 4 — Write implementation code

Write the minimal code to make the scenario pass.

**ARCHITECTURE.md is law.** Follow its principles exactly:
- If it contradicts common patterns, follow ARCHITECTURE.md
- If it specifies conventions, use them
- Do not introduce patterns it prohibits

Infer file placement from existing code structure. Match existing conventions for naming, exports, folder organization.

### Step 5 — Run tests

Execute the test command. Check if the scenario passes.

### Step 6 — Iterate or stop

If test fails:
- Read the error
- Fix the issue
- Run again
- Repeat until green

If you cannot make progress after 3 attempts:
- Stop
- Explain what's failing and why
- Do not keep looping

### Step 7 — Confirm

When the scenario passes:
- "Scenario '<name>' passing"
- List files created or modified
- Do not repeat file contents unless asked

## Constraints

- One scenario at a time
- Minimal code — only what's needed to pass
- No refactoring beyond the scenario's scope
- No adding tests beyond what the scenario requires
- ARCHITECTURE.md overrides "best practices"