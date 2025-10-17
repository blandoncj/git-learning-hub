# SSH Problems

Specific solutions for SSH authentication problems with Git.

## Table of Contents

1. [Connection Problems](#connection-problems)
2. [Permission Problems](#permission-problems)
3. [Configuration Problems](#configuration-problems)
4. [Agent Problems](#agent-problems)

---

## Connection Problems

### "Permission denied (publickey)"

**Symptom:** Can't connect to GitHub/GitLab/Bitbucket.

```
Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Diagnosis:**

```bash
# Test connection with verbose
ssh -vvv git@github.com
```

**Common causes:**

1. **You don't have SSH key**
2. **Key not added to SSH agent**
3. **Public key not added to platform**
4. **Incorrect file permissions**

**Complete solution:**

```bash
# 1. Check existing keys
ls -la ~/.ssh/

# 2. If doesn't exist, generate key
ssh-keygen -t ed25519 -C "your.email@example.com"

# 3. Start SSH agent
eval "$(ssh-agent -s)"

# 4. Add key to agent
ssh-add ~/.ssh/id_ed25519

# 5. Copy public key
cat ~/.ssh/id_ed25519.pub

# 6. Add to platform
# GitHub: https://github.com/settings/keys
# GitLab: https://gitlab.com/-/profile/keys
# Bitbucket: https://bitbucket.org/account/settings/ssh-keys/

# 7. Test connection
ssh -T git@github.com
```

---

### "ssh: connect to host github.com port 22: Connection refused"

**Symptom:** Firewall blocks port 22.

**Solution:** Use SSH over HTTPS (port 443):

```bash
# Edit ~/.ssh/config
nano ~/.ssh/config

# Add
Host github.com
  Hostname ssh.github.com
  Port 443
  User git

# Test
ssh -T git@github.com
```

---

### "Connection timed out"

**Symptom:** Connection times out.

**Causes:**

- Corporate firewall
- VPN blocking
- Network issue

**Solution:**

```bash
# Check connectivity
ping github.com

# Try HTTPS instead of SSH
git remote set-url origin https://github.com/username/repo.git

# Or configure proxy if applicable
git config --global http.proxy http://proxy.example.com:8080
```

---

## Permission Problems

### "bad permissions: ignore key"

**Symptom:** SSH ignores your key due to incorrect permissions.

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/home/user/.ssh/id_rsa' are too open.
```

**Cause:** SSH files must have restrictive permissions.

**Solution:**

```bash
# Fix directory permissions
chmod 700 ~/.ssh/

# Fix private key permissions
chmod 600 ~/.ssh/id_ed25519

# Fix public key permissions
chmod 644 ~/.ssh/id_ed25519.pub

# Fix config
chmod 600 ~/.ssh/config

# Verify
ls -la ~/.ssh/
# Should show:
# drwx------  .ssh/
# -rw-------  id_ed25519
# -rw-r--r--  id_ed25519.pub
```

---

### "Host key verification failed"

**Symptom:** SSH can't verify the host.

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
```

**Causes:**

- Legitimate server change
- Man-in-the-middle attack (rare)

**Solution (if you're sure it's legitimate):**

```bash
# Remove entry from known_hosts
ssh-keygen -R github.com

# Connect again (accept new key)
ssh -T git@github.com
```

---

## Configuration Problems

### "Could not open a connection to your authentication agent"

**Symptom:** SSH agent is not running.

**Solution:**

```bash
# Start SSH agent
eval "$(ssh-agent -s)"

# Add key
ssh-add ~/.ssh/id_ed25519

# Verify
ssh-add -l
```

---

### "sign_and_send_pubkey: no mutual signature supported"

**Symptom:** Your SSH key uses old algorithm.

**Cause:** GitHub/GitLab disabled old algorithms (RSA-SHA1).

**Solution:**

```bash
# Option 1: Generate new ED25519 key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Option 2: Use RSA-SHA256 if you need RSA
# Edit ~/.ssh/config
Host github.com
  HostkeyAlgorithms +ssh-rsa
  PubkeyAcceptedAlgorithms +ssh-rsa
```

---

### Multiple SSH keys for different accounts

**Problem:** You have personal and work accounts.

**Solution:**

```bash
# 1. Generate separate keys
ssh-keygen -t ed25519 -f ~/.ssh/id_personal -C "personal@example.com"
ssh-keygen -t ed25519 -f ~/.ssh/id_work -C "work@company.com"

# 2. Configure ~/.ssh/config
nano ~/.ssh/config

# 3. Add configuration
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_personal

Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_work

# 4. Clone using alias
git clone git@github-personal:username/personal-repo.git
git clone git@github-work:company/work-repo.git
```

---

## Agent Problems

### "Could not open a connection to your authentication agent"

**Symptom:** SSH agent is not running.

**Solution:**

```bash
# Start agent
eval "$(ssh-agent -s)"

# Add key
ssh-add ~/.ssh/id_ed25519

# Verify it's loaded
ssh-add -l
```

---

### "Identity added" but still asks for passphrase

**Symptom:** SSH agent doesn't persist between sessions.

**Solution (macOS):**

```bash
# Edit ~/.ssh/config
nano ~/.ssh/config

# Add
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

**Solution (Linux):**

```bash
# Add to ~/.bashrc or ~/.zshrc
if [ -z "$SSH_AUTH_SOCK" ]; then
  eval $(ssh-agent -s)
  ssh-add ~/.ssh/id_ed25519
fi
```

---

### SSH agent has too many keys

**Symptom:** Error when trying to connect.

```
Received disconnect from x.x.x.x: Too many authentication failures
```

**Solution:**

```bash
# View loaded keys
ssh-add -l

# Remove all keys
ssh-add -D

# Add only the necessary one
ssh-add ~/.ssh/id_ed25519

# Or specify in ~/.ssh/config
Host github.com
  IdentitiesOnly yes
  IdentityFile ~/.ssh/id_ed25519
```

---

## Advanced Diagnosis

### Test connection with verbose

```bash
# Level 1 verbose
ssh -v git@github.com

# Level 3 (very detailed)
ssh -vvv git@github.com

# Look in output for:
# - "debug1: Offering public key" (key being used)
# - "debug1: Server accepts key" (key accepted)
# - "debug1: Authentication succeeded" (success)
```

---

### Check which key Git is using

```bash
# View current configuration
git config --get remote.origin.url

# Verify with SSH
ssh -vT git@github.com 2>&1 | grep "identity file"
```

---

### Test connection without config

```bash
# Test with specific key
ssh -i ~/.ssh/id_ed25519 -T git@github.com

# Test alternative port
ssh -p 443 -T git@ssh.github.com
```

---

## Prevention Tips

### Recommended ~/.ssh/config configuration

```bash
# GitHub
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes

# GitLab
Host gitlab.com
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes

# Bitbucket
Host bitbucket.org
  HostName bitbucket.org
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes
```

---

### Verification script

```bash
#!/bin/bash
# Save as check-ssh.sh

echo "=== Checking SSH configuration ==="

# 1. Check keys
echo "1. SSH Keys:"
ls -la ~/.ssh/*.pub 2>/dev/null || echo "No public keys"

# 2. Check permissions
echo "2. Permissions:"
ls -ld ~/.ssh/
ls -l ~/.ssh/id_*

# 3. Check agent
echo "3. SSH Agent:"
ssh-add -l || echo "Agent not running or no keys"

# 4. Test connections
echo "4. Testing GitHub:"
ssh -T git@github.com

echo "5. Testing GitLab:"
ssh -T git@gitlab.com

echo "6. Testing Bitbucket:"
ssh -T git@bitbucket.org
```

---

## Additional Resources

- 📖 **SSH Cheat Sheet:** [/cheat-sheets/ssh-gpg-keys.md](/cheat-sheets/ssh-gpg-keys.md)
- 🔐 **SSH vs HTTPS Guide:** [/guides/en/authentication-ssh-https.md](/guides/en/authentication-ssh-https.md)
- 🛠️ **Common Problems:** [common-issues.md](common-issues.md)

---

**Solved the problem?** Share your solution or report new issues.
