# 📅 Day 10 - GitHub Integration

# What is GitHub?

GitHub is a cloud-based platform that hosts Git repositories.

It allows developers to:

- Store code online
- Collaborate with team members
- Track project history
- Review code
- Create Pull Requests
- Integrate with CI/CD tools such as Jenkins, GitHub Actions, Azure DevOps, etc.

Git performs version control.

GitHub hosts Git repositories.

---

# Git vs GitHub

| Git | GitHub |
|------|---------|
| Version Control System | Cloud Platform |
| Installed on Local Machine | Hosted on Internet |
| Tracks Changes | Stores Repositories |
| Works Offline | Collaboration Platform |

---

# Local Repository

A Local Repository exists on the developer's computer.

Example:

```
EmployeeApp/

├── Login.java
├── Payment.java
├── README.md
└── .git
```

The `.git` folder contains:

- Commit History
- Branches
- Configuration
- Remote Information
- Objects

---

# Remote Repository

A Remote Repository is stored on GitHub.

Example:

```
https://github.com/company/EmployeeApp.git
```

Multiple developers can access it.

---

# Local vs Remote Repository

| Local Repository | Remote Repository |
|------------------|------------------|
| Present on Local Computer | Hosted on GitHub |
| Used for Development | Used for Collaboration |
| Private Workspace | Shared Workspace |

---

# Why do we need GitHub?

Without GitHub:

- Code remains only on local computer
- Difficult to collaborate
- No centralized repository
- No backup

With GitHub:

- Centralized code storage
- Team collaboration
- Code reviews
- Pull Requests
- Backup
- CI/CD integration

---

# What is a Remote?

A Remote is a connection between the Local Repository and the Remote Repository.

Example:

Local Repository

↓

origin

↓

GitHub Repository

---

# What is origin?

origin is the default alias (nickname) of the remote repository.

Example:

```bash
git remote add origin https://github.com/user/project.git
```

origin is not a Git keyword.

It can be renamed.

Example:

```bash
git remote add company https://github.com/company/project.git
```

Now push using:

```bash
git push company main
```

---

# Adding a Remote Repository

Command:

```bash
git remote add origin https://github.com/user/project.git
```

Purpose:

Connects the Local Repository with the Remote Repository.

This command is executed only once.

---

# Verify Remote

Command:

```bash
git remote -v
```

Output:

```
origin https://github.com/user/project.git (fetch)
origin https://github.com/user/project.git (push)
```

---

# Fetch URL

Used for downloading changes from GitHub.

Commands using Fetch URL:

- git fetch
- git pull

---

# Push URL

Used for uploading commits to GitHub.

Command:

- git push

Usually Fetch URL and Push URL are the same.

---

# Upload Code to GitHub

Command:

```bash
git push origin main
```

Purpose:

Uploads local commits to the remote repository.

---

# What does -u mean?

Command:

```bash
git push -u origin main
```

-u means:

Set upstream branch.

Git remembers:

```
main

↓

origin/main
```

Future pushes:

```bash
git push
```

Future pulls:

```bash
git pull
```

---

# git clone

Purpose:

Download an existing repository from GitHub.

Command:

```bash
git clone https://github.com/company/project.git
```

git clone automatically:

- Downloads files
- Creates .git folder
- Downloads commit history
- Configures origin
- Checks out default branch

---

# git init vs git clone

| git init | git clone |
|----------|-----------|
| Creates Empty Repository | Downloads Existing Repository |
| No Commit History | Complete History |
| No Remote | Remote Configured |
| Used for New Project | Used for Existing Project |

---

# git fetch

Purpose:

Download latest commits from GitHub.

Does NOT update Working Directory.

Workflow:

GitHub

↓

Local Repository

Working Directory remains unchanged.

---

# git pull

Purpose:

Download latest commits and merge them.

Updates Working Directory.

Workflow:

GitHub

↓

Local Repository

↓

Merge

↓

Working Directory Updated

---

# Difference Between Fetch and Pull

| git fetch | git pull |
|------------|-----------|
| Downloads Changes | Downloads + Merges |
| Safe | May create conflicts |
| Working Directory Unchanged | Working Directory Updated |

---

# Real Company Workflow

Morning:

```bash
git pull
```

Start Development

↓

Code Changes

↓

```bash
git add .
git commit -m "Implemented Feature"
git push
```

---

# Best Practices

- Commit frequently
- Push meaningful commits
- Pull latest changes before starting work
- Never work on outdated code
- Verify remote using git remote -v

---

# Interview Questions

## What is GitHub?

GitHub is a cloud platform used to host Git repositories and enable collaboration.

---

## Difference between Git and GitHub?

Git is a Version Control System.

GitHub is a hosting platform for Git repositories.

---

## What is origin?

Default alias of the Remote Repository.

---

## Difference between git init and git clone?

git init creates a new repository.

git clone downloads an existing repository.

---

## Difference between git fetch and git pull?

git fetch downloads changes only.

git pull downloads and merges changes.

---

## What does git remote -v do?

Displays configured remotes with Fetch and Push URLs.

---

## What does git push -u origin main do?

Pushes commits and sets the upstream branch.

---

# Quick Revision

- Git manages versions.
- GitHub hosts repositories.
- Local Repository → Local Computer.
- Remote Repository → GitHub.
- origin → Default Remote Alias.
- git clone → Download Existing Repository.
- git fetch → Download Changes.
- git pull → Download + Merge.
- git push → Upload Commits.

#  Git Branching

# What is a Branch?

A branch is an independent line of development.

It allows developers to work on new features or bug fixes without affecting the main branch.

Example:

                main
                  │
        ┌─────────┼─────────┐
        │         │         │
 feature/login feature/payment feature/profile

Each developer works independently.

---

# Why Do We Need Branches?

Imagine 10 developers working directly on the main branch.

Problems:

- Unfinished code affects everyone.
- Bugs in one feature affect the entire application.
- Difficult to test individual features.
- Increased merge conflicts.
- Main branch becomes unstable.
- Production deployments become risky.

Branches solve these problems by isolating development.

---

# Why Should Main Branch Be Stable?

The main branch represents the stable version of the application.

In most companies:

- Jenkins builds from main.
- Docker images are created from main.
- Kubernetes deployments use main.
- Production deployments come from main.

Only tested and reviewed code should reach the main branch.

---

# Creating a Branch

Command:

```bash
git branch feature/login
```

This command:

- Creates a new branch.
- Does NOT switch to it.

---

# Checking Current Branch

Command:

```bash
git branch
```

Example:

```
* main
  feature/login
```

The (*) symbol indicates the current branch.

---

# Switching Branches

Modern Git:

```bash
git switch feature/login
```

Older Command:

```bash
git checkout feature/login
```

After switching:

```
* feature/login
  main
```

Now all changes will be committed to feature/login.

---

# Create & Switch in One Command

Modern Git:

```bash
git switch -c feature/login
```

Older Git:

```bash
git checkout -b feature/login
```

This command:

- Creates a branch.
- Switches to it immediately.

---

# Listing All Branches

Command:

```bash
git branch
```

Example:

```
* feature/login
  feature/payment
  feature/profile
  main
```

Current branch is identified using (*).

---

# Deleting Branches

After a branch is merged into main, it is no longer required.

Safe Delete:

```bash
git branch -d feature/login
```

Deletes only if the branch has already been merged.

---

# Force Delete

```bash
git branch -D feature/login
```

Force deletes the branch even if it is not merged.

Use with caution.

---

# Difference Between -d and -D

| git branch -d | git branch -D |
|---------------|---------------|
| Safe Delete | Force Delete |
| Deletes only merged branches | Deletes merged or unmerged branches |

---

# Branch Naming Convention

Feature Branch

```
feature/login
feature/payment
feature/dashboard
```

Bug Fix Branch

```
bugfix/payment
bugfix/login
```

Hotfix Branch

```
hotfix/security
hotfix/payment
```

Release Branch

```
release/v1.0
release/v2.0
```

---

# Real Company Workflow

Manager assigns:

Implement Login Feature.

Step 1

Create Branch

```bash
git switch -c feature/login
```

↓

Develop Feature

↓

Commit Changes

```bash
git add .

git commit -m "Implemented Login Feature"
```

↓

Push Branch

```bash
git push origin feature/login
```

↓

Create Pull Request

↓

Code Review

↓

Merge into Main

↓

Delete Branch

```bash
git branch -d feature/login
```

This workflow is followed for every feature.

---

# Why Create a New Branch for Every Feature?

One branch should represent one feature.

Correct:

```
feature/login

↓

Merged

↓

Deleted

---------------------

feature/payment

↓

Merged

↓

Deleted

---------------------

feature/profile
```

Incorrect:

```
feature/payment

↓

Payment Feature

↓

Forgot Password

↓

Profile Feature

↓

Dashboard Changes
```

Using the same branch for multiple features makes code review and maintenance difficult.

---

# DevOps Workflow

Developer

↓

Feature Branch

↓

Commit

↓

Push

↓

GitHub

↓

Pull Request

↓

Code Review

↓

Merge to Main

↓

Jenkins

↓

Docker Image

↓

Kubernetes Deployment

↓

Production

---

# Best Practices

- Never develop directly on main.
- Create one branch per feature.
- Keep branch names meaningful.
- Delete merged branches.
- Pull latest changes before creating a new feature branch.
- Keep main stable and deployable.

---

# Common Mistakes

❌ Developing directly on main.

❌ Forgetting to switch after creating a branch.

❌ Using one branch for multiple features.

❌ Force deleting branches without verification.

---

# Interview Questions

## What is a Branch?

A branch is an independent line of development used to isolate feature development and bug fixes.

---

## Why do companies use branches?

To allow multiple developers to work independently without affecting the stable main branch.

---

## Does git branch switch to the new branch?

No.

It only creates the branch.

---

## Which command creates and switches to a branch?

Modern Git:

```bash
git switch -c feature/login
```

Older Git:

```bash
git checkout -b feature/login
```

---

## Difference between git branch -d and git branch -D?

-d deletes only merged branches.

-D force deletes even if not merged.

---

## Why should developers avoid working directly on main?

Because main should always contain stable, tested and deployable code.

---

## Should one feature branch be reused for another feature?

No.

Each new feature should have its own branch.

---

# Quick Revision

✔ Branch = Independent line of development

✔ Main = Stable branch

✔ git branch = Create branch

✔ git switch = Switch branch

✔ git switch -c = Create + Switch

✔ git branch = List branches

✔ git branch -d = Safe delete

✔ git branch -D = Force delete

✔ One Feature = One Branch

✔ Merge after Testing & Code Review

✔ Delete merged branches

---

# Commands Cheat Sheet

```bash
# Create Branch
git branch feature/login

# List Branches
git branch

# Switch Branch
git switch feature/login

# Create + Switch
git switch -c feature/login

# Push Branch
git push origin feature/login

# Safe Delete
git branch -d feature/login

# Force Delete
git branch -D feature/login
```

---
