# Branch Management

Complete guide on how to create, switch, delete, and manage branches in Git. You'll learn branching strategies, naming conventions, and best practices.

## Table of Contents

1. [What is a Branch?](#what-is-a-branch)
2. [Special Branches](#special-branches)
3. [Create Branches](#create-branches)
4. [Switch Between Branches](#switch-between-branches)
5. [View Branches](#view-branches)
6. [Delete Branches](#delete-branches)
7. [Branch Naming](#branch-naming)
8. [Branching Strategies](#branching-strategies)
9. [Sync Branches](#sync-branches)
10. [Troubleshooting](#troubleshooting)

---

## What is a Branch?

### Definition

A **branch** is an independent line of development. It allows you to work on new features, fixes, or experiments without affecting the main code.

### How It Works

Each branch has its own commit history. You can:

- Work on multiple features simultaneously
- Isolate experimental changes
- Collaborate without conflicts
- Merge changes when ready

### Analogy

Think of a tree:

- **Trunk:** The main branch (main/master)
- **Branches:** Independent lines of work
- **Leaves:** Commits in each branch
- **Grafts:** Merges that join branches

---

## Special Branches

### Main (or Master)

**Before:** Called `master`
**Now:** Called `main` (for inclusivity)

**Characteristics:**

- ✅ The main branch of the project
- ✅ Stable code ready for production
- ✅ Usually protected (requires review)
- ✅ Each commit is a stable version

**Typical Protection:**

- Requires Pull Request to merge
- Requires code review
- Must pass all tests
- No force push

### Develop (or Development)

**Characteristics:**

- ✅ Development branch of the project
- ✅ Integrates features before main
- ✅ May be less stable than main
- ✅ Integration point for features

**Typical Use:**

- Features branch from develop
- Tested in develop
- Merged to main when ready

### Feature Branches

**Characteristics:**

- ✅ Temporary branches for features
- ✅ Created from develop
- ✅ Deleted after merge
- ✅ Isolated from main code

### Hotfix Branches

**Characteristics:**

- ✅ For urgent production fixes
- ✅ Created from main
- ✅ Merged immediately to main and develop
- ✅ Deleted after merge

---

## Create Branches

### Option 1: Create Branch Locally

```bash
# Create branch
git branch branch-name

# Verify it was created
git branch
```

**Note:** Only creates locally, doesn't switch to it.

### Option 2: Create and Switch in One Step

```bash
# Create and switch (recommended)
git checkout -b branch-name

# Or with git switch (more modern)
git switch -c branch-name
```

### Option 3: Create Branch from Another Branch

```bash
# Be on the branch you want to create from
git checkout feature/other-branch

# Create new branch (secondary branch)
git checkout -b feature/new-branch
```

### Option 4: Create Branch from Specific Commit

```bash
# Create branch at a previous commit
git checkout -b branch-name abc123def

# abc123def is the commit hash
```

### Option 5: Create Remote Branch

```bash
# Create locally
git checkout -b branch-name

# Push (creates in remote)
git push -u origin branch-name

# -u sets the remote branch as upstream
```

**Verify:**

```bash
git branch -a  # View all (local and remote)
```

---

## Switch Between Branches

### Option 1: git checkout

```bash
# Switch to another branch
git checkout branch-name

# Switch to main
git checkout main
```

**Note:** Must not have uncommitted changes. Otherwise, Git will warn you.

### Option 2: git switch (More Modern)

```bash
# Switch to another branch
git switch branch-name

# Switch to main
git switch main
```

**Advantage:** Clearer syntax than checkout.

### Option 3: git switch - (Previous Branch)

```bash
# Switch to previous branch (like cd -)
git switch -

# Or with checkout
git checkout -
```

### Verify Current Branch

```bash
# View current branch
git branch

# Current branch has an asterisk (*)
# Example:
#   main
# * feature/new-branch
#   develop
```

### Switch and Create if Doesn't Exist

```bash
# Creates if doesn't exist
git switch -c branch-name

# Or with checkout
git checkout -b branch-name
```

---

## View Branches

### View Local Branches

```bash
git branch
```

You'll see:

```
  develop
* main
  feature/login
```

The asterisk (\*) indicates the current branch.

### View Remote Branches

```bash
git branch -r
```

You'll see:

```
  origin/main
  origin/develop
  origin/feature/login
```

### View All Branches

```bash
git branch -a
```

You'll see:

```
  develop
* main
  feature/login
  remotes/origin/main
  remotes/origin/develop
  remotes/origin/feature/login
```

### View Branches with More Info

```bash
# Last commit of each branch
git branch -v

# You'll see:
#   develop    abc123d Merge branch 'feature/logout'
# * main       def456e Add production build script
#   feature/l  ghi789f Add login form
```

### View Merged Branches

```bash
# Branches already merged to main
git branch --merged

# Branches NOT merged
git branch --no-merged
```

### View Branches by Date

```bash
# Branches sorted by last activity
git branch -v --sort=-committerdate
```

---

## Delete Branches

### Delete Local Branch

```bash
# Delete branch (safe - checks if merged)
git branch -d branch-name

# Force delete (without checking)
git branch -D branch-name
```

**Difference:**

- `-d`: Safe. Doesn't delete if not merged.
- `-D`: Forces. Deletes regardless.

**Example:**

```bash
# You developed feature/login and merged it
git branch -d feature/login
# ✓ Deleted

# If you want to force
git branch -D feature/login
```

### Delete Remote Branch

```bash
# Delete on origin
git push origin --delete branch-name

# Or alternative syntax
git push origin -d branch-name

# Or old version
git push origin :branch-name
```

**Example:**

```bash
git push origin --delete feature/logout
```

### Clean Up Deleted Remote Branches

If someone deletes a remote branch, your local still sees it:

```bash
# Sync remote branches
git fetch --prune

# Or specific remote
git fetch --prune origin
```

---

## Branch Naming

### Recommended Convention

```
type/description-in-lowercase
```

### Common Types

| Type     | Purpose                  | Example                 |
| -------- | ------------------------ | ----------------------- |
| feature  | New feature              | feature/add-cart        |
| fix      | Bug fix                  | fix/email-validation    |
| hotfix   | Urgent fix               | hotfix/payment-crash    |
| refactor | Code reorganization      | refactor/improve-api    |
| docs     | Documentation changes    | docs/update-readme      |
| test     | Add/improve tests        | test/login-coverage     |
| perf     | Performance improvements | perf/optimize-db        |
| ci       | CI/CD changes            | ci/github-actions-setup |
| chore    | Minor tasks              | chore/update-deps       |

### Good Examples

```
✓ feature/add-oauth-authentication
✓ fix/404-error-product-page
✓ hotfix/payment-crash
✓ refactor/simplify-modal-component
✓ docs/installation-guide
```

### Bad Examples

```
✗ new-branch
✗ changes
✗ temporary
✗ test123
✗ FEATURE/ADD-CART (use lowercase)
```

### Naming Rules

✅ **Use lowercase**
✅ **Separate with hyphens (-)**
✅ **No spaces**
✅ **No special characters**
✅ **Descriptive but concise**
✅ **Maximum 50 characters**

---

## Branching Strategies

### Git Flow

**Structure:**

```
main (releases)
 └─ develop (integration)
     ├─ feature/... (development)
     ├─ release/... (release prep)
     └─ hotfix/... (urgent fixes)
```

**Flow:**

1. You create feature from develop
2. Work on feature
3. Make Pull Request to develop
4. Review and merge
5. When ready, release goes to main
6. If emergency, hotfix goes to main and develop

**Advantages:**
✅ Clear structure
✅ Good for planned releases
✅ Clear separation between dev and production

**Disadvantages:**
❌ More complex
❌ Lots of merging
❌ Not ideal for frequent CI/CD

### GitHub Flow (Recommended for Agile Teams)

**Structure:**

```
main (always deployable)
 └─ feature/... (work in progress)
```

**Flow:**

1. You create feature from main
2. Work on feature
3. Make Pull Request to main
4. Review and merge
5. Deploy to production immediately

**Advantages:**
✅ Simple and direct
✅ Continuous deployment
✅ Fewer conflicts
✅ Ideal for CI/CD

**Disadvantages:**
❌ Main must always be stable
❌ Less control over versions

### Trunk-Based Development

**Structure:**

```
main (everyone commits here)
```

**Flow:**

1. Everyone works on main
2. Small, frequent commits
3. Feature flags to control new functionality
4. Automated tests on each commit

**Advantages:**
✅ Very simple
✅ Fewer conflicts
✅ Maximum continuous integration

**Disadvantages:**
❌ Requires very good tests
❌ Requires team discipline
❌ Difficult for large teams

### Recommendation by Team

- **Small teams:** GitHub Flow
- **Medium teams:** GitHub Flow or Git Flow
- **Large teams:** Git Flow
- **Very agile teams:** Trunk-Based

---

## Sync Branches

### Update Local Branch from Remote

```bash
# Fetch changes from remote
git fetch origin branch-name

# Update local branch
git pull origin branch-name

# Or in one step
git pull
```

### Update Feature from Develop

```bash
# Be on feature branch
git checkout feature/login

# Pull changes from develop
git pull origin develop

# If conflicts, resolve and commit
git add .
git commit -m "Resolve conflicts with develop"
git push
```

### Sync All Branches

```bash
# Get all updates
git fetch --all

# View all (you'll see remote changes)
git branch -a
```

---

## Troubleshooting

### "error: pathspec 'branch-name' did not match any file"

**Cause:** Branch doesn't exist.

**Solution:**

```bash
# View available branches
git branch -a

# Create the branch first
git checkout -b branch-name
```

### "fatal: Cannot switch to a branch with uncommitted changes"

**Cause:** You have unsaved changes.

**Solution:**

```bash
# Option 1: Commit changes
git add .
git commit -m "Intermediate changes"
git checkout other-branch

# Option 2: Save temporarily (stash)
git stash
git checkout other-branch
git stash pop

# Option 3: Discard changes
git checkout -- .
git checkout other-branch
```

### "fatal: A branch named 'name' already exists"

**Cause:** Branch already exists.

**Solution:**

```bash
# View branches
git branch -a

# Use different name
git checkout -b different-name

# Or switch to existing branch
git checkout branch-name
```

### "error: src refspec main does not match any"

**Cause:** Branch doesn't exist in remote or Git is confused.

**Solution:**

```bash
# Create commit in main first
git commit --allow-empty -m "Initial commit"

# Or push current branch
git push -u origin branch-name
```

### Remote branch deleted but still visible locally

```bash
# Sync
git fetch --prune

# Or specific
git fetch --prune origin
```

---

## Command Summary

```bash
# CREATE
git checkout -b branch-name          # Create and switch
git branch branch-name                # Only create

# VIEW
git branch                           # Local
git branch -a                        # All
git branch -v                        # With info

# SWITCH
git checkout branch-name             # Switch (old)
git switch branch-name               # Switch (modern)

# DELETE
git branch -d branch-name            # Safe
git branch -D branch-name            # Force
git push origin --delete branch-name # Remote

# SYNC
git fetch origin                     # Fetch changes
git pull origin branch-name          # Fetch and merge
git push -u origin branch-name       # Push

# INFO
git branch --merged                  # Merged
git branch --no-merged              # Not merged
```

---

## Next Steps

1. **[Workflow](workflow.md)** - Team workflows
2. **[Tracking Changes](tracking-changes.md)** - Audit code
3. **[SSH/HTTPS Authentication](authentication-ssh-https.md)** - Review

---

**Previous:** [GPG Configuration](gpg-configuration.md) | **Next:** [Workflow](workflow.md)
