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

## language
- everything written to git or github is in english, whatever language the conversation is in: commit messages, pull request titles and descriptions, and squash commit titles and bodies.

## execution: delegate to haiku
- you write every message and proposal yourself (branch, commit messages, pr title and description, squash title and body); never delegate the thinking.
- after approval, do not run the commands yourself: launch one `Agent` call with `model: "haiku"` whose prompt contains:
  - the working directory.
  - the exact, final commands in order, passing messages via heredoc (`git commit -F -`, `gh pr create --body-file -`, `gh pr merge --body-file -`) so the text is never re-quoted or rewritten.
  - the rules: run sequentially; do not change any message, branch name or file list; do not retry with different flags or try to fix anything; stop on the first failure.
  - what to report: each command's exit status, the full output of a failing command, and the pr url and number when created.
- relay the result to the user. on failure, diagnose it yourself and re-propose; the agent never improvises.

## publishing: one proposal, one approval
1. run the validation commands the repo's CLAUDE.md requires. stop and report if any fails.
2. in a single message, propose everything at once:
   - branch name (if not already on one)
   - files to stage
   - commit message(s)
   - push target
   - pull request title (same convention as commits) and description against `main`
3. wait for one approval. if the user asks for changes, apply them and re-propose the full set.
4. after approval, without asking again, delegate to the haiku agent (see execution): `git switch -c`, `git add <files>`, `git commit`, `git push -u`, `gh pr create`.

## merge
- once the pr is created and the work is done, proactively propose the merge without waiting to be asked: in a single message, give the squash commit title (same convention as commits, ending with the pr number as ` (#<n>)`) and body, built from the pr's commits and description.
- the `(#<n>)` suffix is mandatory, so every commit on `main` links back to its pr. `gh pr merge --subject` replaces github's default title, which is what adds the suffix, so always write it explicitly.
- the user only approves or asks for changes; if they ask for changes, apply them and re-propose.
- only after the user approves, delegate to the haiku agent (see execution) in one call: `gh pr merge --squash --delete-branch --subject "<title> (#<n>)" --body-file -` (so the remote branch is deleted with the merge), then `git switch main && git pull --prune` to update the local repo.
