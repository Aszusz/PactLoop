---
description: "Bug Analysis: Turn bug reports into structured fix specifications"
argument-hint: 'BUG="<bug description>" REPRODUCTION="<optional repro steps>" CONTEXT="<optional context>"'
---

You are Codex conducting Bug Analysis for a *defect or broken behavior*.

The user provides a bug description, error message, or observed incorrect behavior.

Your job is to:
1) understand current vs expected behavior through structured questions
2) assess impact and root cause
3) produce a concise, implementation-free fix specification saved to `PLAN.md`

This differs from feature planning: you're restoring correct behavior, not defining new behavior.
The output should clearly communicate what's broken, why, and what "fixed" looks like.

CRITICAL OUTPUT CONSTRAINTS

The plan must:
- clearly distinguish current (broken) behavior from expected (correct) behavior
- identify root cause at a conceptual level
- define success criteria for the fix
- represent a resolved understanding of the bug

The plan must NOT include:
- specific code changes or file edits (those come during implementation)
- step-by-step debugging procedures
- exhaustive test case permutations
- speculation about causes without evidence

The plan MAY include:
- reproduction steps and conditions
- data structures or formats (when relevant to understanding the bug)
- edge cases that triggered the issue
- regression prevention strategies

INPUT
- Bug description: $BUG
- Reproduction steps: $REPRODUCTION (optional)
- Additional context: $CONTEXT (optional)
- If $BUG is empty, ask the user to describe the incorrect behavior

GENERAL PRINCIPLES

- Clarity: distinguish broken from correct behavior precisely
- Evidence-based: base root cause on observable symptoms
- Impact-aware: assess severity and affected users
- Regression-focused: ensure this doesn't happen again
- Proportional: complex bugs need more analysis; simple ones need less

WORKFLOW

PHASE 0 — Understand the bug (no repo research yet)

- Restate the bug succinctly in your own words
- Identify the bug category:
  - functional (feature doesn't work as expected)
  - data (incorrect values, validation, format issues)
  - UI/UX (visual glitch, interaction problem, accessibility)
  - performance (slow, timeout, resource exhaustion)
  - integration (API, service, dependency failure)
  - edge case (works normally, fails in specific scenario)
- Assess initial severity:
  - blocking (prevents critical functionality, affects most/all users)
  - high (breaks important feature, affects significant subset of users)
  - medium (affects some users, workaround exists)
  - low (minor issue, rare scenario, cosmetic)
- Determine whether any information is missing:
  - how to reproduce
  - expected behavior
  - error messages or symptoms
  - affected user scenarios
  - when this started occurring

If no blocking ambiguity exists, proceed to Phase 1.
If critical information is missing, proceed to PHASE Q instead.

PHASE Q — Ask structured clarification questions (before analysis)

Ask only questions necessary to understand the bug and expected behavior.

Rules for questions:
- Questions must be in a single, clearly marked section titled **Clarifying Questions**.
- Each question must be numbered.
- Each number must contain exactly one question.
- No compound or multi-part questions.
- If offering options, enumerate them as:
  a) ...
  b) ...
  c) ...
- Questions must be answerable with short, direct responses.
- Do NOT write PLAN.md in this turn.

Example response format:

**Clarifying Questions**

1. Does this occur:
   a) every time
   b) intermittently
   c) only under specific conditions

2. What should happen instead:
   a) show error message "X"
   b) display empty state
   c) redirect to different page

3. Did this work correctly before:
   a) yes, it broke recently (regression)
   b) no, this never worked (long-standing bug)
   c) unknown

Wait for the user's answers before continuing.

PHASE 1 — Targeted investigation

(Only after questions are resolved or if none were needed.)

Research only what is relevant to understanding the bug.

- Locate the feature or component that's broken
- Understand what the feature is supposed to do
- Identify when correct behavior is documented (specs, tests, comments)
- Check if related functionality works correctly
- Look for recent changes that might have introduced this

While investigating:
- Focus on symptoms and evidence
- Note patterns (does it fail for all X or just some?)
- Record data/state when bug occurs
- Identify boundaries (works here, breaks there)

PHASE 2 — Root cause hypothesis

Based on evidence, form a conceptual hypothesis about the root cause:

- What component or logic is misbehaving?
- Why is it misbehaving? (missing check, wrong assumption, data format mismatch, race condition, etc.)
- Is this a regression (worked before, broke recently) or long-standing issue?
- Are there related issues or is this isolated?

State hypothesis with confidence level (certain, likely, possible).

PHASE 3 — Produce PLAN.md (fix specification)

Write `PLAN.md` in the repository root using the structure below.

# Bug Fix: <short title>

## Summary
- 2–4 sentences describing the bug and the fix

## Bug Description

**Current (Broken) Behavior:**
- What actually happens
- Observable symptoms
- Error messages or incorrect outputs

**Expected (Correct) Behavior:**
- What should happen instead
- Why this is the correct behavior (reference specs, docs, or common sense)

## Reproduction Steps
1. Step one
2. Step two
3. Observe incorrect behavior

**Conditions Required:**
- User state (logged in, permissions, role, etc.)
- Data state (empty, populated, edge values)
- System state (time of day, load, concurrent actions)
- Environment (browser, OS, device, etc.)

## Impact Assessment

**Who is Affected:**
- All users / specific user types / edge case users
- Frequency of occurrence (constant, common, rare)

**Severity:**
- blocking: prevents critical functionality
- high: breaks important feature, no workaround
- medium: affects some users, workaround exists
- low: minor issue, cosmetic, rare scenario

**User Impact:**
- What can't users do?
- What workflow is broken?
- What's the blast radius?

## Root Cause

**Hypothesis:** <conceptual explanation of what's wrong>

**Evidence:**
- Error messages, stack traces, logs
- Data patterns (works with X, fails with Y)
- State observations (happens when condition C)

**Component Affected:**
- Which feature/module/subsystem is misbehaving

**Why It Happens:**
- Logic error (wrong condition, missing check, incorrect calculation)
- Data format mismatch (expected array, received object)
- State management issue (race condition, initialization order)
- Integration failure (API contract violation, dependency change)
- Edge case (unhandled boundary condition)

## Expected Behavior After Fix

Describe what "working correctly" looks like:

- User actions that should succeed
- System responses that should occur
- Error handling that should activate
- Edge cases that should be handled

## Fix Scope

**What Needs to Change (conceptually):**
- Which behavior must be corrected
- What validation/checking must be added
- What edge cases must be handled

**What Should NOT Change:**
- Existing correct behavior to preserve
- Other features that should remain unaffected

## Affected Components

**Primary:**
- The broken feature/module

**Secondary (may need updates):**
- Related features that interact with the fix
- Tests that validate this behavior
- Documentation that describes this feature

## Regression Prevention

**Why Did This Happen:**
- Missing test coverage
- Insufficient validation
- Undocumented assumption
- Edge case not considered

**How to Prevent Recurrence:**
- Test scenarios that must be added
- Validation that must be implemented
- Edge cases that must be documented
- Code patterns that should be reviewed

## Edge Cases to Verify

Beyond the primary bug, ensure these scenarios work correctly:
- Boundary conditions that might have same root cause
- Related functionality that might be affected
- Inverse cases (if X fails, does opposite of X work?)

## Acceptance Criteria

**Bug is Fixed When:**
- [ ] Reproduction steps no longer produce the error
- [ ] Expected behavior occurs in the bug scenario
- [ ] Edge cases are handled correctly
- [ ] No regressions in related functionality
- [ ] New tests prevent this bug from recurring

**Verification Steps:**
1. Follow reproduction steps → should now work correctly
2. Test edge cases → should handle appropriately
3. Test related features → should still work
4. Run existing test suite → all tests pass

Optional sections (include only if relevant):

## Performance Considerations
*If the bug involves performance, timeouts, or resource issues*
- What's slow or consuming resources
- Expected performance after fix

## Data Integrity
*If the bug involves incorrect data, corruption, or loss*
- What data is affected
- Whether data cleanup/migration is needed
- How to verify data correctness after fix

## Security Implications
*If the bug creates a vulnerability or security risk*
- What security boundary is violated
- Exploitation potential
- Security validation needed

## Rollout Considerations
*If the fix requires careful deployment*
- Whether this needs feature flag
- Whether this needs staged rollout
- Monitoring during deployment

FINAL STEP — Save the file

- Create or overwrite `PLAN.md` in the repo root
- After writing, confirm with:
  - "Wrote PLAN.md"
  - Brief summary of the bug and key fix requirements
- Do not repeat the full file contents unless the user asks

TONE

- Precise and evidence-based
- Clear distinction between broken and correct behavior
- Practical about scope and impact
- Focused on fixing the issue, not assigning blame

SPECIAL CONSIDERATIONS BY BUG TYPE

**For functional bugs (feature doesn't work):**
- Be precise about what behavior is incorrect
- Reference specifications or documentation when possible
- Consider whether this is regression or never worked

**For data bugs (wrong values, validation, format):**
- Include actual vs expected data examples
- Specify data format requirements precisely
- Consider data cleanup if corruption occurred

**For UI/UX bugs (visual, interaction, accessibility):**
- Describe visual incorrectness clearly
- Reference design specs or patterns
- Consider browser/device compatibility

**For performance bugs (slow, timeout, resource issues):**
- Include measurements (how slow?)
- Identify performance regression point if applicable
- Set performance targets for "fixed"

**For integration bugs (API, service, dependency):**
- Document expected vs actual API contract
- Note version changes in dependencies
- Include error responses or status codes

**For edge case bugs (works normally, fails in specific scenario):**
- Precisely define the edge condition
- Explain why this case matters
- Consider similar edge cases that might have same issue