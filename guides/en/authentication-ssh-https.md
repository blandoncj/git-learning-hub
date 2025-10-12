# SSH vs HTTPS Authentication

Complete guide to the two main protocols to connect to remote repositories: SSH and HTTPS. You'll understand their differences, advantages and disadvantages, and when to use each one.

## Table of Contents

1. [Quick Comparison](#quick-comparison)
2. [SSH: What is it?](#ssh-what-is-it)
3. [HTTPS: What is it?](#https-what-is-it)
4. [Key Differences](#key-differences)
5. [Setting Up SSH](#setting-up-ssh)
6. [Setting Up HTTPS](#setting-up-https)
7. [When to Use Each](#when-to-use-each)
8. [Switch Between Protocols](#switch-between-protocols)
9. [Troubleshooting](#troubleshooting)

---

## Quick Comparison

| Aspect              | SSH                            | HTTPS                   |
| ------------------- | ------------------------------ | ----------------------- |
| **Security**        | Very high (public-private key) | High (SSL certificates) |
| **Setup**           | More complex                   | Simpler                 |
| **Password**        | Not needed                     | Needed each time        |
| **Firewall**        | May have issues                | More compatible         |
| **Speed**           | Faster                         | Slightly slower         |
| **Recommended for** | Professional work              | Beginners               |

---

## SSH: What is it?

### Definition

**SSH (Secure Shell)** is a secure network protocol that uses public-private key cryptography to authenticate to remote repositories without needing to enter a password each time.

### How it Works

1. You generate a pair of keys:
   - **Private key:** You keep it on your computer (NEVER share it)
   - **Public key:** You add it to your GitHub/GitLab/Bitbucket account

2. When you do a `git push` or `git pull`:
   - Git sends your private key (without transmitting it, just signs)
   - The server verifies with your public key
   - A secure connection is established

### Advantages of SSH

✅ **No password needed each time** - Once configured, it's automatic
✅ **More secure** - Uses public-key cryptography
✅ **Faster** - Protocol optimized for git
✅ **Ideal for CI/CD** - Automation without passwords
✅ **Professional** - Industry standard for development

### Disadvantages of SSH

❌ **More complex setup** - Requires generating keys
❌ **Firewall issues** - Some corporate firewalls block SSH
❌ **Requires terminal** - Not as beginner-friendly for GUI users

---

## HTTPS: What is it?

### Definition

**HTTPS (HyperText Transfer Protocol Secure)** is the same protocol your browser uses to access websites. It uses SSL certificates to encrypt communication.

### How it Works

1. You use your username and password (or token)
2. Git encrypts the communication with HTTPS
3. The server verifies your credentials
4. A connection is established

### Advantages of HTTPS

✅ **Easy to use** - Only need username/password
✅ **Firewall compatible** - Port 443 usually open
✅ **No key configuration** - Nothing to generate
✅ **Good for beginners** - Fewer initial steps

### Disadvantages of HTTPS

❌ **Need to enter credentials** - Or use Git Credentials Manager
❌ **Less secure than SSH** - Passwords can be intercepted
❌ **Slower** - SSL certificate overhead
❌ **2FA issues** - Requires special tokens

---

## Key Differences

### Authentication

**SSH:**

```
You (Private key) → Server (Public key) → Verified ✓
```

**HTTPS:**

```
You (Username + Password) → Server → Database → Verified ✓
```

### Ports

**SSH:** Port 22

```bash
ssh -T git@github.com
```

**HTTPS:** Port 443 (same as browsers)

```bash
https://github.com/username/repo.git
```

### URLs

**SSH:**

```
git@github.com:username/repository.git
```

**HTTPS:**

```
https://github.com/username/repository.git
```

---

## Setting Up SSH

### Step 1: Generate SSH Keys

Open your terminal and run:

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

**Notes:**

- `-t ed25519`: Key type (modern and secure)
- `-C "your.email@example.com"`: Identifier comment

**Alternative (if your system doesn't support ed25519):**

```bash
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"
```

### Step 2: Save the Keys

When asked where to save:

```
Enter file in which to save the key (/home/user/.ssh/id_ed25519):
```

Press **Enter** to save to the default location, or specify a path.

### Step 3: Protect with Passphrase (Recommended)

You'll be asked for a passphrase:

```
Enter passphrase (empty for no passphrase):
```

It's recommended to enter a passphrase for extra security. Then:

```
Confirm passphrase:
```

**Result:**

```
Your public key has been saved in /home/user/.ssh/id_ed25519.pub
Your private key has been saved in /home/user/.ssh/id_ed25519
```

### Step 4: Start SSH Agent (macOS/Linux)

This prevents you from entering your passphrase each time:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

**On Windows (Git Bash):**

```bash
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_ed25519
```

### Step 5: Copy Your Public Key

```bash
# macOS
pbcopy < ~/.ssh/id_ed25519.pub

# Linux
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard

# Windows (Git Bash)
cat ~/.ssh/id_ed25519.pub | clip
```

Or simply open the file and copy:

```bash
cat ~/.ssh/id_ed25519.pub
```

### Step 6: Add Key to GitHub/GitLab/Bitbucket

#### GitHub

1. Go to [github.com/settings/keys](https://github.com/settings/keys)
2. Click "New SSH key"
3. Paste your public key
4. Give it a title (e.g., "My laptop")
5. Click "Add SSH key"

#### GitLab

1. Go to [gitlab.com/-/profile/keys](https://gitlab.com/-/profile/keys)
2. Paste your public key in the field
3. Give it a title
4. Click "Add key"

#### Bitbucket

1. Go to [bitbucket.org/account/settings/ssh-keys/](https://bitbucket.org/account/settings/ssh-keys/)
2. Click "Add key"
3. Paste your public key
4. Click "Add SSH key"

### Step 7: Verify Connection

```bash
# GitHub
ssh -T git@github.com

# GitLab
ssh -T git@gitlab.com

# Bitbucket
ssh -T git@bitbucket.org
```

**Expected response:**

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## Setting Up HTTPS

### Option 1: Enter Password Each Time

When cloning or doing push/pull with HTTPS:

```bash
git clone https://github.com/username/repository.git
# It asks for username and password
```

**Not recommended:** It's tedious to enter credentials constantly.

### Option 2: Use Git Credentials Manager (Recommended)

Git Credentials Manager stores your credentials securely.

#### On Windows

Installed automatically with Git for Windows. If not:

```bash
git config --global credential.helper manager
```

#### On macOS

Installed automatically. If not:

```bash
git config --global credential.helper osxkeychain
```

#### On Linux

First install:

```bash
sudo apt install git-credential-manager

# Or for Fedora:
sudo dnf install git-credential-manager
```

Then configure:

```bash
git config --global credential.helper manager-core
```

#### Verify it works

```bash
git config --global credential.helper
# Should show: manager, manager-core, osxkeychain, etc.
```

### Option 3: Use Personal Access Token (Extra Security)

Instead of a password, GitHub/GitLab/Bitbucket let you generate tokens.

#### GitHub

1. Go to [github.com/settings/tokens](https://github.com/settings/tokens)
2. Click "Generate new token"
3. Give it `repo` permissions (repository access)
4. Copy the token
5. Use as password when doing git clone/push

#### GitLab

1. Go to [gitlab.com/-/profile/personal_access_tokens](https://gitlab.com/-/profile/personal_access_tokens)
2. Create a new token
3. Copy the token
4. Use as password

#### Bitbucket

1. Go to [bitbucket.org/account/settings/app-passwords/new](https://bitbucket.org/account/settings/app-passwords/new)
2. Create a new app password
3. Copy the password
4. Use as password in Git

---

## When to Use Each

### Use SSH If

✅ You're a professional developer
✅ You work on multiple repositories
✅ You want automation without passwords
✅ Your company doesn't block port 22
✅ You value maximum security
✅ You work with CI/CD

**Recommendation: SSH is better for professional work.**

### Use HTTPS If

✅ You're a beginner
✅ Your firewall blocks SSH (port 22)
✅ You work on restricted networks
✅ You prefer not to deal with keys
✅ You only use Git occasionally

**Recommendation: HTTPS is better to get started.**

---

## Switch Between Protocols

### From HTTPS to SSH

If you cloned with HTTPS but want to switch to SSH:

```bash
# View current remote
git remote -v
# origin  https://github.com/username/repo.git (fetch)

# Change to SSH
git remote set-url origin git@github.com:username/repo.git

# Verify
git remote -v
# origin  git@github.com:username/repo.git (fetch)
```

### From SSH to HTTPS

If you cloned with SSH but want to switch to HTTPS:

```bash
# View current remote
git remote -v
# origin  git@github.com:username/repo.git (fetch)

# Change to HTTPS
git remote set-url origin https://github.com/username/repo.git

# Verify
git remote -v
# origin  https://github.com/username/repo.git (fetch)
```

---

## Troubleshooting

### "Permission denied (publickey)"

**Cause:** SSH key not configured correctly.

**Solution:**

```bash
# Verify key exists
ls -la ~/.ssh/

# Start SSH agent
eval "$(ssh-agent -s)"

# Add key to agent
ssh-add ~/.ssh/id_ed25519

# Verify connection
ssh -T git@github.com
```

### "HTTPS authentication failed"

**Cause:** Wrong credentials or Git Credentials Manager not configured.

**Solution:**

```bash
# Configure Git Credentials Manager
git config --global credential.helper manager

# Clear saved credentials (Windows)
cmdkey /delete:git:https://github.com

# Clear saved credentials (macOS)
security delete-internet-password -s github.com

# Try again (will ask for credentials)
git push
```

### "Port 22 connection refused"

**Cause:** Firewall blocks port 22.

**Solution:** Use HTTPS instead of SSH.

```bash
git remote set-url origin https://github.com/username/repo.git
```

### "Could not read from remote repository"

**Cause:** Remote not configured correctly.

**Solution:**

```bash
# View remotes
git remote -v

# If empty, add:
git remote add origin git@github.com:username/repo.git

# Or if wrong, change:
git remote set-url origin git@github.com:username/repo.git
```

### SSH Key has wrong permissions

If you get permission errors with SSH key:

```bash
# On macOS/Linux
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

---

## Summary

### SSH Flow

```
1. Generate SSH keys
   ↓
2. Copy public key
   ↓
3. Add to GitHub/GitLab/Bitbucket
   ↓
4. Verify connection
   ↓
5. Use repositories without password
```

### HTTPS Flow

```
1. Install Git Credentials Manager
   ↓
2. (Optional) Generate personal token
   ↓
3. Clone with HTTPS URL
   ↓
4. Enter credentials once
   ↓
5. Git Credentials Manager saves them
```

---

## Next Steps

1. **[GPG Configuration](gpg-configuration.md)** - Sign your commits
2. **[Branch Management](branch-management.md)** - Work with branches
3. **[Workflow](workflow.md)** - Team workflows

---

**Next:** [GPG Configuration](gpg-configuration.md)
