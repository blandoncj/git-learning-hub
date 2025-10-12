# GPG Configuration - Sign Your Commits

Complete guide on how to use GPG (GNU Privacy Guard) to digitally sign your Git commits. You'll learn why it's important, how to set it up, and how to verify signed commits.

## Table of Contents

1. [What is GPG?](#what-is-gpg)
2. [Why Sign Commits?](#why-sign-commits)
3. [Generate a GPG Key](#generate-a-gpg-key)
4. [Configure Git with GPG](#configure-git-with-gpg)
5. [Sign Commits](#sign-commits)
6. [Verify Signed Commits](#verify-signed-commits)
7. [Setup in GitHub/GitLab/Bitbucket](#setup-in-githubgitlabbitbucket)
8. [Troubleshooting](#troubleshooting)

---

## What is GPG?

### Definition

**GPG (GNU Privacy Guard)** is open-source encryption software that implements the OpenPGP standard. It allows you to encrypt data and create digital signatures using asymmetric cryptography (public-private key).

### In the Context of Git

When you sign a commit with GPG:

- Git creates a digital signature using your private key
- The signature is attached to the commit
- Anyone can verify that you (and only you) created that commit using your public key
- GitHub/GitLab/Bitbucket mark the commit as "Verified"

### Difference Between SSH and GPG

**SSH:** Authenticates who you are (repository access)
**GPG:** Verifies commit authenticity (digital signature)

Both should be used together.

---

## Why Sign Commits?

### Problems Without Signatures

Without GPG, anyone could:

```bash
git config user.name "Linus Torvalds"
git commit -m "Important change"
git push
```

The commit would appear as if Linus Torvalds made it, but it was someone else.

### Benefits of Signing

✅ **Authenticity:** Proves you created the commit
✅ **Non-repudiation:** You can't deny you created it
✅ **Integrity:** Commit can't be modified after signing
✅ **Compliance:** Many companies and projects require it
✅ **Trust:** Collaborators know code is legitimate
✅ **Audit:** Verifiable history of who did what

### Use Cases

- Important open source projects
- Critical production code
- Regulatory compliance (financial, medical, etc.)
- Distributed teams requiring maximum security

---

## Generate a GPG Key

### Step 1: Install GPG

**Windows:**
Download from [gpg4win.org](https://www.gpg4win.org/) or use:

```bash
choco install gpg4win
```

**macOS:**

```bash
brew install gnupg
```

**Linux (Debian/Ubuntu):**

```bash
sudo apt install gnupg
```

**Linux (Fedora/RHEL):**

```bash
sudo dnf install gnupg
```

### Step 2: Verify Installation

```bash
gpg --version
```

You should see: `gpg (GnuPG) 2.x.x` or similar.

### Step 3: Generate the Key

```bash
gpg --full-generate-key
```

You'll be asked several questions:

**1. Key Type:**

```
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
  (14) Existing key from card
Your selection? 1
```

Choose **1** (RSA and RSA)

**2. Key Size:**

```
What keysize do you want? (3072)
```

Choose **4096** (more secure):

```
What keysize do you want? 4096
```

**3. Key Validity:**

```
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
```

Press **Enter** for no expiration (0), or choose a time.

**4. Confirmation:**

```
Is this correct? (y/N) y
```

Enter **y** (yes)

**5. Personal Information:**

```
GnuPG needs to construct a user ID to identify your key.

Real name: Your Full Name
Email address: your.email@example.com
Comment: My work key (optional)
```

Enter your real information (must match your Git config).

**6. Final Confirmation:**

```
Is this correct? (y/N) y
```

Enter **y**

**7. Passphrase:**

```
You need a Passphrase to protect your secret key.
```

A dialog will open. Enter a strong password (you'll use it to sign commits).

**Expected Result:**

```
gpg: key 1234567890ABCDEF generated
gpg: revocation certificate stored as '/home/user/.gnupg/revocation-certs.d/1234567890ABCDEF.rev'
public key created successfully
```

### Step 4: Verify the Key

```bash
gpg --list-secret-keys --keyid-format LONG
```

You'll see something like:

```
sec   rsa4096/1234567890ABCDEF 2024-01-15 [SC]
      1234567890ABCDEF1234567890ABCDEF12345678
uid                 [ultimate] Your Name <your.email@example.com>
ssb   rsa4096/FEDCBA0987654321 2024-01-15 [E]
```

Your key ID is: **1234567890ABCDEF** (16 characters after rsa4096/)

---

## Configure Git with GPG

### Step 1: Tell Git Your Key ID

```bash
git config --global user.signingkey 1234567890ABCDEF
```

Replace `1234567890ABCDEF` with your actual ID.

**To Verify:**

```bash
git config --global user.signingkey
```

### Step 2: Configure Git to Sign by Default (Optional)

To automatically sign all your commits:

```bash
git config --global commit.gpgsign true
```

**Advantage:** No need to add `-S` to each commit.
**Disadvantage:** Each commit will ask for your passphrase.

### Step 3: Configure GPG Agent (Optional but Recommended)

To avoid entering your passphrase each time, configure GPG Agent:

**macOS/Linux:**

Edit or create `~/.gnupg/gpg-agent.conf`:

```bash
default-cache-ttl 600
max-cache-ttl 7200
```

This caches your passphrase for 10 minutes (600 seconds).

Then restart GPG Agent:

```bash
gpgconf --kill gpg-agent
```

**Windows:**

It's already configured automatically. If you have issues, reinstall GPG4Win.

---

## Sign Commits

### Option 1: Sign an Individual Commit

```bash
git commit -S -m "Your commit message"
```

The `-S` means "sign". Git will ask for your GPG passphrase.

### Option 2: Sign with Automatic Configuration

If you configured `commit.gpgsign = true`:

```bash
git commit -m "Your commit message"
```

Git will sign automatically.

### Option 3: Sign an Existing Commit (Amend)

If you forgot to sign:

```bash
git commit --amend -S
```

### Complete Examples

```bash
# Sign and push
git add file.txt
git commit -S -m "Add new feature"
git push origin main

# View log with signatures
git log --show-signature
```

---

## Verify Signed Commits

### In the Terminal

**View commits with signatures:**

```bash
git log --show-signature
```

You'll see:

```
commit abc123 (HEAD -> main)
gpg: Signature made Mon Jan 15 10:30:45 2024
gpg:                using RSA key 1234567890ABCDEF
gpg:                issuer "your.email@example.com"
gpg: Good signature from "Your Name <your.email@example.com>"
```

**View commits in one line with signature status:**

```bash
git log --oneline --pretty='%h %G? %s'
```

You'll see:

```
abc123 G Add new feature
def456 N Unsigned change
```

**Meanings:**

- **G:** Good signature (Good)
- **B:** Bad signature (Bad)
- **U:** Unknown trust (Unknown)
- **X:** Signature expired (eXpired)
- **Y:** Signature expiring soon (expireY)
- **R:** Signature revoked (Revoked)
- **E:** No signature, could not verify
- **N:** No signature (None)

### On GitHub

Once you add your GPG public key to GitHub (see next section):

1. Go to your repository
2. View the commit history
3. Signed commits will show a green "Verified" badge

### On GitLab

GitLab shows a "Verified" icon on correctly signed commits.

### On Bitbucket

Bitbucket shows signature information in the commit details.

---

## Setup in GitHub/GitLab/Bitbucket

### GitHub

#### Step 1: Copy Your GPG Public Key

```bash
gpg --armor --export your.email@example.com
```

Copy the entire output (including `-----BEGIN PGP PUBLIC KEY BLOCK-----` and `-----END PGP PUBLIC KEY BLOCK-----`).

#### Step 2: Add to GitHub

1. Go to [github.com/settings/keys](https://github.com/settings/keys)
2. Click "New GPG key"
3. Paste your public key
4. Click "Add GPG key"

#### Step 3: Verify

Make a signed commit and push:

```bash
git commit -S -m "Test signature"
git push
```

You'll see the "Verified" badge on GitHub.

### GitLab

#### Step 1: Copy Your Public Key

```bash
gpg --armor --export your.email@example.com
```

#### Step 2: Add to GitLab

1. Go to [gitlab.com/-/profile/gpg_keys](https://gitlab.com/-/profile/gpg_keys)
2. Paste your public key
3. Click "Add key"

#### Step 3: Verify

Signed commits will show "Verified" on GitLab.

### Bitbucket

#### Step 1: Copy Your Public Key

```bash
gpg --armor --export your.email@example.com
```

#### Step 2: Add to Bitbucket

1. Go to [bitbucket.org/account/settings/gpg-keys/](https://bitbucket.org/account/settings/gpg-keys/)
2. Paste your public key
3. Click "Add key"

#### Step 3: Verify

Signed commits will show an indicator on Bitbucket.

---

## Troubleshooting

### "gpg: signing failed: No secret key"

**Cause:** Git couldn't find your GPG key.

**Solution:**

```bash
# Verify you have keys
gpg --list-secret-keys

# Configure the correct key ID
git config --global user.signingkey YOUR_KEY_ID
```

### "gpg: failed to sign the data"

**Cause:** Problem with GPG Agent.

**Solution:**

```bash
# Restart GPG Agent
gpgconf --kill gpg-agent

# Try again
git commit -S -m "Test"
```

### "error: gpg failed to sign the data"

**Cause:** Wrong passphrase or GPG not configured.

**Solution:**

```bash
# Test GPG directly
echo "test" | gpg --clearsign

# If it asks for passphrase and works, the issue is Git
# Verify Git configuration
git config --global user.signingkey
```

### GPG asks for passphrase every time

**Cause:** GPG Agent not caching.

**Solution:**

Edit `~/.gnupg/gpg-agent.conf` and add:

```
default-cache-ttl 600
max-cache-ttl 7200
pinentry-program /usr/bin/pinentry-tty
```

Then restart:

```bash
gpgconf --kill gpg-agent
```

### "No public key available"

**Cause:** Your public key not added to GitHub/GitLab/Bitbucket.

**Solution:**

Follow the steps in [Setup in GitHub/GitLab/Bitbucket](#setup-in-githubgitlabbitbucket).

### Verify GPG Works

```bash
# Sign test
echo "test message" | gpg --clearsign

# If it works, you'll see:
# -----BEGIN PGP SIGNED MESSAGE-----
# ...
# -----END PGP SIGNATURE-----
```

---

## Summary: Complete Workflow

```
1. Install GPG
   ↓
2. Generate GPG key
   ↓
3. Configure Git with your key ID
   ↓
4. Copy public key to GitHub/GitLab/Bitbucket
   ↓
5. Sign commits (-S flag)
   ↓
6. See "Verified" on remote repository
```

---

## Quick Commands

```bash
# Generate key
gpg --full-generate-key

# List keys
gpg --list-secret-keys --keyid-format LONG

# Export public key
gpg --armor --export your.email@example.com

# Configure Git
git config --global user.signingkey KEY_ID
git config --global commit.gpgsign true

# Sign commit
git commit -S -m "Message"

# View signed commits
git log --show-signature
git log --oneline --pretty='%h %G? %s'

# Restart GPG Agent
gpgconf --kill gpg-agent
```

---

## Next Steps

1. **[Branch Management](branch-management.md)** - Create and work with branches
2. **[Workflow](workflow.md)** - Team workflows
3. **[Tracking Changes](tracking-changes.md)** - Code audit

---

**Previous:** [SSH/HTTPS Authentication](authentication-ssh-https.md) | **Next:** [Branch Management](branch-management.md)
