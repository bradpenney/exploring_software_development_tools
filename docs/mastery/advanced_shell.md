# Advanced Shell: Functions, Aliases, and Logic

You're comfortable with `ls`, `cd`, and `grep`. You use `fzf` and `rg` to find things. But you still find yourself typing the same complex pipes over and over. **This is where you move from using the shell to commanding it.**

Advanced shell usage is about building your own internal toolkit. By using functions, aliases, and logic, you can turn a 5-step manual process into a single, precise command.

## Functions vs. Aliases

<div class="grid cards" markdown>

-   :material-alias: **Aliases**

    ---

    **Best for:** Simple command replacements and adding default flags.

    ``` bash title="Essential Aliases" linenums="1"
    alias k='kubectl'
    alias g='git'
    alias ls='eza --icons'
    ```

    **Key insight:** Use aliases for "shorthand."

-   :material-function: **Functions**

    ---

    **Best for:** Complex logic, accepting arguments, and multi-step pipes.

    ``` bash title="Pod Logs Function" linenums="1"
    kl() {
      kubectl logs -f $(kubectl get pods -o name | fzf)
    }
    ```

    **Key insight:** Use functions for "workflows."

</div>

## Quick Start: The "Instant Value" Functions

Add these to your `~/.zshrc` or `~/.bashrc` to immediately improve your daily life.

```bash title="Productivity Functions" linenums="1"
# Create a directory and enter it immediately
mkd() {
  mkdir -p "$1" && cd "$1"
}

# Find a file and open it in your editor
fo() {
  local file
  file=$(fd --type f | fzf)
  [ -n "$file" ] && ${EDITOR:-vim} "$file"
}

# Search shell history and put the result on the command line
fh() {
  print -z $(history | fzf +s --tac | sed 's/^[ ]*[0-9]*[ ]*//')
}
```

## Why Advanced Shell Matters for Platform Work

SREs often work with irregular data and complex APIs. Standard tools might get you 90% of the way, but that last 10% requires custom logic.

### Common Scenarios

=== ":material-kubernetes: Context-Aware Kubernetes"

    Instead of constantly typing the namespace, create a function that handles it:
    ```bash title="Scoped Kubectl" linenums="1"
    kn() {
      local ns=$1
      shift
      kubectl -n "$ns" "$@"
    }
    # Usage: kn production get pods
    ```

=== ":material-api: Parsing Complex API Responses"

    Build functions that wrap `curl` and `jq` for your most common internal APIs:
    ```bash title="Internal API Helper" linenums="1"
    get_user() {
      curl -s "https://api.internal/users/$1" | jq -r '.email'
    }
    ```

=== ":material-swap-horizontal: Bulk Operations"

    Use `xargs` and loops for operations that aren't natively supported by your tools:
    ```bash title="Cleanup Old Branches" linenums="1"
    git branch --merged | grep -v "\*" | xargs -n 1 git branch -d
    ```

## Logic and Flow Control in the Shell

The shell is a full programming language. Understanding its logic allows you to build robust automation.

```bash title="Robust Scripting Pattern" linenums="1"
#!/bin/bash
set -euo pipefail  # (1)!

TARGET=${1:-"default_value"}  # (2)!

if [[ -z "$TARGET" ]]; then
  echo "Error: Target is required"
  exit 1
fi

main() {
  echo "Processing $TARGET..."
  # ... your logic here ...
}

main "$@"
```

1.  **Strict Mode**: Exit on error, unset variables, and pipe failures.
2.  **Parameter Expansion**: Set a default value if the first argument is missing.

## Practice Problems

??? question "Practice Problem 1: Arguments in Aliases"

    Why can't you use an argument in the middle of an alias (e.g., `alias mygrep="grep $1 config.txt"`)?

    ??? tip "Answer"

        Aliases are simple text replacements. When you run `mygrep pattern`, the shell expands it to `grep $1 config.txt pattern`. It doesn't know how to "inject" the argument into the `$1` position. This is why you must use **Functions** for anything involving arguments.

??? question "Practice Problem 2: Parameter Expansion"

    In the command `${VAR:-"tmp"}`, what happens if `$VAR` is not set?

    ??? tip "Answer"

        The shell will use the string `"tmp"` as the value. This is a very common pattern in SRE scripts to provide sensible defaults for environment variables or script arguments.

## Key Takeaways

| Feature | Syntax | Best Used For |
|:--------|:-------|:--------------|
| **Alias** | `alias name='cmd'` | Shorthand for simple commands |
| **Function** | `name() { ... }` | Complex logic and arguments |
| **Default Value**| `${VAR:-default}` | Handling missing inputs |
| **Strict Mode** | `set -euo pipefail` | Writing safe, reliable scripts |
| **Silent Fail** | `command || true` | When a non-zero exit is acceptable |

## Further Reading

### Official Documentation
- [Bash Reference Manual](https://www.gnu.org/software/bash/manual/) - The definitive guide to Bash.
- [Zsh Guide](https://zsh.sourceforge.io/Guide/) - Comprehensive documentation for Zsh features.

### Related Tools & Alternatives
- [ShellCheck](https://www.shellcheck.net/) - A linter that finds bugs in your shell scripts.
- [ExplainShell](https://explainshell.com/) - Visual breakdown of complex command lines.

### Deep Dives
- [Regular Expressions in Shell](https://cs.bradpenney.io/building_blocks/regular_expressions/) - Using regex with `sed` and `awk`.
- [Process Management](https://cs.bradpenney.io/building_blocks/computational_thinking/) - Understanding signals, subshells, and job control.
