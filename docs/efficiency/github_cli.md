---
date: "2026-08-05 11:30"
title: "GitHub CLI (gh): Workflow Automation"
description: "Manage PRs, watch CI, and hit the GitHub API without leaving the shell — gh brings the browser's GitHub workflow to the terminal, scriptable and pipeable into jq."
---

# GitHub CLI (gh): Workflow Automation

<!-- PATHWAY_ROADMAP:START -->
<div class="pathway-pills" markdown>
:material-map-marker-path: <span class="pathway-pills__label">Part of a pathway:</span> [Debugging With Nothing But a Terminal](https://bradpenney.io/pathways/nothing-but-a-terminal){: .pathway-pill }
</div>

??? abstract ":material-map-legend: Consult the map"

    <div class="grid cards" markdown>

    -   :material-console: __Debugging With Nothing But a Terminal__ — step 19 of 20

        ---

        ← [Git Workflows for Infrastructure](https://tools.bradpenney.io/efficiency/git_workflows/) · **you are here** · *GitHub Actions for SREs (coming soon)* →

        [Start the pathway →](https://bradpenney.io/pathways/nothing-but-a-terminal)

    </div>
<!-- PATHWAY_ROADMAP:END -->

A finished code change usually means opening a browser, navigating to the repo, clicking "New Pull Request," filling out the form, and adding reviewers. **There is a better way.**

The GitHub CLI (`gh`) brings the power of GitHub directly to your terminal. For SREs, this means you can manage PRs, view CI/CD status, and even interact with GitHub Actions without ever leaving your shell.

## Installation

=== ":material-linux: Linux"

    ```bash title="Install gh on Linux" linenums="1"
    # Debian/Ubuntu
    type -p curl >/dev/null || sudo apt install curl -y
    curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
    sudo apt update && sudo apt install gh -y

    # RHEL/CentOS/Fedora
    sudo dnf install gh
    ```

=== ":material-apple: macOS"

    ```bash title="Install gh on macOS" linenums="1"
    brew install gh
    ```

=== ":material-microsoft-windows: Windows"

    ```bash title="Install gh on Windows" linenums="1"
    choco install gh
    # or
    winget install --id GitHub.cli
    ```

Then authenticate once — this is the first step in Quick Start below.

## Quick Start: Get Productive in 3 Minutes

1.  **Auth**: `gh auth login`
2.  **Create a PR**: `gh pr create --title "feat: add vpc logging" --body "See Jira-123"`
3.  **Check CI**: `gh run watch` (Watch your GitHub Actions run in real-time)
4.  **Merge**: `gh pr merge --auto --squash`

Four commands, one continuous loop, no browser tab required:

```mermaid
graph LR
    Auth["gh auth login"] --> Create["gh pr create"]
    Create --> Watch["gh run watch"]
    Watch --> Merge["gh pr merge"]

    style Auth fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style Create fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style Watch fill:#d69e2e,stroke:#cbd5e0,stroke-width:2px,color:#000
    style Merge fill:#2f855a,stroke:#cbd5e0,stroke-width:2px,color:#fff
```

## Why GitHub CLI Matters for Platform Work

SREs often manage dozens of repositories simultaneously. Using a browser for every PR or Action becomes a major friction point. `gh` allows you to treat GitHub as a command-line tool that can be integrated into your scripts and aliases.

### Common Scenarios

=== ":material-rocket-launch: Tracking Deployments"

    Instead of refreshing a browser tab, watch your deployment pipeline from your terminal:
    ```bash title="Watch Workflow" linenums="1"
    gh run list --workflow deploy.yml --limit 5
    gh run watch # Follow the live logs of the latest run
    ```

=== ":material-source-pull: Mass PR Review"

    Need to review three dependency updates from Dependabot?
    ```bash title="Review PRs" linenums="1"
    gh pr list # See all open PRs
    gh pr checkout 123 # Quickly pull the code locally to test
    gh pr review --approve 123
    ```

=== ":material-api: Scripting GitHub"

    Use `gh api` to perform complex tasks that aren't available in standard commands, returning clean JSON that you can pipe to `jq`.
    ```bash title="Find Large Repos" linenums="1"
    gh api /orgs/my-org/repos | jq '.[] | select(.size > 100000) | .name'
    ```

## Essential Commands

Three command groups cover nearly everything above:

<div class="grid cards" markdown>

-   :material-source-branch: **PR Management (`gh pr`)**

    ---

    **Why it matters:** Create, list, view, and merge pull requests without a mouse.

    ```bash title="Check a PR's Status and Diff" linenums="1"
    gh pr status        # (1)!
    gh pr diff 123       # (2)!
    ```

    1. Shows PRs relevant to you: created, review-requested, or assigned.
    2. Shows the actual code diff for PR #123, right in the terminal.

-   :material-play-circle: **Actions Workflow (`gh run`)**

    ---

    **Why it matters:** Monitor and trigger GitHub Actions. Invaluable for debugging failing CI/CD pipelines.

    ```bash title="Find and Read a Failed Run's Logs" linenums="1"
    gh run list --limit 5   # (1)!
    gh run view --log       # (2)!
    ```

    1. See recent runs and their status at a glance.
    2. Stream the full log of the most recent run — no need to open the Actions tab.

-   :material-code-json: **API Access (`gh api`)**

    ---

    **Why it matters:** Full access to the GitHub REST API for anything the built-in commands don't cover — already authenticated, no separate token to manage.

    ```bash title="Check Your Own Rate Limit" linenums="1"
    gh api rate_limit
    ```

</div>

## Practice Problems

??? question "Practice Problem 1: Speeding Up Merges"

    You've opened a PR and you know the CI will take 5 minutes. You don't want to wait around to click "Merge." What command can you run to tell GitHub to merge the PR as soon as the checks pass?

    ??? tip "Answer"

        ```bash title="Auto-Merge Once Checks Pass" linenums="1"
        gh pr merge --auto --squash
        ```
        The `--auto` flag enables "auto-merge," which will complete the merge automatically once all required status checks have passed.

??? question "Practice Problem 2: Debugging Actions"

    A GitHub Action failed, and you need to see the logs for the specific failed step. How do you do this without the UI?

    ??? tip "Answer"

        ```bash title="Stream Logs from the Latest Run" linenums="1"
        gh run view --log
        ```
        This will stream the logs of the most recent run directly to your terminal. You can even pipe this to `grep` or `jq` if the logs are structured!

## Key Takeaways

| Command | Purpose |
|:--------|:--------|
| `gh pr create` | Open a new Pull Request |
| `gh run watch` | Monitor Actions in real-time |
| `gh pr checkout` | Pull a PR's branch locally |
| `gh repo clone` | Intelligent cloning (handles SSH/HTTPS) |
| `gh alias set` | Create custom GitHub commands |

## What's Next

`gh api` returning raw JSON you can pipe into `jq` is the same wire-level data [Seeing API Traffic](../essentials/web/inspecting_http_traffic.md) taught you to read with `curl -v`, this time from an authenticated CLI instead of a raw request.

This is the last free step in the [Debugging With Nothing But a Terminal](https://bradpenney.io/pathways/nothing-but-a-terminal) pathway. *Turning everything you just did by hand into something that runs itself — GitHub Actions for SREs — is Mastery tier, coming soon.*

## Further Reading

### Official Documentation
- [GitHub CLI Manual](https://cli.github.com/manual/) - Complete reference for every command.
- [GitHub CLI Extension Guide](https://github.com/cli/cli/blob/trunk/docs/extensions.md) - Learn how to build your own `gh` subcommands.

### Related Tools & Alternatives
- [Lab (for GitLab)](https://github.com/zaquestion/lab) - A similar tool for GitLab users.
- [Octokit](https://github.com/octokit) - Official GitHub SDKs for building deeper integrations.

### Deep Dives
- [Seeing API Traffic: curl -v and the Network Tab](https://tools.bradpenney.io/essentials/web/inspecting_http_traffic/) - The same request/response anatomy `gh` is automating for you underneath.
