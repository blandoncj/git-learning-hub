# Workflow

Complete guide on how to work efficiently with Git in teams. You'll learn best practices, commit conventions, Pull Request flow, and conflict resolution.

## Table of Contents

1. [Basic Workflow](#basic-workflow)
2. [Commit Conventions](#commit-conventions)
3. [Pull Requests](#pull-requests)
4. [Code Review](#code-review)
5. [Merge and Conflicts](#merge-and-conflicts)
6. [Rebase vs Merge](#rebase-vs-merge)
7. [Squash Commits](#squash-commits)
8. [Best Practices](#best-practices)

---

## Basic Workflow

### Complete Workflow Step by Step

#### Step 1: Clone the Repository

```bash
git clone https://github.com/username/project.git
cd project
```

#### Step 2: Create a Branch for Your Feature

```bash
# Make sure you're on the main branch
git checkout main
git pull origin main

# Create feature branch
git checkout -b feature/new-functionality
```

#### Step 3: Make Changes

```bash
# Edit files
nano file.txt

# View changes
git status
```

#### Step 4: Prepare Changes (Staging)

```bash
# Add specific file
git add file.txt

# Or add all changes
git add .
```

#### Step 5: Make Commit

```bash
git commit -m "Clear description of change"
```

#### Step 6: Make Push

```bash
# First time (sets upstream)
git push -u origin feature/new-functionality

# Next times
git push
```

#### Step 7: Create Pull Request

1. Go to GitHub/GitLab/Bitbucket
2. Click "Create Pull Request" or "New Merge Request"
3. Select base branch (main or develop)
4. Add description
5. Assign reviewers
6. Click "Create"

#### Step 8: Code Review

- Reviewers examine your code
- May request changes
- You make adjustments and push again

#### Step 9: Merge

When PR is approved:

1. A maintainer clicks "Merge"
2. Chooses merge strategy (merge commit, squash, rebase)
3. Confirms merge

#### Step 10: Clean Up

```bash
# Delete local branch
git branch -d feature/new-functionality

# Update main locally
git checkout main
git pull origin main
```

---

## Commit Conventions

### Conventional Commits Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Commit Types

| Type     | Use                       | Example                           |
| -------- | ------------------------- | --------------------------------- |
| feat     | New feature               | feat(auth): add Google login      |
| fix      | Bug fix                   | fix(checkout): fix payment error  |
| docs     | Documentation changes     | docs(readme): update instructions |
| style    | Format changes (no logic) | style: fix indentation            |
| refactor | Code reorganization       | refactor(api): simplify endpoints |
| test     | Add/modify tests          | test(login): increase coverage    |
| perf     | Performance improvements  | perf(db): optimize queries        |
| ci       | CI/CD changes             | ci: add GitHub Actions            |
| chore    | Minor tasks               | chore: update dependencies        |

### Good Commit Examples

```
✓ feat(cart): add quantity functionality
✓ fix(validation): fix empty email error
✓ docs(installation): add Windows steps
✓ refactor(components): extract common logic
✓ test(auth): add JWT tests
```

### Bad Commit Examples

```
✗ changes
✗ fix bug
✗ Updates
✗ WIP
✗ asdfghjkl
✗ TODO: fix later
```

### Message Writing Guide

**Subject:**

- Maximum 50 characters
- First letter capitalized
- Imperative ("add", not "added")
- No period at the end
- Clear and descriptive

**Body (optional):**

- Maximum 72 characters per line
- Explains WHAT and WHY, not HOW
- Separate from subject with blank line

**Complete example:**

```
feat(cart): add stock validation

When a user tries to add a product,
it now checks if stock is sufficient.
If no stock, displays error message.

Fixes issue #123
```

---

## Pull Requests

### Create a Pull Request

#### On GitHub

1. Push to your branch
2. Go to repository
3. You'll see "Compare & pull request"
4. Click it
5. Fill title and description
6. Click "Create pull request"

#### On GitLab

1. Push to your branch
2. Go to repository
3. Go to "Merge requests"
4. Click "New merge request"
5. Select branches (source → target)
6. Fill description
7. Click "Create merge request"

#### On Bitbucket

1. Push to your branch
2. Go to repository
3. Go to "Pull requests"
4. Click "Create pull request"
5. Select branches
6. Fill description
7. Click "Create"

### Pull Request Description

```markdown
## Description

Brief description of what this PR does.

## Type of Change

- [ ] New feature
- [ ] Bug fix
- [ ] Breaking change

## Changes Made

- Change 1
- Change 2
- Change 3

## Testing

Describe how you tested the changes.

## Checklist

- [ ] My code follows project conventions
- [ ] I've updated documentation
- [ ] I've added tests
- [ ] All tests pass
- [ ] No conflicts with base branch

## Screenshots (if applicable)

Add screenshots of visual changes.

## Related Issues

Closes #123
```

### Handle Feedback

**When reviewer requests changes:**

1. Make changes locally
2. Commit and push (no need to create new PR)
3. PR updates automatically
4. Reply to comments

**Never force push after creating a PR:**

```bash
# WRONG - Don't do this
git push --force

# RIGHT - Do this
git push
```

---

## Code Review

### For Developers

**Before creating a PR:**

- ✅ Your code works and is tested
- ✅ You followed project conventions
- ✅ No conflicts with base branch
- ✅ You wrote tests
- ✅ You documented important changes

**During review:**

- 📝 Reply to all comments
- 🔄 Make changes if needed
- 💬 Explain your decisions
- 🙏 Thank reviewers for feedback

### For Reviewers

**What to review:**

✅ **Functionality:** Does it do what it promises?
✅ **Readability:** Is the code understandable?
✅ **Tests:** Is there sufficient coverage?
✅ **Performance:** Are there obvious optimizations?
✅ **Security:** Are there vulnerabilities?
✅ **Conventions:** Does it follow project standards?

**How to comment:**

```
Bad:
"This is wrong"

Good:
"Here we could use .map() instead of a for loop for clarity"

Better:
"Here we could use .map() instead of a for loop for clarity.
Example:
const result = data.map(item => item.value);
This is more functional and easier to read."
```

---

## Merge and Conflicts

### Merge Types

#### Merge Commit

```bash
git checkout main
git pull origin main
git merge feature/new-functionality
```

**Result:** History shows the branch
**Use:** When you want to preserve branch history

#### Squash and Merge

```bash
git merge --squash feature/new-functionality
git commit
```

**Result:** Single commit in main
**Use:** When you want a clean history

#### Rebase and Merge

```bash
git rebase main
git checkout main
git merge --ff-only feature/new-functionality
```

**Result:** Linear history
**Use:** When you want clean, linear history

### Resolve Conflicts

#### Detect Conflict

```bash
git merge other-branch
# CONFLICT (content merge conflict) in file.txt
```

#### View Conflict

The file will have markers:

```
<<<<<<< HEAD
Your version of the code
=======
Version from the other branch
>>>>>>> branch-name
```

#### Resolve Manually

1. Open the file
2. Decide which code to keep
3. Delete the markers
4. Save the file

**Example:**

```
// BEFORE (with conflict)
<<<<<<< HEAD
function add(a, b) {
  return a + b;
}
=======
function add(a, b) {
  return a + b + 1;
}
>>>>>>> bug-fix

// AFTER (resolved)
function add(a, b) {
  return a + b;
}
```

#### Complete Merge

```bash
git add resolved-file.js
git commit -m "Resolve merge conflict"
git push
```

#### Use Merge Tools

```bash
# Visual Studio Code
git mergetool

# Or specific tool
git config merge.tool vscode
git mergetool
```

#### Abort Merge

If you want to cancel:

```bash
git merge --abort
```

---

## Rebase vs Merge

### Merge

**How it works:**

```
Before:
feature: A - B
           \
main:   C - D

After (merge):
feature: A - B
           \ |
main:   C - D - M (merge commit)
```

**Advantages:**
✅ Preserves complete history
✅ Safe for shared branches
✅ Easy to understand

**Disadvantages:**
❌ More complex history
❌ Many merge commits

**When to use:**

- On shared branches
- When history matters
- On public repositories

### Rebase

**How it works:**

```
Before:
feature: A - B
           \
main:   C - D

After (rebase):
feature:       A' - B'
               /
main:       C - D
```

**Advantages:**
✅ Linear, clean history
✅ Easy to follow
✅ Fewer commits

**Disadvantages:**
❌ Rewrites history
❌ Dangerous on shared branches
❌ Can be confusing

**When to use:**

- On local branches
- Before merging to main
- To clean personal history

### Interactive Rebase

```bash
# Rebase last 3 commits
git rebase -i HEAD~3
```

Opens an editor with options:

```
pick a1b2c3d Commit 1
pick d4e5f6g Commit 2
pick h7i8j9k Commit 3

# Options:
# p, pick = use commit
# r, reword = use but edit message
# s, squash = use but merge with previous
# f, fixup = like squash but without message
# d, drop = remove commit
```

---

## Squash Commits

### What is Squash?

Combine multiple commits into one.

**Before:**

```
a1b2c3d Add login function
d4e5f6g Fix login bug
h7i8j9k Update login tests
```

**After (squash):**

```
a1b2c3d Add login function with bugfixes
```

### Interactive Squash

```bash
# View last 3 commits
git log --oneline -3

# Squash
git rebase -i HEAD~3
```

Change `pick` to `squash` (or `s`):

```
pick a1b2c3d Add login function
squash d4e5f6g Fix login bug
squash h7i8j9k Update login tests
```

Save, and edit final message.

### Squash When Merging

On GitHub/GitLab/Bitbucket, many platforms allow "Squash and merge" directly.

---

## Best Practices

### ✅ What YOU SHOULD Do

**1. Atomic Commits**

```bash
# Good: Each commit does one thing
git commit -m "feat: add email field"
git commit -m "test: add email test"

# Bad: One commit does many things
git commit -m "feat: add email field, update tests, fix bug"
```

**2. Rebase Before Push**

```bash
# Update branch with main
git pull --rebase origin main

# This avoids unnecessary merge commits
```

**3. Review Before Push**

```bash
# See what you're about to push
git diff main origin/main
```

**4. Name Branches Descriptively**

```
✓ feature/add-shopping-cart
✗ feature/changes
```

### ❌ What YOU SHOULD NOT Do

**1. Force Push on Shared Branches**

```bash
# NEVER do this on public branches
git push --force
```

**2. Generic Commit Messages**

```bash
# Bad
git commit -m "fix"
git commit -m "updates"

# Good
git commit -m "fix(login): fix password validation"
```

**3. Unrelated Changes in One Commit**

```bash
# Bad: Mix changes
git add .
git commit -m "Various changes"

# Good: Separate commits
git add file1.js
git commit -m "feat: add function"
git add file2.js
git commit -m "fix: fix bug"
```

**4. Forget to Update with Main**

```bash
# Always update before creating PR
git pull origin main
```

**5. Incomplete Commits**

```bash
# Don't commit broken code
git add .
git commit -m "WIP - doesn't work yet"
```

### Recommended Daily Workflow

```bash
# 1. Morning: Update
git pull origin main

# 2. Create branch
git checkout -b feature/my-work

# 3. During day: Frequent commits
git add file1.js
git commit -m "feat: part 1"
git add file2.js
git commit -m "feat: part 2"

# 4. Before push: Clean up
git rebase -i origin/main  # Squash if needed

# 5. Push
git push -u origin feature/my-work

# 6. Create PR
# (on GitHub/GitLab/Bitbucket)

# 7. After merge: Clean up
git checkout main
git pull
git branch -d feature/my-work
```

---

## Next Steps

1. **[Tracking Changes](tracking-changes.md)** - Audit and tracking
2. **[Branch Management](branch-management.md)** - Review branches
3. **[Authentication](authentication-ssh-https.md)** - Review security

---

**Previous:** [Branch Management](branch-management.md) | **Next:** [Tracking Changes](tracking-changes.md)
