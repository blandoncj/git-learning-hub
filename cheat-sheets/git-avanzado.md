# Git Advanced - Cheat Sheet

Quick reference of advanced Git commands for experienced users.

## Interactive Rebase

```bash
# Rebase last 3 commits
git rebase -i HEAD~3

# Rebase on branch
git rebase -i main

# Options in editor:
# pick = use commit
# reword = edit message
# squash = merge with previous
# fixup = like squash without message
# drop = remove
```

---

## Cherry-pick

```bash
# Apply specific commit to current branch
git cherry-pick abc123def

# Cherry-pick with editing
git cherry-pick -e abc123def

# Cherry-pick multiple commits
git cherry-pick abc123def ghi789jkl

# Cherry-pick range
git cherry-pick abc123def..mno456pqr
```

---

## Reflog and Recovery

```bash
# View HEAD history
git reflog

# View specific branch history
git reflog show branch-name

# Go to specific point in reflog
git checkout @{2}

# Recover deleted branch
git checkout -b branch-name abc123def
```

---

## Advanced Stash

```bash
# Save with message
git stash save "Description"

# Save only tracked files
git stash push -m "Message"

# Save specific file changes
git stash push file.js

# Apply stash without deleting
git stash apply

# View stash content
git stash show -p stash@{0}

# Create branch from stash
git stash branch branch-name

# Clean all stashes
git stash clear
```

---

## Rebase

```bash
# Rebase on branch
git rebase main

# Continue rebase after conflict
git rebase --continue

# Abort rebase
git rebase --abort

# Rebase and merge with fast-forward
git rebase main && git merge --ff-only

# Interactive rebase
git rebase -i main
```

---

## Reset and Restore

```bash
# Soft reset (keeps changes)
git reset --soft abc123def

# Mixed reset (default)
git reset --mixed abc123def

# Hard reset (discard everything)
git reset --hard abc123def

# Reset specific file
git reset abc123def -- file.js

# Restore file
git restore file.js

# Restore from specific commit
git restore -s abc123def file.js
```

---

## Clean

```bash
# Preview what would be cleaned
git clean -fd --dry-run

# Clean untracked files
git clean -fd

# Clean including directories
git clean -fdx

# Interactive clean
git clean -fdi
```

---

## Bisect - Find the Bug

```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark old commit as good
git bisect good abc123def

# Git shows commits to test
# Mark as good or bad
git bisect good  # or git bisect bad

# When you find the bug
git bisect reset
```

---

## Filter-branch

```bash
# Change author globally
git filter-branch --env-filter '
if [ "$GIT_COMMITTER_NAME" = "Old Name" ]
then
  export GIT_COMMITTER_NAME="New Name"
  export GIT_AUTHOR_NAME="New Name"
fi' -- --all

# Remove file from history
git filter-branch --tree-filter 'rm -f file.txt' HEAD
```

---

## Advanced Merge

```bash
# Merge without fast-forward
git merge --no-ff branch-name

# Squash merge
git merge --squash branch-name
git commit

# Merge with specific strategy
git merge -s recursive -X theirs branch-name

# Merge with automatic resolution
git merge -X ours branch-name
```

---

## Advanced Log

```bash
# View changes line by line
git log -p

# View statistics
git log --stat

# View only files changed
git log --name-only

# View with custom format
git log --pretty=format:"%h %an %ar %s"

# View commits that touched specific lines
git log -L 10,20:file.js

# View commits that added/removed text
git log -S "specific_text"

# View commits with regex search
git log -G "pattern"

# View with decorated tree
git log --decorate --graph --oneline
```

---

## Advanced Diff

```bash
# Diff between commits
git diff abc123d def456g

# Diff between branches
git diff main feature/branch

# Diff only filenames
git diff --name-only

# Diff only changed files
git diff --name-status

# Diff ignoring spaces
git diff -w

# Diff with extended context
git diff -U10

# Diff word by word
git diff --word-diff

# Diff specific code
git diff -p file.js

# Diff with statistics
git diff --stat
```

---

## Advanced Blame

```bash
# Blame ignoring spaces
git blame -w file.js

# Blame range of lines
git blame -L 10,50 file.js

# Blame with email
git blame -e file.js

# Blame detecting line movements
git blame -M file.js

# Blame ignoring format changes
git blame --ignore-rev abc123def
```

---

## Worktree

```bash
# Create worktree in new branch
git worktree add ../path/to/worktree -b branch-name

# List worktrees
git worktree list

# Remove worktree
git worktree remove path/to/worktree

# Create worktree in existing branch
git worktree add ../path/to/worktree existing-branch-name
```

---

## Submodules

```bash
# Add submodule
git submodule add https://github.com/user/repo.git path

# Clone with submodules
git clone --recurse-submodules url

# Update submodules
git submodule update --remote

# Initialize submodules
git submodule init

# Remove submodule
git rm --cached path && rm -rf path
```

---

## Tags

```bash
# Create lightweight tag
git tag v1.0.0

# Create annotated tag
git tag -a v1.0.0 -m "Version 1.0.0"

# List tags
git tag -l

# View specific tag
git show v1.0.0

# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0

# Push tag
git push origin v1.0.0

# Push all tags
git push origin --tags
```

---

## Sparse Checkout

```bash
# Clone only part of repo
git clone --sparse url

# Define what folders to clone
git sparse-checkout init
git sparse-checkout set folder1 folder2

# View configuration
git sparse-checkout list

# Disable sparse checkout
git sparse-checkout disable
```

---

## Patch

```bash
# Create patch
git format-patch -1 abc123def

# Apply patch
git apply file.patch

# Apply patch with am (preserves author)
git am file.patch

# View patch before applying
git apply --check file.patch
```

---

## Advanced Shortcuts

```bash
# View unstaged changes
git diff

# View staged changes
git diff --staged

# View all differences
git diff HEAD

# View what changes between branches
git diff main...feature

# View commits in one branch but not other
git log main..feature

# View common commits
git merge-base main feature

# View divergence
git log --oneline --graph main feature

# View upcoming changes to sync
git log @{u}..
```

---

## Performance

```bash
# Check repository integrity
git fsck

# Clean and optimize repo
git gc

# Aggressive clean
git gc --aggressive

# View repo size
du -sh .git

# View large objects
git rev-list --all --objects | \
  sed -n $(git rev-list --objects --all | \
  cut -f1 -d' ' | \
  git cat-file --batch-check | \
  grep blob | \
  sort -k3 -n | \
  tail -10 | \
  while read hash type size; do \
    echo -n "-e s/$hash/$size/p "; \
  done) | \
  sort -k1 -n -r | \
  head -10
```

---

## Advanced Aliases

```bash
# View unmerged commits
git config --global alias.unmerged \
  "branch --no-merged"

# View complete tree
git config --global alias.tree \
  "log --oneline --graph --all --decorate"

# Repository statistics
git config --global alias.stats \
  "shortlog -s -n"

# View file changes
git config --global alias.changes \
  "log -p"

# Diff between branches
git config --global alias.diffb \
  "diff main..."

# Show current branch name
git config --global alias.branch-name \
  "rev-parse --abbrev-ref HEAD"

# Push current branch
git config --global alias.pushc \
  "push -u origin HEAD"

# Automatic sync
git config --global alias.sync \
  "pull --rebase && push"
```

---

**For more information:** [Advanced Guides](/guides/en) | [Complete Documentation](/docs/en)
