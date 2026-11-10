---
name: git-workflow
description: Use this skill whenever you create a branch, write a commit message, open or update a pull request, rebase, or merge in this repo. Covers branch naming, Conventional Commits, splitting work into small commits, recording what was and wasn't tested, rebase-only merging, and cleaning up after a merge.
---

## Skill: Branches, Commits, Pull Requests and Merging

**Trigger:** Any git or GitHub work that will reach `main`: a new branch, a commit, a PR, a rebase or a merge.

GitHub doesn't enforce these conventions for this repo (see "Repo settings" below), so follow them by hand.

---

## Branches

- Branch from the latest `origin/main`: `git fetch origin && git switch -c <type>/<short-description> origin/main`
- Name branches `<type>/<kebab-case-description>`, using a Conventional Commits type, e.g. `fix/benchmark-done-marker`, `refactor/migrate-to-uv-only`, `docs/contributing-workflow`.
- Prefer a separate git worktree per branch over switching branches in a checkout that has uncommitted work.

## Commits

- **Conventional Commits:** `<type>(<optional scope>): <imperative summary>`, e.g. `fix(jobs): sync conversion jobs into their own torch-free environment`.
  - Types: `build`, `chore`, `ci`, `docs`, `feat`, `fix`, `perf`, `refactor`, `revert`, `style`, `test`.
  - Scopes in use: `deps`, `jobs`, `experiments`, `data`, `training`, `config`, `benchmark`.
- **One logical change per commit.** Split a change into commits that each make sense alone (e.g. dependency change, then the scripts that use it, then the docs), rather than one large commit.
- **The body explains why,** not just what: the problem, the fix, and anything a reader of `git log` would need.
- **Record testing in the commit it applies to,** because the rebase merge copies each commit to `main` as written, so `git log` is the lasting record:
  - what was tested and where (machine, GPU, driver)
  - what wasn't tested, and why
  - known limits or follow-up checks someone should run before relying on the change
- Keep the attribution trailer (e.g. `Co-Authored-By:`) as the last lines of the message.

## Pull requests

- **Title:** the same Conventional Commits format as a commit.
- **Description sections:** Summary, Changes, Testing (a table of check → result), Not tested, and Notes for reviewers. Keep it in step with the branch as it changes.
- **Overlapping PRs:** if another open PR touches the same lines, say so in both descriptions.

## Merging

- **Rebase and merge only.** Never squash or create a merge commit: both lose or wrap the individual Conventional Commits.
  ```bash
  gh pr merge <number> --rebase --delete-branch
  ```
- **Bring a branch up to date** by rebasing it onto `main`, never by merging `main` into it:
  ```bash
  git fetch origin && git rebase origin/main
  git push --force-with-lease=<branch>:<sha-you-last-pushed> origin <branch>
  ```
  Use `--force-with-lease` with the expected SHA, never plain `--force`.
- **Resolve conflicts commit by commit** so every commit in the rebased branch still contains both sides' changes, then rerun the tests.
- Merging changes to the environment, such as dependencies, Python version or CUDA build, affects everyone's runs. Tell the team before merging.

## After merging

- Delete the remote branch (`--delete-branch` does this) and the local branch and worktree:
  ```bash
  git worktree remove <path>
  git branch -D <branch>   # -d refuses: rebase merging gives commits new IDs
  git switch main && git pull --ff-only
  ```
- Before `git branch -D`, confirm the PR shows as merged on GitHub.

## Repo settings

These would enforce the rules above but need admin access to `bold-lab-ai/proj-lom`. Until an admin sets them, follow the rules by hand:

- Settings → General → Pull Requests: allow **rebase merging** only (untick merge commits and squash merging).
- Settings → General → Pull Requests: tick **Automatically delete head branches**.
