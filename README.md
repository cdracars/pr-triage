# PR Triage Codex Skill

`pr-triage` provides a read-only, current status report for the authenticated GitHub user's open pull requests across repositories. It classifies CI, merge, and review state while retaining every active blocker.

## Install

In Codex, install this skill from this repository, then invoke it with `$pr-triage` or ask for PR status, CI, mergeability, conflicts, or review-state triage.

## Safety

This skill is strictly read-only. It never modifies GitHub pull requests or local repositories.

See [SKILL.md](SKILL.md) for the complete workflow and safety boundary.
