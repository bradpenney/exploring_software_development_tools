# Exploring Software Development Tools

A comprehensive guide to essential command-line tools and development environments for software engineers. Part of the [BradPenney.io](https://bradpenney.io) learning ecosystem.

**Live Site:** [tools.bradpenney.io](https://tools.bradpenney.io)

## Overview

This Material for MkDocs site covers developer tools including:

- **Version Control**: Git workflows, configuration, and best practices
- **Text Editors**: NeoVim setup, modal editing, and customization
- **Data Processing**: `jq`, `yq`, and command-line data transformation
- **Build Automation**: Makefiles and task automation
- **Command-Line Utilities**: `gh`, shell scripting, and productivity tools

## Project Structure

```
exploring_software_development_tools/
├── docs/                    # Markdown content
│   ├── git/                # Git tutorials
│   ├── nvim/               # NeoVim guides
│   ├── automation/         # Build tools and automation
│   ├── images/             # Site images and screenshots
│   └── stylesheets/        # Custom CSS
├── mkdocs.yaml             # Site configuration
├── pyproject.toml          # Poetry dependencies
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

Articles follow these standards:

- Professional tone balanced with accessibility
- Strong openings grounded in real-world problems
- Code examples with titles, line numbers, and annotations
- Practice problems with expandable solutions
- Key takeaways tables
- Further reading sections
- Cross-links to [cs.bradpenney.io](https://cs.bradpenney.io) for CS fundamentals

See `CLAUDE.md` for detailed content guidelines.

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

- [cs.bradpenney.io](https://cs.bradpenney.io) - Computer Science fundamentals
- [python.bradpenney.io](https://python.bradpenney.io) - Python programming
- [linux.bradpenney.io](https://linux.bradpenney.io) - Linux administration
- [bradpenney.io](https://bradpenney.io) - Main hub

---

Built with Material for MkDocs | Deployed on GitHub Pages
