# 📘 Day 12 – Advanced Git 

# 1. Git Branch (Quick Revision)

## What is a Branch?

A Git branch is an independent line of development.

Instead of writing all code directly in the `main` branch, developers create separate branches to develop features, fix bugs, or experiment with changes without affecting the stable code.

### Why do we use branches?

- Protect the stable `main` branch
- Allow multiple developers to work simultaneously
- Isolate new features and bug fixes
- Make code review easier

---

## Real Company Scenario

Suppose your team is developing an E-commerce application.

Developer A works on:

- Login Feature

Developer B works on:

- Payment Feature

Developer C works on:

- Wishlist Feature

Instead of all modifying `main`, they create:

```
main

├── feature/login

├── feature/payment

└── feature/wishlist
```

Each developer works independently.

Only after testing and code review are the changes merged into `main`.

---

## Important Commands

### Create a Branch

```bash
git branch feature/login
```

Creates a new branch.

---

### Switch Branch

```bash
git checkout feature/login
```

Moves to another branch.

---

### Create and Switch Together

```bash
git checkout -b feature/login
```

Creates a branch and switches to it immediately.

---

### View Branches

```bash
git branch
```

Current branch is marked with `*`.

Example:

```
* main
  feature/login
  feature/payment
```

---

### Delete Branch

```bash
git branch -d feature/login
```

Deletes a merged branch.

Force delete:

```bash
git branch -D feature/login
```

---

# Interview Question

### Why should we never develop directly on the main branch?

### Answer

The main branch represents the stable production-ready code.

Developing directly on it increases the risk of introducing bugs into production.

Feature branches isolate changes, making testing, code review, and collaboration much safer.

---

# 2. Git Merge

## What is Git Merge?

Git Merge combines the changes from one branch into another.

It preserves the history of both branches.

---

## Example

Suppose:

```
main

A ---- B

feature/login

A ---- B ---- C ---- D
```

Commits:

```
A -> Project Setup

B -> Initial Configuration

C -> Login Backend

D -> Login UI
```

Feature development is complete.

Switch to main:

```bash
git checkout main
```

Merge:

```bash
git merge feature/login
```

History becomes:

```
          C ---- D
         /
A ---- B -------- M
```

Where:

```
M = Merge Commit
```

---

## Why Merge Commit?

Git creates a merge commit because it preserves the history of both branches.

This allows developers to know:

- Where feature development started
- Where it ended
- Which branch contributed the changes

---

## Advantages

- Preserves complete branch history
- Easy to understand feature development
- Safer for collaborative development

---

## Disadvantages

Many feature branches create many merge commits.

History becomes:

```
Merge

Merge

Merge

Merge

Merge
```

Commit history becomes cluttered.

---

## Real Company Scenario

Developer A

```
feature/login
```

Developer B

```
feature/payment
```

Both complete development.

After code review:

```
git merge feature/login

git merge feature/payment
```

Main contains both features while preserving branch history.

---

# Interview Question

### What happens when Git Merge is executed?

### Answer

Git Merge combines the histories of two branches.

If the histories have diverged, Git creates a merge commit to preserve both timelines.

---

# 3. Git Rebase

## What is Git Rebase?

Git Rebase moves or replays commits from one branch on top of another branch.

Unlike Merge, Rebase creates a clean linear history without merge commits.

---

## Example

Initially:

```
main

A ---- B

feature/login

A ---- B ---- C ---- D
```

Meanwhile another developer updates main.

```
main

A ---- B ---- E ---- F
```

Feature still has:

```
A ---- B ---- C ---- D
```

Now run:

```bash
git checkout feature/login

git rebase main
```

Git temporarily removes:

```
C

D
```

Moves feature branch to:

```
A ---- B ---- E ---- F
```

Then replays:

```
C'

D'
```

Final history:

```
A ---- B ---- E ---- F ---- C' ---- D'
```

Notice:

- C and D are replayed.
- Their commit IDs change.
- History becomes completely linear.
- No merge commit is created.

---

## Why Commit IDs Change

Rebase rewrites commit history.

Since each commit now has a different parent, Git creates new commit IDs.

Original:

```
C

D
```

After Rebase:

```
C'

D'
```

Same changes.

Different commit IDs.

---

## Merge vs Rebase

| Git Merge | Git Rebase |
|------------|------------|
| Preserves branch history | Rewrites history |
| Creates merge commit | No merge commit |
| Branch timelines remain separate | Creates linear history |
| Safer for shared branches | Better before sharing local branches |

---

## When Should We Use Rebase?

Use Rebase:

- Before creating Pull Requests
- To clean commit history
- On your own local feature branch

Avoid Rebase:

- On branches already shared with other developers

---

## Real Company Scenario

Before creating a Pull Request:

```
feature/login
```

Developer updates latest main changes:

```bash
git checkout feature/login

git rebase main
```

Now Pull Request contains clean linear commits.

---

# Interview Question

### Why do many companies prefer Git Rebase before creating Pull Requests?

### Answer

Git Rebase creates a clean linear commit history by replaying feature commits on top of the latest main branch.

This avoids unnecessary merge commits and makes project history easier to read and maintain.

---

# Key Takeaways

✅ Feature branches keep the main branch stable.

✅ Git Merge combines branches while preserving both histories.

✅ Git Rebase creates a clean linear history by replaying commits.

✅ Rebase changes commit IDs because history is rewritten.

✅ Merge is safer for shared branches.

✅ Rebase is ideal for cleaning local feature branches before merging.

# 4. Git Cherry-pick

## What is Git Cherry-pick?

Git Cherry-pick copies a specific commit from one branch and applies it to another branch.

Unlike Merge, Cherry-pick does **not** merge the complete branch.

It copies **only one selected commit**.

---

## Why do we use Cherry-pick?

Suppose:

```
main
```

Production branch.

```
feature/payment
```

Contains 20 commits.

Only one commit contains an urgent production bug fix.

You don't want all 20 commits.

You only need that one fix.

Git Cherry-pick is the perfect solution.

---

## Example

Feature Branch

```
A ---- B ---- C ---- D ---- E
```

Where

```
C = Payment Bug Fix
```

Main Branch

```
A ---- B
```

Run

```bash
git checkout main

git cherry-pick <commit-id-of-C>
```

Result

```
main

A ---- B ---- C'
```

Notice:

Git copied only commit C.

It did NOT copy D and E.

---

## Important Point

Cherry-picked commit gets a **new Commit ID**.

Original

```
C
```

Copied

```
C'
```

Because Git creates a new commit while applying the same changes.

---

## Real Company Scenario

Developer fixes a critical production bug.

Feature branch has:

```
20 commits
```

Manager says

> We only need the payment bug fix.

Instead of merging all 20 commits

```
git cherry-pick <bug-fix-commit-id>
```

Production receives only the required fix.

---

## Useful Commands

View commit history

```bash
git log --oneline
```

Cherry-pick

```bash
git cherry-pick <commit-id>
```

---

## Advantages

- Copy only required commits
- Avoid unnecessary features
- Useful for production hotfixes

---

## Disadvantages

- Duplicate commits
- Commit IDs change
- Too much cherry-picking can complicate history

---

# Interview Question

### When should Git Cherry-pick be used?

### Answer

Git Cherry-pick is used when only a specific commit is required from another branch instead of merging the complete branch.

It is commonly used for production hotfixes.

---

# 5. Git Stash

## What is Git Stash?

Git Stash temporarily stores uncommitted changes so that you can switch branches without committing incomplete work.

Think of it as a temporary locker.

---

## Why do we use Git Stash?

Suppose you are working on:

```
feature/payment
```

You have modified several files.

Nothing is committed yet.

Suddenly manager says

> Production issue.

You must immediately switch to

```
main
```

Git won't allow branch switching if changes conflict.

Instead of creating an unnecessary commit

Store work temporarily

```
git stash
```

Now working directory becomes clean.

Switch branches

Fix production

Return

Restore work

---

## Practical Example

Working Directory

```
Login.java

Payment.java
```

Run

```bash
git stash
```

Output

```
Saved working directory and index state.
```

Now

```
git status
```

Shows

```
Working tree clean
```

---

## Restore Changes

```bash
git stash pop
```

Git restores the latest stash.

---

## View Stashes

```bash
git stash list
```

Example

```
stash@{0}

stash@{1}

stash@{2}
```

---

## Apply Without Removing

```bash
git stash apply
```

Restores stash

Keeps it in stash list.

---

## Delete One Stash

```bash
git stash drop stash@{0}
```

---

## Delete All Stashes

```bash
git stash clear
```

---

## Real Company Scenario

Developer is implementing Payment Feature.

Production suddenly breaks.

Developer

```
git stash

↓

git checkout main

↓

Fix production

↓

git checkout feature/payment

↓

git stash pop
```

Continues work exactly where they left off.

---

## Advantages

- No unnecessary commits
- Quickly switch branches
- Safe temporary storage

---

## Interview Question

### Difference between git stash pop and git stash apply?

### Answer

| git stash pop | git stash apply |
|---------------|-----------------|
| Restores stash | Restores stash |
| Removes stash | Keeps stash |

---

# 6. Git Reset

Git Reset moves the current branch pointer (HEAD) to another commit.

It is mainly used for undoing commits.

There are three types.

---

# Git Reset --soft

Command

```bash
git reset --soft HEAD~1
```

Removes

✅ Commit

Keeps

✅ Staging Area

✅ Working Directory

Best when

You want to rewrite commit message or recommit immediately.

---

# Git Reset --mixed

(Default)

```bash
git reset HEAD~1
```

Removes

✅ Commit

Clears

✅ Staging Area

Keeps

✅ Working Directory

Best when

You want to stage files again selectively.

---

# Git Reset --hard

Command

```bash
git reset --hard HEAD~1
```

Removes

✅ Commit

✅ Staging Area

✅ Working Directory

Everything returns to the previous commit.

---

## Comparison Table

| Reset Type | Commit | Staging | Working Directory |
|------------|---------|----------|-------------------|
| Soft | Removed | Kept | Kept |
| Mixed | Removed | Cleared | Kept |
| Hard | Removed | Cleared | Cleared |

---

## Real Company Scenario

Developer accidentally commits debug files.

Uses

```
git reset --mixed HEAD~1
```

Now code remains

Files become unstaged

Developer stages only correct files

Creates a clean commit.

---

## Interview Question

### Difference between Soft, Mixed and Hard Reset?

### Answer

Soft

Undo commit

Keep everything staged.

Mixed

Undo commit

Unstage everything

Keep code.

Hard

Undo commit

Delete everything.

Used carefully because it may permanently remove local work.

---

# Key Takeaways

✅ Cherry-pick copies one specific commit.

✅ Cherry-picked commits receive new commit IDs.

✅ Git Stash temporarily stores uncommitted work.

✅ Stash Pop removes the stash after restoring.

✅ Stash Apply restores without removing.

✅ Reset rewrites history.

✅ Soft keeps staging.

✅ Mixed unstages changes.

✅ Hard deletes local changes.

# 7. Git Revert

## What is Git Revert?

Git Revert creates a **new commit** that reverses the changes made by a previous commit.

Unlike Git Reset, Git Revert **does not delete commit history**.

Instead, it preserves history by creating another commit that undoes the selected commit.

---

## Why do we use Git Revert?

Git Revert is mainly used when the commit has already been pushed to a shared repository.

Since other developers may already have that commit, deleting history using Git Reset can create conflicts.

Instead, Git Revert safely preserves history.

---

## Example

Current History

```
A ---- B ---- C ---- D
```

Suppose

```
D = Added Payment Feature
```

But the Payment Feature contains bugs.

Run

```bash
git revert <commit-id-of-D>
```

Git creates

```
A ---- B ---- C ---- D ---- E
```

Where

```
E = Reverted "Added Payment Feature"
```

Notice

Commit D still exists.

Git simply created another commit that cancels its changes.

---

## Real Company Scenario

A new feature is deployed to Production.

After deployment users report serious issues.

The feature has already been pushed to GitHub.

Instead of deleting commit history

Developer runs

```bash
git revert <commit-id>
```

Production receives another commit that safely removes the feature.

---

## Git Reset vs Git Revert

| Git Reset | Git Revert |
|------------|------------|
| Deletes commits | Keeps commits |
| Rewrites history | Preserves history |
| Best for local work | Best for shared repositories |
| Dangerous after pushing | Safe after pushing |

---

## Golden Rule

If commit is

Not pushed

→ Use Git Reset

Already pushed

→ Use Git Revert

---

## Interview Question

### Why is Git Revert preferred over Git Reset on shared branches?

### Answer

Git Revert preserves commit history by creating a new commit that reverses previous changes.

Since history remains unchanged, all developers continue working with the same repository history, avoiding conflicts.

---

# 8. Git Tag

## What is Git Tag?

Git Tag is a permanent label attached to a specific commit.

Tags are mainly used to mark software releases.

Instead of remembering long commit IDs, we use meaningful names such as

```
v1.0

v2.0

v3.0
```

---

## Why do we use Tags?

Suppose your project has

```
500 commits
```

But Production was released only

```
12 times
```

Only those release commits receive Git Tags.

---

## Practical Example

History

```
A ---- B ---- C ---- D (HEAD)
```

QA tests the application.

Manager approves Production Release.

Create Tag

```bash
git tag -a v1.0 -m "First Production Release"
```

History becomes

```
A ---- B ---- C ---- D
                    ↑
                  v1.0
```

Notice

No new commit is created.

Git simply attaches a label to commit D.

---

## Create Lightweight Tag

```bash
git tag v1.0
```

---

## Create Annotated Tag

```bash
git tag -a v1.0 -m "Production Release"
```

---

## View Tags

```bash
git tag
```

---

## Show Tag Details

```bash
git show v1.0
```

---

## Push Single Tag

```bash
git push origin v1.0
```

---

## Push All Tags

```bash
git push origin --tags
```

---

## Important Note

Creating a Git Tag does NOT automatically push it to GitHub.

Tags are local.

They must be pushed separately.

---

## Real Company Scenario

Application Version

```
v2.3
```

is currently running in Production.

A critical issue occurs after deploying

```
v3.0
```

Manager says

Rollback to

```
v2.3
```

Since releases are tagged

Developers immediately know which commit belongs to Version 2.3.

---

## Interview Question

### Are Git Tags automatically pushed to GitHub?

### Answer

No.

Git Tags are created locally.

They must be pushed separately using

```bash
git push origin <tag-name>
```

or

```bash
git push origin --tags
```

---

# 9. Git Log

## What is Git Log?

Git Log displays commit history.

It shows

- Commit ID
- Author
- Date
- Commit Message

---

## Why do we use Git Log?

Developers use Git Log to understand

- Who changed the code
- When it was changed
- Why it was changed

---

## View Complete History

```bash
git log
```

---

## Compact History

```bash
git log --oneline
```

Example

```
8c8fa23 Fixed Payment Bug

7b34de1 Added Payment

3ab9dc2 Added Login
```

---

## Last Five Commits

```bash
git log -5 --oneline
```

---

## Visual Graph

```bash
git log --graph --oneline --all
```

Example

```
* Merge feature/login

|\
| * Login UI

| * Login API

|/

* Payment

* Initial Project
```

---

## View Commits by Author

```bash
git log --author="Sahithi"
```

---

## Real Company Scenario

Manager asks

Who modified Payment Service yesterday?

Developer runs

```bash
git log
```

Immediately knows

- Author
- Date
- Commit Message

---

## Interview Question

### Which Git Log command do developers use most frequently?

### Answer

```bash
git log --oneline
```

because it provides a clean and compact commit history suitable for daily development.

---

# Key Takeaways

✅ Git Revert creates a new commit to undo previous changes.

✅ Git Reset rewrites history.

✅ Git Tag marks important release commits.

✅ Tags are local until pushed.

✅ Git Log shows commit history.

✅ Git Log helps identify author, date and commit message.

# 10. Git Diff

## What is Git Diff?

Git Diff shows the line-by-line differences between two versions of files.

Unlike Git Log, which answers

> Who changed?

Git Diff answers

> What exactly changed?

---

# Example

Before

```java
String name = "John";
```

After

```java
String name = "Sahithi";
```

Run

```bash
git diff
```

Output

```diff
- String name = "John";
+ String name = "Sahithi";
```

---

## Types of Git Diff

### Compare Working Directory with Staging Area

```bash
git diff
```

Used before staging files.

---

### Compare Staging Area with Repository

```bash
git diff --staged
```

Shows changes that will be included in the next commit.

---

### Compare Two Commits

```bash
git diff commit1 commit2
```

Useful for comparing releases.

---

### Compare Branches

```bash
git diff main feature/login
```

Useful before Pull Requests.

---

## Real Company Scenario

Before committing code, developers usually run

```bash
git diff
```

to ensure

- No unwanted changes
- No debug code
- No accidental modifications

---

## Interview Question

### Difference between

```bash
git diff
```

and

```bash
git diff --staged
```

### Answer

| git diff | git diff --staged |
|-----------|-------------------|
| Working Directory vs Staging Area | Staging Area vs Repository |
| Shows unstaged changes | Shows staged changes |

---

# 11. Git Reflog

## What is Git Reflog?

Git Reflog records every movement of HEAD.

Even if commits disappear from Git Log, Reflog often allows recovery.

---

## Why do we use Git Reflog?

Suppose

```
A ---- B ---- C ---- D
```

You accidentally run

```bash
git reset --hard HEAD~2
```

History becomes

```
A ---- B
```

Git Log no longer shows

```
C

D
```

Run

```bash
git reflog
```

Output

```
HEAD@{0}

HEAD@{1}

HEAD@{2}
```

Git remembers where HEAD pointed previously.

Recover

```bash
git reset --hard HEAD@{1}
```

or

```bash
git reset --hard <commit-id>
```

History is restored.

---

## Real Company Scenario

Developer accidentally executes

```bash
git reset --hard
```

before pushing.

Instead of panicking

Developer uses

```bash
git reflog
```

finds previous HEAD

and recovers work.

---

## Git Log vs Git Reflog

| Git Log | Git Reflog |
|----------|------------|
| Commit History | HEAD History |
| Shared History | Local History |
| Doesn't show removed commits | Helps recover removed commits |

---

# 12. Git Best Practices

✅ Always create feature branches.

---

✅ Keep commit messages meaningful.

Example

Good

```
Added Login Authentication
```

Bad

```
Update
```

---

✅ Pull latest changes before pushing.

```bash
git pull
```

---

✅ Review changes before commit.

```bash
git diff
```

---

✅ Review staged changes.

```bash
git diff --staged
```

---

✅ Never use

```bash
git reset --hard
```

without understanding the consequences.

---

✅ Never rebase shared branches.

---

✅ Use Git Revert instead of Reset for Production branches.

---

✅ Tag every Production Release.

---

# 13. Git Command Cheat Sheet

## Branch

```bash
git branch

git checkout feature/login

git checkout -b feature/login
```

---

## Merge

```bash
git merge feature/login
```

---

## Rebase

```bash
git rebase main
```

---

## Cherry Pick

```bash
git cherry-pick <commit-id>
```

---

## Stash

```bash
git stash

git stash list

git stash pop

git stash apply

git stash clear
```

---

## Reset

```bash
git reset --soft HEAD~1

git reset --mixed HEAD~1

git reset --hard HEAD~1
```

---

## Revert

```bash
git revert <commit-id>
```

---

## Tag

```bash
git tag

git tag -a v1.0 -m "Release"

git push origin v1.0

git push origin --tags
```

---

## Log

```bash
git log

git log --oneline

git log --graph --oneline --all

git log -5 --oneline
```

---

## Diff

```bash
git diff

git diff --staged

git diff commit1 commit2

git diff main feature
```

---

## Reflog

```bash
git reflog
```

---

# 14. Frequently Asked Interview Questions

## Q1. Difference between Merge and Rebase?

Merge preserves history.

Rebase creates linear history.

---

## Q2. Difference between Reset and Revert?

Reset removes commits.

Revert creates another commit to undo previous changes.

---

## Q3. What is Git Cherry-pick?

Copies a specific commit from one branch to another.

---

## Q4. Difference between Stash Pop and Apply?

Pop restores and removes stash.

Apply restores but keeps stash.

---

## Q5. Difference between Soft, Mixed and Hard Reset?

Soft

Undo commit only.

Mixed

Undo commit and unstage files.

Hard

Undo commit and delete local changes.

---

## Q6. Why do companies use Git Tags?

To mark Production Releases.

---

## Q7. What happens if you forget to push a tag?

The tag exists only locally.

Other developers cannot see it.

---

## Q8. Difference between Git Log and Git Reflog?

Git Log shows commit history.

Git Reflog shows HEAD movement history.

---

## Q9. How can you recover lost commits?

Use

```bash
git reflog
```

then

```bash
git reset --hard <commit-id>
```

---

## Q10. Difference between

```bash
git diff

git diff --staged
```

git diff

Working Directory ↔ Staging Area

git diff --staged

Staging Area ↔ Repository

---

# 15. Final Summary

## Merge

Preserves history.

---

## Rebase

Creates clean history.

---

## Cherry Pick

Copies one commit.

---

## Stash

Temporary storage.

---

## Reset

Rewrites history.

---

## Revert

Preserves history.

---

## Tag

Marks releases.

---

## Log

Shows commit history.

---

## Diff

Shows code changes.

---

## Reflog

Recovers lost commits.

---
