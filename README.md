# PR Triage Codex Skill

`pr-triage` provides a read-only, current status report for the authenticated GitHub user's open pull requests across repositories. It classifies CI, merge, and review state while retaining every active blocker.

## Install

In a Codex chat, ask the built-in installer:

> `$skill-installer install the skill from https://github.com/cdracars/pr-triage/tree/main/skills/pr-triage`

The skill will be available in your next turn. Invoke it with `$pr-triage`, or ask for PR status, CI, mergeability, conflicts, or review-state triage.

For a manual installation, copy `skills/pr-triage/` into `~/.codex/skills/pr-triage/`.

## Safety

This skill is strictly read-only. It never modifies GitHub pull requests or local repositories.

See [the skill instructions](skills/pr-triage/SKILL.md) for the complete workflow and safety boundary.

## License

This project is licensed under the [MIT License](LICENSE).
