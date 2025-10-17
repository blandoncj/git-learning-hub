# Basic Git Concepts

A comprehensive guide to the terminology and fundamental concepts you need to understand to work with Git.

## Table of Contents

1. [Repository](#repository)
2. [Commit](#commit)
3. [Branch](#branch)
4. [Push and Pull](#push-and-pull)
5. [Merge](#merge)
6. [Conflict](#conflict)
7. [Remote](#remote)
8. [Staging Area](#staging-area)
9. [Head](#head)
10. [Tag](#tag)

---

## Repository

### Definition

A **repository** (or "repo") is a data storage that contains:

- All the files of your project
- The complete history of changes
- Information about authors and dates
- Branches and tags

### Types

**Local Repository**

- Located on your computer
- Contains a complete copy of the history
- Allows you to work offline

**Remote Repository**

- Located on a server (GitHub, GitLab, Bitbucket, etc.)
- Shared between developers
- The "official" version of the project

### Create a Repository

```bash
# Create a new local repository
git init

# Clone an existing repository
git clone https://github.com/username/repository.git
```

---

## Commit

### Definition

A **commit** is a "snapshot" of your project's state at a specific point in time. It's like a photograph of all your files at that moment.

### Components of a Commit

- **Changes:** Added, modified, or deleted lines
- **Message:** Description of what changed and why
- **Author:** Who made the change
- **Date:** When it was made
- **Hash:** Unique identifier (e.g., `a1b2c3d4e5f6...`)

### Examples of Commits

```bash
# Create a simple commit
git commit -m "Add login function"

# Commit with more detailed description
git commit -m "Add user authentication" -m "Implements JWT for session tokens"

# View commit history
git log
```

### Best Practices

✓ **Clear and descriptive messages**

```
Correct: "Fix email validation bug"
Incorrect: "fixes"
```

✓ **Atomic commits** (one change per commit)

```
Correct: 3 separate commits for 3 changes
Incorrect: 1 giant commit with 10 changes
```

✓ **Messages in present tense**

```
Correct: "Add validation"
Incorrect: "Added validation" or "Adding validation"
```

---

## Branch

### Definition

A **branch** is an independent line of development. It allows you to work on new features without affecting the main branch.

### Special Branches

**Main (or Master)**

- The main and stable branch of the project
- Contains production-ready code
- Usually protected and requires review

**Develop**

- Development branch
- Integrates features before going to main
- May be less stable than main

### Basic Operations

```bash
# View local branches
git branch

# View all branches (local and remote)
git branch -a

# Create a new branch
git branch branch-name

# Switch to another branch
git checkout branch-name

# Or combined: create and switch
git checkout -b branch-name

# Delete a local branch
git branch -d branch-name

# Delete a remote branch
git push origin --delete branch-name
```

### Naming Conventions

It's recommended to follow conventions:

```
feature/feature-name             # New feature
fix/bug-description              # Bug fix
hotfix/description               # Urgent fix
refactor/description             # Code refactoring
docs/description                 # Documentation changes
```

Example: `feature/add-shopping-cart`

---

## Push and Pull

### Push (Upload)

**Definition:** Send your local commits to the remote repository.

```bash
# Push current branch to remote
git push

# Push specific branch
git push origin branch-name

# Push all branches
git push --all
```

### Pull (Download)

**Definition:** Download changes from the remote repository and integrate them locally.

```bash
# Pull changes from current branch
git pull

# Pull changes from specific branch
git pull origin branch-name
```

### Difference: Pull = Fetch + Merge

```bash
# Pull is equivalent to:
git fetch        # Download changes
git merge        # Integrate changes
```

---

## Merge

### Definition

**Merge** combines changes from one branch with another. Typically, you integrate a feature branch with the main branch.

### Example

```bash
# Be on the branch where you want to bring changes
git checkout main

# Merge another branch
git merge feature/new-feature
```

### Types of Merge

**Fast-Forward Merge**

- Occurs when there are no changes in the destination branch
- The history is linear

**Merge Commit**

- Creates a new merge commit
- The history shows the merged branch

```bash
# Force a merge commit even if fast-forward is possible
git merge --no-ff feature/branch
```

---

## Conflict

### Definition

A **conflict** occurs when Git cannot automatically merge changes because two branches modify the same lines of code.

### Detect a Conflict

```bash
git merge branch-name
# Error: CONFLICT (content merge conflict)
```

### Resolve a Conflict

Git marks conflicts in the files:

```
<<<<<<< HEAD
your version of the code
=======
version from the other branch
>>>>>>> branch-name
```

**Steps to resolve:**

1. Open the conflicted file
2. Decide which changes to keep
3. Delete the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
4. Save the file
5. Make a commit

```bash
git add resolved-file
git commit -m "Resolve merge conflict"
```

---

## Remote

### Definition

A **remote** is a reference to a remote repository. By default, it's called `origin`.

### Operations

```bash
# View configured remotes
git remote -v

# Add a new remote
git remote add name url

# Change URL of a remote
git remote set-url origin new-url

# Remove a remote
git remote remove name

# View detailed information about a remote
git remote show origin
```

### Example

```bash
# GitHub creates origin by default
git remote add origin https://github.com/username/repo.git

# View what's configured
git remote -v
# origin  https://github.com/username/repo.git (fetch)
# origin  https://github.com/username/repo.git (push)
```

---

## Staging Area

### Definition

The **staging area** (or "index") is an intermediate area where you prepare changes before making a commit.

### Why It Exists

It allows you to select which changes to include in the next commit, not all at once.

### Commands

```bash
# View current status
git status

# Add specific file to staging area
git add file.txt

# Add all changes
git add .

# Remove file from staging area (without losing changes)
git restore --staged file.txt

# View changes in staging area
git diff --staged
```

### Complete Flow

```bash
# 1. Make changes to files
# ... edit file.txt ...

# 2. View status
git status
# file.txt modified

# 3. Add to staging area
git add file.txt

# 4. Make commit
git commit -m "Describe change"

# 5. Send to remote
git push
```

---

## HEAD

### Definition

**HEAD** is a pointer that indicates which branch you're currently on. It usually points to the branch and the most recent commit.

### Examples

```bash
# View what HEAD points to
cat .git/HEAD

# Change HEAD (move to another branch)
git checkout main

# View differences between HEAD and your current branch
git diff HEAD

# View differences between commits
git show HEAD~1  # One commit before
git show HEAD~2  # Two commits before
```

---

## Tag

### Definition

A **tag** is a reference to a specific point in history, usually to mark important software versions.

### Types

**Lightweight Tag:** Just a name pointing to a commit

```bash
git tag v1.0.0
```

**Annotated Tag:** Includes additional information (author, date, message)

```bash
git tag -a v1.0.0 -m "Version 1.0.0"
```

### Operations

```bash
# List tags
git tag

# View tag information
git show v1.0.0

# Create a tag
git tag v1.0.0

# Delete a local tag
git tag -d v1.0.0

# Delete a remote tag
git push origin --delete v1.0.0

# Push tags to remote
git push origin v1.0.0
git push origin --tags  # Push all
```

---

## Visual Summary

```
REMOTE REPOSITORY (GitHub, GitLab, etc.)
         ↓ git clone / git pull
         ↓
┌─────────────────────┐
│  LOCAL REPOSITORY   │
│                     │
│  ┌───────────────┐  │
│  │  Working Dir  │  │  ← Your project
│  │  (workspace)  │  │
│  └────────┬──────┘  │
│           │ git add
│           ↓         │
│  ┌─────────────────┐ │
│  │  Staging Area   │ │  ← Files ready
│  │   (Index)       │ │
│  └────────┬────────┘ │
│           │ git commit
│           ↓          │
│  ┌─────────────────┐ │
│  │ Git Directory   │ │  ← Complete history
│  │ (.git)          │ │
│  │                 │ │
│  │ Commits, Branches,│
│  │ Tags, etc.      │ │
│  └─────────────────┘ │
└─────────────────────┘
         ↑ git push
         ↑
REMOTE REPOSITORY
```

---

## Next Steps

Now that you understand the basic concepts:

1. **[SSH vs HTTPS Authentication](/guides/en/authentication-ssh-https.md)** - How to connect to remote repositories
2. **[Basic Level Exercises](/exercises/basic-level/)** - Practice what you've learned

---

**Previous:** [Initial Configuration](initial-configuration.md)
