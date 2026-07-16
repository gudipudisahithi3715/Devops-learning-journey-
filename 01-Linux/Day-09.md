# Git Fundamentals

## What is Version Control?

Version Control is a system that tracks changes made to files over time. It allows developers to maintain version history, restore previous versions, and collaborate efficiently.

### Why do we need Version Control?

Without Version Control:

- No history of changes
- Difficult to restore previous versions
- Poor collaboration
- High chance of overwriting code

With Version Control:

- Tracks every change
- Easy rollback
- Better collaboration
- Supports branching and merging
- Maintains complete project history

---

# Types of Version Control Systems

## 1. Centralized Version Control System (CVCS)

Examples:

- SVN
- CVS

Architecture:

Developer → Central Server ← Developer

Characteristics:

- Single central repository
- Internet/server required
- Single point of failure

---

## 2. Distributed Version Control System (DVCS)

Examples:

- Git
- Mercurial

Architecture:

Developer ↔ Local Repository ↔ Remote Repository

Characteristics:

- Every developer has a complete repository
- Can work offline
- Faster
- No single point of failure

---

# What is Git?

Git is a Distributed Version Control System (DVCS) used to:

- Track source code changes
- Maintain version history
- Collaborate with teams
- Restore previous versions
- Manage branches

Created by **Linus Torvalds** in 2005.

---

# Git vs GitHub

| Git | GitHub |
|------|---------|
| Version Control Tool | Cloud Hosting Platform |
| Installed locally | Hosted online |
| Tracks changes | Stores repositories |
| Works offline | Requires internet for synchronization |

Git = Tool

GitHub = Cloud Storage for Git Repositories

---

# Git Architecture

```
Working Directory
        │
        ▼
Staging Area
        │
        ▼
Local Repository
        │
        ▼
Remote Repository (GitHub)
```

---

# Working Directory

The Working Directory is the project folder where developers:

- Create files
- Modify files
- Delete files

Example:

```
Project/

Main.java

README.md

application.yml
```

Files are **not tracked** until they are added to the Staging Area.

---

# Staging Area (Index)

The Staging Area is a temporary area where selected changes are prepared before creating a commit.

Purpose:

- Select specific files
- Prepare changes
- Create meaningful commits

Command:

```bash
git add filename
```

---

# Local Repository

The Local Repository stores:

- Commit history
- Branches
- Tags
- Git Objects
- Metadata

Location:

```
.git
```

Changes reach the Local Repository after:

```bash
git commit
```

---

# Remote Repository

A Remote Repository is hosted on:

- GitHub
- GitLab
- Azure Repos
- Bitbucket

Purpose:

- Backup
- Collaboration
- Code Sharing
- CI/CD Integration

Changes are uploaded using:

```bash
git push
```

---

# Git Repository

A Git Repository is a project directory containing a hidden:

```
.git
```

folder.

The `.git` folder stores:

- Commit history
- Branches
- References
- Configuration
- Objects

Without `.git`, Git cannot track versions.

---

# Git Configuration

Check Git Version:

```bash
git --version
```

Configure Username:

```bash
git config --global user.name "Your Name"
```

Configure Email:

```bash
git config --global user.email "your@email.com"
```

View Configuration:

```bash
git config --list
```

Set Default Branch:

```bash
git config --global init.defaultBranch main
```

---

# Global vs Local Configuration

Global:

- Applies to every repository
- Uses:

```bash
git config --global
```

Local:

- Applies only to the current repository
- Uses:

```bash
git config
```

---

# Basic Git Commands

## Initialize Repository

```bash
git init
```

Creates the hidden `.git` directory.

---

## Check Repository Status

```bash
git status
```

Shows:

- Untracked files
- Modified files
- Staged files

---

## Stage Files

```bash
git add filename
```

Stage all files:

```bash
git add .
```

Moves changes to the Staging Area.

---

## Commit Changes

```bash
git commit -m "Commit Message"
```

Creates a snapshot of staged changes.

---

## View Commit History

```bash
git log
```

Displays:

- Commit ID
- Author
- Date
- Commit Message

---

## Compare Changes

```bash
git diff
```

Displays differences between file versions.

---

# Complete Git Workflow

```
Create File
      │
      ▼
Working Directory
      │
git status
      │
Untracked
      │
git add
      │
Staging Area
      │
git commit
      │
Local Repository
      │
git push
      │
Remote Repository (GitHub)
```

---

# File Lifecycle

```
Create File

↓

Untracked

↓

git add

↓

Staged

↓

git commit

↓

Committed

↓

git push

↓

Remote Repository
```

---

# Common Interview Questions

## What is Git?

Git is a Distributed Version Control System used to track changes in source code.

---

## What is GitHub?

GitHub is a cloud-based platform that hosts Git repositories.

---

## Difference between Git and GitHub?

Git tracks changes locally.

GitHub stores repositories online.

---

## What does git init do?

Creates a hidden `.git` folder and converts a normal folder into a Git Repository.

---

## What is the Staging Area?

A temporary area where selected changes are prepared before creating a commit.

---

## Difference between git add and git commit?

git add:

Moves changes to the Staging Area.

git commit:

Stores staged changes permanently in the Local Repository.

---

## Difference between git commit and git push?

git commit:

Stores changes locally.

git push:

Uploads commits to the Remote Repository.

---

## What happens if the .git folder is deleted?

The project files remain.

Git history, branches, tags, and version tracking are lost.

---

## What does git status display?

Shows:

- Untracked files
- Modified files
- Staged files
- Current branch

---

## What does "working tree clean" mean?

It means:

- No modified files
- No staged files
- No untracked files

The Working Directory matches the latest commit.

---

# Key Points

- Git is a Distributed Version Control System.
- GitHub is a cloud platform for Git repositories.
- `git init` creates the `.git` folder.
- Files start as **Untracked**.
- `git add` stages files.
- `git commit` stores changes in the Local Repository.
- `git push` uploads commits to GitHub.
- `git status` is the most frequently used Git command.
- The Staging Area allows selective commits.
- The `.git` folder contains all version history and repository metadata.