# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## Repository Overview

This is a Material for MkDocs site documenting essential tools for SREs and Platform Engineers—Git, JQ, YQ, terminal IDEs, and productivity tools. The site exists to **remove tool friction** so platform engineers can focus on their actual job: keeping systems running and building reliable infrastructure.

**The "Force Multiplier" Philosophy**: While exploring_linux, exploring_python, and exploring_kubernetes teach *what* to do, this site teaches *how* to do it efficiently. Master these tools and you'll work faster, make fewer mistakes, and spend less time fighting your environment.

**Strategic Alignment**: This site integrates with the other exploring_* sites:
- **Linux Link**: Terminal productivity and vim skills directly enhance Linux work
- **Python Link**: These tools (Git, linters, editors) are essential for writing Python automation
- **Kubernetes Link**: JQ/YQ and terminal skills are critical for K8s troubleshooting and manifest editing

## Target Persona

**CRITICAL: This site is for ACTIVE WORKING PROFESSIONALS.**

This site serves platform engineers and SREs who are **currently employed** and managing production systems. These are professionals who need these tools to do their job better, not hobbyists or students learning to code for the first time.

**Who they ARE:**

- Platform engineers/SREs currently employed in the industry
- Managing production systems, infrastructure, and platforms
- Various backgrounds: former sysadmin → platform engineer, developer → SRE, DevOps engineer
- Already understand operations, production environments, and infrastructure
- Need these tools to work more efficiently and professionally

**Who they are NOT:**

- College students learning to code for the first time
- Hobbyists teaching themselves programming with no industry experience
- Junior developers in application development roles (unless transitioning to platform/SRE work)
- People casually exploring "what is programming"

**Tone Implications:**

- Write colleague-to-colleague, not teacher-to-student
- Respect their professional experience with systems and operations
- Focus on work scenarios: production systems, team collaboration, incident response
- Quick practical value - they need this working TODAY for their job
- No condescension - they're professionals who happen to have a gap in tool knowledge

**Who they are:** SRE or Platform Engineer responsible for keeping platforms stable and/or building integrations into platforms

**Background:** May or may not have traditional software development background. Could be:

- Former sysadmin learning infrastructure as code
- Developer transitioning to platform/reliability work
- DevOps engineer building CI/CD integrations
- Platform engineer building internal tools and services

**Their job:**

- Maintain platform stability (uptime, performance, reliability)
- Respond to incidents (on-call rotations, debugging under pressure)
- Build integrations (APIs, data pipelines, automation)
- Manage infrastructure as code (Terraform, Ansible, K8s manifests)
- Work primarily in terminal environments (SSH, remote servers)

**Core tool friction points:**

- "I need to parse this JSON API response but `grep` isn't cutting it" (needs JQ)
- "I'm editing YAML on a server with `nano` and made an indentation mistake" (needs Vim basics)
- "I made a git mistake and I'm afraid to fix it" (needs Git confidence)
- "My SSH connection dropped and I lost all my work" (needs tmux)
- "I type the same 15 commands every deployment" (needs automation/aliases)

**The transformation:** From fighting their tools → commanding their tools. Remove tool friction so they can focus on their actual job: keeping platforms running and building reliable systems.

**Opening pattern:** Start with THEIR specific frustration → introduce the tool as the solution

Example opening:
> "It's 2am. The API is returning errors. You SSH into the server, curl the endpoint, and get back 500 lines of JSON. You squint at the terminal trying to find the error message buried somewhere in that wall of text. **This is why `jq` exists.**"

## Site Structure

**NOT organized by learning levels** - SREs/Platform Engineers need tools by urgency and job context, not academic progression.

### Structure: Essentials → Efficiency → Mastery

**The Progression for Working Professionals:**

- **📦 Essentials**: Basic skills needed to function in the platform engineer/SRE role. College students COULD read these articles, but they're not the target audience - we're writing for professionals who need these basics for their job.
- **⚡ Efficiency**: "Getting good" - professional optimization, team collaboration, workflow improvements that make you more effective at work.
- **🎯 Mastery**: Advanced professional techniques, deep understanding, power user level for those who want to truly master these tools.

**📦 Essentials** (Can't do the job without these)

- Git for Infrastructure (not app dev - IaC, configs, scripts)
- JQ: Parsing API responses and logs
- YQ: Wrangling YAML configs (K8s, Ansible, etc.)
- Vim Survival Mode (because sometimes you're on a server with only vim)

**⚡ Efficiency** (Make your day 10x easier)

- Terminal Multiplexing (tmux/screen for persistent sessions)
- VS Code Remote (edit on servers without pain)
- Shell Productivity (fzf, aliases, better prompts)
- Git Workflows (branches, conflicts, "oh shit" moments)

**🎯 Mastery** (Optional power tools for speed demons)

- NeoVim Full Setup (terminal IDE for remote work)
- Advanced Shell (zsh/fish, custom functions)
- Automation Patterns (Makefiles, pre-commit hooks)
- Custom Tooling (your own scripts and utilities)

**Why this works:**

- **No arbitrary limits** - Each tool gets a SERIES at each level, not just one article
- **Scalable by complexity** - Git might have 5 essentials articles, 10 efficiency articles, 35 mastery articles
- **Clear value prop** - "Essentials" means you need this NOW
- **Respects their time** - Mastery is optional, not required
- **Job-focused** - organized by "what helps me do my job"

**Series-Based Structure:**

Each tool can have multiple articles at each level:

- **📦 Essentials Series**: Typically 3-5 articles - fundamentals needed to function
- **⚡ Efficiency Series**: Typically 5-10 articles - professional workflows and optimization
- **🎯 Mastery Series**: Can be extensive (35+ articles) - deep dives, internals, advanced techniques

Example: Git could have:
- **Essentials**: Git Basics, Git Collaboration, Git Configuration, Git Safety
- **Efficiency**: Git Workflows, Git Branching, Git Conflict Resolution, Git Hooks, Git Aliases
- **Mastery**: Git Internals, Git Performance, Advanced Rebasing, Submodules, Worktrees, etc.

## Content Inventory by Category

This is the planned content organized as **series** at each level. Each tool can have multiple articles per level.

### 📦 Essentials (Fundamentals - 3-5 articles per tool)

**Git Essentials Series:**

1. **Git Basics** - Your first repository (install, init, add, commit, three states)
2. **Git Collaboration** - Remote repositories (GitHub/GitLab, clone, pull, push)
3. **Git Safety** - Avoiding mistakes (.gitignore, secrets, undo commands)
4. **Git Configuration** - Identity, aliases, editor setup, line endings

**JQ Essentials Series:**

1. **JQ Basics** - Parsing API responses during incidents
2. **JQ Filters** - Extracting and transforming fields
3. **JQ for Logs** - Common patterns for K8s and cloud APIs

**YQ Essentials Series:**

1. **YQ Basics** - Editing K8s manifests safely
2. **YQ Validation** - Validating YAML syntax
3. **YQ Batch Operations** - Updating multiple files

**Vim Essentials Series:**

1. **Vim Survival Mode** - Essential motions (navigation, editing, saving, quitting)
2. **Vim Configuration** - Basic .vimrc for server work
3. **Vim for Configs** - Editing YAML/JSON/configs efficiently

### ⚡ Efficiency (Professional Workflows - 5-10 articles per tool)

**Git Efficiency Series:**

1. **Git Workflows** - Feature branch workflow for Infrastructure as Code
2. **Git Branching Strategies** - When to branch, how to merge
3. **Git Pull Requests** - Writing good PR descriptions for infra changes
4. **Git Code Review** - Reviewing Terraform/Ansible/Python changes
5. **Git Conflict Resolution** - Resolving merge conflicts in YAML files
6. **Git Hooks** - Pre-commit automation (linting, security scans)
7. **Git Aliases** - Speed up common operations
8. **GitHub CLI** - Working with PRs/issues from terminal

**Terminal Multiplexing Series:**

1. **tmux Basics** - Persistent SSH sessions
2. **tmux Session Management** - Multiple servers, workspace organization
3. **tmux Configuration** - Customization for SRE workflows
4. **tmux Integration** - Shell prompts, automation

**VS Code Remote Series:**

1. **VS Code Remote SSH** - Setup for server editing
2. **VS Code Extensions for Ops** - YAML, Python, Terraform, Docker
3. **VS Code Settings** - Optimizing for infrastructure work
4. **VS Code Remote Debugging** - Debugging remote Python scripts

**Shell Productivity Series:**

1. **Modern Shell Tools** - fzf, ripgrep, z, eza overview
2. **Better Prompts** - Starship with git/k8s context
3. **Fuzzy Finding** - fzf for files, history, commands
4. **Fast Searching** - ripgrep patterns for infrastructure work
5. **Better Shells** - Zsh/Fish configuration
6. **Nerd Fonts** - Terminal icons and visual enhancement

### 🎯 Mastery (Deep Dives - Can be extensive, 10-35+ articles per tool)

**Git Mastery Series** (Extensive - could be 35+ articles):

1. **Git Internals** - Blobs, trees, commits, DAG structure
2. **Git Performance** - Large repositories, shallow clones, sparse checkout
3. **Advanced Rebasing** - Interactive rebase, fixup, squashing
4. **Git Submodules** - Managing dependencies
5. **Git Worktrees** - Multiple working directories
6. **Git Bisect** - Finding bugs in history
7. **Git Reflog** - Recovering lost commits
8. **Git Filter-Branch** - Rewriting history
9. **Git LFS** - Large file storage
10. **Custom Git Commands** - Creating your own git subcommands
11. ... (and many more advanced topics)

**NeoVim Mastery Series:**

1. **NeoVim Full Setup** - LazyVim, Kickstart, or custom config
2. **Modal Editing Philosophy** - Why modal editing exists
3. **Vim Motions Mastery** - Text objects (`ciw`, `dt"`, `yy`)
4. **LSP Integration** - Language servers for Python/YAML/Terraform
- LazyVim as a starting point
- LSP integration for Python/YAML

**Advanced Shell:**
- Custom functions for repeated workflows
- Complex aliases and abbreviations
- Shell scripting patterns for Ops
- History management and search

**Automation:**
- Makefiles as the universal "run button"
- Pre-commit hooks (black, ruff, yamllint, shellcheck)
- GitHub Actions for linting/testing
- Semantic versioning for internal tools

**Local Testing:**
- Docker/Podman Desktop for local dev
- Docker Compose for multi-service stacks
- Dev Containers for consistent environments
- Testing infrastructure changes locally

## Visual & Editorial Style

**Screenshots:**
- Heavy use of screenshots for VS Code/Terminal setup
- Before/after comparisons showing tool impact
- Annotated screenshots highlighting key UI elements
- Terminal output should be readable (good contrast, reasonable width)

**GIFs/Videos:**
- Essential for showing Vim motions (show the keystrokes + result)
- Terminal workflows benefit from animation (tmux, fzf navigation)
- Keep GIFs short (< 10 seconds) and focused on one concept
- Consider embedding asciinema recordings for terminal demos

**Tone:**
- "Let me show you a trick that will save you 100 hours this year"
- Time-saving value proposition front and center
- Acknowledge the learning curve honestly
- Urgency without stress (tools that help during incidents)

## Important Preferences

**Git Operations**: The user handles all git operations (commits, pushes, etc.) themselves. Do not commit or push changes.

**MkDocs Operations**: The user handles running `mkdocs serve` and `mkdocs build` themselves. Do not run these commands.

## SEO Strategy and Publication Process

**CRITICAL**: This site uses a draft/publish workflow to ensure only vetted content appears in search engines and the sitemap.

### SEO Configuration Overview

The site has comprehensive SEO optimization:

1. **Sitemap**: Auto-generated by MkDocs at `/sitemap.xml` when `site_url` is configured
2. **robots.txt**: Located at `docs/robots.txt`, points to sitemap
3. **Meta plugin**: Injects canonical URLs to prevent duplicate content
4. **Social cards**: Open Graph images auto-generated for social media sharing
5. **Google Analytics**: Configured with tracking ID
6. **Exclude plugin**: Prevents unpublished content from appearing in builds and sitemap

### Required Metadata for Every Article

**MANDATORY**: Every article MUST have frontmatter metadata before being published:

```yaml
---
title: Clear, Descriptive Title (50-60 chars ideal)
description: Compelling description for search results (150-160 chars ideal)
---
```

**Rules:**

- **Title**: Should be unique across the site, descriptive, include key terms
- **Description**: Summarize what the reader will learn, compelling call-to-action
- **No keywords needed**: Modern search engines don't rely on keyword meta tags
- **Check length**: Titles >60 chars and descriptions >160 chars get truncated in search results

### The Exclude Plugin Strategy

**Problem**: MkDocs by default includes ALL `.md` files in builds and sitemaps, even draft/unpublished content.

**Solution**: The `mkdocs-exclude` plugin configured in `mkdocs.yaml` excludes unpublished directories from:
- Site builds
- Sitemap generation
- Search indexing
- Navigation (even if accidentally uncommented)

**Current exclude configuration** (as of 2026-02-16):

```yaml
plugins:
  - search
  - meta
  - exclude:
      glob:
        - "level_1/*"
        - "level_2/*"
        - "level_3/*"
        - "level_4/*"
        - "level_5/*"
        - "level_6/*"
  # ... other plugins
```

**What this means:**
- Draft articles can exist in these directories without appearing in search results
- Articles can be worked on incrementally without affecting SEO
- Only vetted, published content appears in sitemap and builds

### How to Publish an Article

When an article is ready for publication (passes all quality checks), follow these steps **in order**:

#### 1. Pre-Publication Checklist

Complete the [Quality Standards Checklist](#quality-standards-checklist) below. Do NOT proceed until all items are checked.

#### 2. Remove from Exclude List

Edit `mkdocs.yaml` and remove the directory from the exclude plugin:

**Before (draft):**
```yaml
- exclude:
    glob:
      - "level_1/*"  # Article is excluded
      - "level_2/*"
```

**After (published):**
```yaml
- exclude:
    glob:
      # - "level_1/*"  # REMOVED - now published
      - "level_2/*"
```

**IMPORTANT**:
- Remove the ENTIRE line, don't just comment it
- If publishing individual files (not whole directories), use specific paths:
  ```yaml
  - "level_1/overview.md"  # Still draft
  - "level_1/pods.md"      # Still draft
  # level_1/services.md is published (not in exclude list)
  ```

#### 3. Add to Navigation

Uncomment the article in the `nav:` section of `mkdocs.yaml`:

**Before:**
```yaml
# - Level 1 - Core Primitives:
#     - Overview: level_1/overview.md
#     - Pods Deep Dive: level_1/pods.md
```

**After:**
```yaml
- Level 1 - Core Primitives:
    - Overview: level_1/overview.md
    - Pods Deep Dive: level_1/pods.md
    # - Services: level_1/services.md  # Still in draft
```

#### 4. Verify Publication

Run these commands to verify:

```bash
# Build the site
poetry run mkdocs build --strict

# Check sitemap includes the new article
grep -o '<loc>[^<]*</loc>' site/sitemap.xml | grep level_1

# Verify the article appears in search index
grep -i "pods deep dive" site/search/search_index.json
```

**Expected results:**
- Sitemap includes the new article URL
- Search index contains the article
- No build errors from htmlproofer
- All internal links work

#### 5. Update CLAUDE.md Exclude List

Update the "Current exclude configuration" section above to reflect what's NOW excluded (for future reference).

### SEO Checklist for Published Articles

Before removing an article from the exclude list, verify:

- [ ] **Frontmatter metadata present** - Title and description in YAML frontmatter
- [ ] **Title is unique** - Not duplicated across other published articles
- [ ] **Description is compelling** - 150-160 chars, summarizes value
- [ ] **All images have alt text** - Check with `grep -r '!\[' article.md`
- [ ] **All links work** - Internal links point to published articles only
- [ ] **No "coming soon" dead links** - Replace with plain text or link to overview
- [ ] **External links are valid** - Use WebFetch to verify important URLs
- [ ] **Headings are hierarchical** - One H1, logical H2-H6 structure
- [ ] **No duplicate content** - Cross-link instead of repeating other articles

### Common SEO Mistakes to Avoid

1. **Linking to unpublished articles** - Always check if target article is in exclude list before linking
2. **Forgetting to update exclude list** - When publishing, remove from exclude glob
3. **Missing metadata** - Every article needs title and description
4. **Publishing incomplete articles** - Follow full quality checklist before publishing
5. **Leaving articles in navigation but excluded** - Navigation and exclude list must align

### SEO Monitoring

After publication, verify search engine indexing:

1. **Check Google Search Console** - Verify pages are indexed
2. **Monitor sitemap errors** - Search Console reports sitemap issues
3. **Verify canonical URLs** - Use browser dev tools to check `<link rel="canonical">`
4. **Test social cards** - Share on social media to verify Open Graph images

## CRITICAL: No Repetition - Respect Reader's Time

**This is an absolute deal-breaker for content quality.**

### The Principle

Avoid duplication and repetition at all costs. Every time we repeat information, we waste the reader's time and make the content feel bloated.

### The Rules

1. **Cross-link instead of repeating** - If a concept is explained elsewhere, link to it
2. **Only repeat for significantly different perspectives** - Brief intro vs. deep dive is acceptable; same explanation twice is not
3. **Progressive depth, not repetition** - Each article builds WITHOUT re-explaining previous articles
4. **Audit before publishing** - Search for repeated concepts across published articles

### Before Explaining Any Concept, Ask:

1. Have we explained this elsewhere in this section?
2. If yes, is my perspective SIGNIFICANTLY different?
3. If no, add a cross-link: "Remember X from [Article]? Now let's see how..."
4. If yes, explicitly state the new angle: "Earlier we covered X in Git basics - now let's see how NeoVim integrates with it"

### Required: Pre-Publication Repetition Audit

Before marking any article complete, use the Explore agent to search for repeated concepts across published articles in the same section. If found, consolidate and cross-link.

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

Articles must balance **urgency with clarity** and be **immediately actionable** while remaining **technically accurate**. The goal: help SREs and Platform Engineers overcome tool friction so they can focus on keeping platforms running.

**Core Principles:**

- **Strong openings**: Ground in real-world SRE/platform problems (incident response, config debugging, repetitive tasks, SSH environments)
- **Professional yet engaging**: Use wit in parentheticals and asides, not emoji spam (limit to 1-3 per article, used strategically)
- **Technical rigor**: Include precise terminology, explain underlying concepts, provide historical context where relevant
- **Structured learning**: Build from simple to complex examples; use clear section headers
- **Thoughtful closings**: Tie tools to broader software development philosophy; avoid jokey endings
- **Direct voice**: Address reader as "you"; be confident but not arrogant; educational but not preachy
- **Practical focus**: Show real-world use cases, not just abstract examples

**Required Sections:**

1. Opening paragraph(s) - hook with real-world SRE/platform problem (incident at 2am, config debugging, SSH-only environment)
2. Clear explanation of what the tool is and how it solves their problem
3. Quick Start - get productive in 5 minutes (essential commands/config)
4. Technical details (commands, configuration, workflows)
5. "Why [Tool] Matters for Platform Work" section - practical importance for SRE/platform engineering
6. Common Scenarios with solutions (incident response, routine tasks, automation)
7. Practice Problems with full solutions (use `??? question` and `??? tip`)
8. "Key Takeaways" table - summarize essential concepts
7. "Further Reading" section with links to official docs and quality resources
8. Closing paragraph(s) - thoughtful reflection on tool's philosophy or impact

**Examples of Good Tone:**
- "It's 2am. The API is returning errors. You need to parse 500 lines of JSON. This is why `jq` exists." (urgent, real-world scenario)
- "Your SSH connection just dropped. Without tmux, you lost all your work. With tmux, you just reconnect." (clear value proposition)
- "You've been typing `kubectl get pods -n production | grep api` for the hundredth time this week. Let's fix that." (acknowledges their pain)

**Avoid:**
- Excessive emojis (🛠️✨🎮😄 scattered everywhere)
- Over-the-top phrases like "amazing!", "incredible!", "mind-blowing!"
- Condescending language or talking down to readers
- Jokey closings that undermine the technical content
- Creating files unless absolutely necessary

### Critical Formatting Rules

#### Command Names in Prose
**ALWAYS wrap command names in backticks when mentioned in regular text.**

- ✅ Correct: "Use `jq` to parse JSON responses"
- ✅ Correct: "The `git`, `tmux`, and `fzf` commands"
- ❌ Wrong: "Use jq to parse JSON responses"
- ❌ Wrong: "The git, tmux, and fzf commands"

**Exceptions:**
- Command names inside code blocks (already formatted)
- Command names in mermaid diagrams (doesn't render backticks)
- URLs or file paths

**Common commands to watch for:**
`git`, `jq`, `yq`, `vim`, `nvim`, `tmux`, `screen`, `fzf`, `ripgrep`, `docker`, `kubectl`, `make`, `ssh`, `curl`, `wget`, `cat`, `grep`, `sed`, `awk`

This applies to:
- Prose paragraphs
- Table cells
- List items
- Admonition text
- Further reading descriptions
- Exercise instructions

#### Markdown List Formatting
**ALWAYS add a blank line before bulleted or numbered lists.**

This is a recurring issue that breaks list rendering in MkDocs.

- ❌ Wrong:
  ```markdown
  Here's what you need:
  - Item 1
  - Item 2
  ```

- ✅ Correct:
  ```markdown
  Here's what you need:

  - Item 1
  - Item 2
  ```

**When to check:**
- After paragraphs that introduce lists
- After headings that introduce lists
- Inside admonitions/callouts
- Inside card grids
- Inside collapsible sections

**Pre-publication audit:** Use Grep to find potential issues:
```bash
# Find paragraphs followed immediately by lists (no blank line)
grep -B1 "^- " file.md | grep -v "^--$" | grep -v "^- "
```

#### External Link Validation
**ALWAYS test external links before publishing an article.**

URLs can break, move, or change over time. Use WebFetch to verify each external link:

**Process:**
1. Extract all external URLs from the article (https://...)
2. Test each with WebFetch to confirm it's accessible
3. For broken links:
   - Search for updated URL with WebSearch
   - Find authoritative replacement sources
   - Update link and description if needed

**Common link issues:**
- GitHub repos archived or moved
- Official documentation reorganized
- Blog posts deleted or moved
- Tool websites restructured

**Tools:**
```bash
# Extract external links from markdown
grep -o 'https://[^)]*' file.md
```

Then test each with WebFetch tool.

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

### Article Layout and Visual Structure

**CRITICAL**: Articles must not be simple command lists. They must teach skills with context and serve multiple audiences through varied layout.

#### Visual Elements Required

1. **Mermaid Diagrams** (when workflow exists)
   - Place at article top to show the big picture
   - Use for: workflows, decision trees, tool relationships, architecture
   - Keep them simple and focused on one concept
   - Use TD (top-down) vertical flow for readability
   - Example: Git workflow from modified → staged → committed

2. **Card Grids** (for categorizing related concepts)
   - Use `<div class="grid cards" markdown>` for grouping related concepts
   - Each card should explain "Why it matters" BEFORE showing commands
   - Ideal for: 3-4 related tools/workflows that are peers (not sequential)
   - Format:
     ```markdown
     <div class="grid cards" markdown>

     -   :material-icon: **Card Title**

         ---

         **Why it matters:** Context and relevance for SRE/platform work

         ``` bash title="Command" linenums="1"
         command
         ```

         **Key insight:** What to look for or understand

     </div>
     ```

3. **Content Tabs** (for comparing alternatives or platforms)
   - Use `=== "Tab Name"` for alternative approaches or tools
   - Perfect for: tool comparisons, different workflows, platform-specific instructions
   - Examples:
     - Tool tabs: "jq" vs "Python json module" vs "yq"
     - Editor tabs: "Vim" vs "NeoVim" vs "VS Code"
     - Platform tabs: "macOS" vs "Linux" vs "Windows"

#### Layout Patterns by Article Type

**Tool Introduction Articles (Git, JQ, YQ, tmux, etc.):**

Structure should be:

1. **Opening** - Urgent SRE/platform problem hook (incident scenario, tool friction pain)
2. **Quick Start** - Get productive in 5 minutes (essential commands)
3. **Mermaid Diagram** - Show the workflow or tool architecture
4. **Card Grid** - Group related concepts with "why it matters" context
5. **Scenario Tabs** - Different use cases (incident response, daily work, automation)
6. **Common Patterns** - Real-world examples from platform work
7. **Troubleshooting** - Common issues and solutions
8. **Practice Exercises** - Hands-on reinforcement
9. **Key Takeaways** - Table summarizing essential concepts
10. **Further Reading** - Organized by category
11. **What's Next** - Progression to related tools

**Workflow Articles (Git workflows, tmux sessions, shell productivity):**

Structure should be:

1. **Opening** - Why you need this workflow (pain point)
2. **Prerequisites** - What tools you need first
3. **Workflow Tabs** - Different approaches or scenarios
4. **Step-by-step with explanations** - The "how" with the "why"
5. **Real-world Example** - Complete walkthrough with actual commands
6. **Verification** - How to check it worked
7. **Troubleshooting** - Common issues (collapsible)
8. **Practice Exercises**
9. **Key Takeaways**
10. **Further Reading**

**Comparison Articles (Editor choices, shell options, automation tools):**

Structure should be:

1. **Opening** - Why this choice matters for SREs/platform engineers
2. **Comparison Table** - Quick decision matrix
3. **Tool Tabs** - Deep dive into each option
4. **Use Case Scenarios** - Which tool for which situation
5. **Migration Guide** - Moving between tools if needed
6. **Practice Exercises**
7. **Key Takeaways**
8. **Further Reading**

#### Context Before Commands

**NEVER start with command syntax.** Always provide context first:

- ❌ Bad: "Run `jq '.items[]'` to extract array elements"
- ✅ Good: "You need to extract pod names from kubectl JSON output. The `.items[]` filter in `jq` lets you..."

**In cards: "Why it matters" must come before the command**
**In scenarios: Goal and context must come before commands**
**In sections: Real-world relevance must precede technical details**

#### Serve Multiple Audiences

Every substantial article should provide:

1. **Visual learners** → Mermaid diagrams, card layouts, screenshots
2. **Hands-on learners** → Quick start sections, practice exercises, scenario tabs
3. **Reference seekers** → Quick reference tables/checklists, command summaries
4. **Deep divers** → Comprehensive explanations, links to official docs
5. **Speed demons** → TL;DR sections, quick recap tables

#### Further Reading Organization

**REQUIRED**: Organize Further Reading into categories:

```markdown
## Further Reading

### Official Documentation
- [Tool Official Docs](url) - Comprehensive reference
- [Tool GitHub](url) - Source code and issues

### Related Tools & Alternatives
- [Alternative Tool](url) - When to use instead
- [Complementary Tool](url) - Works well with this tool

### Deep Dives
- [Advanced Article](url) - What you'll learn
- [Performance Tuning](url) - Optimization strategies

### Community Resources
- [Tutorial Series](url) - Step-by-step guides
- [Blog Post](url) - Real-world experiences
```

**Include when relevant:**
- Official tool documentation
- GitHub repositories
- Tool creator's blog posts
- Community tutorials and guides
- Related cs.bradpenney.io articles

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

### Pre-Publication Link Audit

**REQUIRED** before marking an article as complete:

#### Step 1: Check for Broken Links

```bash
# Extract all internal markdown links
grep -o '\[.*\](.*\.md)' article.md
```

Verify each linked article is:
- Published (uncommented in mkdocs.yaml)
- Located at the correct path
- Accessible from the current article's location

#### Step 2: Add Cross-Links Between Published Articles

Every article should link forward and backward in its logical progression:

**For tool introduction articles:**
- Link to prerequisite tools ("Before using jq, make sure you understand JSON basics from...")
- Link to related tools ("After mastering jq, you might want to learn yq for YAML")
- Link to workflow articles that use this tool

**For workflow articles:**
- Link back to tool introduction articles
- Link to related workflows
- Link to troubleshooting guides

**Example cross-link patterns:**
- "Remember from [Git Basics](../git/core_functionality/intro_to_git.md), commits are snapshots..."
- "We covered tmux session management in [Terminal Multiplexing](../efficiency/tmux.md). Now let's integrate it with..."
- "This builds on the concepts from [JQ Basics](../essentials/jq_basics.md) but adds..."

#### Step 3: Add External Authoritative Links

**Official Documentation:**
- Link to tool's official documentation site
- Link to GitHub repository (if open source)
- Link to official tutorials or guides

**Related Resources:**
- Link to cs.bradpenney.io for CS fundamentals (DAG, parsing, regex, etc.)
- Link to complementary tools' documentation
- Link to relevant RFCs or specifications (JSON RFC, YAML spec, etc.)

**Deep Dives:**
- Link to tool creator's blog posts
- Link to detailed technical articles
- Link to performance analysis or benchmarks

#### Step 4: Verify Article Series Callouts

If the article is part of a logical series (Essentials, Efficiency, Mastery), include a callout:

```markdown
!!! tip "Part of Essentials"
    This article is part of the [Essentials](../index.md#essentials) series - the core tools every SRE needs.
```

Format:
- Use `!!! tip` admonition
- Title format: "Part of [Category Name]"
- Link to category overview or index

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

## Quality Standards Checklist

Before uncommenting an article in `mkdocs.yaml`:

**✅ Content Quality:**

- [ ] **NO REPETITION AUDIT** - Searched for repeated concepts across published articles (CRITICAL!)
- [ ] Opening hooks with real-world SRE/platform problem (incident at 2am, tool friction, SSH environment)
- [ ] Clear learning objectives stated upfront
- [ ] Quick Start section - get productive in 5 minutes with essential commands
- [ ] Tool installation instructions for major platforms (macOS, Linux, Windows)
- [ ] Real-world use cases and examples from platform engineering
- [ ] Common workflows demonstrated (incident response, daily operations, automation)
- [ ] "Why [Tool] Matters for Platform Work" section present
- [ ] Tips and best practices for SRE workflows
- [ ] Common pitfalls and troubleshooting section
- [ ] Practice exercises with nested solutions (`??? tip "Solution"` inside question)
- [ ] Key takeaways table or quick recap
- [ ] What's Next progression to related tools
- [ ] Further Reading organized into categories (Official Documentation, Related Tools, Deep Dives, Community Resources)

**✅ Technical Accuracy:**

- [ ] Commands tested and verified to work
- [ ] Output examples match what users will see
- [ ] Version information provided (which versions of tool covered)
- [ ] Installation verified for all platforms mentioned
- [ ] Cross-checked against official documentation
- [ ] Alternative tools mentioned where relevant

**✅ Tone and Style:**

- [ ] Matches urgent-yet-clear tone for SRE/platform engineers
- [ ] Time-saving value proposition is clear
- [ ] Addresses tool friction pain points
- [ ] Professional language (not overly playful or marketing-speak)
- [ ] Command names in backticks throughout (CRITICAL!)
- [ ] Emoji usage limited (1-3 max, strategic placement)

**✅ Layout and Structure:**

- [ ] Article is NOT a simple command list—teaches skills with context
- [ ] Uses visual elements appropriately (mermaid diagrams, card grids, tabs)
- [ ] Mermaid diagram at top if workflow/architecture exists
- [ ] Card grids explain "Why it matters" before commands
- [ ] Scenario tabs for different use cases when applicable
- [ ] Quick reference table/checklist for experienced users
- [ ] Serves multiple learning styles (visual, hands-on, reference, deep-dive, speed)
- [ ] Context provided BEFORE commands (never starts with syntax)
- [ ] Layout matches article type pattern (tool intro, workflow, comparison)

**✅ Formatting:**

- [ ] All code blocks have `title=` and `linenums="1"` attributes
- [ ] **CRITICAL: Blank lines before ALL lists** (recurring issue - check every list in article)
- [ ] Command names wrapped in backticks in prose (not in code blocks)
- [ ] Command examples show actual output with explanations
- [ ] Platform-specific tabs for installation/usage when needed
- [ ] Admonitions used appropriately (`??? tip`, `???+ warning`)
- [ ] Code annotations for complex commands (`# (1)!`, `# (2)!`, etc.)
- [ ] Internal links use relative paths
- [ ] **External links validated with WebFetch before publishing** (URLs break over time)

**✅ Integration and Links:**

- [ ] **Pre-publication link audit completed (4-step process)**
- [ ] **NEVER link to unpublished articles** - only link to articles uncommented in mkdocs.yaml
- [ ] Cross-links added between all published articles in logical progression
- [ ] Links to cs.bradpenney.io for CS fundamentals where relevant
- [ ] "Part of [Category]" callout with link to overview (if part of series)
- [ ] Referenced in "What's Next" from prerequisite articles
- [ ] Links to related tool articles
- [ ] Links to official documentation included
- [ ] Navigation uncommented in `mkdocs.yaml`
- [ ] No broken links (validate with `--strict` build)
