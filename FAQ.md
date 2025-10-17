# Frequently Asked Questions (FAQ)

Answers to the most common questions about Git, GitHub, GitLab, and Bitbucket.

## Table of Contents

1. [General Questions](#general-questions)
2. [Getting Started](#getting-started)
3. [Authentication](#authentication)
4. [Branches](#branches)
5. [Commits](#commits)
6. [Push and Pull](#push-and-pull)
7. [Merges and Conflicts](#merges-and-conflicts)
8. [Common Errors](#common-errors)
9. [Security](#security)

---

## General Questions

### What is Git?

Git is a **distributed version control system** that allows you to track changes in your code, collaborate with other developers, and maintain a complete history of your project.

**More information:** [Introduction to Git](/docs/en/introduction.md)

### What's the difference between Git and GitHub?

- **Git:** Version control software (works on your computer)
- **GitHub:** Online platform to host Git repositories and collaborate

**Analogy:** Git is like a car's engine, GitHub is the gas station where you fuel it.

### Do I need to pay to use Git?

No, Git is **completely free** and open source. The same goes for GitHub (for public repositories), GitLab, and Bitbucket.

### Where do I start?

1. **[Install Git](/docs/en/installation.md)**
2. **[Configure your user](/docs/en/initial-configuration.md)**
3. **[Learn basic concepts](/docs/en/basic-concepts.md)**
4. **[Create your first repository](/guides/en/workflow.md#basic-workflow)**

---

## Getting Started

### How do I create my first repository?

```bash
# Option 1: Local repository
git init project-name
cd project-name

# Option 2: Clone existing one
git clone https://github.com/username/repo.git
cd repo
```

**More information:** [Basic Cheat Sheet](/cheat-sheets/git-basico.md)

### How do I see the status of my repository?

```bash
git status
```

This shows you:

- Modified files
- Untracked files
- Changes in staging area

### What is a commit?

A **commit** is a "snapshot" of your project at a specific moment. It contains the changes made, who made them, when, and why.

**More information:** [Basic Concepts](/docs/en/basic-concepts.md#commit)

### How do I make my first commit?

```bash
# 1. View changes
git status

# 2. Add changes
git add .

# 3. Make commit
git commit -m "Clear description of change"

# 4. View history
git log
```

---

## Authentication

### SSH or HTTPS? Which should I use?

**Use SSH if:**

- You're a professional developer
- You work on multiple repositories
- You want automation

**Use HTTPS if:**

- You're a beginner
- Your firewall blocks SSH
- You prefer simplicity

**More information:** [SSH vs HTTPS](/guides/en/authentication-ssh-https.md)

### How do I generate SSH keys?

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

Then add the public key to GitHub/GitLab/Bitbucket.

**More information:** [SSH/GPG Cheat Sheet](/cheat-sheets/ssh-gpg-keys.md)

### It asks for my password every time I push

Configure **Git Credentials Manager**:

```bash
# Windows
git config --global credential.helper manager

# macOS
git config --global credential.helper osxkeychain

# Linux
git config --global credential.helper cache
```

### Can I use multiple Git accounts?

Yes, you can configure multiple SSH keys in `~/.ssh/config`:

```bash
Host github-personal
    HostName github.com
    IdentityFile ~/.ssh/id_personal

Host github-work
    HostName github.com
    IdentityFile ~/.ssh/id_work
```

**More information:** [Advanced SSH](/cheat-sheets/ssh-gpg-keys.en.md#ssh---advanced-configuration)

---

## Branches

### What is a branch?

A **branch** is an independent line of development. It allows you to work on new features without affecting the main code.

**Analogy:** Like branches of a tree, each is independent but connected to the trunk.

**More information:** [Branch Management](/guides/en/branch-management.md)

### How do I create a branch?

```bash
# Create and switch
git checkout -b branch-name

# Or modern version
git switch -c branch-name
```

### What's the difference between main, master, and develop?

- **main/master:** The main branch, stable production code
- **develop:** Development branch where features are integrated
- **feature/...:** Temporary branches for working on new features

**More information:** [Branching Strategies](/guides/en/branch-management.md#branching-strategies)

### How do I delete a branch?

```bash
# Delete local
git branch -d branch-name

# Delete remote
git push origin --delete branch-name
```

---

## Commits

### How do I write good commit messages?

**Rules:**

- ✅ Maximum 50 characters in first line
- ✅ Use imperative ("add", not "added")
- ✅ Clear and descriptive
- ✅ Present tense

**Example:**

```
feat: add email validation
```

**More information:** [Commit Conventions](/guides/en/workflow.md#commit-conventions)

### Can I change a commit I already made?

Yes, with `git commit --amend`:

```bash
# Change last commit
git commit --amend -m "New message"

# Or change changes
git add file.js
git commit --amend --no-edit
```

### How do I view commit history?

```bash
# View complete
git log

# View in one line
git log --oneline

# View with branch graph
git log --oneline --graph --all
```

**More information:** [Basic Cheat Sheet](/cheat-sheets/git-basico.md#history)

---

## Push and Pull

### What's the difference between push and pull?

- **Push:** Send your local changes to the remote repository
- **Pull:** Download changes from remote to your local machine

```bash
git push  # Send
git pull  # Download
```

### Why is my push rejected?

Usually because the remote has changes you don't have locally.

**Solution:**

```bash
# Fetch changes first
git pull

# Resolve conflicts if any
# Then push
git push
```

### Do I need to push after every commit?

No, you can make several local commits and push them all at once:

```bash
git commit -m "Change 1"
git commit -m "Change 2"
git commit -m "Change 3"

# All together
git push
```

---

## Merges and Conflicts

### What is a merge?

A **merge** combines changes from one branch with another. Typically you merge a feature branch with the main branch.

**More information:** [Merge and Conflicts](/guides/en/workflow.md#merge-and-conflicts)

### What is a conflict?

A **conflict** occurs when Git can't automatically merge because two branches modify the same lines.

**Solution:**

1. Edit the conflicted file
2. Choose which changes to keep
3. Delete conflict markers
4. Make a commit

### How do I resolve conflicts?

Conflicts look like this in the file:

```
<<<<<<< HEAD
your version
=======
other version
>>>>>>> branch-name
```

Choose one, delete markers, and commit.

**More information:** [Resolve Conflicts](/guides/en/workflow.md#merge-and-conflicts)

### What is a Pull Request?

A **Pull Request** (or Merge Request in GitLab) is a request to review your changes before merging. It allows:

- Code review
- Discussion of changes
- Requesting improvements
- Ensuring quality

**More information:** [Pull Requests](/guides/en/workflow.md#pull-requests)

---

## Common Errors

### "fatal: not a git repository"

**Problem:** You're not in a Git repository.

**Solution:**

```bash
# Initialize repository
git init

# Or clone existing one
git clone url
```

### "error: pathspec 'branch' did not match any file"

**Problem:** The branch doesn't exist.

**Solution:**

```bash
# View available branches
git branch -a

# Create the branch
git checkout -b branch
```

### "Please commit your changes before switching branches"

**Problem:** You have unsaved changes.

**Solution 1:** Make a commit

```bash
git add .
git commit -m "Intermediate changes"
git checkout other-branch
```

**Solution 2:** Save temporarily

```bash
git stash
git checkout other-branch
git stash pop
```

### "Updates were rejected because the tip of your current branch is behind"

**Problem:** The remote has changes you don't have.

**Solution:**

```bash
git pull origin branch
git push
```

### "Your branch has diverged"

**Problem:** Your branch and remote have evolved differently.

**Solution:**

```bash
# Option 1: Rebase (linear history)
git pull --rebase

# Option 2: Merge (preserves history)
git pull
```

---

## Security

### Should I share my SSH keys?

**NO!** Never share:

- Your private SSH key
- Your private GPG key
- Your access tokens

Only share:

- Your public SSH key (`.pub`)

### What if I accidentally committed sensitive information?

**Option 1:** If you haven't pushed yet

```bash
git reset --soft HEAD~1
# Edit the commit
git commit -m "New message"
```

**Option 2:** If you already pushed

```bash
# Remove file from history
git filter-branch --tree-filter 'rm -f sensitive-file.txt' HEAD

# Force push
git push --force
```

### Should I commit passwords in my code?

**No.** Never commit:

- Passwords
- Tokens
- API keys

**Use instead:**

- Environment variables
- `.env` files (not committed)
- Secrets in platforms (GitHub Secrets, etc.)

### How do I sign my commits?

Use GPG to digitally sign commits:

```bash
git commit -S -m "Message"
```

**More information:** [GPG Configuration](/guides/en/gpg-configuration.md)

---

## Still have questions?

- 📚 **Complete documentation:** [Home](/docs/en)
- 📖 **Detailed guides:** [Guides](/guides/en)
- ⚡ **Quick references:** [Cheat Sheets](/cheat-sheets)
- 🔧 **Configuration:** [Getting started](/docs/en/initial-configuration.md)

---

**Found this FAQ helpful?** Share feedback or suggest new questions.
