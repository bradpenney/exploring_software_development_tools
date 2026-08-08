---
date: "2026-08-05 11:15"
title: "Git Workflows for Infrastructure"
description: "The feature-branch workflow built for IaC — peer review and CI before anything touches production, plus the one extra validation step a YAML merge conflict needs that code doesn't."
---

# Git Workflows for Infrastructure

<!-- PATHWAY_ROADMAP:START -->
<div class="pathway-pills" markdown>
:material-map-marker-path: <span class="pathway-pills__label">Part of a deep dive and a pathway:</span> [Git Essentials](../essentials/git/git_basics.md){: .pathway-pill } [Debugging With Nothing But a Terminal](https://bradpenney.io/pathways/nothing-but-a-terminal){: .pathway-pill }
</div>

??? abstract ":material-map-legend: Consult the map"

    <div class="grid cards two-col" markdown>

    -   :material-source-repository: __Git Essentials__ — step 3 of 3

        ---

        ← [Git Collaboration](../essentials/git/git_collaboration.md) · **you are here** · *(last step)* →

        [Start the deep dive →](../essentials/git/git_basics.md)

    -   :material-console: __Debugging With Nothing But a Terminal__ — step 18 of 20

        ---

        ← [Git Collaboration](https://tools.bradpenney.io/essentials/git/git_collaboration/) · **you are here** · [GitHub CLI (gh)](https://tools.bradpenney.io/efficiency/github_cli/) →

        [Start the pathway →](https://bradpenney.io/pathways/nothing-but-a-terminal)

    </div>
<!-- PATHWAY_ROADMAP:END -->

This article assumes Git itself — commits, branches, remotes — is already comfortable; see [Git Basics](../essentials/git/git_basics.md) if it isn't, including install instructions.

A change lands in a Terraform module on a feature branch. Meanwhile, a colleague merges a change to the same file in `main`. The result: a merge conflict in a 500-line YAML file. **This is where a solid workflow saves the day.**

In Platform Engineering, we don't just use Git to save code; we use it to coordinate changes to live environments. A structured workflow ensures that infrastructure changes are reviewed, tested, and deployed without causing outages.

## The Feature Branch Workflow (IaC Edition)

This is the industry standard for managing infrastructure changes. It prioritizes peer review and automated testing (CI) before any change touches production.

```mermaid
graph LR
    Main[main branch] -- "git checkout -b" --> Feature[feature/fix-lb-rule]
    Feature -- "git commit" --> Feature
    Feature -- "PR / Plan" --> Review{Peer Review}
    Review -- "Approve" --> Main
    Main -- "CD / Apply" --> Prod((Production))

    style Main fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style Feature fill:#d69e2e,stroke:#cbd5e0,stroke-width:2px,color:#000
    style Review fill:#d69e2e,stroke:#cbd5e0,stroke-width:2px,color:#000
    style Prod fill:#2f855a,stroke:#cbd5e0,stroke-width:2px,color:#fff
```

## Quick Start: The 5-Step Workflow

1.  **Sync**: `git checkout main && git pull origin main`
2.  **Branch**: `git checkout -b feat/add-logging`
3.  **Work**: Make changes, `git add`, and `git commit -m "feat: add logging to api"`
4.  **Update**: `git fetch origin main && git rebase origin/main` (Keep your branch up to date)
5.  **Push**: `git push origin feat/add-logging` and open a Pull Request.

## Why Workflows Matter for Platform Work

A bad Git workflow in application dev might delay a feature. A bad Git workflow in platform engineering can take down an entire region.

### Common Scenarios

=== ":material-source-merge: Resolving YAML Conflicts"

    YAML is whitespace-sensitive. When Git shows a conflict, it can be hard to see where the indentation broke.

    - Use a visual merge tool or VS Code's conflict resolution UI.
    - **Always** run `yq` or a linter on the file after resolving a conflict to ensure the syntax is still valid.
    - `yq eval '.' config.yaml` (If this fails, your resolution is broken).

=== ":material-history: Linear History with Rebasing"

    Platform teams often prefer `rebase` over `merge` for feature branches. This keeps the history linear and makes it easier to track when a specific infrastructure change was introduced.

    - `git rebase main` moves your changes to the "tip" of the current main branch.
    - It avoids "Merge branch 'main' into feature" noise in your logs.

=== ":material-comment-check: Atomic Infrastructure Commits"

    Each commit should represent a single, logical change to the infrastructure.

    - **Bad**: `git commit -m "updated stuff"` (Changes firewall, DNS, and IAM in one go).
    - **Good**: `git commit -m "feat: add egress rule for database"` (Focused and easy to revert).

## Core Workflow Patterns

Three habits separate a workflow that scales to a team from one that only works solo:

<div class="grid cards" markdown>

-   :material-source-pull: **Pull Request (PR) Culture**

    ---

    **Why it matters:** PRs aren't just for code review; they are for "Plan" review. Attach your `terraform plan` or `kustomize build` output so reviewers see the actual diff to production, not just the YAML diff.

    ```bash title="Include the Plan Output in the PR Body" linenums="1"
    terraform plan -no-color > plan.txt
    gh pr create --title "feat: add egress rule" --body-file plan.txt
    ```

    (More on `gh` itself in the next article.)

-   :material-tag-check: **Protected Branches**

    ---

    **Why it matters:** Ensure that no one — including you, on a bad day — can push directly to `main`. Require at least one approval and passing CI checks before a merge is even possible.

    ```bash title="Check Whether main Is Actually Protected" linenums="1"
    gh api repos/:owner/:repo/branches/main/protection
    ```

    An empty or `404` response means it isn't — worth knowing before you assume it is.

-   :material-undo: **Revert Strategy**

    ---

    **Why it matters:** If an infrastructure change causes an incident, the fastest way back is often `git revert`, not a hand-rolled fix. Practice the command before you need it under pressure.

    ```bash title="Revert a Bad Commit Without Rewriting History" linenums="1"
    git revert a1b2c3d
    git push origin main
    ```

    ![Running git revert on a bad commit, showing the new commit it creates that undoes the change](../images/terminal/git_revert.gif)

    `revert` creates a new commit that undoes the change — the bad commit stays in history, which is what you want during an incident review.

</div>

## Practice Problems

??? question "Practice Problem 1: Rebase vs Merge"

    You are on a feature branch and `main` has moved forward. You want to incorporate those changes while keeping your own commits at the top of the history. Which command do you use?

    ??? tip "Answer"

        ```bash title="Rebase Your Branch onto Updated main" linenums="1"
        git fetch origin
        git rebase origin/main
        ```
        `rebase` reapplies your commits on top of the new base, resulting in a cleaner, linear history.

??? question "Practice Problem 2: Validating After Conflict"

    You just resolved a conflict in a Kubernetes `Deployment.yaml`. What is the most important thing to do before committing the resolution?

    ??? tip "Answer"

        Validate the syntax! Use a tool like `yq`, `yamllint`, or `kubectl diff`. A single indentation error during a manual merge resolution can prevent the file from being parsed by your CD pipeline.

## Key Takeaways

| Action | Command / Strategy |
|:-------|:-------------------|
| **Syncing** | `git pull --rebase origin main` |
| **Branching**| `git checkout -b <type>/<description>` |
| **Updating** | `git rebase main` |
| **Cleaning** | `git commit --amend` (for fixups) |
| **Validating**| `yq eval '.' <file>` |

## What's Next

If you're following the [Debugging With Nothing But a Terminal](https://bradpenney.io/pathways/nothing-but-a-terminal) pathway, the next step is **[GitHub CLI](github_cli.md)** — opening the PR this workflow builds toward doesn't have to mean leaving the terminal.

## Further Reading

### Official Documentation
- [Git Branching - Workflows](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows) - From the Pro Git book.
- [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow) - A simple, branch-based workflow.

### Related Tools & Alternatives
- [Trunk Based Development](https://trunkbaseddevelopment.com/) - An alternative to long-lived feature branches.
- [GitLab Flow](https://docs.gitlab.com/ee/topics/gitlab_flow.html) - Workflow incorporating environment branches (Staging, Production).

### Deep Dives
- [How Parsers Work](https://cs.bradpenney.io/efficiency/how_parsers_work/) - Why a YAML file either parses or it doesn't, and why validating after a manual conflict resolution isn't optional.
