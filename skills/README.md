# Example skills

Small, general-purpose skills you can use to try Xainvault. Each one is a
folder with a `SKILL.md` manifest.

| Skill | What it does |
| --- | --- |
| [`commit-message-writer`](commit-message-writer/SKILL.md) | Writes Conventional Commits messages from a staged diff. |
| [`pr-review-checklist`](pr-review-checklist/SKILL.md) | Reviews a pull request against a correctness, tests, security and docs checklist. |
| [`release-notes-drafter`](release-notes-drafter/SKILL.md) | Turns merged pull requests or commits into user-facing release notes. |

They contain instructions only: no scripts, no network access and no
credentials.

## Skill format

```text
release-notes-drafter/
  SKILL.md
  references/
    template.md
```

`SKILL.md` starts with YAML frontmatter that names the skill and describes
when to use it, followed by the instructions:

```markdown
---
name: commit-message-writer
description: Writes clear Conventional Commits messages from staged changes or a described change. Use when the user asks to write, improve or review a commit message ...
---

# Commit Message Writer

## Instructions
...
```

Clients read the `description` to decide when the skill applies, so it should
say what the skill does and when to use it. Longer material, such as templates
or reference docs, goes in a `references/` folder next to `SKILL.md` and is
read only when needed. The examples here keep folder names and `name` values in
lowercase kebab-case and matching.

## Adding a skill to your vault

In Xainvault, open **Skills → Import** and choose one of:

- **Upload** a folder or ZIP containing the `SKILL.md`.
- **Scan a public GitHub repository**. The vault finds the skills inside and
  imports them with their provenance attached. You can point it at this
  repository to import the examples.
- **Marketplace**: discover a public skill, review its package and add it as a
  private Draft.

You can also ask a connected MCP client (Claude, ChatGPT, Claude Code, Codex
and others) to write a skill and save it to Xainvault. That creates a Draft; it
does not make the version Ready.

Every path creates a **Draft**. Importing always creates a new skill: if the
name matches a skill that already exists, the import is rejected rather than
overwriting it.

## How versions work

Every change to a skill produces a new version that moves through explicit
states:

| State | Meaning |
| --- | --- |
| **Draft** | Just imported or edited. Visible in the vault. |
| **Review** | Submitted for approval in a Team Vault. Waits for a reviewer. |
| **Ready** | Approved and served to agents. |
| **Superseded** | Replaced by a newer Ready version. Kept for history. |

To change an existing skill, open it and use **New version**. The editor opens
with the latest version's `SKILL.md` and package, and the uploaded package must
share the skill's name. A skill keeps one open Draft at a time: uploading again
while a Draft is open updates that Draft, and while a version is in review new
uploads are blocked until the reviewer decides.

Before a Draft can become Ready, Xainvault runs a security scan on the exact
package:

- **Personal Vault**: the owner releases the Draft after the automated scan.
  Medium risk requires an acknowledgement note.
- **Team and Enterprise Vaults**: the same automated scan runs first, then the
  version waits in **Reviews** until an assigned reviewer approves it. Only then
  does it become Ready.
- High or critical risk blocks release in every vault type.

## What reaches your agents

Only **Ready** versions are distributed to agents, and only to the people,
groups or whole vault they are granted to. A newer Ready version supersedes the
previous one without rewriting history, and the name and description agents
see come from the Ready version, so editing a Draft does not change what agents
see until it is approved.

The one exception is testing: an author may explicitly test their own current
Draft from a connected client. It is marked **Unreviewed**, stays private to
that author and is never included in collections or update checks.

Further reading:
[Skills & versions](https://xainvault.com/docs/skills-and-versions) ·
[Skill security](https://xainvault.com/docs/skill-security) ·
[Connect through MCP](https://xainvault.com/docs/connect-agents)
