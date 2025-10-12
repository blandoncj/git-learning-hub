# Tracking Changes

Complete guide on how to audit, track, and analyze code changes. You'll learn to use Git commands and visual tools to understand who did what and when.

## Table of Contents

1. [View History](#view-history)
2. [Blame - Who Changed What](#blame---who-changed-what)
3. [Diff - View Differences](#diff---view-differences)
4. [Show - Commit Details](#show---commit-details)
5. [GitLens in VSCode](#gitlens-in-vscode)
6. [SourceTree](#sourcetree)
7. [Search History](#search-history)
8. [Advanced Analysis](#advanced-analysis)

---

## View History

### git log - Basic Command

```bash
# View complete history
git log

# View last N commits
git log -n 5

# View last 5 commits
git log -5
```

**Output:**

```
commit abc123def456 (HEAD -> main)
Author: John Doe <john@example.com>
Date:   Mon Jan 15 10:30:45 2024 -0500

    feat: add shopping cart

commit ghi789jkl012
Author: Jane Smith <jane@example.com>
Date:   Sun Jan 14 15:20:30 2024 -0500

    fix: fix email validation
```

### git log - One-Line Format

```bash
# View compactly
git log --oneline

# Last 10
git log --oneline -10
```

**Output:**

```
abc123d (HEAD -> main) feat: add shopping cart
ghi789j fix: fix email validation
klm456n docs: update readme
```

### git log - Custom Format

```bash
# Custom format
git log --pretty=format:"%h %an %ar %s"

# Where:
# %h = short hash
# %an = author name
# %ar = relative date (2 days ago)
# %s = commit subject
```

**Output:**

```
abc123d John Doe 2 days ago feat: add shopping cart
ghi789j Jane Smith 3 days ago fix: fix email validation
```

### git log - Branch Graph

```bash
# View all branches visually
git log --oneline --graph --all

# More detailed version
git log --oneline --graph --all --decorate
```

**Output:**

```
* abc123d (main) feat: add shopping cart
|\
| * def456g (feature/login) feat: add login
|/
* ghi789j fix: fix email validation
* jkl012m docs: update readme
```

### git log - Filter by Author

```bash
# View commits from specific author
git log --author="John Doe"

# Or with email
git log --author="john@example.com"
```

### git log - Filter by Date

```bash
# Last 24 hours
git log --since="24 hours ago"

# From specific date
git log --since="2024-01-15" --until="2024-01-20"

# Last 2 weeks
git log --since="2 weeks ago"
```

### git log - Filter by File

```bash
# View changes to specific file
git log -- file.js

# View changes to folder
git log -- folder/

# With oneline
git log --oneline -- file.js
```

---

## Blame - Who Changed What

### git blame - Basic

The `blame` command shows who modified each line of a file.

```bash
# See who changed each line
git blame file.js
```

**Output:**

```
abc123d (John Doe 2024-01-15 10:30) function add(a, b) {
abc123d (John Doe 2024-01-15 10:30)   return a + b;
def456g (Jane Smith 2024-01-14 15:20) }
```

### git blame - More Context

```bash
# View line numbers
git blame -n file.js

# View date and time
git blame -t file.js

# View author email
git blame -e file.js
```

### git blame - Ignore Whitespace

```bash
# Ignore whitespace changes
git blame -w file.js

# Ignore blank lines
git blame --ignore-all-space file.js
```

### git blame - Range of Lines

```bash
# View only lines 10-50
git blame -L 10,50 file.js

# View lines from 10 onwards
git blame -L 10 file.js
```

### Interpret Blame

```
abc123d (John Doe 2024-01-15 10:30:45 -0500) function add(a, b) {
         │           │      │       │        │        │
         │           │      │       │        │        └── Code
         │           │      │       │        └── Exact time
         │           │      │       └── Date
         │           │      └── Author name
         │           └── Author email
         └── Commit hash
```

---

## Diff - View Differences

### git diff - Changes Without Commit

```bash
# View changes in current file
git diff

# View changes to specific file
git diff file.js

# View changes to folder
git diff folder/
```

**Output:**

```
diff --git a/file.js b/file.js
index abc123d..def456g 100644
--- a/file.js
+++ b/file.js
@@ -1,5 +1,6 @@
 function add(a, b) {
   return a + b;
+  // New comment
 }
```

### git diff - Changes in Staging Area

```bash
# View changes that will be committed
git diff --staged

# Or short version
git diff --cached
```

### git diff - Compare Commits

```bash
# Difference between two commits
git diff abc123d def456g

# Difference between HEAD and 2 commits ago
git diff HEAD~2

# Difference between main and feature
git diff main feature/new-branch
```

### git diff - Statistics

```bash
# View summary of changes
git diff --stat

# More details
git diff --stat --summary
```

**Output:**

```
file.js      | 3 +-
function.js  | 15 +++++-
README.md    | 2 ++
---------
3 files changed, 19 insertions(+), 1 deletion(-)
```

### git diff - Unified Format

```bash
# Preview before/after
git diff -U3  # 3 lines of context

# Much more context
git diff -U10
```

---

## Show - Commit Details

### git show - View Complete Commit

```bash
# View specific commit
git show abc123d

# View HEAD (last commit)
git show HEAD

# View 2 commits ago
git show HEAD~2
```

**Output:**

```
commit abc123def456
Author: John Doe <john@example.com>
Date:   Mon Jan 15 10:30:45 2024 -0500

    feat: add shopping cart

    - Adds cart functionality
    - Persists in localStorage
    - Shows total

diff --git a/cart.js b/cart.js
new file mode 100644
index 0000000..abc123d
--- /dev/null
+++ b/cart.js
@@ -0,0 +1,50 @@
+function createCart() {
...
```

### git show - Only Message

```bash
# View only commit message
git show --format=fuller abc123d

# View only diff
git show --no-patch abc123d
```

### git show - Specific File in Commit

```bash
# View how file was in commit
git show abc123d:file.js

# View file 2 commits ago
git show HEAD~2:file.js
```

---

## GitLens in VSCode

GitLens is a powerful extension that integrates Git information directly into VSCode.

### Installation

1. Open VSCode
2. Go to Extensions (Ctrl+Shift+X)
3. Search "GitLens"
4. Click "Install"

### Main Features

#### Inline Blame

Shows commit info next to each line:

```
│ John Doe  3 days ago │ function add(a, b) {
│                      │   return a + b;
│ Jane Smith 1 day ago │ }
```

**How to use:**

- Hover over a line
- See who changed it and when
- Click to see full commit

#### Code Lens

Shows commit info above functions:

```
│ 2 commits │
│ function add(a, b) {
```

Click to see history of that function.

#### Command Palette

Press `Ctrl+Shift+P` and search "GitLens":

- `GitLens: Show Commit` - See commit details
- `GitLens: Show File History` - See file history
- `GitLens: Compare with Branch` - Compare branches
- `GitLens: Blame` - View inline blame

#### Side Panel

GitLens sidebar shows:

- **Repository** - Repo information
- **File History** - Current file history
- **Line History** - Selected line history
- **Commits** - All commits

---

## SourceTree

SourceTree is a popular graphical Git tool.

### Installation

Download from [sourcetreeapp.com](https://www.sourcetreeapp.com/)

Available for Windows and macOS.

### Main Features

#### Log Graph

Graphical visualization of commits:

- Shows all branches
- Connections between commits
- Easy to see history

#### Blame View

Right-click file → "Show in Blame View":

- Shows who changed each line
- Click to see commit
- Colorized by author

#### Search

Search commits:

- By author
- By message
- By content

#### Compare

Compare files between commits:

- View before/after
- Highlighted differences
- Line-by-line changes

---

## Search History

### git log -S - Search Content

```bash
# Search commits that added/removed a word
git log -S "variable_name"

# View what changed
git log -S "variable_name" -p
```

### git log -G - Regex Search

```bash
# Search with regular expression
git log -G "function.*add"

# With changes
git log -G "function.*add" -p
```

### git log --grep - Search Messages

```bash
# Search in commit messages
git log --grep="bug"

# Case insensitive
git log --grep="bug" -i

# Multiple terms (any)
git log --grep="bug\|error" -E
```

### git log --all --grep - Search All Branches

```bash
# Search entire history
git log --all --grep="feature"

# With graph
git log --all --grep="feature" --oneline --graph
```

---

## Advanced Analysis

### Statistics by Author

```bash
# View commits by author
git shortlog -s -n

# View commits with detail
git shortlog
```

**Output:**

```
    15 John Doe
    12 Jane Smith
     8 Carlos Rodriguez
```

### Lines of Code Changed

```bash
# See lines added/removed by file
git diff --stat HEAD~10

# View lines by author
git log --format="%an" -n 1000 | sort | uniq -c | sort -rn
```

### View Branch Changes

```bash
# Commits in main not in feature
git log feature..main

# Commits in feature not in main
git log main..feature

# Commits in either branch
git log main...feature
```

### Reflog - HEAD History

```bash
# View all HEAD movements
git reflog

# Useful for recovering deleted commits
git reflog show
```

**Use:** If you accidentally deleted a commit, you can recover it:

```bash
git checkout abc123d  # Commit ID from reflog
```

---

## Typical Audit Workflow

```bash
# 1. Who changed this line?
git blame file.js

# 2. When was that change made?
git show abc123d

# 3. What other changes did that person make that day?
git log --author="John Doe" --since="2024-01-15" --until="2024-01-16"

# 4. What changes did commit abc123d make?
git show abc123d --stat

# 5. How was the file before?
git show HEAD~1:file.js

# 6. View in GitLens
# (Open file in VSCode and use GitLens)
```

---

## Next Steps

1. **[Workflow](workflow.md)** - Review workflow
2. **[Branch Management](branch-management.md)** - Review branches
3. **[GPG Configuration](gpg-configuration.md)** - Review security

---

**Previous:** [Workflow](workflow.md)
