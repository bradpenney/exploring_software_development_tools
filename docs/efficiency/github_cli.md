# GitHub CLI (gh): Workflow Automation

You've finished your code change. Now you have to open a browser, navigate to the repo, click "New Pull Request," fill out the form, and add reviewers. **There is a better way.**

The GitHub CLI (`gh`) brings the power of GitHub directly to your terminal. For SREs, this means you can manage PRs, view CI/CD status, and even interact with GitHub Actions without ever leaving your shell.

## Quick Start: Get Productive in 3 Minutes

1.  **Auth**: `gh auth login`
2.  **Create a PR**: `gh pr create --title "feat: add vpc logging" --body "See Jira-123"`
3.  **Check CI**: `gh run watch` (Watch your GitHub Actions run in real-time)
4.  **Merge**: `gh pr merge --auto --squash`

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

<div class="grid cards" markdown>

-   :material-source-branch: **PR Management (`gh pr`)**

    ---

    **Why it matters:** Create, list, view, and merge pull requests without a mouse.

    - `gh pr create`
    - `gh pr status`
    - `gh pr diff`

-   :material-play-circle: **Actions Workflow (`gh run`)**

    ---

    **Why it matters:** Monitor and trigger GitHub Actions. Invaluable for debugging failing CI/CD pipelines.

    - `gh run list`
    - `gh run view --log`
    - `gh workflow run deploy.yml`

-   :material-code-json: **API Access (`gh api`)**

    ---

    **Why it matters:** Full access to the GitHub REST API. Perfect for building custom SRE tools.

</div>

## Practice Problems

??? question "Practice Problem 1: Speeding Up Merges"

    You've opened a PR and you know the CI will take 5 minutes. You don't want to wait around to click "Merge." What command can you run to tell GitHub to merge the PR as soon as the checks pass?

    ??? tip "Answer"

        ```bash
        gh pr merge --auto --squash
        ```
        The `--auto` flag enables "auto-merge," which will complete the merge automatically once all required status checks have passed.

??? question "Practice Problem 2: Debugging Actions"

    A GitHub Action failed, and you need to see the logs for the specific failed step. How do you do this without the UI?

    ??? tip "Answer"

        ```bash
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

## Further Reading

### Official Documentation
- [GitHub CLI Manual](https://cli.github.com/manual/) - Complete reference for every command.
- [GitHub CLI Extension Guide](https://github.com/cli/cli/blob/trunk/docs/extensions.md) - Learn how to build your own `gh` subcommands.

### Related Tools & Alternatives
- [Lab (for GitLab)](https://github.com/zaquestion/lab) - A similar tool for GitLab users.
- [Octokit](https://github.com/octokit) - Official GitHub SDKs for building deeper integrations.

### Deep Dives
- [API Driven Development](https://cs.bradpenney.io/building_blocks/computational_thinking/) - How the GitHub CLI uses the same APIs you can access via `curl`.
- [Workflow Automation](https://cs.bradpenney.io/building_blocks/how_parsers_work/) - Principles of removing manual steps from the software lifecycle.
