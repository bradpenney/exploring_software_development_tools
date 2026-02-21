# Exploring Software Development Tools

**Remove tool friction. Work faster. Keep platforms running.**

A practical guide to essential tools for SREs and Platform Engineers. Part of the [BradPenney.io](https://bradpenney.io) learning ecosystem.

**Live Site:** [tools.bradpenney.io](https://tools.bradpenney.io)

## Overview

It's 2am. The API is down. You SSH into the server, and suddenly you're fighting your tools instead of fixing the problem. You're parsing JSON by squinting at your terminal. Your SSH connection drops and you lose your work. You're typing the same 15 commands for the hundredth time this week.

**This is tool friction. This site exists to remove it.**

### The "Force Multiplier" Philosophy

While [exploring_linux](https://linux.bradpenney.io), [exploring_python](https://python.bradpenney.io), and [exploring_kubernetes](https://k8s.bradpenney.io) teach *what* to do, this site teaches *how* to do it efficiently. Master these tools and you'll:

- Save 10+ hours per week on repetitive tasks
- Debug incidents faster with proper tooling
- Stop losing work to dropped SSH connections
- Parse JSON/YAML without squinting
- Edit configs on remote servers without pain

### Who This Is For

**SREs and Platform Engineers** who:

- Maintain platform stability (uptime, performance, reliability)
- Respond to incidents (on-call rotations, debugging under pressure)
- Build integrations (APIs, data pipelines, automation)
- Manage infrastructure as code (Terraform, Ansible, K8s manifests)
- Work primarily in terminal environments (SSH, remote servers)

You may or may not have a traditional developer background. You might be a former sysadmin learning IaC, a developer transitioning to platform work, or a DevOps engineer building CI/CD pipelines. **This site is for you.**

## Site Structure

Content is organized by **urgency and job context**, not academic progression:

### 📦 Essentials (Core Tools You Need Today)

- **Git for Infrastructure** - Version control for IaC, configs, and scripts
- **JQ** - Parse JSON from APIs and logs during incidents
- **YQ** - Wrangle YAML configs (Kubernetes, Ansible, etc.)
- **Vim Survival Mode** - Edit files on servers when that's all you have

### ⚡ Efficiency (Make Your Day 10x Easier)

- **Terminal Multiplexing** - tmux/screen for persistent SSH sessions
- **VS Code Remote** - Edit on servers without fighting vim
- **Shell Productivity** - fzf, aliases, better prompts (save hours typing)
- **Git Workflows** - Branches, conflicts, and "oh shit" moments

### 🎯 Mastery (Optional Power Tools)

- **NeoVim Full Setup** - Terminal IDE for remote work
- **Advanced Shell** - Custom functions, complex automation
- **Automation Patterns** - Makefiles, pre-commit hooks, CI/CD
- **Local Testing** - Docker/Podman for safe experimentation

**No arbitrary levels.** Pick what you need when you need it. Essentials are must-have. Efficiency is highly recommended. Mastery is optional.

## Project Structure

```
exploring_software_development_tools/
├── docs/                    # Markdown content organized by tool
│   ├── git/                # Git version control
│   │   ├── essentials/     # Git basics for IaC
│   │   ├── efficiency/     # Workflows, branches, conflicts
│   │   └── mastery/        # Advanced Git techniques
│   ├── jq/                 # JSON parsing
│   │   ├── essentials/     # Basic filtering and extraction
│   │   └── efficiency/     # Complex queries and patterns
│   ├── yq/                 # YAML processing
│   │   ├── essentials/     # Basic YAML manipulation
│   │   └── efficiency/     # Advanced YAML transformations
│   ├── vim/                # Vim/NeoVim editors
│   │   ├── essentials/     # Vim survival mode
│   │   ├── efficiency/     # Comfortable vim usage
│   │   └── mastery/        # Full NeoVim setup
│   ├── tmux/               # Terminal multiplexing
│   │   ├── efficiency/     # Basic tmux sessions
│   │   └── mastery/        # Advanced tmux workflows
│   ├── shell/              # Shell productivity
│   │   ├── efficiency/     # fzf, aliases, prompts
│   │   └── mastery/        # Custom functions, scripting
│   ├── vscode/             # VS Code Remote
│   │   └── efficiency/     # Remote SSH setup
│   ├── automation/         # Build tools and CI/CD
│   │   └── mastery/        # Makefiles, pre-commit, GitHub Actions
│   ├── images/             # Site images and screenshots
│   └── stylesheets/        # Custom CSS
├── mkdocs.yaml             # Site configuration and navigation
├── pyproject.toml          # Poetry dependencies
├── CLAUDE.md               # Content guidelines and quality standards
└── README.md               # This file
```

## Development

### Prerequisites

- Python 3.8+
- Poetry (for dependency management)

### Setup

```bash
# Install dependencies
poetry install

# Serve locally (http://localhost:8000)
poetry run mkdocs serve

# Build static site with link validation
poetry run mkdocs build --strict
```

### Writing Content

Articles follow rigorous quality standards:

**Content Standards:**
- Urgent SRE/platform problem openings (incident scenarios, tool friction)
- Quick Start sections (productive in 5 minutes)
- Mermaid diagrams for workflows and architecture
- Card grids and tabs for visual organization
- Real-world examples from platform engineering
- Practice problems with nested solutions
- Key takeaways tables
- Further reading organized by category

**Formatting Requirements:**
- Command names in backticks (`git`, `jq`, `tmux`)
- Blank lines before all lists (critical for rendering)
- External links validated with WebFetch
- Code blocks with titles and line numbers
- Context before commands (never start with syntax)

**Quality Checklist:**
- NO REPETITION audit across published articles
- Pre-publication link audit (4-step process)
- Visual elements present (diagrams, cards, tabs)
- Serves multiple learning styles
- Cross-links to [cs.bradpenney.io](https://cs.bradpenney.io) for CS fundamentals

See `CLAUDE.md` for comprehensive content guidelines (~50 quality standards).

## Deployment

The site deploys automatically via GitHub Actions on push to `main`:

1. GitHub Actions builds the site using `mkdocs gh-deploy`
2. Static files are pushed to the `gh-pages` branch
3. GitHub Pages serves the content
4. Custom domain: `tools.bradpenney.io` (via CNAME in `docs/`)

## Tech Stack

- **Framework**: [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
- **Theme**: Material (slate color scheme, black/amber palette)
- **Features**: Code annotations, tabs, Mermaid diagrams, copy buttons
- **Plugins**: Search, htmlproofer (link validation)
- **Deployment**: GitHub Pages with custom domain

## Contributing

This is a personal learning repository. Content improvements and corrections are welcome via issues or pull requests.

## License

Content is available for personal and educational use. See individual files for specific licensing.

## Related Sites

This site is part of an integrated learning ecosystem:

- **[cs.bradpenney.io](https://cs.bradpenney.io)** - Computer Science fundamentals (data structures, algorithms, theory behind the tools)
- **[linux.bradpenney.io](https://linux.bradpenney.io)** - Linux for SREs (terminal skills enhanced by this site's tools)
- **[k8s.bradpenney.io](https://k8s.bradpenney.io)** - Kubernetes (jq/yq essential for manifest debugging)
- **[python.bradpenney.io](https://python.bradpenney.io)** - Python programming (tools for writing automation scripts)
- **[bradpenney.io](https://bradpenney.io)** - Main hub

**How they connect:**
- Linux site + Tools site = Terminal mastery
- Python site + Tools site = Automation capability
- Kubernetes site + Tools site = Platform debugging skills
- CS site provides theory behind Git (DAG), parsing (jq), and more

---

Built with Material for MkDocs | Deployed on GitHub Pages
