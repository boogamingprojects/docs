# Git Workflow Documentation

## What is Git?

Git is a version control system. It keeps track of changes in your code, so you can:

- work together with other people;
- see what changed;
- go back to older versions;
- create separate branches for new work;
- review code before it is merged.

---

## Basic Git Terms

| Term | Meaning |
|---|---|
| Repository / repo | A project folder that Git tracks |
| Commit | A saved change in Git |
| Branch | A separate version of the code |
| Remote | The online version of the repo, for example GitHub, GitLab or Forgejo |
| Pull | Download the latest changes from the remote |
| Push | Upload your local commits to the remote |
| Pull request / PR | A request to merge your branch into another branch |
| Merge | Combine changes from one branch into another |
| Conflict | When Git cannot automatically combine changes |

---

## Common Branches

Most projects use branches like this:

| Branch | Purpose |
|---|---|
| `main` / `master` | Stable production code |
| `staging` | Code that is being tested before production |
| `prd` | Production branch, if used |
| `feature/...` | Branch for new features |
| `fix/...` | Branch for bug fixes |
| `docs/...` | Branch for documentation changes |

Example feature branch:

```bash
git checkout -b feature/add-login-page
```

Example fix branch:

```bash
git checkout -b fix/login-error
```

Do not work directly on `main`, `staging` or `prd` unless the team specifically allows it.

---

## Normal Git Workflow

This is the usual workflow when working on a task.

### 1. Start from the correct branch

First, switch to the branch you need to work from.

Example:

```bash
git checkout staging
```

Then pull the latest changes:

```bash
git pull
```

This makes sure your local branch is up to date.

---

### 2. Create a new branch

Create a new branch for your task.

```bash
git checkout -b feature/my-new-feature
```

Example:

```bash
git checkout -b feature/add-read-only-db-user
```

---

### 3. Make your changes

Edit the code, documentation or configuration files.

After making changes, check what changed:

```bash
git status
```

You can also see the exact changes:

```bash
git diff
```

---

### 4. Add your files

Add all changed files:

```bash
git add .
```

Or add a specific file:

```bash
git add README.md
```

---

### 5. Commit your changes

A commit saves your changes locally.

```bash
git commit -m "feat: add read-only database user"
```

Good commit examples:

```text
feat: add Superset read-only database user
fix: correct SolarWinds log size handling
docs: update Gatus documentation
refactor: simplify database migration logic
test: add migration tests
```

---

## Commit Message Scheme

A common commit message scheme is:

```text
type: short description
```

Common types:

| Type | Use for |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation changes |
| `refactor` | Code cleanup without changing behavior |
| `test` | Adding or changing tests |
| `chore` | Maintenance tasks |
| `style` | Formatting only |

Examples:

```bash
git commit -m "docs: add Git workflow documentation"
git commit -m "fix: handle missing environment variables"
git commit -m "refactor: simplify log forwarding script"
```

---

## Push Your Branch

After committing, push your branch to the remote.

```bash
git push origin feature/my-new-feature
```

If this is the first time pushing the branch, Git may suggest this command:

```bash
git push --set-upstream origin feature/my-new-feature
```

You can also use the shorter version:

```bash
git push -u origin feature/my-new-feature
```

After this, future pushes can usually be done with:

```bash
git push
```

---

## Pull Requests

A pull request is used to ask if your branch can be merged into another branch.

Example:

```text
feature/add-read-only-db-user -> staging
```

This means:

> Merge my feature branch into the staging branch.

A pull request is useful because other people can:

- review your code;
- leave comments;
- approve or reject the changes;
- check if tests pass;
- discuss the changes before merging.

---

## Creating a Pull Request

Usually you do this in GitHub, GitLab or Forgejo.

Steps:

1. Push your branch.
2. Open the repository in the browser.
3. Click **New Pull Request** or **Create Pull Request**.
4. Select your branch as the source branch.
5. Select the target branch, for example `staging`.
6. Add a clear title.
7. Add a short description of what changed.
8. Create the pull request.

Example PR title:

```text
Add Superset read-only database user
```

Example PR description:

```markdown
## What changed

- Added a read-only database user for Superset.
- Added environment variables for the Superset database user and password.
- Updated documentation.

## Why

Superset should not connect to the database with a full-access user.
Using a read-only user limits damage if Superset is compromised.

## Testing

- Ran migration locally.
- Verified that the user can read tables.
- Verified that the user cannot write to tables.
```

---

## Updating Your Pull Request

When someone asks you to change something in your PR:

1. Make the changes locally.
2. Add the changed files.
3. Commit again.
4. Push the same branch.

Example:

```bash
git add .
git commit -m "fix: update Superset database permissions"
git push
```

The pull request updates automatically.

You do not need to create a new pull request.

---

## Merging

Merging means combining one branch into another.

Example:

```text
feature/add-read-only-db-user -> staging
```

After the PR is approved, the branch can be merged.

Usually this is done with a button in GitHub, GitLab or Forgejo:

```text
Merge pull request
```

---

## Merge Types

There are different ways to merge.

### 1. Merge Commit

This keeps all commits and adds one extra merge commit.

Good when you want to keep the full history.

Example result:

```text
Merge branch 'feature/add-read-only-db-user' into staging
```

---

### 2. Squash Merge

This combines all commits from the branch into one commit.

Good when your branch has many small commits like:

```text
fix typo
test
small change
try again
```

After squash merge, the target branch gets one clean commit.

Example:

```text
feat: add Superset read-only database user
```

---

### 3. Rebase and Merge

This replays your commits on top of the target branch.

It creates a cleaner history, but can be more confusing for beginners.

Use this only if your team prefers it.

---

## Pulling Latest Changes

Before starting work, always pull:

```bash
git pull
```

If you are on your feature branch and want the latest changes from `staging`, you can do this:

```bash
git checkout staging
git pull
git checkout feature/my-new-feature
git merge staging
```

This brings the latest `staging` changes into your feature branch.

---

## Merge Conflicts

A merge conflict happens when Git cannot decide which version of a file is correct.

Example conflict:

```text
<<<<<<< HEAD
old version
=======
new version
>>>>>>> feature/my-branch
```

You need to manually choose what the file should look like.

After fixing the file:

```bash
git add .
git commit
```

If Git already created a merge commit message, you can usually just save and close the editor.

---

## Rebase

Rebase moves your branch on top of another branch.

Example:

```bash
git checkout feature/my-new-feature
git rebase staging
```

This means:

> Take my feature branch and place it on top of the latest staging branch.

Rebase can make history cleaner, but it can also be risky if you do not know what you are doing.

Do not rebase shared branches like:

```text
main
staging
prd
```

Also be careful rebasing a branch that other people are also using.

---

## Force Push

Sometimes after a rebase, Git asks you to force push.

Do not use this unless you understand why.

If you need to force push, use:

```bash
git push --force-with-lease
```

Avoid this:

```bash
git push --force
```

`--force-with-lease` is safer because it checks if someone else pushed changes before overwriting the remote branch.

Never force push to shared branches like:

```text
main
staging
prd
```

---

## Stashing Changes

If you have local changes but need to switch branches, use stash.

Save your changes temporarily:

```bash
git stash
```

Switch branch:

```bash
git checkout staging
```

Bring your changes back:

```bash
git stash pop
```

This is useful when Git says you cannot switch branches because you have uncommitted changes.

---

## Undoing Changes

### Undo changes in one file

```bash
git checkout -- filename
```

Example:

```bash
git checkout -- README.md
```

### Undo all local uncommitted changes

Be careful: this deletes your local changes.

```bash
git reset --hard
```

### Remove untracked files

Be careful: this deletes files Git does not track.

```bash
git clean -fd
```

---

## Useful Commands

### Check current branch and changes

```bash
git status
```

### Show branches

```bash
git branch
```

### Switch branch

```bash
git checkout branch-name
```

Example:

```bash
git checkout staging
```

### Create new branch

```bash
git checkout -b feature/my-feature
```

### Pull latest changes

```bash
git pull
```

### Push current branch

```bash
git push
```

### Push new branch for the first time

```bash
git push --set-upstream origin branch-name
```

### See commit history

```bash
git log --oneline
```

### See remote URL

```bash
git remote -v
```

---

## Recommended Daily Workflow

Use this flow for most tasks:

```bash
git checkout staging
git pull
git checkout -b feature/my-task
```

Make your changes.

```bash
git status
git add .
git commit -m "feat: add my task"
git push --set-upstream origin feature/my-task
```

Then create a pull request:

```text
feature/my-task -> staging
```

After the PR is approved, merge it.

---

## Working With PyCharm

In PyCharm you can do most Git actions from the UI.

### Change branch

Bottom-right corner:

```text
Current branch name -> choose another branch
```

### Commit

Left menu:

```text
Commit -> select files -> write commit message -> Commit
```

### Push

After committing:

```text
Git -> Push
```

Or use the push button in the Git window.

### Pull

Top menu:

```text
Git -> Pull
```

### Create Pull Request

Usually this is done in GitHub, GitLab or Forgejo in the browser.

---

## Good Rules to Follow

1. Always pull before starting new work.
2. Do not work directly on `main`, `staging` or `prd`.
3. Use a separate branch for every task.
4. Write clear commit messages.
5. Push your branch and create a pull request.
6. Let someone review your pull request before merging.
7. Be careful with `rebase`.
8. Be very careful with force push.
9. Never force push to shared branches.
10. When unsure, run:

```bash
git status
```

---

## Example Full Workflow

```bash
git checkout staging
git pull
git checkout -b docs/add-git-workflow
```

Make changes.

```bash
git add .
git commit -m "docs: add Git workflow documentation"
git push --set-upstream origin docs/add-git-workflow
```

Create a pull request:

```text
docs/add-git-workflow -> staging
```

After review and approval, merge the pull request.

---

## Simple Explanation

A good way to think about Git is:

```text
branch = your own workspace
commit = save point
push = upload your save points
pull = download latest changes
pull request = ask to add your changes to the main code
merge = accept and combine the changes
conflict = Git needs help choosing the correct version
```
