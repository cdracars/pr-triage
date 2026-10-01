---
name: pr-triage
description: "Read-only triage of the current user's open GitHub pull requests across repositories. Use for PR status, CI, mergeability, conflicts, review state, or a cross-repository PR report; never alter GitHub or local Git state."
---

# PR Triage

Produce an accurate, current status report for the authenticated GitHub user's open pull requests. Preserve every active blocker per PR; the primary category is only a sorting aid.

## Discover

Use GitHub CLI search to discover open PRs authored by the current user. Request only fields supported by `gh search prs`: repository, PR number, title, URL, and draft status. Use a limit high enough to avoid silently omitting PRs.

Do not treat draft PRs as actionable. Report their count separately.

## Enrich

For every non-draft PR, collect its PR detail, including the check rollup. This supplies branch, review, mergeability, and CI fields that search does not expose. Run at most eight detail queries concurrently.

- `gh pr view <number> --repo <owner/repo> --json mergeStateStatus,mergeable,headRefName,baseRefName,reviewDecision,statusCheckRollup`

Use `gh pr checks <number> --repo <owner/repo> --json name,state,conclusion` only when the detail response does not include a usable check rollup.

Record unavailable or incomplete GitHub data explicitly. An empty check rollup means no checks were returned; it does not itself establish whether checks are required.

## Classify

Record independent facts before assigning a category:

- CI: `PASSING`, `FAILING`, `PENDING`, `NONE_RETURNED`, or `UNAVAILABLE`. A PR may be both failing and pending when checks have mixed states.
- Merge: `CONFLICTING`, `BEHIND`, `CLEAN`, `BLOCKED`, or `UNKNOWN` from GitHub's merge fields.
- Review: `CHANGES_REQUESTED`, `REVIEW_REQUIRED`, `APPROVED`, or `NOT_REPORTED`. Do not treat `NOT_REPORTED` as a missing approval.

Create a `blockers` list from every active condition: conflicts, failed checks, pending checks, requested changes, required review, behind base, and unknown or unavailable mergeability. A PR can have multiple blockers.

Assign one primary category only for grouping, in this order:

1. `CONFLICTING`
2. `CI_FAILING`
3. `CHANGES_REQUESTED`
4. `PENDING_CI`
5. `BEHIND_BASE`
6. `NEEDS_REVIEW`
7. `READY` — mergeable and clean, with no failed or pending checks and no reported review blocker.
8. `BLOCKED` — any remaining indeterminate or non-mergeable state.

The primary category never suppresses facts in `blockers`.

## Report

Return a readable table with repository and PR, title, CI summary, merge state, review state, blockers, primary category, and a short next action. Include totals by category and blocker type. Link each PR where possible.

Mention drafts separately. Keep recommendations clearly distinct from facts.

## Safety Boundary

This skill is strictly read-only. Do not, unless the user makes a separate explicit request:

- merge, close, or edit a pull request;
- check out, rebase, merge, reset, or otherwise alter a local repository;
- commit, push, force-push, delete branches, or modify GitHub state;
- approve, comment on, request review for, rerun, cancel, or wait/poll CI checks.

If the user asks to take an action after triage, obtain clear scope for the specific PR or set of PRs, then use the relevant workflow outside this skill.
