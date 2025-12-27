# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## Repository Overview

This is a Material for MkDocs site documenting essential software development tools—Git, NeoVim, command-line utilities, and automation tools. The site serves as a practical guide for developers at all levels, from students learning workflows to professionals sharpening their toolkit.

## Important Preferences

**Git Operations**: The user handles all git operations (commits, pushes, etc.) themselves. Do not commit or push changes.

**MkDocs Operations**: The user handles running `mkdocs serve` and `mkdocs build` themselves. Do not run these commands.

## Project Structure

- `docs/` - Markdown content organized by tool category
  - `git/core_functionality/` - Git fundamentals and configuration
  - `nvim/` - NeoVim guides and philosophy
  - `automation/` - Makefiles, semantic versioning (planned)
  - `images/` - Screenshots and visual assets
  - `stylesheets/` - Custom CSS (extra.css)
- `mkdocs.yaml` - Site configuration and navigation
- `pyproject.toml` - Poetry dependencies

**Important:** Directory structure mirrors site navigation. Articles reference each other using relative paths.

## Common Commands

```bash
# Install dependencies
poetry install

# Serve locally (http://localhost:8000)
poetry run mkdocs serve

# Build static site (ALWAYS use --strict for link validation)
poetry run mkdocs build --strict

# Validate all internal links
# The htmlproofer plugin is configured to:
# - Validate all internal links (raise_error: true)
# - Skip external URL validation (validate_external_urls: false)
# - Treat warnings as errors (--strict flag)
```

**Link Validation:** The project uses `mkdocs-htmlproofer-plugin` to validate all internal links. Always build with `--strict` flag to catch broken links.

## Content Guidelines

### Tone and Style

Articles must balance **playfulness with professionalism** and be **technically accurate** while remaining **accessible**. The goal: meaningful for beginners learning development workflows, yet useful for experienced professionals reviewing fundamentals.

**Core Principles:**

- **Strong openings**: Ground in real-world developer problems (broken code, collaboration challenges, workflow bottlenecks)
- **Professional yet engaging**: Use wit in parentheticals and asides, not emoji spam (limit to 1-3 per article, used strategically)
- **Technical rigor**: Include precise terminology, explain underlying concepts, provide historical context where relevant
- **Structured learning**: Build from simple to complex examples; use clear section headers
- **Thoughtful closings**: Tie tools to broader software development philosophy; avoid jokey endings
- **Direct voice**: Address reader as "you"; be confident but not arrogant; educational but not preachy
- **Practical focus**: Show real-world use cases, not just abstract examples

**Required Sections:**

1. Opening paragraph(s) - hook with real-world problem or scenario
2. Clear explanation of what the tool is and how it works
3. Technical details (architecture, commands, configuration)
4. "Why [Tool] Matters" section - practical importance in software development
5. Practice Problems with full solutions (use `??? question` and `??? tip`)
6. "Key Takeaways" table - summarize essential concepts
7. "Further Reading" section with links to official docs and quality resources
8. Closing paragraph(s) - thoughtful reflection on tool's philosophy or impact

**Examples of Good Tone:**
- "That's not hyperbole—it's Git's actual architecture." (confident assertion)
- "(Pack a lunch.)" (playful aside in parenthetical)
- "Your keyboard becomes a language for manipulating text—once you internalize the grammar, editing happens at the speed of thought." (precise yet poetic)

**Avoid:**
- Excessive emojis (🛠️✨🎮😄 scattered everywhere)
- Over-the-top phrases like "amazing!", "incredible!", "mind-blowing!"
- Condescending language or talking down to readers
- Jokey closings that undermine the technical content
- Creating files unless absolutely necessary

### Content Structure

- Articles should be teaching-focused, not just command references
- Use mermaid diagrams for visual concepts (workflow diagrams, architecture)
- Include practice problems with expandable solutions (`??? question` / `??? tip`)
- Cross-link related articles using markdown links
- **Cross-link to cs.bradpenney.io** for computer science fundamentals
- Use admonitions for tips and callouts:
  - Prefer `??? tip` (collapsible tips) for helpful insights
  - Use `???+ warning` (expanded warnings) for important caveats like learning curves
  - **Avoid** `??? note` or `!!! note` (the "note" style doesn't render well in Material for MkDocs)

### Code Examples

**Code examples must include titles, line numbers, and annotations:**

- Format: ` ```language title="Descriptive Title" linenums="1" `
- Example: ` ```bash title="Configure Git User Identity" linenums="1" `
- The title should describe what the code demonstrates
- Material for MkDocs provides copy button automatically
- **Add code annotations** to explain key concepts:
  - Use `# (1)!`, `# (2)!`, etc. for inline annotations
  - After the code block, provide numbered explanations
  - Annotate important flags, options, or non-obvious behavior
  - Example:

    ```bash
    git config --global user.name "Jane Developer"  # (1)!
    git config --global user.email "jane@example.com"  # (2)!
    ```

    1. Your name appears in commit metadata
    2. Your email links commits to your GitHub/GitLab account

### Interactive Elements: Tabs

Use tabs for comparing tools, showing alternatives, or presenting domain-specific examples:

```markdown
=== ":material-icon-name: Tab Title"

    Content here with 4-space indentation.

    **All content** must be indented to stay within the tab.

    ```bash title="Code Example" linenums="1"
    # Code must also be indented
    command --flag
    ```

=== ":material-icon-name: Another Tab"

    Second tab content, also indented.
```

**Critical tab formatting rules:**
- Tab declarations (`===`) start at column 0
- ALL content inside tabs must be indented with 4 spaces
- Code blocks need indentation: ` ```language` line AND all code content
- Blank line between tabs (but not required)
- Use Material icons for visual appeal: `:material-rocket-launch:`, `:material-timer-outline:`, etc.

**Examples from the site:**
- NeoVim distributions (LazyVim, Kickstart, AstroNvim) - comparing alternatives

### Cross-Linking Strategy

**IMPORTANT**: Always cross-link to cs.bradpenney.io for computer science fundamentals.

**When to Link to CS Site:**
- Computational thinking concepts (problem decomposition, abstraction, pattern recognition)
- Data structures (DAG → Binary Trees, graphs)
- Algorithms and theory (parsing → How Parsers Work, FSMs → Finite State Machines)
- Computer science concepts underlying tools (regex → Regular Expressions)

**Example Cross-Links Added:**
- Git's DAG → `https://cs.bradpenney.io/building_blocks/binary_trees_and_representation/`
- Modal editing composability → `https://cs.bradpenney.io/building_blocks/computational_thinking/`
- Tree-sitter parsing → `https://cs.bradpenney.io/building_blocks/how_parsers_work/`
- Regex in aliases → `https://cs.bradpenney.io/building_blocks/regular_expressions/`

**Internal Links:**
- Link related tool concepts using relative paths
- Git articles link to each other for workflows and configuration
- NeoVim articles reference Git for version control context

### Markdown Formatting

- **Markdown list formatting**: Always add a blank line before lists that follow text/bold headers
- Use tables for structured comparisons (three config levels, mode behaviors, etc.)
- Use admonitions for important notes, warnings, and tips
- Code blocks must have titles and line numbers

### Practice Problems

Every article should include 2-4 practice problems:

- Use `??? question "Practice Problem N: Descriptive Title"`
- Provide solutions in nested `??? tip "Answer"` blocks
- Problems should test understanding, not just memorization
- Build from basic recall to application to synthesis
- Include explanation in answers, not just commands or facts

Example:
```markdown
??? question "Practice Problem 1: Git States"

    If you modify a file but don't run `git add`, which state is the file in?

    ??? tip "Answer"

        The file is in the **modified** state. It exists in your working directory with changes, but those changes haven't been staged for commit yet. Running `git status` would show it under "Changes not staged for commit."
```

### Key Takeaways Format

End each article with a table summarizing core concepts:

```markdown
## Key Takeaways

| Concept | What It Means |
|:--------|:--------------|
| **DVCS** | Every repository has complete history; no single point of failure |
| **Three States** | Modified → Staged → Committed workflow enables precise control |
| **Branches** | Lightweight pointers to commits; cheap to create and merge |
```

### Tool-Specific Guidelines

**Git Articles:**
- Focus on concepts, not just command syntax
- Explain the "why" behind Git's architecture
- Provide practical workflows (feature branches, collaboration)
- Link to Pro Git book and official docs
- Show command examples with annotations

**NeoVim Articles:**
- Explain modal editing philosophy
- Show real configuration examples in Lua
- Acknowledge the learning curve honestly
- Compare distributions objectively
- Provide keyboard shortcuts and motion patterns

**Automation Articles:**
- Show real Makefiles and build scripts
- Explain automation philosophy (DRY, reproducibility)
- Practical examples from real projects
- Cross-link to version control and scripting

**Command-Line Utilities:**
- Real-world use cases first
- Command examples with explanations
- Integration with other tools (pipes, scripting)
- Performance and efficiency benefits

## Working with This Repository

- Read existing content before modifying to match established tone
- Preview changes locally with `mkdocs serve` before committing
- Keep navigation in `mkdocs.yaml` organized and logical
- Ensure all internal links work (Material for MkDocs htmlproofer will validate)
- Cross-link to cs.bradpenney.io for CS fundamentals
- Use code annotations extensively
- Follow the practice problem → key takeaways → further reading structure

## Configuration Features

The site uses Material for MkDocs with these key features:

- **Theme**: Slate scheme with black/amber palette
- **Code Features**: Annotations, copy buttons, syntax highlighting
- **Content Features**: Tabs, admonitions, Mermaid diagrams
- **Validation**: htmlproofer plugin catches broken internal links
- **Extensions**: emoji, tabbed content, superfences for code/diagrams

Always build with `--strict` to catch configuration issues and broken links.
