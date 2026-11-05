# Issue tracker: GitHub

Issues and PRDs for this repo live as GitHub issues on the fork **`djsteilss/sandcastle`**, not on upstream `mattpocock/sandcastle`. Use the `gh` CLI for all operations.

Remotes: `origin` is the fork, `upstream` is `mattpocock/sandcastle`. `gh repo set-default` points at the fork, but pass `--repo djsteilss/sandcastle` on any write (create, comment, label, close) so nothing lands on upstream by accident.

## Conventions

- **Create an issue**: `gh issue create --title "..." --body "..."`. Use a heredoc for multi-line bodies.
- **Read an issue**: `gh issue view <number> --comments`, filtering comments by `jq` and also fetching labels.
- **List issues**: `gh issue list --state open --limit 100 --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'` with appropriate `--label` and `--state` filters. Keep `--limit` high (default `gh` returns only 30) so the full backlog is visible.
- **Comment on an issue**: `gh issue comment <number> --body "..."`
- **Apply / remove labels**: `gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- **Close**: `gh issue close <number> --comment "..."`

The target repo is always `djsteilss/sandcastle`. Never create issues, comments or labels on `upstream`.

## Pull requests as a triage surface

**PRs as a request surface: no.** This is a personal fork, so outside PRs go to upstream, not here. If that changes, flip this to `yes`; `/triage` will then pull external PRs into the same queue as issues, using the `gh pr` equivalents:

- **Read a PR**: `gh pr view <number> --comments` and `gh pr diff <number>` for the diff.
- **List external PRs for triage**: `gh pr list --state open --json number,title,body,labels,author,authorAssociation,comments` then keep only `authorAssociation` of `CONTRIBUTOR`, `FIRST_TIME_CONTRIBUTOR`, or `NONE` (drop `OWNER`/`MEMBER`/`COLLABORATOR` — collaborators' in-flight PRs are left alone).
- **Comment / label / close**: `gh pr comment`, `gh pr edit --add-label`/`--remove-label`, `gh pr close`.

GitHub shares one number space across issues and PRs, so a bare `#42` may be either — resolve with `gh pr view 42` and fall back to `gh issue view 42`.

## When a skill says "publish to the issue tracker"

Create a GitHub issue.

## When a skill says "fetch the relevant ticket"

Run `gh issue view <number> --comments`.
