# Automation Patterns: Makefiles and Beyond

You're tired of typing the same five commands every time you deploy a service. You have a README with a "Getting Started" section that is already out of date. You're copy-pasting complex `docker run` commands from a Slack message. **This is why we automate.**

In platform engineering, automation isn't just about saving time; it's about **reducing variance**. When a task is automated, it's performed the same way every time, regardless of who is running it or how much sleep they had.

## The Makefile: The Universal "Run Button"

`make` is a build automation tool from the 1970s that remains one of the most powerful tools in an SRE's toolkit. It provides a simple, standard interface for running complex tasks.

```makefile title="Example Makefile" linenums="1"
.PHONY: test build deploy  # (1)!

test:
	pytest tests/  # (2)!

build:
	docker build -t my-service:latest .

deploy: build
	kubectl apply -f k8s/
```

1.  Tells `make` that these are not actual files on disk.
2.  Commands must be indented with a **Tab**, not spaces.

## Why Makefiles?

<div class="grid cards" markdown>

-   :material-console-line: **Standard Interface**

    ---

    **Why it matters:** New team members don't need to know *how* to test or build. They just run `make test` or `make build`.

-   :material-sync: **Dependency Management**

    ---

    **Why it matters:** `make` knows that you shouldn't `deploy` until you have successfully `built`.

-   :material-language-python: **Language Agnostic**

    ---

    **Why it matters:** One Makefile can handle Python, Go, Terraform, and Shell scripts in a single project.

</div>

## Advanced Automation Patterns

### Pre-commit Hooks

Don't wait for CI to catch a linting error. Use `pre-commit` to run checks locally every time you run `git commit`.

```yaml title=".pre-commit-config.yaml" linenums="1"
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
    -   id: trailing-whitespace
    -   id: end-of-file-fixer
-   repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.0.272
    hooks:
    -   id: ruff
```

### Task Runners (Taskfile)

While `make` is great, some teams prefer `Task` (written in Go), which uses YAML and handles variables more gracefully.

```yaml title="Taskfile.yml" linenums="1"
version: '3'

tasks:
  build:
    cmds:
      - go build -v main.go
  
  test:
    cmds:
      - go test ./...
```

## Why This Matters for Platform Work

For SREs, automation patterns are about **Codifying Knowledge**. A Makefile is a living document of how a project is built, tested, and deployed. It moves documentation from a static Wiki into an executable file.

### Common Scenarios

=== ":material-rocket-launch: One-Click Environment Setup"

    Create a `make setup` target that installs dependencies, configures local environment variables, and spins up a local database via Docker Compose.

=== ":material-shield-check: Security Linting"

    Add `tfsec` or `checkov` to your Makefile to ensure every Terraform change is audited for security vulnerabilities before it ever reaches a PR.

=== ":material-auto-fix: Automated Formatting"

    Don't argue about indentation in code reviews. Add `black`, `ruff`, or `terraform fmt` to a `make format` target and make it a prerequisite for your build.

## Practice Problems

??? question "Practice Problem 1: Make Prerequisites"

    In the Makefile example above, what happens when you run `make deploy`?

    ??? tip "Answer"

        `make` sees that `deploy` depends on `build`. It will run the `build` target first. If the `docker build` command fails (returns a non-zero exit code), `make` will stop immediately and will **not** attempt to run the `kubectl apply` command.

??? question "Practice Problem 2: PHONY Targets"

    Why do we need the `.PHONY` declaration at the top of a Makefile?

    ??? tip "Answer"

        By default, `make` thinks its targets are files. If you have a target named `test` and a *folder* named `test` in your directory, `make` will see the folder and think the "file" is already up to date, so it won't run your commands. `.PHONY` tells `make` to ignore the filesystem and always run the commands for that target.

## Key Takeaways

| Pattern | Tool | Best Used For |
|:--------|:-----|:--------------|
| **Standard Interface** | `make` | Unifying commands across different languages |
| **Local Quality** | `pre-commit` | Catching errors before they are committed |
| **Modern Task Running**| `go-task` | Complex workflows needing YAML/Variables |
| **CI Consistency** | GitHub Actions | Ensuring the same automation runs in the cloud |

## Further Reading

### Official Documentation
- [GNU Make Manual](https://www.gnu.org/software/make/manual/make.html) - The definitive (and very dry) guide to `make`.
- [Pre-commit.com](https://pre-commit.com/) - Documentation for the pre-commit framework.

### Related Tools & Alternatives
- [Just](https://github.com/casey/just) - A modern command runner with a cleaner syntax than `make`.
- [Mage](https://magefile.org/) - A make-like build tool using Go.

### Deep Dives
- [Idempotency in Automation](https://cs.bradpenney.io/building_blocks/computational_thinking/) - Why running the same automation twice should be safe.
- [DAGs in Build Systems](https://cs.bradpenney.io/building_blocks/binary_trees_and_representation/) - How `make` calculates the order of operations.
