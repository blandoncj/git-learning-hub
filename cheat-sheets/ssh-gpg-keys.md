# SSH and GPG Keys - Cheat Sheet

Quick reference for generating, configuring, and managing SSH and GPG keys.

## SSH - Generate Keys

```bash
# Generate ED25519 key (recommended)
ssh-keygen -t ed25519 -C "your.email@example.com"

# Generate RSA key (alternative)
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"

# Generate without passphrase
ssh-keygen -t ed25519 -N "" -C "your.email@example.com"

# Use custom location
ssh-keygen -t ed25519 -f ~/.ssh/my-key -C "comment"
```

---

## SSH - Manage Keys

```bash
# View existing keys
ls -la ~/.ssh/

# View public key content
cat ~/.ssh/id_ed25519.pub

# Copy public key (macOS)
pbcopy < ~/.ssh/id_ed25519.pub

# Copy public key (Linux)
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard

# Copy public key (Windows Git Bash)
cat ~/.ssh/id_ed25519.pub | clip

# View key fingerprint
ssh-keygen -lf ~/.ssh/id_ed25519.pub

# Change passphrase
ssh-keygen -p -f ~/.ssh/id_ed25519

# Delete key
rm ~/.ssh/id_ed25519 ~/.ssh/id_ed25519.pub
```

---

## SSH - Agent

```bash
# Start SSH Agent (macOS/Linux)
eval "$(ssh-agent -s)"

# Start SSH Agent (Windows Git Bash)
eval $(ssh-agent -s)

# Add key to agent
ssh-add ~/.ssh/id_ed25519

# View keys in agent
ssh-add -l

# View keys with fingerprint
ssh-add -L

# Remove key from agent
ssh-add -d ~/.ssh/id_ed25519

# Remove all keys
ssh-add -D

# Configure auto-add (macOS, in ~/.ssh/config)
Host *
  AddKeysToAgent yes
  UseKeychain yes
```

---

## SSH - Configure Git

```bash
# View current remote
git remote -v

# Change to SSH
git remote set-url origin git@github.com:username/repo.git

# Change to HTTPS
git remote set-url origin https://github.com/username/repo.git

# Verify SSH connection GitHub
ssh -T git@github.com

# Verify SSH connection GitLab
ssh -T git@gitlab.com

# Verify SSH connection Bitbucket
ssh -T git@bitbucket.org
```

---

## SSH - Advanced Configuration

```bash
# View SSH configuration
cat ~/.ssh/config

# Create configuration file
touch ~/.ssh/config
chmod 600 ~/.ssh/config

# Example ~/.ssh/config
# Host github.com
#     HostName github.com
#     User git
#     IdentityFile ~/.ssh/id_ed25519
#     AddKeysToAgent yes
```

---

## SSH - Troubleshooting

```bash
# Test connection with verbose
ssh -vvv git@github.com

# View SSH file permissions
ls -la ~/.ssh/
# Should be: -rw------- for files
# Should be: drwx------ for directory

# Fix permissions
chmod 700 ~/.ssh/
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub

# Check which key SSH uses
ssh -v git@github.com 2>&1 | grep "identity"
```

---

## GPG - Generate Keys

```bash
# Generate GPG key
gpg --full-generate-key

# Quick key generation
gpg --quick-generate-key "Name <email@example.com>"

# Generate key without interface
gpg --batch --generate-key <<EOF
Key-Type: RSA
Key-Length: 4096
Name-Real: Your Name
Name-Email: your.email@example.com
Expire-Date: 0
%no-protection
EOF
```

---

## GPG - Manage Keys

```bash
# List private keys
gpg --list-secret-keys --keyid-format LONG

# List public keys
gpg --list-keys

# View specific key
gpg --list-keys your.email@example.com

# Export public key (ASCII)
gpg --armor --export your.email@example.com

# Export public key (binary)
gpg --export your.email@example.com > my-key.gpg

# Export private key
gpg --armor --export-secret-keys your.email@example.com

# Import key
gpg --import my-key.gpg

# Delete public key
gpg --delete-keys KEY_ID

# Delete private key
gpg --delete-secret-keys KEY_ID

# View long key ID
gpg --keyid-format LONG --list-keys
```

---

## GPG - Configure Git

```bash
# View key ID
gpg --list-secret-keys --keyid-format LONG
# Look for: sec   rsa4096/XXXXXXXXXXXXXXXX

# Configure Git with key ID
git config --global user.signingkey XXXXXXXXXXXXXXXX

# Sign commits automatically
git config --global commit.gpgsign true

# Verify configuration
git config --global user.signingkey
git config --global commit.gpgsign
```

---

## GPG - Sign Commits

```bash
# Sign individual commit
git commit -S -m "Message"

# Sign with automatic configuration
git commit -m "Message"

# Amend commit to sign
git commit --amend -S

# Sign previous commit
git commit -S --amend abc123def

# Sign multiple commits
git rebase --exec 'git commit --amend --no-edit -S' -i abc123def^
```

---

## GPG - Verify Commits

```bash
# View commits with signatures
git log --show-signature

# View log with signature status
git log --oneline --pretty='%h %G? %s'

# Verify specific commit
git verify-commit abc123def

# View signature details
git show abc123def

# %G? meanings:
# G = good signature (Good)
# B = bad signature (Bad)
# U = unknown signature (Unknown)
# X = signature expired (eXpired)
# Y = signature expiring soon (expireY)
# R = signature revoked (Revoked)
# E = error
# N = no signature (None)
```

---

## GPG - Add to Platforms

```bash
# Export public key
gpg --armor --export your.email@example.com > key.asc

# Copy key
cat key.asc

# GitHub: https://github.com/settings/keys
# GitLab: https://gitlab.com/-/profile/gpg_keys
# Bitbucket: https://bitbucket.org/account/settings/gpg-keys/

# Verify on platform
# Make signed commit and push
# Should show "Verified" (GitHub/GitLab)
```

---

## GPG - Configure Agent

```bash
# Edit agent configuration
nano ~/.gnupg/gpg-agent.conf

# Add lines to cache passphrase
default-cache-ttl 600
max-cache-ttl 7200

# Restart agent
gpgconf --kill gpg-agent

# Or on Windows
gpg-connect-agent killagent /bye
gpg-connect-agent /bye
```

---

## GPG - Troubleshooting

```bash
# Test GPG
echo "test" | gpg --clearsign

# Check if GPG can access terminal
gpg --version

# Change passphrase entry program
nano ~/.gnupg/gpg-agent.conf
# Add: pinentry-program /usr/bin/pinentry-tty

# Issues with VSCode
# In Git configuration:
git config --global core.editor "code --wait"
git config --global gpg.program "gpg"

# Issues on macOS
# Install pinentry
brew install pinentry-mac

# Configure
echo "pinentry-program /usr/local/bin/pinentry-mac" \
  >> ~/.gnupg/gpg-agent.conf
```

---

## Git Credentials Manager

```bash
# View configured helper
git config --global credential.helper

# Configure on Windows
git config --global credential.helper manager

# Configure on macOS
git config --global credential.helper osxkeychain

# Configure on Linux
git config --global credential.helper cache

# Or persistent storage
git config --global credential.helper store

# Delete saved credentials (Windows)
cmdkey /delete:git:https://github.com

# Delete saved credentials (macOS)
security delete-internet-password -s github.com

# View credentials (if accessible)
security find-internet-password -s github.com -w
```

---

## HTTPS Token

```bash
# GitHub: Generate token at
# https://github.com/settings/tokens

# GitLab: Generate at
# https://gitlab.com/-/profile/personal_access_tokens

# Bitbucket: Generate at
# https://bitbucket.org/account/settings/app-passwords/

# Use token as password
git clone https://github.com/username/repo.git
# Username: your-github-username
# Password: your-github-token
```

---

**For more information:** [SSH vs HTTPS](/guides/en/authentication-ssh-https.md) | [GPG Configuration](/guides/en/gpg-configuration.md)
