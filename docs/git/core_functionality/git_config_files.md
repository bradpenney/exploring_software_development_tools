# Git Configuration Files

You run `git commit`, and Git knows your name and email. You clone a repository, and Git remembers your preferred merge strategy. You check out a branch, and your editor automatically uses the right tab settings. None of this happens by accident—it's all controlled by Git's configuration system.

Git's configuration is hierarchical, allowing settings to cascade from system-wide defaults to repository-specific overrides. Understanding this system is essential for customizing Git's behavior to match your workflow and your team's standards.

## The Three Levels of Configuration

Git stores configuration in three locations, each with different scope and precedence:

| Level | Location | Scope | Command Flag |
|:------|:---------|:------|:-------------|
| **System** | `/etc/gitconfig` | All users on the system | `--system` |
| **Global** | `~/.gitconfig` or `~/.config/git/config` | Current user, all repositories | `--global` |
| **Local** | `.git/config` | Specific repository only | `--local` (default) |

**Precedence Order**: Local > Global > System

Settings in `.git/config` (local) override settings in `~/.gitconfig` (global), which override settings in `/etc/gitconfig` (system). This allows you to set sensible defaults globally while overriding specific settings per-project when needed.

## System Configuration (`/etc/gitconfig`)

System-level configuration affects all users on a machine. This file is rarely used on personal computers but can be useful in shared development environments, educational labs, or enterprise settings where administrators want to enforce certain defaults.

```bash title="View System Configuration" linenums="1"
git config --system --list
```

```bash title="Set System-Level Configuration" linenums="1"
sudo git config --system core.editor vim  # (1)!
```

1. Requires administrator privileges since `/etc/gitconfig` is system-wide

On most systems, this file either doesn't exist or is empty. That's normal—most configuration happens at the global or local level.

## Global Configuration (`~/.gitconfig`)

Global configuration applies to all Git repositories for the current user. This is where you set your identity, preferred tools, and personal workflow preferences.

**Essential Global Settings**

```bash title="Configure User Identity" linenums="1"
git config --global user.name "Jane Developer"     # (1)!
git config --global user.email "jane@example.com"  # (2)!
```

1. Your name appears in commit metadata
2. Your email links commits to your GitHub/GitLab account

```bash title="Configure Default Editor" linenums="1"
git config --global core.editor nvim  # (1)!
```

1. Sets NeoVim as the default editor for commit messages and interactive rebase

```bash title="Configure Default Branch Name" linenums="1"
git config --global init.defaultBranch main  # (1)!
```

1. New repositories will use `main` instead of `master`

**Viewing Global Configuration**

```bash title="List All Global Settings" linenums="1"
git config --global --list
```

```bash title="View a Specific Setting" linenums="1"
git config --global user.email
```

**Editing the Config File Directly**

```bash title="Open Config in Editor" linenums="1"
git config --global --edit
```

The `~/.gitconfig` file uses INI format:

```ini title="~/.gitconfig Example" linenums="1"
[user]
    name = Jane Developer
    email = jane@example.com

[core]
    editor = nvim
    autocrlf = input

[init]
    defaultBranch = main

[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    unstage = reset HEAD --
    last = log -1 HEAD

[pull]
    rebase = true

[merge]
    conflictstyle = diff3
```

## Local Configuration (`.git/config`)

Local configuration applies only to a specific repository. Use this for project-specific settings that shouldn't affect other repositories.

**Common Use Cases**

```bash title="Override User Email for Work Project" linenums="1"
cd ~/work/project
git config user.email "jane@company.com"  # (1)!
```

1. Without `--global`, changes apply only to this repository

```bash title="Set Repository-Specific Remote URL" linenums="1"
git config remote.origin.url git@github.com:company/repo.git
```

```bash title="Configure Branch-Specific Merge Strategy" linenums="1"
git config branch.main.rebase true
```

**Viewing Local Configuration**

```bash title="List Local Settings Only" linenums="1"
git config --local --list
```

The `.git/config` file has the same INI format:

```ini title=".git/config Example" linenums="1"
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true

[remote "origin"]
    url = git@github.com:user/repo.git
    fetch = +refs/heads/*:refs/remotes/origin/*

[branch "main"]
    remote = origin
    merge = refs/heads/main
    rebase = true

[user]
    email = jane@company.com
```

## Useful Configuration Settings

### Line Ending Handling

Different operating systems use different line endings (Windows: CRLF, Unix/Mac: LF). Git can normalize these:

```bash title="Line Ending Configuration" linenums="1"
# Linux/Mac: convert CRLF to LF on commit
git config --global core.autocrlf input

# Windows: convert LF to CRLF on checkout, CRLF to LF on commit
git config --global core.autocrlf true
```

### Aliases for Common Commands

```bash title="Create Git Aliases" linenums="1"
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD --'
git config --global alias.visual 'log --oneline --graph --all'
```

Now `git st` is shorthand for `git status`. Advanced aliases can use [regular expressions](https://cs.bradpenney.io/building_blocks/regular_expressions/) for pattern matching in Git commands.

### Merge and Diff Tools

```bash title="Configure Merge Tool" linenums="1"
git config --global merge.tool vimdiff
git config --global mergetool.prompt false
```

```bash title="Configure Diff Tool" linenums="1"
git config --global diff.tool vimdiff
```

### Pull Behavior

```bash title="Configure Pull to Rebase by Default" linenums="1"
git config --global pull.rebase true  # (1)!
```

1. Avoid merge commits when pulling; rebase local changes on top of remote

## Why Configuration Matters

**Identity and Attribution**

Your `user.name` and `user.email` are embedded in every commit you make. This isn't just metadata—it's how teams track contributions, how GitHub/GitLab links commits to accounts, and how `git blame` attributes code changes. Getting this right from the start avoids messy history rewrites later.

**Workflow Efficiency**

Aliases, editor configuration, and default behaviors save keystrokes and reduce friction. A well-configured Git setup feels natural and gets out of your way.

**Team Consistency**

Local configuration lets teams enforce project-specific standards (line endings, merge strategies, hooks) without affecting developers' personal global settings.

**Cross-Platform Development**

Line ending configuration prevents Windows and Unix systems from creating spurious diffs where only line endings changed.

## Practice Problems

??? question "Practice Problem 1: Configuration Precedence"

    You have `user.email` set to "personal@gmail.com" globally and "work@company.com" locally in a repository. When you commit in that repository, which email is used?

    ??? tip "Answer"

        The **local** setting (`work@company.com`) is used. Local configuration has higher precedence than global, allowing you to override personal defaults for work projects.

??? question "Practice Problem 2: Finding a Setting"

    How do you determine which configuration file is providing a specific setting, especially if it's set at multiple levels?

    ??? tip "Answer"

        Use `git config --show-origin <key>`:

        ```bash
        git config --show-origin user.email
        ```

        This shows both the value and the file it came from, like:

        ```
        file:/home/user/.gitconfig    personal@gmail.com
        ```

??? question "Practice Problem 3: Alias Design"

    Why might `git config --global alias.uncommit 'reset --soft HEAD~1'` be a useful alias?

    ??? tip "Answer"

        This alias lets you "undo" the most recent commit while keeping the changes staged. Use cases:

        - You committed too early and want to add more changes
        - You made a typo in the commit message
        - You want to split one commit into multiple smaller commits

        `reset --soft HEAD~1` moves the branch pointer back one commit but leaves your staging area and working directory untouched.

## Key Takeaways

| Concept | What It Means |
|:--------|:--------------|
| **Three Levels** | System → Global → Local, with local having highest precedence |
| **user.name / user.email** | Required for commits; set globally but can override per-project |
| **core.editor** | Determines which editor Git opens for commit messages and rebases |
| **Aliases** | Custom shortcuts for common Git commands |
| **autocrlf** | Controls line ending conversion across platforms |
| **--show-origin** | Reveals which config file provides a setting |

## Further Reading

- [Git Configuration Documentation](https://git-scm.com/docs/git-config) - Complete reference for all config options
- [Pro Git: Chapter 8.1](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration) - Customizing Git Configuration
- [Git Aliases](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases) - Creating powerful aliases
- [Configuring Git (GitHub)](https://docs.github.com/en/get-started/getting-started-with-git/setting-your-username-in-git) - GitHub's guide to Git setup

---

Git's configuration system embodies the Unix philosophy: sensible defaults, easy customization, and clear precedence rules. Invest time in configuring Git once, and you'll reap the benefits every day. Whether it's avoiding cross-platform line ending issues, streamlining your workflow with aliases, or maintaining separate identities for personal and professional work, Git's configuration system makes it all manageable.
