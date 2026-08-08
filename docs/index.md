---
date: "2025-06-17 22:57"
title: Exploring Software Development Tools - Remove Tool Friction
description: Professional guides for SREs and Platform Engineers to master Git, JQ, Tmux, and Neovim. Remove tool friction and focus on building reliable infrastructure.
---

# Exploring Software Development Tools

**Remove tool friction. Work faster. Keep platforms running.**

<img src="images/exploring_software_development_tools.png" alt="Exploring Software Development Tools" class="img-responsive-right" width="300">

Welcome to a practical guide for SREs and Platform Engineers who need to master the tools that make infrastructure work scale.

## The Problem

It's 2am. The API is down. You SSH into the server, and suddenly you're fighting your tools instead of fixing the problem. You're parsing JSON by squinting at your terminal. Your SSH connection drops and you lose your work. You're typing the same 15 commands for the hundredth time this week.

**This is tool friction. This site exists to remove it.**

## The Solution

While [Exploring Linux](https://linux.bradpenney.io), [Exploring Python](https://python.bradpenney.io), and [Exploring Kubernetes](https://k8s.bradpenney.io) teach *what* to do, this site teaches *how* to do it efficiently. Master these tools and you'll:

- Save 10+ hours per week on repetitive tasks
- Debug incidents faster with proper tooling
- Stop losing work to dropped SSH connections
- Parse JSON/YAML without squinting
- Edit configs on remote servers without pain

## How It's Organized

Content is structured by **urgency and job context**:

<div class="grid cards" markdown>

-   :material-package-variant: **[📦 Essentials](essentials/overview.md)**

    ---

    Core tools you need today — can't do the job without these. Git past "commit and push," inspecting real HTTP traffic, filtering JSON and YAML with [`jq`](essentials/jq_parsing_json.md) and [`yq`](essentials/yq_wrangling_yaml.md), [regex for SREs](essentials/regex_for_sres.md), [terminal diagnostics](essentials/terminal_diagnostics.md), and [Vim survival mode](essentials/vim_survival_mode.md).

-   :material-lightning-bolt: **[⚡ Efficiency](efficiency/overview.md)**

    ---

    **Make your day 10x easier.** Tools that transform how you work.

    - [SSH Mastery](efficiency/ssh_mastery.md) - Config aliases, jump hosts, tunneling
    - [Terminal Multiplexing (`tmux`)](efficiency/tmux.md) - Persistent SSH sessions
    - [FZF Mastery](efficiency/fzf_advanced.md) - Filter anything without memorizing names
    - [Multiple AI CLIs, One tmux Session](efficiency/multiple_ai_clis_tmux.md) - Cross-examining more than one model
    - [Git Workflows](efficiency/git_workflows.md) - Branches and conflict resolution
    - [GitHub CLI (`gh`)](efficiency/github_cli.md) - PRs and CI without leaving the shell

-   :material-target: **🎯 Mastery**

    ---

    **Optional power tools.** For when you need maximum efficiency.

    - NeoVim Full Setup - Terminal IDE for remote work
    - Advanced Shell - Custom functions, automation
    - Automation Patterns - Makefiles, pre-commit hooks
    - GitHub Actions - Infrastructure CI/CD

</div>

## Deep Dives

<div class="grid cards" markdown>

-   :material-map-marker-path: **[Deep Dives](https://bradpenney.io/deep-dives#dev-tools)**

    ---

    An ordered article series that stays inside this site, start to finish — Git Essentials: your first repository through remote collaboration and the feature-branch workflow production infrastructure uses.

</div>

## Who This Is For

You're an **SRE or Platform Engineer** who:

- Responds to incidents (on-call rotations, debugging under pressure)
- Manages infrastructure as code (Terraform, Ansible, K8s manifests)
- Works primarily in terminal environments (SSH, remote servers)
- Needs to parse JSON/YAML constantly (APIs, kubectl output, logs)
- Wants to automate repetitive tasks

You may or may not have a traditional developer background. **This site meets you where you are.**

## Part of the BradPenney.io Network

This site is part of a family of progressive technical learning resources:

- **[Exploring Linux](https://linux.bradpenney.io)** - Linux commands and system administration
- **[Exploring Kubernetes](https://k8s.bradpenney.io)** - Kubernetes for platform engineers
- **[Exploring Python](https://python.bradpenney.io)** - Python automation and scripting
- **[Exploring Computer Science](https://cs.bradpenney.io)** - Computer science fundamentals

**How they connect:**

- Linux site + Tools site = Terminal mastery
- Python site + Tools site = Automation capability
- Kubernetes site + Tools site = Platform debugging skills

---

**Ready to remove tool friction?** Start with [Essentials](essentials/overview.md) to professionalize your scripts and infrastructure code, or jump to [Efficiency](efficiency/overview.md) once the basics are solid. Prefer a guided route through an entire incident, start to shipped fix? Follow **[Debugging With Nothing But a Terminal](https://bradpenney.io/pathways/nothing-but-a-terminal)**.

## Subscribe by RSS

New articles publish straight to the [RSS feed](https://tools.bradpenney.io/feed_rss_created.xml) — no algorithm, no email required.

<a href="https://iheartrss.com/"><img src="https://iheartrss.com/iheartrss-dark.svg" alt="I ♥ RSS" width="88" height="31"></a>
