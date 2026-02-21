# GitHub Actions for SREs: Beyond CI

Most developers use GitHub Actions to run tests and build Docker images. But for an SRE, Actions are **programmable infrastructure**. You can use them to automate incident response, rotate secrets, audit cloud resource usage, and manage your "ops" toil without ever running a dedicated cron server.

## Why GitHub Actions for Platform Work?

<div class="grid cards" markdown>

-   :material-server-off: **No Servers to Manage**

    ---

    **Why it matters:** Stop managing Jenkins or cron boxes. GitHub provides the compute, the logs, and the secrets management.

-   :material-security: **Identity Federation**

    ---

    **Why it matters:** Use **OIDC (OpenID Connect)** to allow Actions to talk to AWS, Azure, or GCP without long-lived access keys.

-   :material-clock-outline: **Scheduled Operations**

    ---

    **Why it matters:** Use `schedule: - cron: '...'` for recurring audits, cleanup tasks, and status checks.

</div>

## Quick Start: The SRE Automation Template

Here is a robust starting point for an Action that performs a platform task (like checking for unencrypted S3 buckets).

```yaml title=".github/workflows/s3-audit.yml" linenums="1"
name: S3 Security Audit
on:
  schedule:
    - cron: '0 0 * * *' # Run daily at midnight
  workflow_dispatch:     # Allow manual trigger

jobs:
  audit:
    runs-on: ubuntu-latest
    permissions:
      id-token: write    # Required for OIDC
      contents: read
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          role-to-assume: ${{ secrets.AWS_ROLE_ARN }}
          aws-region: us-east-1

      - name: Run Audit Script
        run: |
          ./scripts/check-s3-encryption.sh
```

## Why This Matters for Platform Work

Automating your repetitive tasks in GitHub Actions provides a **transparent audit trail**. Anyone on the team can see when a task ran, what the output was, and how the logic is defined in code.

### Common Scenarios

=== ":material-lock-check: Secret Rotation"

    Automate the rotation of API keys or IAM users.
    - Action triggers every 30 days.
    - Script generates a new key, updates the target system, and updates GitHub Secrets via the API.
    - Notify the team via Slack if it fails.

=== ":material-掃除: Environment Cleanup"

    Automatically delete "dev" environments that haven't been touched in 7 days.
    - Action runs every morning.
    - Uses `gh api` or cloud SDKs to find idle resources.
    - Sends a warning 24 hours before deletion.

=== ":material-alert-circle: Auto-Remediation"

    Trigger a workflow via a webhook from your monitoring system (like PagerDuty or Prometheus).
    - If a specific service is stuck, the Action can restart it or clear a cache.
    - This "self-healing" reduces on-call fatigue.

## Best Practices for SRE Workflows

1.  **Use OIDC**: Never store `AWS_ACCESS_KEY_ID` in secrets. Use OIDC for short-lived, identity-based access.
2.  **Pin Versions**: Use `@v3` or specific SHAs for actions. Don't use `@master`.
3.  **Concurrency Control**: Use `concurrency` groups to prevent two "deploy" or "cleanup" jobs from running at the same time and conflicting.
4.  **Least Privilege**: Give the `GITHUB_TOKEN` only the permissions it needs (e.g., `contents: read`, `pull-requests: write`).

## Practice Problems

??? question "Practice Problem 1: Manual Triggers"

    How do you make a GitHub Action available to be run manually from the GitHub UI with a custom input field?

    ??? tip "Answer"

        Use the `workflow_dispatch` trigger:
        ```yaml
        on:
          workflow_dispatch:
            inputs:
              environment:
                description: 'Target Environment'
                required: true
                default: 'staging'
        ```

??? question "Practice Problem 2: Security"

    Why is it safer to use `permissions: { id-token: write }` and OIDC instead of a long-lived API key stored in `secrets.AWS_ACCESS_KEY_ID`?

    ??? tip "Answer"

        OIDC provides **short-lived, scoped credentials**. There is no "secret" to leak or rotate. If an attacker gains access to your Action, the credentials expire in minutes. If someone steals your `AWS_ACCESS_KEY_ID`, they have permanent access until you manually rotate the key.

## Key Takeaways

| Feature | Purpose |
|:--------|:--------|
| `workflow_dispatch` | Allow manual runs via UI or API |
| `schedule` | Cron-like recurring tasks |
| `OIDC` | Secure, keyless cloud access |
| `concurrency` | Prevent overlapping job runs |
| `outputs` | Pass data between jobs |

## Further Reading

### Official Documentation
- [GitHub Actions Documentation](https://docs.github.com/en/actions) - The comprehensive guide.
- [Security Hardening for GitHub Actions](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions) - Essential reading for SREs.

### Related Tools & Alternatives
- [Action-validator](https://github.com/mpalmer/action-validator) - Tool to validate your YAML syntax.
- [Act](https://github.com/nektos/act) - Run your GitHub Actions locally in Docker for faster testing.

### Deep Dives
- [Identity and Access Management](https://cs.bradpenney.io/building_blocks/computational_thinking/) - How OIDC and identity federation work.
- [Event-Driven Systems](https://cs.bradpenney.io/building_blocks/how_parsers_work/) - Understanding the webhook and trigger model of Actions.
