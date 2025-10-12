# Introduction to Git

## What is Git?

Git is a **distributed version control system** designed to track changes in files and coordinate work between multiple developers. It was created by Linus Torvalds in 2005 to manage the development of the Linux kernel.

### Key Features

- **Distributed:** Each developer has a complete copy of the project's history.
- **Fast and Efficient:** Optimized to work with projects of any size.
- **Secure:** Uses SHA-1 hashes to guarantee data integrity.
- **Flexible:** Supports multiple workflows and branching strategies.
- **Offline:** Allows you to work without an internet connection.

## Why Use Git?

### 1. Change Control

Git allows you to record every change made to your code, who made it, and when. You can see the complete history of your project at any time.

### 2. Collaboration

Multiple developers can work on the same project simultaneously without overwriting each other's changes. Git facilitates integrating changes from different people.

### 3. Branching

You can create branches to work on new features without affecting the main code. You can then merge the changes when they're ready.

### 4. Error Recovery

If you make a mistake, you can easily revert changes or go back to a previous version of your code.

### 5. Traceability

Every change includes information about who made it, when, and why (through the commit message). This is crucial for auditing and maintenance.

### 6. Professional Experience

Git is an industry standard tool. Learning it is essential for any software developer.

## Basic Concepts

### Repository (Repo)

A repository is a folder that contains all the files of your project and the complete history of changes. There are two types:

- **Local Repository:** Located on your computer
- **Remote Repository:** Located on a server (GitHub, GitLab, Bitbucket, etc.)

### Commit

A commit is a "snapshot" of the state of your project at a specific moment. It contains:

- The changes made
- A descriptive message
- Author and date
- A unique identifier (hash)

### Branch

A branch is an independent line of development. The default branch is called `main` or `master`. Branches allow you to work on new features without affecting the main code.

### Push and Pull

- **Push:** Send your local commits to the remote repository
- **Pull:** Download changes from the remote repository to your local copy

### Merge (Fusion)

Combine changes from one branch with another. For example, merging a feature branch with the main branch.

### Conflict

Occurs when Git cannot automatically merge two changes because they affect the same lines of code.

## Remote Repository Platforms

### GitHub

- The most popular platform for hosting Git repositories
- Owned by Microsoft
- Free for public and private repositories
- Excellent for collaboration and open source projects

### GitLab

- Open source alternative to GitHub
- Offers both cloud hosting and self-hosted options
- Advanced CI/CD features built-in
- Popular in enterprise environments

### Bitbucket

- Owned by Atlassian
- Integration with other Atlassian tools (Jira, Confluence)
- Free for small teams
- Popular in enterprise environments

## Basic Workflow

```
1. Clone (Clone)
   └─> Download remote repository to your machine

2. Create Branch (Branch)
   └─> Create a branch to work on a new feature

3. Modify Files
   └─> Make changes to the code

4. Prepare Changes (Stage)
   └─> Mark files for the next commit

5. Make Commit
   └─> Save changes with a descriptive message

6. Make Push
   └─> Send commits to the remote repository

7. Create Pull Request
   └─> Request that changes be reviewed and integrated

8. Merge
   └─> Merge changes into the main branch
```

## Next Steps

Once you understand these basic concepts, you can:

1. **[Install Git](installation.md)** - Set up Git on your computer
2. **[Initial Configuration](initial-configuration.md)** - First steps after installation
3. **[Basic Concepts](basic-concepts.md)** - Dive deeper into key terminology

## Additional Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [GitHub Guides](https://guides.github.com/)
- [Pro Git Book](https://git-scm.com/book/en/v2)

---

**Next:** [Installation](installation.md)
