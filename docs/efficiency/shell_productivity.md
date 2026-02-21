# Shell Productivity: FZF, Ripgrep, and Beyond

You've been typing `kubectl get pods -n production | grep api` for the hundredth time this week. You're scrolling through your shell history using the `Up` arrow, hoping to find that one `curl` command from Tuesday. **There is a better way.**

Modern CLI tools can transform your terminal from a basic text interface into a high-performance development environment. These tools prioritize speed, fuzzy matching, and sensible defaults to help you find information in milliseconds.

## The "Big Three" Multipliers

If you install nothing else, install these three tools to immediately accelerate your workflow.

<div class="grid cards" markdown>

-   :material-magnify-expand: **fzf (Fuzzy Finder)**

    ---

    **Why it matters:** Interactive fuzzy searching for files, history, and processes. It makes "finding" feel like "browsing."

    ``` bash title="Fuzzy History Search" linenums="1"
    # Press Ctrl+r in your terminal
    # Type a few characters of a command you used yesterday
    ```

    **Key insight:** `fzf` can be piped into *anything*. `cat $(fzf)` is a game changer.

-   :material-text-search: **ripgrep (rg)**

    ---

    **Why it matters:** The fastest search tool in existence. It respects `.gitignore` and skips binary/hidden files by default.

    ``` bash title="Search for Text" linenums="1"
    rg "api-key" --type yaml
    ```

    **Key insight:** It's often 10-100x faster than `grep`. Use it to find config keys across thousands of files.

-   :material-folder-arrow-up: **z (or zoxide)**

    ---

    **Why it matters:** "Smart" directory jumping. It learns which directories you visit most often and lets you jump to them with a few keystrokes.

    ``` bash title="Jump to Directory" linenums="1"
    z infrastructure  # Jumps to /home/user/projects/company/infrastructure
    ```

    **Key insight:** Stop typing `cd ../../../path/to/thing`. Just `z thing`.

</div>

## Why Shell Productivity Matters for Platform Work

SREs spend a significant portion of their lives in the terminal. Saving 5 seconds on a command you run 50 times a day adds up to over 15 hours a year. More importantly, it reduces the cognitive load of "managing the terminal," letting you focus on the actual problem.

### Common Scenarios

=== ":material-history: Searching History"

    Instead of `history | grep command`, use `fzf`.
    - Press `Ctrl+r`
    - Start typing parts of the command in any order (e.g., `k8s api post`)
    - Use arrows to select and `Enter` to run.

=== ":material-file-find: Finding the Right Config"

    You know an environment variable is defined *somewhere* in your `docs/` or `k8s/` folder.
    - `rg "MY_ENV_VAR"`
    - Use `rg -l` to just see the filenames.
    - Pipe into `fzf` to select the file to edit: `vi $(rg -l "pattern" | fzf)`

=== ":material-cached: Better Listing (eza)"

    Replace `ls` with `eza`. It provides colors, file type icons, and an integrated `git` status for every file.
    - `alias ls='eza --icons --git'`

## Modern Tool Recap

| Tool | Replacement For | Main Benefit |
|:-----|:----------------|:-------------|
| **`fzf`** | `grep` (for lists) | Interactive, fuzzy selection |
| **`rg`** | `grep` | Incredible speed, respects `.gitignore` |
| **`eza`** | `ls` | Better visual info and Git integration |
| **`bat`** | `cat` | Syntax highlighting and line numbers |
| **`fd`** | `find` | Simpler syntax, faster defaults |
| **`zoxide`** | `cd` | Memory-based jumping |

## Practice Problems

??? question "Practice Problem 1: Combining Tools"

    How would you use `fzf` and `fd` together to find a file and open it in `vim` in a single command?

    ??? tip "Answer"

        ```bash
        vi $(fd --type f | fzf)
        ```
        `fd` generates the list of files, `fzf` lets you interactively pick one, and the `$()` subshell passes that filename to `vi`.

??? question "Practice Problem 2: Search and Edit"

    You need to find every YAML file that contains the string `deprecated-api` and open them all for editing.

    ??? tip "Answer"

        ```bash
        vi $(rg -l "deprecated-api" --type yaml)
        ```
        `rg -l` (lowercase L) outputs only the names of files containing matches.

## Further Reading

### Official Documentation
- [fzf GitHub](https://github.com/junegunn/fzf) - Installation and advanced configuration.
- [ripgrep User Guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md) - Deep dive into search patterns.
- [Modern Unix](https://github.com/ibraheemdev/modern-unix) - A curated list of modern replacements for classic Unix tools.

### Related Tools & Alternatives
- [Starship](https://starship.rs/) - A fast, customizable shell prompt that shows Git and K8s context.
- [Fish Shell](https://fishshell.com/) - A shell with many of these features built-in.

### Deep Dives
- [Regex Mastery](https://cs.bradpenney.io/building_blocks/regular_expressions/) - Mastering the search patterns used by `rg` and `grep`.
- [Fuzzy Matching Algorithms](https://cs.bradpenney.io/building_blocks/computational_thinking/) - How `fzf` calculates relevance from partial strings.
