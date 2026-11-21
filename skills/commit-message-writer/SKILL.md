---
name: commit-message-writer
description: Writes clear Conventional Commits messages from staged changes or a described change. Use when the user asks to write, improve or review a commit message, mentions conventional commits, or is about to commit and wants a summary of the diff.
---

# Commit Message Writer

## Instructions

1. Look at what is being committed. Prefer the staged diff (`git diff --staged`).
   If nothing is staged, ask the user which changes belong in this commit.
2. If the diff mixes unrelated changes, say so and suggest splitting it into
   separate commits before writing anything.
3. Pick one type:
   - `feat`: a new user-facing capability
   - `fix`: a bug fix
   - `docs`: documentation only
   - `refactor`: a code change that neither fixes a bug nor adds a feature
   - `test`: adding or correcting tests
   - `perf`: a performance improvement
   - `build` or `ci`: build system, dependencies or CI configuration
   - `chore`: maintenance that fits none of the above
4. Add a scope in parentheses only when the repository already uses scopes, and
   reuse an existing scope name.
5. Write the subject line:
   - imperative mood ("add", not "added" or "adds")
   - lowercase after the colon, no trailing period
   - 72 characters or fewer
6. Add a body only when the reason for the change is not obvious from the
   subject. Explain why, not how. Wrap lines at 72 characters.
7. If the change breaks existing behavior, add `!` after the type or scope and a
   `BREAKING CHANGE:` footer describing what users must do.
8. Reference issues in a footer (for example `Refs: #123`) only when the user
   gives you the issue number. Never invent one.

## Output

Return only the commit message in a code block, followed by one sentence
explaining the chosen type. Do not run `git commit` unless the user asks.

## Examples

```text
fix(auth): reject expired session tokens

Expired tokens were accepted until the next server restart because the
expiry check compared against a cached clock value.
```

```text
docs: add Windsurf setup to the connection guide
```
