# What is Git?

Every developer has experienced the problem: you're working on a feature, everything breaks, and you desperately wish you could rewind to when things worked. Or worse, you and a teammate both edit the same file, and now someone's work will be lost. These aren't edge cases—they're fundamental challenges in software development that version control systems exist to solve.

Git is a distributed version control system (DVCS) that tracks changes to files over time. Created by Linus Torvalds in 2005 for Linux kernel development, Git has become the de facto standard for source code management. Unlike centralized systems (like SVN or CVS), every Git repository contains the complete project history, making operations fast and enabling offline work.

## How Git Works

At its core, Git is a content-addressable filesystem with a version control interface built on top. When you commit changes, Git creates a snapshot of your project at that moment, storing only the differences (deltas) from the previous version. Each commit is identified by a unique SHA-1 hash, forming a [directed acyclic graph (DAG)](https://cs.bradpenney.io/building_blocks/binary_trees_and_representation/) of project history.

**Three States of Git**

Files in a Git repository exist in one of three states:

| State | Location | Meaning |
|:------|:---------|:--------|
| **Modified** | Working Directory | Changed but not staged for commit |
| **Staged** | Staging Area (Index) | Marked to be included in next commit |
| **Committed** | Git Directory (.git) | Safely stored in repository history |

This three-state model is Git's secret weapon. The staging area lets you craft precise commits, including only the changes you want while leaving experimental work uncommitted.

## Why Git Matters

**Collaboration at Scale**

Git enables distributed workflows where hundreds of developers can work simultaneously on the same codebase without stepping on each other's toes. Pull requests, code review, and branching strategies all build on Git's foundation.

**Fearless Experimentation**

With Git, you can try risky refactorings, experiment with new approaches, or explore alternative implementations—all without fear of losing working code. Branches are cheap, merging is sophisticated, and you can always revert if things go wrong.

**Accountability and Traceability**

Every change is attributed to a specific author with a timestamp and message explaining *why* the change was made. When bugs surface months later, `git blame` and `git log` reveal the context behind every line of code.

**Industry Standard**

Understanding Git isn't optional in modern software development. From open source contributions to enterprise deployments, Git knowledge is assumed. Platforms like GitHub, GitLab, and Bitbucket build entire ecosystems on Git's foundation.

## Core Git Concepts

### Repositories

A Git repository (repo) is a directory containing your project and its `.git` subdirectory, which stores all version history and metadata. You create a repo with:

```bash title="Initialize a Git Repository" linenums="1"
git init
```

Or clone an existing one:

```bash title="Clone a Remote Repository" linenums="1"
git clone https://github.com/user/repo.git
```

### Commits

Commits are snapshots of your project at specific points in time. Each commit has:

- A unique SHA-1 hash identifier
- Author information and timestamp
- A commit message describing the changes
- A pointer to its parent commit(s)

```bash title="Create a Commit" linenums="1"
git add file.txt              # (1)!
git commit -m "Add feature X" # (2)!
```

1. Stage changes to `file.txt` for commit
2. Create a commit with a descriptive message

### Branches

Branches are lightweight, movable pointers to commits. The default branch is typically `main` or `master`. Creating and switching branches is instantaneous:

```bash title="Branch Operations" linenums="1"
git branch feature-x      # Create branch
git checkout feature-x    # Switch to branch
# Or do both at once:
git checkout -b feature-x # (1)!
```

1. Creates and checks out `feature-x` in one command

### Merging

Merging integrates changes from one branch into another:

```bash title="Merge a Branch" linenums="1"
git checkout main         # Switch to target branch
git merge feature-x       # Merge feature-x into main
```

Git uses sophisticated algorithms to merge changes automatically when possible. When it can't (merge conflict), it asks you to resolve the conflict manually.

## Common Workflows

**Basic Local Workflow**

1. Modify files in working directory
2. Stage changes: `git add <files>`
3. Commit staged changes: `git commit -m "message"`
4. Repeat

**Collaborating with Remotes**

1. Fetch changes: `git fetch origin`
2. Merge or rebase: `git merge origin/main`
3. Push your commits: `git push origin main`

**Feature Branch Workflow**

1. Create feature branch: `git checkout -b feature-name`
2. Make commits on feature branch
3. Merge back to main when complete
4. Delete feature branch: `git branch -d feature-name`

## Practice Problems

??? question "Practice Problem 1: Three States"

    If you modify a file but don't run `git add`, which state is the file in?

    ??? tip "Answer"

        The file is in the **modified** state. It exists in your working directory with changes, but those changes haven't been staged for commit yet. Running `git status` would show it under "Changes not staged for commit."

??? question "Practice Problem 2: Commit Identification"

    Why does Git use SHA-1 hashes instead of sequential numbers (like revision 1, 2, 3) to identify commits?

    ??? tip "Answer"

        SHA-1 hashes enable **distributed development**. In a centralized system, a central server can assign sequential numbers. But in Git, thousands of developers might create commits simultaneously in their local repositories. SHA-1 hashes are effectively unique (probability of collision is astronomically low), so each commit has a globally unique identifier without requiring coordination with a central authority.

??? question "Practice Problem 3: Branching Cost"

    How does Git make branching so "cheap" compared to older version control systems that copied the entire codebase?

    ??? tip "Answer"

        A Git branch is just a **41-byte file** containing a SHA-1 hash (40 characters) plus a newline. It's a pointer to a commit, not a copy of the code. Creating a branch doesn't duplicate any files—it just creates a new pointer. This is why creating, deleting, and switching branches is nearly instantaneous regardless of project size.

## Key Takeaways

| Concept | What It Means |
|:--------|:--------------|
| **DVCS** | Every repository has complete history; no single point of failure |
| **Three States** | Modified → Staged → Committed workflow enables precise control |
| **Commits** | Immutable snapshots identified by SHA-1 hash |
| **Branches** | Lightweight pointers to commits; cheap to create and merge |
| **Staging Area** | Intermediate area for crafting commits with exactly the right changes |
| **DAG** | Git history is a directed acyclic graph, enabling complex branching |

## Further Reading

- [Pro Git Book](https://git-scm.com/book/en/v2) - Comprehensive, free guide to Git (especially Chapters 1-3)
- [Git Documentation](https://git-scm.com/doc) - Official reference documentation
- [A Visual Git Reference](https://marklodato.github.io/visual-git-guide/index-en.html) - Visual diagrams of Git operations
- [Git from the Bottom Up](https://jwiegley.github.io/git-from-the-bottom-up/) - Understanding Git's internals
- [Learn Git Branching](https://learngitbranching.js.org/) - Interactive visual tutorial

---

Git isn't just a tool for tracking changes—it's a way of thinking about software evolution, embodying the principles of [computational thinking](https://cs.bradpenney.io/building_blocks/computational_thinking/)—decomposition, pattern recognition, and abstraction applied to code management. The developers who master Git don't just avoid losing work; they use version control as a thinking tool, crafting clear history that tells the story of how and why their software evolved. That clarity compounds over time, making codebases easier to understand, debug, and maintain.
