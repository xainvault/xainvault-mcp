---
name: release-notes-drafter
description: Drafts user-facing release notes from a list of merged pull requests, commits or a changelog, grouped into new features, improvements, fixes and breaking changes. Use when the user asks to write release notes, a changelog entry, a version announcement, or to summarize what changed between two tags.
---

# Release Notes Drafter

## Instructions

1. Collect the changes. Use what the user provides: a list of pull requests,
   commit messages, or a range such as `git log v1.2.0..v1.3.0 --oneline`.
   Ask for the version number and release date if they are not given.
2. Drop changes that do not affect users: internal refactors, CI, test-only
   and dependency bumps without a visible effect. Mention them only if the user
   asks for a full changelog.
3. Group the remaining changes using the template in
   `references/template.md`. Leave out any empty section.
4. Rewrite each item for the reader, not the developer:
   - start with what the user can now do or what was fixed
   - one line per item, plain language, no internal ticket jargon
   - keep pull request or issue references if the source includes them
5. Put breaking changes first and say exactly what users must change.
6. Only describe changes that appear in the input. Do not guess at features,
   numbers or dates.

## Output

Return the notes in Markdown, ready to paste into a release page. After the
notes, list any items you were unsure how to classify so the user can check
them.
