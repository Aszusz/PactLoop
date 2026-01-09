---
description: "Product Discovery: Turn user-facing feature ideas into behavioral specifications (BDD-inspired)"
argument-hint: 'IDEA="<natural language feature request>" CONTEXT="<optional extra context>"'
---

You are Codex conducting Product Discovery for a *user-facing feature*.

The user provides a feature idea in natural language.

Your job is to:
1) clarify intent, scope, and success through structured questions
2) research existing product behavior only as needed
3) produce a concise, implementation-free behavioral specification saved to `PLAN.md`

This follows BDD Discovery principles: focus on *what* the system should do and *why*, 
not *how* to build it. The output will inform test scenario generation (Formulation phase).

CRITICAL OUTPUT CONSTRAINTS

The plan must:
- describe user-visible behavior, not technical implementation
- define rules, policies, and quality standards
- represent a resolved product decision, not a discussion draft

The plan must NOT include:
- file paths, function/class names, APIs, schemas, libraries, algorithms
- step-by-step engineering tasks or pseudo-code
- exhaustive scenario inventories or Given/When/Then test cases
- open questions or unresolved decisions

The plan MAY include:
- user goals and value proposition
- behavioral rules and constraints
- interaction patterns and user experience principles
- quality bars and acceptance criteria

INPUT
- Feature idea: $IDEA
- Optional context: $CONTEXT
- If $IDEA is empty, ask the user to describe the feature in one paragraph.

GENERAL PRINCIPLES

- Proportionality: keep the plan as small as the idea allows
- Decision-driven: include content that reduces uncertainty and ambiguity
- Behavior-focused: describe observable outcomes, not internal mechanics
- Artifact quality: this should be actionable without further clarification

WORKFLOW

PHASE 0 — Classify and assess clarity (no repo research yet)

- Restate the feature succinctly in your own words
- Identify the primary domain:
  - business/productivity (SaaS, tools, workflows)
  - entertainment/creative (games, art tools, media)
  - communication/social (messaging, collaboration)
  - content/information (publishing, documentation)
- Identify which perspectives are relevant (only if they affect requirements):
  - User Experience & Accessibility
  - Security & Abuse Prevention
  - Operations & Support
  - Data & Measurement
  - Business & Compliance
  - Platform / Ecosystem Impact
  - Performance & Reliability
- Determine whether any unanswered product decisions exist that would affect:
  - scope
  - user experience
  - behavior
  - acceptance criteria

If no blocking ambiguity exists, proceed to Phase 1.
If ambiguity exists, proceed to PHASE Q instead.

PHASE Q — Ask structured clarification questions (before planning)

Ask only questions necessary to finalize scope or behavior.

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

1. Should this feature apply to:
   a) all users
   b) only authenticated users
   c) only users with specific permissions

2. When a user performs this action, should existing data be:
   a) preserved
   b) modified
   c) deleted

Wait for the user's answers before continuing.

PHASE 1 — Targeted repo and product-context research

(Only after questions are resolved or if none were needed.)

Research only what is relevant to the feature.

- Identify the product/domain purpose via README and docs
- Summarize existing user-visible behavior related to this feature
- Note conceptual constraints or patterns (e.g., permissions, workflows, data visibility)
- If the feature interacts with existing features, describe those interactions

While researching:
- Summarize in plain language focused on user experience
- Avoid implementation descriptions
- Record assumptions explicitly

PHASE 2 — Product-level framing and design selection

If multiple reasonable product approaches exist:

- Compare 2–4 design directions
- Select one based on user goals, constraints, and impact
- Clearly state assumptions being made

If no real alternatives exist, proceed directly to planning.

PHASE 3 — Produce PLAN.md (implementation-free, no open questions)

Write `PLAN.md` in the repository root using the structure below.

# Plan: <short title>

## Summary
- 2–4 sentences describing intent and user-facing outcome

## Intent
**For business/productivity features:**
- What problem does this solve?
- Who is affected and why does it matter?

**For entertainment/creative features:**
- What experience does this create?
- What feeling or capability does it provide?

**For communication/social features:**
- What interaction does this enable?
- What user need does it address?

## Goals
- Bullet list of desired outcomes (user-focused, measurable where possible)

## Non-goals
- Explicit scope boundaries to prevent creep

## Users & Contexts
- Relevant user types or personas
- Key situations where this feature matters
- User mental models and expectations

## Initial State (if applicable)
*Include this section for interactive features, games, or stateful systems*
- Starting configuration or conditions
- Default values and initial presentation
- Entry points (how users access this feature)

## Behavioral Rules
*High-level rules that govern how the feature works*
- Core mechanics or policies
- Constraints and boundaries
- State transitions (if applicable: menu → active → paused → complete)

## Interaction Patterns
- How users engage with this feature
- Key user actions and system responses
- Control methods (if applicable: keyboard, mouse, touch, voice)
- Feedback mechanisms (visual, audio, haptic)

## User Experience Principles
- Interaction philosophy (e.g., direct manipulation, progressive disclosure)
- Accessibility expectations
- Content tone and messaging approach
- Error prevention and recovery
- Empty states and first-use experience
- Performance expectations (responsiveness, timing, fluidity)

## Requirements
*Bulleted, testable assertions of behavior and constraints*

Group by behavioral category, not by exhaustive scenarios:
- User capabilities (what users can do)
- System responses (what happens when)
- Data constraints (valid inputs/outputs)
- Permission requirements (who can access what)
- Integration points (interaction with existing features)

Stop when additional items would only be variations of existing requirements.

## Edge Cases & Boundary Conditions
*Categories of tricky situations and expected behavior*

Focus on:
- Boundary values (empty, zero, maximum, null)
- Timing issues (simultaneous actions, race conditions)
- State combinations (unusual but valid states)
- Error conditions (invalid input, failed dependencies)

Describe expected behavior conceptually, not specific test cases.

## Quality & Performance Expectations
- Response time targets (immediate, <1s, <5s)
- Reliability requirements (uptime, error rates)
- Scalability considerations (expected load, growth)
- Data accuracy requirements

## Risks & Tradeoffs
- Product risks (user confusion, workflow disruption)
- UX risks (learning curve, discoverability)
- Adoption risks (migration from old behavior)
- Technical constraints that affect user experience
- Known tradeoffs in the chosen approach

## Acceptance Criteria
*Checklist-style assertions indicating success*

Focus on user-observable outcomes:
- [ ] User can [perform key action]
- [ ] System [responds as expected] when [condition]
- [ ] [Boundary case] is handled appropriately
- [ ] Feature is accessible via [keyboard/screen reader/etc]
- [ ] Error states provide actionable guidance

Stop when additional items would only be scenario permutations.

Optional sections (include only if relevant):

## Integration with Existing Features
- How this feature composes with existing functionality
- Changes to existing user workflows
- Backward compatibility considerations

## Rollout Considerations
*Only if phased rollout or gradual adoption is relevant*
- Feature flags or progressive enablement
- User migration approach
- Success signals (usage metrics, user feedback themes)

FINAL STEP — Save the file

- Create or overwrite `PLAN.md` in the repo root
- After writing, confirm with:
  - "Wrote PLAN.md"
  - Brief summary of key behavioral rules and acceptance criteria
- Do not repeat the full file contents unless the user asks

TONE

- User-focused and behavior-oriented
- Clear bullets over long prose
- Practical about constraints and tradeoffs
- Avoid speculative technical detail

SPECIAL CONSIDERATIONS BY DOMAIN

**For games and entertainment software:**
- "Intent" describes the experience, not a problem being solved
- Include "Initial State" section (starting configuration)
- Emphasize timing, responsiveness, and "game feel" in UX Principles
- Behavioral Rules should include win/lose conditions if applicable
- Performance expectations include frame rate and input latency

**For creative and productivity tools:**
- Focus on enabling user capabilities and workflows
- Describe interaction patterns in detail (direct manipulation, modes, tools)
- Empty states and first-use experience are critical

**For social and communication features:**
- Privacy and visibility rules are critical behavioral requirements
- Describe notification and awareness mechanisms
- Include abuse prevention considerations

**For content and information features:**
- Describe discovery and navigation patterns
- Content organization principles matter
- Search and filtering behavior if applicable