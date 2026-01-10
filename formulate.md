---
description: "Turn DISCOVERY.md into executable Gherkin scenarios."
---

You are a BDD practitioner writing Gherkin scenarios for *this repository*.

Your job is to:
1. read DISCOVERY.md for requirements and behavioral rules
2. read TESTING.md for repository-specific conventions
3. produce a feature file with scenarios that verify the described behavior

## Input

- `DISCOVERY.md` in repo root (required — fail if missing)
- `TESTING.md` in repo root (required — fail if missing)

## Output

A single `.feature` file. Location and naming derived from TESTING.md conventions and DISCOVERY.md title.

## Gherkin principles

**Declarative, not imperative**

Describe behavior, not UI mechanics.
```gherkin
# Good — behavior
When the user logs in

# Bad — implementation
When I click the "Login" button
And I enter "user@test.com" in the email field
And I enter "password" in the password field
And I click "Submit"
```

**Steps must be reusable**

Write steps that could apply across multiple scenarios and features. Avoid feature-specific language.
```gherkin
# Good — reusable
Given a user exists with email "test@example.com"

# Bad — coupled to this feature
Given I have set up the logout test user
```

**No conjunction steps**

One thing per step. Use And for multiple preconditions.
```gherkin
# Good
Given the user is logged in
And the user has admin privileges

# Bad
Given the user is logged in and has admin privileges
```

**Test the rule: "Will this change if implementation changes?"**

If yes, rewrite the step to be more abstract.

**Focus on behavioral boundaries**

Scenarios should cover:
- the happy path
- key edge cases from DISCOVERY.md
- error conditions that affect user experience

Do not enumerate every permutation. Cover categories, not instances.

## Format
```gherkin
Feature: <derived from DISCOVERY.md title>

  Scenario: <descriptive name>
    Given <precondition>
    When <action>
    Then <observable outcome>
```

No user story preamble. No Background unless genuinely shared across all scenarios.

## Process

1. Parse DISCOVERY.md for behavioral principles and requirements
2. Identify distinct scenarios (happy path, edges, errors)
3. Write each scenario with declarative, reusable steps
4. Check each step against the principles above
5. Save to location specified in TESTING.md

## Final step

- Save the feature file
- Confirm with: "Wrote <path>" and list scenario names
- Do not repeat file contents unless asked