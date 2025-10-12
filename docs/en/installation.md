# Git Installation

Step-by-step guide to install Git on Windows, macOS, and Linux.

## Windows

### Option 1: Git for Windows (Recommended)

1. Download the installer from [git-scm.com](https://git-scm.com/download/win)
2. Run the downloaded `.exe` file
3. During installation, we recommend keeping the default options:
   - **Git Bash Here:** ✓ (useful for Git terminal)
   - **Git GUI Here:** ✓ (optional graphical interface)
   - **Use Git from Windows Command Prompt:** ✓ (recommended)

4. Complete the installation by clicking "Next" until finished

### Verify Installation

Open **PowerShell** or **Command Prompt** and run:

```bash
git --version
```

You should see something like: `git version 2.42.0.windows.1`

### Option 2: Windows Package Manager

If you have Windows Package Manager installed:

```bash
winget install Git.Git
```

---

## macOS

### Option 1: Official Installer

1. Download from [git-scm.com](https://git-scm.com/download/mac)
2. Run the `.dmg` installer
3. Follow the wizard's instructions
4. Complete the installation

### Option 2: Homebrew (Recommended for macOS)

If you have [Homebrew](https://brew.sh/) installed:

```bash
brew install git
```

### Option 3: Xcode Command Line Tools

Install Xcode tools which include Git:

```bash
xcode-select --install
```

### Verify Installation

Open **Terminal** and run:

```bash
git --version
```

---

## Linux

### Ubuntu / Debian

```bash
sudo apt update
sudo apt install git
```

### Fedora / RHEL / CentOS

```bash
sudo dnf install git
```

Or for older versions:

```bash
sudo yum install git
```

### Arch Linux

```bash
sudo pacman -S git
```

### Verify Installation

```bash
git --version
```

---

## General Verification

After installing on any system, run these commands to ensure everything works correctly:

```bash
# Check Git version
git --version

# Check Git location
which git        # macOS/Linux
where git        # Windows

# View help
git --help
```

---

## Installing Visual Studio Code (Recommended)

For a better experience working with Git, we recommend using Visual Studio Code, which has built-in Git support.

### Download VS Code

1. Go to [code.visualstudio.com](https://code.visualstudio.com/)
2. Download for your operating system
3. Follow the installer instructions

### Recommended Extensions for Git

Once VS Code is installed, you can add these extensions (optional but useful):

- **GitLens** - Advanced Git visualization
- **Git Graph** - Graphical visualization of branches and commits
- **GitHub Copilot** - Code assistance

---

## Installing a Graphical Git Client (Optional)

If you prefer a graphical interface instead of command line:

### SourceTree

- **Platform:** Windows and macOS
- **Cost:** Free
- **Download:** [sourcetreeapp.com](https://www.sourcetreeapp.com/)

### GitKraken

- **Platform:** Windows, macOS, Linux
- **Cost:** Free (basic version)
- **Download:** [gitkraken.com](https://www.gitkraken.com/)

### GitHub Desktop

- **Platform:** Windows and macOS
- **Cost:** Free
- **Download:** [desktop.github.com](https://desktop.github.com/)

### Tortoise Git (Windows)

- **Platform:** Windows only
- **Cost:** Free
- **Download:** [tortoisegit.org](https://tortoisegit.org/)

---

## Troubleshooting

### Error: "git command not found"

**On Windows:**

- Verify that Git is in your PATH. Try restarting your terminal or computer.
- Reinstall Git selecting "Use Git from Windows Command Prompt"

**On macOS/Linux:**

- Check installation with: `which git`
- If not installed, follow the installation steps again

### Error: "Permission denied" (Linux/macOS)

If you get permission errors:

```bash
sudo chown -R $USER:$USER ~/.git-credentials
```

### Git is Slow

Try updating to the latest version:

```bash
git --version
```

And update manually if needed from [git-scm.com](https://git-scm.com/)

---

## Next Steps

Once Git is correctly installed:

1. **[Initial Configuration](initial-configuration.md)** - Configure your user and email
2. **[Basic Concepts](basic-concepts.md)** - Understand key terminology
3. **[Introduction](introduction.md)** - Review general concepts

---

**Previous:** [Introduction](introduction.md) | **Next:** [Initial Configuration](initial-configuration.md)
