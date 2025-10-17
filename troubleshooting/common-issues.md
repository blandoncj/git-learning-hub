# Common Git Problems

Solutions to the most frequent problems when working with Git.

## Table of Contents

1. [Configuration Problems](#configuration-problems)
2. [Branch Problems](#branch-problems)
3. [Commit Problems](#commit-problems)
4. [Push/Pull Problems](#pushpull-problems)
5. [Permission Problems](#permission-problems)
6. [History Problems](#history-problems)

---

## Configuration Problems

### "fatal: not a git repository"

**Symptom:** Git doesn't recognize the directory as a repository.

**Cause:** You're not in a Git repository or there's no `.git/` in the directory.

**Solution:**

```bash
# Check if you're in a repository
ls -la .git

# If it doesn't exist, initialize
git init

# Or clone an existing one
git clone https://github.com/username/repo.git
```

---

### "Author identity unknown"

**Symptom:** Git doesn't know who you are when committing.

```
*** Please tell me who you are.
Run
  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
```

**Solution:**

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Verify
git config --global --list
```

---

### "warning: LF will be replaced by CRLF"

**Symptom:** Warning about line endings on Windows.

**Cause:** Differences between Windows (CRLF) and Unix (LF).

**Solution (Windows):**

```bash
# Configure to convert automatically
git config --global core.autocrlf true
```

**Solution (macOS/Linux):**

```bash
git config --global core.autocrlf input
```

---

## Branch Problems

### "error: pathspec 'branch' did not match any file(s)"

**Symptom:** The branch you're trying to use doesn't exist.

**Solution:**

```bash
# View available branches
git branch -a

# Create branch if it doesn't exist
git checkout -b branch-name

# Or fetch remote branch
git fetch origin
git checkout branch-name
```

---

### "error: cannot delete branch 'main'"

**Symptom:** You can't delete the branch you're on.

**Solution:**

```bash
# Switch to another branch first
git checkout other-branch

# Then delete
git branch -d main
```

---

### "Your branch and 'origin/main' have diverged"

**Symptom:** Your local and remote branches have different histories.

```
Your branch and 'origin/main' have diverged,
and have 3 and 2 different commits each, respectively.
```

**Solution 1: Merge (preserves history)**

```bash
git pull origin main
# Resolve conflicts if any
git push
```

**Solution 2: Rebase (clean history)**

```bash
git pull --rebase origin main
# Resolve conflicts if any
git push
```

---

### "fatal: refusing to merge unrelated histories"

**Symptom:** Trying to merge repositories without common history.

**Solution:**

```bash
# Force merge
git pull origin main --allow-unrelated-histories

# Or when cloning
git merge origin/main --allow-unrelated-histories
```

---

## Commit Problems

### "nothing to commit, working tree clean"

**Symptom:** No changes to commit.

**Cause:** You already committed all changes or there are no changes.

**Verification:**

```bash
# View status
git status

# View differences
git diff
```

---

### "Please enter the commit message"

**Symptom:** A text editor opens asking for a message.

**Cause:** You did `git commit` without `-m`.

**Solution 1: Write message**

- Write the message in the editor
- Save and close (in nano: Ctrl+X, Y, Enter; in vim: ESC, :wq)

**Solution 2: Cancel**

```bash
# In nano: Ctrl+X, n
# In vim: ESC, :q!
```

**Prevention:**

```bash
# Always use -m
git commit -m "Your message"
```

---

### "Changes not staged for commit"

**Symptom:** You made changes but didn't add them to staging area.

**Solution:**

```bash
# Add specific files
git add file.js

# Add all
git add .

# Then commit
git commit -m "Message"
```

---

### "Aborting commit due to empty commit message"

**Symptom:** Commit cancelled due to empty message.

**Solution:**

```bash
# Ensure you write a message
git commit -m "Change description"

# Or allow empty commit (for testing)
git commit --allow-empty-message -m ""
```

---

## Push/Pull Problems

### "Updates were rejected because the remote contains work"

**Symptom:** The remote has commits you don't have locally.

```
! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/user/repo.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
```

**Solution:**

```bash
# 1. Fetch changes
git pull origin main

# 2. Resolve conflicts if any
git add .
git commit -m "Resolve conflicts"

# 3. Push
git push origin main
```

---

### "fatal: refusing to merge unrelated histories"

See section [Branch Problems](#fatal-refusing-to-merge-unrelated-histories).

---

### "! [remote rejected] (pre-receive hook declined)"

**Symptom:** Server rejects your push due to repository policies.

**Common causes:**

- Automatic tests didn't pass
- You don't have permissions
- Protected branch requires Pull Request

**Solution:**

```bash
# Verify your tests pass
npm test  # or your project's command

# Use Pull Request instead of direct push
# (create PR on GitHub/GitLab/Bitbucket)
```

---

### "error: failed to push some refs"

**Symptom:** General push failure.

**Common causes:**

1. You don't have permissions
2. Protected branch
3. Need to pull first

**Solution:**

```bash
# Pull first
git pull --rebase origin main

# Then push
git push origin main
```

---

## Permission Problems

### "Permission denied (publickey)"

**Symptom:** Can't connect via SSH.

```
Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Solution:**

```bash
# 1. Verify you have SSH key
ls -la ~/.ssh/

# 2. If doesn't exist, generate
ssh-keygen -t ed25519 -C "your.email@example.com"

# 3. Add to SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 4. Copy public key
cat ~/.ssh/id_ed25519.pub

# 5. Add to GitHub/GitLab/Bitbucket
# (in platform settings)

# 6. Test connection
ssh -T git@github.com
```

**More information:** [SSH Troubleshooting](/cheat-sheets/ssh-gpg-keys.en.md#ssh---troubleshooting)

---

### "fatal: Authentication failed"

**Symptom:** HTTPS authentication failed.

**Common causes:**

- Wrong password
- Expired token
- 2FA enabled without token

**Solution:**

```bash
# Option 1: Use token instead of password
# Generate token on GitHub/GitLab/Bitbucket
# Use token as password

# Option 2: Switch to SSH
git remote set-url origin git@github.com:username/repo.git

# Option 3: Configure credentials helper
git config --global credential.helper manager
```

---

### "fatal: could not read Username"

**Symptom:** Git can't get credentials.

**Solution:**

```bash
# Configure credentials helper
git config --global credential.helper manager

# Or provide credentials in URL (not recommended)
git clone https://username:token@github.com/username/repo.git
```

---

## History Problems

### "detached HEAD state"

**Symptom:** You're on a specific commit, not on a branch.

```
You are in 'detached HEAD' state.
```

**Solution:**

```bash
# If you want to go back to branch
git checkout main

# If you want to create branch from here
git checkout -b branch-name

# If you just wanted to view commit
git checkout -
```

---

### "error: Your local changes would be overwritten"

**Symptom:** Local changes would be lost when switching branches.

```
error: Your local changes to the following files would be overwritten by checkout:
  file.js
Please commit your changes or stash them before you switch branches.
```

**Solution 1: Save changes**

```bash
git add .
git commit -m "Work in progress"
git checkout other-branch
```

**Solution 2: Save temporarily**

```bash
git stash
git checkout other-branch
# When you return
git stash pop
```

**Solution 3: Discard changes**

```bash
git checkout -- .
git checkout other-branch
```

---

### "warning: refname 'HEAD' is ambiguous"

**Symptom:** Git is confused about what HEAD is.

**Cause:** File or branch named HEAD.

**Solution:**

```bash
# Specify clearly
git checkout main

# View references
git show-ref
```

---

## General Troubleshooting Tips

### View complete error messages

```bash
# Verbose mode
git --verbose command

# Or
GIT_TRACE=1 git command
```

### View current configuration

```bash
# Global
git config --global --list

# Local
git config --local --list

# Everything
git config --list --show-origin
```

### Verify repository integrity

```bash
# Verify objects
git fsck

# Clean and optimize
git gc
```

### Undo almost anything

```bash
# View HEAD change history
git reflog

# Go to specific point
git checkout abc123def
```

---

## Still having problems?

- 📖 **Git Errors:** [git-errors.md](git-errors.md)
- 🔐 **SSH Problems:** [ssh-problems.md](ssh-problems.md)
- 🔀 **Merge Conflicts:** [merge-conflicts.md](merge-conflicts.md)
- 📚 **Documentation:** [/docs/en](/docs/en)

---

**Found the solution?** Share your experience or report new problems.
