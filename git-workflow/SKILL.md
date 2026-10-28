---
name: git-workflow
description: git and github workflow rules: branching, conventional commit messages, approval gates for commit/push/pr/merge, staging. use before any git commit, push, branch creation, pull request or merge.
---

# git workflow

## branches
- never commit to the default branch (`main`): branch from it as `<type>/<topic>` (e.g. `feat/bat`).

## commits
- conventional commits in english lowercase: `<type>(<scope>): <summary>`. the repo's CLAUDE.md defines valid scopes.
- stage files by name; do not use `git add -A` or `git add .`.
- never force-push, rewrite pushed history, or skip hooks.

## publishing: one proposal, one approval
1. run the validation commands the repo's CLAUDE.md requires. stop and report if any fails.
2. in a single message, propose everything at once:
   - branch name (if not already on one)
   - files to stage
   - commit message(s)
   - push target
   - pull request title (same convention as commits) and description against `main`
3. wait for one approval. if the user asks for changes, apply them and re-propose the full set.
4. after approval, run sequentially without asking again: `git switch -c`, `git add <files>`, `git commit`, `git push -u`, `gh pr create`. stop and report on the first failure.

## merge
- merging is a separate step: propose the squash commit title and body; run `gh pr merge --squash --delete-branch` only after the user approves, so the remote branch is deleted with the merge.
- then update the local repo with `git switch main && git pull --prune`.
