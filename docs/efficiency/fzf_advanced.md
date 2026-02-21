# FZF Mastery: The Interactive Glue

You've used `fzf` for basic history searching (`Ctrl+r`). But `fzf` is more than a history tool; it is a **universal interactive filter**. It is the "glue" that allows you to turn static lists into interactive workflows. **This is how you build your own custom CLI interfaces.**

`fzf` takes any list of text, lets you filter it in real-time, and outputs your selection. This simple primitive can be combined with almost any other tool to create powerful, custom workflows.

## The Core Concept: Input → Filter → Output

```mermaid
graph LR
    List[Any List: files, pods, commits] -- "pipe |" --> FZF[fzf]
    FZF -- "selection" --> Command[Next Command]
```

## Quick Start: The One-Liners

Add these to your aliases for immediate impact:

```bash title="FZF Multipliers" linenums="1"
# Select a file and open it in your editor
alias fo='fd --type f | fzf | xargs -r ${EDITOR:-vim}'

# Select a process to kill
alias pk='ps -ef | fzf --header-line=1 | awk "{print \$2}" | xargs kill -9'

# Select a git branch to checkout
alias gc='git branch | fzf | xargs git checkout'
```

## Why FZF Matters for Platform Work

SREs spend their time navigating complex namespaces and large numbers of resources. `fzf` allows you to navigate these without having to memorize names or copy-paste from the terminal.

### Common Scenarios

=== ":material-kubernetes: Interactive Pod Logs"

    Stop typing pod names manually. Select from a live list:
    ```bash title="Interactive Logs" linenums="1"
    kl() {
      kubectl logs -f $(kubectl get pods -o name | fzf --prompt="Pod > ")
    }
    ```

=== ":material-docker: Cleaning Up Containers"

    Select multiple containers to stop and remove at once:
    ```bash title="Bulk Cleanup" linenums="1"
    dc() {
      docker ps -a --format '{{.ID}}	{{.Names}}	{{.Status}}' | 
      fzf --multi --header-line=0 | awk '{print $1}' | xargs docker rm -f
    }
    ```

=== ":material-history: Searching JSON with fzf"

    Use `fzf` to explore large JSON files interactively (requires `jq`):
    ```bash title="JSON Browser" linenums="1"
    # Select a key from a JSON object
    cat config.json | jq -r 'keys[]' | fzf | xargs -I {} jq '.{}' config.json
    ```

## Advanced FZF Features

<div class="grid cards" markdown>

-   :material-eye: **The Preview Window**

    ---

    **Why it matters:** See the contents of a file or the details of a resource *before* you select it.

    `fzf --preview 'bat --color=always {}'`

-   :material-cursor-default-click: **Multi-Select (`-m`)**

    ---

    **Why it matters:** Select multiple items using `Tab`. Perfect for bulk operations like deleting files or stopping services.

-   :material-keyboard: **Custom Bindings**

    ---

    **Why it matters:** Bind keys within `fzf` to perform actions.
    `--bind 'ctrl-e:execute(vim {})'`

</div>

## Practice Problems

??? question "Practice Problem 1: Previews"

    How would you create an `fzf` command that lists all files in the current directory and shows a preview of the first 10 lines of each file as you move through the list?

    ??? tip "Answer"

        ```bash
        fzf --preview 'head -n 10 {}'
        ```

??? question "Practice Problem 2: Extraction"

    When you pipe `ps -ef` into `fzf`, the output contains many columns. How do you extract just the PID (the second column) of the selected line?

    ??? tip "Answer"

        ```bash
        ps -ef | fzf | awk '{print $2}'
        ```
        `awk` is the perfect partner for `fzf` when you need to extract specific fields from a structured line of text.

## Key Takeaways

| Feature | Flag | Purpose |
|:--------|:-----|:--------|
| **Multi-select**| `-m` or `--multi` | Select multiple items with `Tab` |
| **Preview** | `--preview 'cmd'` | Run a command on the current item |
| **Prompt** | `--prompt 'text'` | Change the input prompt |
| **Header** | `--header 'text'` | Add a static header at the top |
| **Exact Match** | `-e` | Use exact matching instead of fuzzy |

## Further Reading

### Official Documentation
- [fzf Wiki: Examples](https://github.com/junegunn/fzf/wiki/examples) - A treasure trove of community-built functions.
- [fzf Manual](https://github.com/junegunn/fzf/blob/master/man/man1/fzf.1) - Detailed reference for all flags and bindings.

### Related Tools & Alternatives
- [Skim](https://github.com/lotabout/skim) - A similar fuzzy finder written in Rust.
- [Peco](https://github.com/peco/peco) - Another interactive filtering tool.

### Deep Dives
- [Pipes and Filters](https://cs.bradpenney.io/building_blocks/computational_thinking/) - The core Unix philosophy that makes `fzf` so powerful.
- [Human-Computer Interaction](https://cs.bradpenney.io/building_blocks/how_parsers_work/) - Why interactive filtering is faster than manual command typing.
