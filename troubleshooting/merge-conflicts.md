# Merge Conflicts

Complete guide to understand, prevent, and resolve merge conflicts in Git.

## Table of Contents

1. [What is a Conflict?](#what-is-a-conflict)
2. [Detect Conflicts](#detect-conflicts)
3. [Resolve Conflicts](#resolve-conflicts)
4. [Tools to Resolve](#tools-to-resolve)
5. [Prevent Conflicts](#prevent-conflicts)
6. [Special Cases](#special-cases)

---

## What is a Conflict?

A **merge conflict** occurs when Git cannot automatically merge two changes because they modify the same lines of code differently.

### Visual Example

```
BEFORE (main branch):
function add(a, b) {
  return a + b;
}

Your change (feature branch):
function add(a, b) {
  return a + b + 1;  // Added +1
}

Other's change (also in main):
function add(a, b) {
  return a + b - 1;  // Added -1
}

RESULT: CONFLICT!
Git doesn't know which version to keep.
```

---

## Detect Conflicts

### During Merge

```bash
git merge feature-branch
# CONFLICT (content): Merge conflict in file.js
# Automatic merge failed; fix conflicts and then commit the result.
```

### During Pull

```bash
git pull origin main
# CONFLICT (content): Merge conflict in file.js
```

### During Rebase

```bash
git rebase main
# CONFLICT (content): Merge conflict in file.js
# Could not apply abc123d... commit message
```

### View Conflicted Files

```bash
# View status
git status

# Output will show:
# Unmerged paths:
#   both modified:   file.js
```

---

## Resolve Conflicts

### Step 1: Identify Files

```bash
git status

# Or view only conflicted files
git diff --name-only --diff-filter=U
```

### Step 2: Open Conflicted File

Conflicts are marked like this:

```javascript
function add(a, b) {
<<<<<<< HEAD
  return a + b + 1;  // Your version
=======
  return a + b - 1;  // Other version
>>>>>>> feature-branch
}
```

**Marker parts:**

- `<<<<<<< HEAD` - Start of your version (current branch)
- `=======` - Separator
- `>>>>>>> branch-name` - End of other version

### Step 3: Decide What to Keep

**Option 1: Keep your change**

```javascript
function add(a, b) {
  return a + b + 1;
}
```

**Option 2: Keep other change**

```javascript
function add(a, b) {
  return a + b - 1;
}
```

**Option 3: Combine both (if it makes sense)**

```javascript
function add(a, b) {
  // Combined implementation
  return a + b;
}
```

**Option 4: Write something completely new**

```javascript
function add(a, b) {
  // New implementation after discussion
  return (a + b) * 2;
}
```

### Step 4: Remove Markers

Make sure to remove:

- `<<<<<<< HEAD`
- `=======`
- `>>>>>>> branch-name`

### Step 5: Save and Mark as Resolved

```bash
# Save file

# Mark as resolved
git add file.js

# Verify status
git status
```

### Step 6: Complete the Merge

```bash
# Make commit
git commit -m "Resolve merge conflict in file.js"

# Or if it's rebase
git rebase --continue
```

---

## Tools to Resolve

### Visual Studio Code

**Advantages:**

- Visual interface
- Buttons to accept changes
- Side-by-side preview

**Usage:**

1. Open conflicted file
2. VSCode shows buttons:
   - "Accept Current Change" (your version)
   - "Accept Incoming Change" (other version)
   - "Accept Both Changes" (both)
   - "Compare Changes" (compare)

### Git Mergetool

```bash
# Configure tool (first time)
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Use mergetool
git mergetool

# This opens your configured editor with the conflict
```

### Recommended Tools

**VSCode:**

```bash
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

**KDiff3:**

```bash
git config --global merge.tool kdiff3
```

**Meld:**

```bash
git config --global merge.tool meld
```

**P4Merge:**

```bash
git config --global merge.tool p4merge
```

---

## Resolution Strategies

### Accept All Your Version

```bash
# During merge, use "ours" strategy
git merge -X ours other-branch
```

### Accept All Other Version

```bash
# During merge, use "theirs" strategy
git merge -X theirs other-branch
```

### Abort Merge

If you decide not to continue:

```bash
# Abort merge
git merge --abort

# Abort rebase
git rebase --abort
```

---

## Prevent Conflicts

### 1. Team Communication

✅ **Coordinate changes in files**
✅ **Notify before modifying critical files**
✅ **Use task assignment system**

### 2. Small, Frequent Commits

```bash
# BAD: One giant commit
git add .
git commit -m "Large changes"

# GOOD: Atomic commits
git add file1.js
git commit -m "feat: add function X"
git add file2.js
git commit -m "fix: fix bug Y"
```

### 3. Update Frequently

```bash
# Every morning
git checkout main
git pull origin main
git checkout your-branch
git merge main  # or git rebase main

# Resolve conflicts early
```

### 4. Use Short-Lived Branches

```bash
# BAD: Branch lasting weeks
# (accumulates many conflicts)

# GOOD: Branches lasting days
# (fewer conflicts, easier to resolve)
```

### 5. Split Large Files

```javascript
// BAD: Everything in one file
// giant-file.js (1000 lines)

// GOOD: Split into modules
// module-a.js
// module-b.js
// module-c.js
```

### 6. Code Ownership

- Assign "owners" to specific modules
- Ask permission before changing others' code
- Use CODEOWNERS in GitHub

---

## Special Cases

### Conflict in package.json

**Problem:** Two people added dependencies.

**Solution:**

```json
<<<<<<< HEAD
{
  "dependencies": {
    "axios": "^1.0.0",
    "lodash": "^4.17.21"
=======
{
  "dependencies": {
    "axios": "^1.0.0",
    "moment": "^2.29.0"
>>>>>>> feature
  }
}
```

**Resolve by combining:**

```json
{
  "dependencies": {
    "axios": "^1.0.0",
    "lodash": "^4.17.21",
    "moment": "^2.29.0"
  }
}
```

**Then reinstall:**

```bash
npm install
git add package.json package-lock.json
git commit -m "Resolve conflict in dependencies"
```

---

### Conflict in Binary File

**Problem:** Can't resolve conflicts in images, PDFs, etc.

**Solution:**

```bash
# Keep your version
git checkout --ours image.png
git add image.png

# Keep other version
git checkout --theirs image.png
git add image.png

# Commit
git commit -m "Resolve conflict in image"
```

---

### Multiple Files in Conflict

```bash
# View all conflicts
git diff --name-only --diff-filter=U

# Resolve one by one
# ... edit file1 ...
git add file1.js

# ... edit file2 ...
git add file2.js

# When all are resolved
git commit
```

---

### Conflict During Rebase

```bash
git rebase main
# CONFLICT in file.js

# 1. Resolve conflict
# 2. Mark as resolved
git add file.js

# 3. Continue rebase
git rebase --continue

# If there are more conflicts, repeat
# If you want to abort
git rebase --abort
```

---

## Useful Commands

```bash
# View conflicted files
git diff --name-only --diff-filter=U

# View complete differences
git diff

# View only conflicts
git diff --check

# Accept all your version
git checkout --ours file.js
git add file.js

# Accept all other version
git checkout --theirs file.js
git add file.js

# View merge history
git log --merge

# View what caused conflict
git log --oneline --left-right HEAD...MERGE_HEAD
```

---

## Resolution Checklist

- [ ] Identify all conflicted files
- [ ] Open each file
- [ ] Understand both changes
- [ ] Decide what to keep
- [ ] Remove all markers (`<<<<`, `====`, `>>>>`)
- [ ] Test that code works
- [ ] Do `git add` for each file
- [ ] Verify with `git status`
- [ ] Make commit or continue rebase
- [ ] Make push
- [ ] Notify team if necessary

---

## Additional Resources

- 📖 **Workflow:** [/guides/en/workflow.md](/guides/en/workflow.md)
- 🔀 **Rebase vs Merge:** [/guides/en/workflow.md#rebase-vs-merge](/guides/en/workflow.md#rebase-vs-merge)
- 🛠️ **Common Problems:** [common-issues.md](common-issues.md)

---

**Resolved the conflict?** Share your experience or ask for help with complex cases.
