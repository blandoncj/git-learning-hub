# Initial Git Configuration

After installing Git, you need to perform basic configuration before starting to work.

## User Configuration

### User Name

The first configuration is setting your name. This name will appear in all your commits.

```bash
git config --global user.name "Your Name"
```

Example:

```bash
git config --global user.name "John Doe"
```

### Email

Next, configure your email. It will also appear in your commits.

```bash
git config --global user.email "your.email@example.com"
```

Example:

```bash
git config --global user.email "john@example.com"
```

### Verify Configuration

To verify it was saved correctly:

```bash
git config --global user.name
git config --global user.email
```

Or view all global configuration:

```bash
git config --global --list
```

---

## Optional but Recommended Configurations

### Default Editor

Configure which editor you want to use when Git needs you to write messages. Default is `nano`.

**Nano (simple, recommended for beginners):**

```bash
git config --global core.editor "nano"
```

**VSCode:**

```bash
git config --global core.editor "code --wait"
```

**Vim:**

```bash
git config --global core.editor "vim"
```

**Notepad++ (Windows):**

```bash
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

### Default Branch Name

Since Git 2.28, you can change the default branch name (previously was `master`, now is `main`):

```bash
git config --global init.defaultBranch main
```

### Useful Aliases

Aliases allow you to create shortcuts for frequently used commands:

```bash
# View commits in one line
git config --global alias.lg "log --oneline --graph --all"

# Short status
git config --global alias.st "status -s"

# View differences
git config --global alias.d "diff"

# View branches
git config --global alias.br "branch -a"

# Commit
git config --global alias.cm "commit -m"

# Add files
git config --global alias.ad "add"
```

After this, you can use:

```bash
git lg      # instead of git log --oneline --graph --all
git st      # instead of git status -s
```

### Colorize Output

Enable colors in Git output for better readability:

```bash
git config --global color.ui true
```

---

## Global vs Local vs System Configuration

Git has three levels of configuration:

### 1. Global (`--global`)

Applies to all repositories of your user.

```bash
git config --global user.name "Your Name"
```

Saved in: `~/.gitconfig`

### 2. Local (no flag)

Applies only to the current repository.

```bash
git config user.name "Specific Name for this Project"
```

Saved in: `.git/config`

### 3. System (`--system`)

Applies to all users on the computer (requires admin permissions).

```bash
sudo git config --system user.name "System Name"
```

Saved in: `/etc/gitconfig`

---

## View Your Configuration

### View global configuration only

```bash
git config --global --list
```

### View local configuration of current repository

```bash
git config --local --list
```

### View system configuration

```bash
git config --system --list
```

### View all configuration (all levels)

```bash
git config --list
```

---

## Configuration Files

### Locations

**Windows:**

- Global: `C:\Users\[your-username]\.gitconfig`
- System: `C:\Program Files\Git\etc\gitconfig`

**macOS/Linux:**

- Global: `~/.gitconfig` or `~/.config/git/config`
- System: `/etc/gitconfig`

### Edit Directly (Advanced)

You can manually edit these files. Example of `.gitconfig`:

```ini
[user]
    name = Your Name
    email = your.email@example.com

[core]
    editor = code --wait

[init]
    defaultBranch = main

[alias]
    st = status -s
    lg = log --oneline --graph --all
    cm = commit -m
```

---

## First Steps After Configuration

Once you've completed the configuration, you're ready to:

1. **Create your first repository:**

   ```bash
   mkdir my-project
   cd my-project
   git init
   ```

2. **Or clone an existing repository:**

   ```bash
   git clone https://github.com/username/repository.git
   ```

3. **Check status:**

   ```bash
   git status
   ```

---

## Multiple Accounts Configuration (Advanced)

If you work with multiple Git accounts (personal and work), see the conditional configuration section in the [official documentation](https://git-scm.com/docs/git-config#_conditional_includes).

---

## Next Steps

Now that you have Git configured:

1. **[Basic Concepts](basic-concepts.md)** - Learn key terminology
2. Read the guide on **[SSH vs HTTPS Authentication](/guias/en/authentication-ssh-https.md)** to connect to remote repositories
3. Start with **[basic level exercises](/ejercicios/nivel-basico)**

---

**Previous:** [Installation](installation.md) | **Next:** [Basic Concepts](basic-concepts.md)
