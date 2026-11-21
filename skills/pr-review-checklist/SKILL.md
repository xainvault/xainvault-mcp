---
name: pr-review-checklist
description: Reviews a pull request or diff against a practical checklist covering correctness, tests, security, readability and documentation, then reports findings by severity. Use when the user asks for a code review, a PR review, a second pair of eyes on a diff, or a pre-merge check.
---

# PR Review Checklist

## Instructions

1. Read the pull request description first so you know what the change is meant
   to do. If there is no description, ask for one or infer the intent from the
   diff and state your assumption.
2. Read the whole diff before commenting. Open surrounding code when a change
   depends on context the diff does not show.
3. Work through the checklist below. Only report items where you found a real
   issue; do not pad the review.
4. Do not run, build or execute code from the pull request. Review it as text.

## Checklist

**Correctness**
- Does the code do what the description says?
- Are edge cases handled: empty input, missing values, large input, errors?
- Are there off-by-one errors, wrong comparisons or inverted conditions?
- Can concurrent calls or retries leave data in a bad state?

**Tests**
- Is new behavior covered by tests?
- Would the tests fail if the change were reverted?
- Are bug fixes accompanied by a test that reproduces the bug?

**Security**
- Is user input validated before use?
- Are secrets, tokens or credentials added to code, config or logs?
- Do new queries, shell commands or file paths include untrusted input?
- Do permission checks still apply on every new code path?

**Readability and maintenance**
- Are names clear and consistent with the rest of the codebase?
- Is there duplicated logic that already exists elsewhere?
- Is any dead code, debug output or commented-out code left behind?

**Documentation**
- Are user-facing changes reflected in the README, docs or changelog?
- Are new configuration options or environment variables documented?

## Output

Group findings by severity:

- **Blocking**: bugs, security problems or data loss risks. Must be fixed.
- **Should fix**: missing tests, unclear behavior, likely future bugs.
- **Nit**: style and naming suggestions. Optional.

For each finding give the file and line, what is wrong, and a concrete
suggestion. End with a one-line verdict: approve, approve with nits, or request
changes. If you found nothing, say so plainly.
