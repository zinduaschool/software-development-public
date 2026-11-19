# Advanced Git & GitHub

[← Day 4 overview](README.md)

1. [Setup check](#1-setup-check)
2. [Branching and merging](#2-branching-and-merging)
3. [Merge conflicts](#3-merge-conflicts)
4. [Undoing changes and stash](#4-undoing-changes-and-stash)
5. [Remotes, pull and rebase](#5-remotes-pull-and-rebase)
6. [Pull requests and code review](#6-pull-requests-and-code-review)
7. [Team workflow and repo settings](#7-team-workflow-and-repo-settings)

## 1. Setup check

Get every machine ready before teaching anything. One broken login stalls the whole room.

### Check the Git version

```bash
git --version
# git version 2.43.0
```

Needs **2.23 or newer**: `git switch` and `git restore`, used all lesson, arrived in 2.23.

### Tell Git who you are

```bash
git config --global user.name "Brian Otieno"
git config --global user.email "brian@example.com"
```

Every commit is stamped with this name and email. Use the email on your GitHub account so commits link to your profile.

### Start new repos on main

```bash
git config --global init.defaultBranch main
```

Without this, older setups call the first branch `master`, which won't match GitHub.

### Use VS Code for messages

```bash
git config --global core.editor "code --wait"
```

`git commit` without `-m`, `git merge` and `git rebase -i` open an editor. This makes it VS Code instead of Vim. `--wait` makes Git wait until you close the tab.

### Add a log shortcut

```bash
git config --global alias.lg "log --oneline --graph --decorate --all"
```

`git lg` shows every branch as a graph, one line per commit. Section 5 uses it.

## 2. Branching and merging

*Example: add a navbar to the Team page without touching the working version on `main`.*

### See what a branch is

```bash
git branch
# * main
```

A branch is a label pointing at a commit, not a copy of your files. The `*` marks `HEAD`, the branch you're on. Each new commit moves that label forward.

### Create a branch and switch to it

```bash
git switch -c feature/navbar
# Switched to a new branch 'feature/navbar'

# older way, same result
git checkout -b feature/navbar
```

`-c` creates the branch where you are now and moves you onto it. Name branches `type/issue-number-short-name`: `feature/12-navbar`, `fix/15-footer-link`.

### Commit on the branch

```bash
# edit index.html and add the <nav>
git add index.html
git commit -m "feat: add navbar"
```

This commit exists only on `feature/navbar`. `main` hasn't moved.

### Go back to main

```bash
git switch main
```

Your files change back to how `main` looks. The navbar isn't lost, it's on the other branch. Show this in the browser, it surprises people.

### Merge with a fast-forward

```bash
git merge feature/navbar
# Updating 3f2a1c4..8b7d9e2
# Fast-forward
```

`git merge` brings a branch's commits into the branch you're on. `main` had no new commits, so Git just slides the `main` label forward. No merge commit.

### Merge when both sides changed

```bash
git switch -c feature/footer
# add a footer, commit
git switch main
# fix a typo in the <h1>, commit
git merge feature/footer
# Merge made by the 'ort' strategy.
```

Both branches moved, so Git creates a merge commit that joins them. The editor opens with a default message: save and close it.

### Delete merged branches

```bash
git branch -d feature/navbar
# Deleted branch feature/navbar (was 8b7d9e2).
```

Removes the label once its work is merged. `-d` refuses if the branch isn't merged yet, so it can't lose work.

## 3. Merge conflicts

*Example: two branches change the same `<h1>` on the Team page.*

### Create the conflict

```bash
git switch -c feature/title
# change <h1> to "Meet the Team", commit
git switch main
# change <h1> to "Our Team", commit
git merge feature/title
# CONFLICT (content): Merge conflict in index.html
# Automatic merge failed; fix conflicts and then commit the result.
```

Both sides edited the same line, so Git stops and asks you to choose. Nothing is broken; the merge is just paused.

### See which files need fixing

```bash
git status
# both modified:   index.html
```

Every file under "both modified" has to be resolved before the merge can finish.

### Read the markers

```text
<<<<<<< HEAD
<h1>Our Team</h1>
=======
<h1>Meet the Team</h1>
>>>>>>> feature/title
```

The top half (`HEAD`) is the branch you're on. The bottom half is the branch coming in. `=======` splits them.

### Resolve and finish

```html
<h1>Meet Our Team</h1>
```

```bash
git add index.html
git commit
```

Edit the file to the version you want (one side, the other, or a mix) and delete all three marker lines. `git add` marks the file resolved, `git commit` completes the merge.

### Use the VS Code merge editor

Above each conflict VS Code shows **Accept Current / Accept Incoming / Accept Both**. **Resolve in Merge Editor** gives a side-by-side view. Always read the result: "Accept Both" often leaves duplicate code.

### Back out

```bash
git merge --abort
```

Cancels the merge and puts everything back to how it was before `git merge`. Use it when you're unsure and want to start over.

## 4. Undoing changes and stash

> [!IMPORTANT]
> **Rule:** not pushed yet → `reset` or `amend` is fine. Already pushed → use `revert`, because teammates may already have the commit.

### Throw away edits you haven't committed

```bash
git diff index.html      # check what you're about to lose
git restore index.html
```

Puts the file back to how it was at the last commit. The edits are gone for good.

### Unstage a file

```bash
git add .
git restore --staged .env
```

Takes `.env` out of the next commit. The file and its edits stay on disk.

### Fix the last commit

```bash
git commit --amend -m "feat: add navbar links"

# forgot a file?
git add style.css
git commit --amend --no-edit
```

Replaces the last commit with a corrected one: a new message, or extra files while keeping the message (`--no-edit`). Only before pushing.

### Undo the last commit, three ways

```bash
git reset --soft HEAD~1   # commit gone, changes still staged
git reset HEAD~1          # commit gone, changes kept but unstaged
git reset --hard HEAD~1   # commit gone, changes deleted
```

`HEAD~1` means one commit before where you are. `--soft` and the default keep your work so you can recommit it. `--hard` throws it away. All three rewrite history, so local commits only.

### Undo a commit that's already pushed

```bash
git log --oneline
# 7c1e9a0 feat: dark mode    <- this broke the page
# 8b7d9e2 feat: add navbar
git revert 7c1e9a0
git push
```

Creates a new commit that does the exact opposite of `7c1e9a0`. History isn't rewritten, so teammates' copies stay fine.

### Recover a "lost" commit

```bash
git reset --hard HEAD~1   # oops
git reflog
# 8b7d9e2 HEAD@{0}: reset: moving to HEAD~1
# a91c0d2 HEAD@{1}: commit: feat: add footer
git reset --hard a91c0d2
```

`reflog` records everywhere `HEAD` has been on your machine, including commits no branch points to anymore. Find the one you want and reset back to it. Do this one live: it's what takes the fear away.

### Shelve work with stash

```bash
git stash push -m "wip: dark mode"
git switch main              # fix the urgent bug
git switch feature/dark-mode
git stash pop
```

`stash` saves your uncommitted changes and leaves a clean working tree, so you can switch branches. `pop` brings them back and removes the stash. Add `-u` to include new files. `git stash list` shows what's saved, `git stash apply` restores without removing.

## 5. Remotes, pull and rebase

*Example: you and a teammate both push to the same Team page repo on GitHub.*

### See your remote

```bash
git remote -v
# origin  git@github.com:zindua/team-page.git (fetch)
# origin  git@github.com:zindua/team-page.git (push)
```

`origin` is the nickname for the GitHub copy. `origin/main` is your local snapshot of GitHub's `main` from your last fetch.

### Download without changing anything

```bash
git fetch
git lg
# * 4d2e8f1 (origin/main) feat: add contact section
# * 8b7d9e2 (HEAD -> main) feat: add navbar
```

`fetch` updates `origin/main` so you can see what teammates pushed. Your branch and your files don't change. `git lg` is the shortcut from [Setup](#add-a-log-shortcut).

### Pull: fetch + merge

```bash
git pull
```

Fetches, then merges `origin/main` into your current branch. If you also have local commits, you get an extra merge commit.

### Pull with rebase

```bash
git pull --rebase
```

Fetches, then replays your local commits on top of what's on GitHub. Same code, straight history, no "Merge branch 'main'" noise. Make it the default with `git config --global pull.rebase true`.

### Rejected push

```bash
git push
# ! [rejected]        main -> main (fetch first)
git pull --rebase
git push
```

GitHub has commits you don't have yet. Pull them in, then push. Never force-push past this on a shared branch.

### Push a new branch

```bash
git push -u origin feature/contact-form
git branch -vv
# * feature/contact-form 9a3c1b2 [origin/feature/contact-form] feat: add form
```

`-u` creates the branch on GitHub and links the two, so later `git push` and `git pull` need no extra words. `git branch -vv` shows each link and whether you're ahead or behind.

### Rebase your branch on the latest main

```bash
git switch feature/contact-form
git fetch
git rebase origin/main
```

Lifts your branch's commits and replays them on top of the newest `main`, as if you'd started today. On a conflict: fix the file, `git add`, then `git rebase --continue`. Bail out with `git rebase --abort`.

### Tidy commits before a PR

```bash
git rebase -i HEAD~3
# pick   9a3c1b2 feat: add contact form
# squash 1f7e2d0 fix typo
# squash 6b0a4c9 fix typo again
```

Opens your last 3 commits in the editor. Change `pick` to `squash` (fold into the commit above), `reword` (change the message) or `drop` (delete it). Save and close: here, three commits become one.

### Push a rebased branch

```bash
git push --force-with-lease
```

Rebasing rewrote your commits, so a normal push is rejected. `--force-with-lease` overwrites your branch on GitHub, but refuses if someone else pushed to it in the meantime. Own feature branches only: never on `main`, never plain `--force`.

> [!WARNING]
> **Rule:** never rebase commits other people already have.

### Clean up

```bash
git push origin --delete feature/contact-form
git fetch --prune
```

Deletes the branch on GitHub, then clears your stale `origin/...` labels for branches that no longer exist there.

## 6. Pull requests and code review

*Example: the `feature/contact-form` branch goes into `main` through a PR.*

### Open the PR

On GitHub, click the **Compare & pull request** banner, or **Pull requests → New pull request**. Set **base: main ← compare: feature/contact-form**. Base is where the code goes, compare is where it comes from.

### Write a description the reviewer can act on

```markdown
## What
Adds a contact form with name, email and message fields.

## Why
Visitors had no way to reach the team.

## How to test
Open index.html and submit the form empty. You should see validation errors.

Closes #4
```

`Closes #4` closes issue 4 automatically when the PR merges into `main` (`Fixes #4` works too). Save these headings as `.github/pull_request_template.md` and every new PR starts with them.

### Draft or ready

**Create draft pull request** gets early feedback on unfinished work. It can't be merged until you click **Ready for review**.

### Update the PR after feedback

```bash
git add index.html
git commit -m "fix: add labels to form inputs"
git push
```

New commits pushed to the same branch show up in the open PR. Never open a second PR for fixes.

### Review it

In the **Files changed** tab, click a line to comment, or use **Add a suggestion** to propose exact replacement code the author can commit in one click. Finish with **Review changes → Comment, Approve, or Request changes**.

What to check: does it do what the issue asked, does it run, are names clear, any leftover `console.log`, any secrets or `.env`. Start minor points with `nit:` so the author knows they're optional.

### Merge it

- **Create a merge commit:** keeps every commit, plus a merge commit.
- **Squash and merge:** turns the whole PR into one commit on `main`. Use this as the class default.
- **Rebase and merge:** replays each commit onto `main`, no merge commit.

### Clean up locally

```bash
git switch main
git pull
git branch -D feature/contact-form
```

Click **Delete branch** on GitHub, then sync your local `main` and delete your copy. After a squash merge, `-d` says the branch isn't merged because the squashed commit has a new id. Use `-D` once you've confirmed the PR merged.

## 7. Team workflow and repo settings

*Example: setting up the Team page repo for a team of four. Demo it on one repo; Maintainers repeat it in the lab.*

### The loop every teammate follows (GitHub flow)

```bash
# 1. pick issue #7 on GitHub and assign yourself
git switch main
git pull                                 # 2. start from the latest main
git switch -c feature/7-dark-mode        # 3. one branch per issue
# ...commit small...
git push -u origin feature/7-dark-mode   # 4. push early
# 5. open a PR with "Closes #7", request a reviewer
git fetch && git rebase origin/main      # 6. stay current while in review
git push --force-with-lease              #    (rebase rewrote your commits)
# 7. squash and merge, delete the branch, everyone pulls main
```

**One issue = one branch = one PR.** `main` always works because nothing lands there without a review.

### Add teammates

**Settings → Collaborators → Add people.** They accept the invite from email or notifications, then they can push branches.

### Protect main (optional)

**Settings → Branches → Add branch protection rule** (or **Settings → Rules → Rulesets**), branch name `main`:

- Require a pull request before merging
- Require approvals: 1
- Do not allow bypassing the above settings (so it applies to the owner too)

Now nobody can push straight to `main`. On free accounts this only works on public repos ([GitHub Docs](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)).

### Write issues people can pick up

```markdown
Title: Add dark mode toggle

- [ ] Button in the navbar switches light/dark
- [ ] Choice survives a page refresh (localStorage)
- [ ] Works on mobile

Label: feature   Assignee: @brian
```

The checklist says what "done" means, so reviewers know what to check. Use labels like `feature`, `bug`, `good first issue`.

### Write commit messages with a prefix

```bash
git commit -m "feat: add dark mode toggle"
git commit -m "fix: footer year shows 1970"
```

`feat:`, `fix:`, `docs:`, `chore:` make the history read like a changelog.

### Fork vs shared repo

**Shared repo:** your team, everyone is a collaborator and pushes branches. **Fork:** your own copy of a repo you can't push to (open source, a classmate's project), and you open PRs from it.

```bash
git remote add upstream https://github.com/classmate/portfolio.git
git fetch upstream
git merge upstream/main
```

`upstream` points at the original repo so you can pull its new commits into your fork. GitHub's **Sync fork** button does the same.
