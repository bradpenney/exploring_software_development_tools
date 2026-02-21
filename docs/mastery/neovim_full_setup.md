# NeoVim Full Setup: The Terminal IDE

You've learned basic Vim - you can move with `h/j/k/l` and edit files over SSH. But you're tired of switching back to VS Code for "real" work. **This is why you graduate to NeoVim.**

NeoVim is a modern fork of Vim designed for extensibility. By leveraging the Lua programming language and a massive ecosystem of plugins, you can transform your terminal editor into a high-performance IDE that rivals VS Code while remaining entirely keyboard-driven.

## Why NeoVim?

<div class="grid cards" markdown>

-   :material-keyboard-variant: **The Power of Composability**

    ---

    **Why it matters:** Vim's "grammar" (`action` + `motion`) allows you to perform complex edits with minimal keystrokes. `di"` deletes everything inside quotes. `cap` changes an entire paragraph.

-   :material-code-json: **LSP (Language Server Protocol)**

    ---

    **Why it matters:** Get the same autocomplete, go-to-definition, and refactoring tools as VS Code, but inside your terminal.

-   :material-flash: **Incredible Speed**

    ---

    **Why it matters:** NeoVim starts instantly and stays responsive even in massive repositories where traditional IDEs struggle.

</div>

## Starting Point: The "Distribution" Approach

Configuring NeoVim from scratch can take weeks. For most SREs, we recommend starting with a **Distribution**—a pre-configured setup with sensible defaults.

=== ":material-rocket-launch: LazyVim"

    **Best for**: SREs who want a "it just works" experience.
    - Highly modular and fast.
    - Excellent defaults for Python, YAML, and Docker.
    - Uses `Lazy.nvim` for lightning-fast plugin management.

=== ":material-tools: Kickstart.nvim"

    **Best for**: Those who want to learn how NeoVim actually works.
    - A single-file configuration that you can read and understand.
    - Teaches you the basics of Lua and LSP setup.

=== ":material-star-face: AstroNvim"

    **Best for**: Users who want a feature-rich, "batteries-included" experience.
    - Extensive UI components and dashboard.
    - Great for those used to full-featured IDEs.

## Essential Plugin Categories for Platform Work

To replace your IDE, you need these four core capabilities:

1.  **LSP (Language Server Protocol)**: Use `mason.nvim` to install servers for Python (Pyright), YAML (yamlls), and Terraform.
2.  **Telescope**: Fuzzy find everything—files, text in files, git commits, and even LSP symbols.
3.  **Treesitter**: Advanced syntax highlighting that "understands" the code structure, making logs and configs much easier to read.
4.  **Neo-tree / nvim-tree**: A sidebar file explorer for when you need to visualize the project structure.

## Quick Start: The LazyVim Install

If you have Git and NeoVim 0.9+ installed:

```bash title="Install LazyVim" linenums="1"
# Backup your current config
mv ~/.config/nvim ~/.config/nvim.bak

# Clone the starter template
git clone https://github.com/LazyVim/starter ~/.config/nvim

# Remove the .git folder so you can start your own history
rm -rf ~/.config/nvim/.git

# Start NeoVim
nvim
```

## Why This Matters for Platform Work

As an SRE, you are often editing code on remote servers where a GUI isn't an option. A full NeoVim setup allows you to maintain 100% of your productivity in those environments. You no longer have to compromise between "terminal speed" and "IDE power."

### Common Scenarios

=== ":material-file-tree: Large Repo Navigation"

    In a massive Terraform repo, use `Telescope` to jump to a module definition in milliseconds:
    - `Space` + `s` + `s`: Search for a symbol (like a module or variable name).
    - `Space` + `f` + `f`: Find file by name.

=== ":material-git: Integrated Git (Lazygit)"

    LazyVim integrates beautifully with `lazygit`.
    - `Space` + `g` + `g`: Open a full-screen, interactive Git interface without leaving the editor.

=== ":material-check-all: Real-time Linting"

    Get immediate feedback on your Kubernetes YAML indentation or Python syntax errors as you type. No more `kubectl apply` failures because of a missing space.

## Practice Problems

??? question "Practice Problem 1: The Leader Key"

    In most NeoVim configs (especially LazyVim), what is the "Leader" key and why is it used?

    ??? tip "Answer"

        The **Leader** key (usually `Space` or ``) is a prefix used to trigger custom commands without conflicting with Vim's built-in motions. For example, `Leader` + `f` + `f` is a common shortcut for "Find Files." It's the gateway to your custom IDE features.

??? question "Practice Problem 2: LSP vs Syntax Highlighting"

    What is the difference between `Treesitter` and a `Language Server (LSP)`?

    ??? tip "Answer"

        **Treesitter** is for high-performance **syntax highlighting** and text objects. it builds a "syntax tree" of the file to understand its structure. 
        **LSP** provides **semantic intelligence**—it knows if a variable is defined, what its type is, and how to jump to its source. Treesitter makes the code look right; LSP makes it "smart."

## Key Takeaways

| Tool | Purpose |
|:-----|:--------|
| **Lua** | The configuration language for NeoVim |
| **Lazy.nvim** | Fast, modern plugin manager |
| **LSP** | Intelligence (Autocomplete, Definitions) |
| **Treesitter** | Advanced Highlighting |
| **Telescope** | Fuzzy Finding everything |

## Further Reading

### Official Documentation
- [NeoVim Documentation](https://neovim.io/doc/) - The official manual.
- [LazyVim Docs](https://www.lazyvim.org/) - Comprehensive guide to the LazyVim distribution.

### Related Tools & Alternatives
- [Mason.nvim](https://github.com/williamboman/mason.nvim) - Portable package manager for LSPs, DAP servers, linters, and formatters.
- [Tree-sitter](https://tree-sitter.github.io/tree-sitter/) - The incremental parsing system.

### Deep Dives
- [Modal Editing Philosophy](https://cs.bradpenney.io/building_blocks/computational_thinking/) - How motions and actions form a language for editing.
- [ASTs and Parsing](https://cs.bradpenney.io/building_blocks/how_parsers_work/) - How Treesitter and LSPs understand your code.
