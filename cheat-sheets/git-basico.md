# Git Basic - Cheat Sheet

Quick reference of the most used Git commands for beginners.

## Initial Configuration

```bash
# Configure name
git config --global user.name "Your Name"

# Configure email
git config --global user.email "your.email@example.com"

# View configuration
git config --global --list
```

---

## Create and Clone Repositories

```bash
# Create new local repository
git init

# Clone remote repository
git clone https://github.com/username/repo.git

# Clone to specific folder
git clone https://github.com/username/repo.git folder-name
```

---

## View Status and Changes

```bash
# View current status
git status

# View short status
git status -s

# View changes without committing
git diff

# View changes in staging area
git diff --staged
```

---

## Add and Confirm Changes

```bash
# Add specific file
git add file.js

# Add all changes
git add .

# Add specific type of files
git add *.js

# Remove file from staging area
git restore --staged file.js

# Make commit
git commit -m "Description of change"

# Commit and add modified files
git commit -a -m "Description"

# Amend last commit
git commit --amend -m "New message"
```

---

## Branches

```bash
# View local branches
git branch

# View all branches
git branch -a

# Create branch
git branch branch-name

# Create and switch to branch
git checkout -b branch-name

# Or modern version
git switch -c branch-name

# Switch to branch
git checkout branch-name

# Or modern version
git switch branch-name

# Switch to previous branch
git checkout -

# Delete local branch
git branch -d branch-name

# Force deletion
git branch -D branch-name

# Delete remote branch
git push origin --delete branch-name
```

---

## Save Changes Temporarily

```bash
# Save changes without committing
git stash

# View list of stashes
git stash list

# Recover latest saved changes
git stash pop

# Recover specific stash
git stash pop stash@{0}

# Discard stash
git stash drop
```

---

## History

```bash
# View complete history
git log

# View last N commits
git log -n 5

# View in one line
git log --oneline

# View with branch graph
git log --oneline --graph --all

# View commits of a file
git log -- file.js

# View commits from author
git log --author="name"

# View commits from last 24 hours
git log --since="24 hours ago"
```

---

## Push and Pull

```bash
# Send changes to remote
git push

# Send specific branch
git push origin branch-name

# Send all branches
git push --all

# Download changes from remote
git pull

# Download changes without merging
git fetch

# Download specific branch changes
git pull origin branch-name
```

---

## Merge (Fusion)

```bash
# Merge branch into current branch
git merge branch-name

# View merged branches
git branch --merged

# View unmerged branches
git branch --no-merged

# Undo merge
git merge --abort
```

---

## Undo Changes

```bash
# Undo changes in file (before add)
git checkout -- file.js

# Or modern version
git restore file.js

# Undo changes in file (after add)
git restore --staged file.js

# Go back to previous commit
git checkout abc123def

# Create branch from previous commit
git checkout -b branch-name abc123def

# Revert commit keeping history
git revert abc123def

# Delete commits (be careful)
git reset --hard abc123def
```

---

## View Information

```bash
# View commit details
git show abc123def

# View only commit message
git show --format=fuller abc123def

# View who changed each line
git blame file.js

# View configured remotes
git remote -v

# View remote information
git remote show origin
```

---

## Useful Aliases

```bash
# View last commit
git config --global alias.last "log -1 HEAD"

# View log in one line
git config --global alias.lo "log --oneline --graph --all"

# View short status
git config --global alias.st "status -s"

# View differences
git config --global alias.d "diff"

# View branches
git config --global alias.br "branch -a"

# Add changes
git config --global alias.ad "add"

# Commit
git config --global alias.cm "commit -m"

# Usage: git last, git lo, git st, etc.
```

---

## Useful Combinations

```bash
# See what will change before push
git diff main origin/main

# View changes of your branch vs main
git diff main..

# Update branch with main without changing commits
git pull --rebase origin main

# View log with better format
git log --oneline --decorate --graph --all

# Make empty commit (for testing)
git commit --allow-empty -m "Empty commit"

# View commits not in main
git log main..
```

---

## Emergencies

```bash
# Recover deleted commit (view reflog)
git reflog

# Go to specific commit
git checkout abc123def

# Discard all changes
git reset --hard

# Clean untracked files
git clean -fd

# View pending changes before reset
git diff HEAD
```

---

## Quick Tips

✅ **Always update before working:**

```bash
git pull origin main
```

✅ **Review what you'll send:**

```bash
git diff main..
```

✅ **Make small, frequent commits**

✅ **Write clear messages**

✅ **Never force push on public branches**

✅ **If something goes wrong, git reflog saves you**

---

**For more information:** [Complete Documentation](/docs/en) | [Detailed Guides](/guides/en)
