---
date: "2026-08-05 12:00"
title: "Efficiency: Terminal Workflows for Platform Engineers"
description: "Professional terminal workflows for platform engineers — persistent sessions, fuzzy navigation, a real Git workflow for infrastructure, and managing GitHub from the shell."
---

# Efficiency: Move Faster in the Terminal

Essentials gets you through an incident without leaving the terminal. Efficiency is what those same moves look like once you've done them a hundred times — persistent sessions that survive a dropped connection, filtering instead of memorizing exact names, a real branching workflow for infrastructure changes, and GitHub without a browser tab.

## Who This Is For

Efficiency assumes you:

- Already SSH into production boxes and know your way around a shell
- Want the moves that become muscle memory with daily use, not a first introduction
- Are ready for colleague-to-colleague tone — no hand-holding, full working context assumed

## What You'll Be Able to Do

| Situation | Article |
|:----------|:--------|
| SSH itself has become a bottleneck — too many hosts, too many flags | [SSH Mastery](ssh_mastery.md) |
| Your SSH connection drops mid-task and you need the session to still be there | [Terminal Multiplexing with tmux](tmux.md) |
| You're navigating dozens of pods, branches, or files and don't want to type exact names | [FZF Mastery](fzf_advanced.md) |
| One AI CLI gave you an answer and you want a second opinion before you act on it | [Multiple AI CLIs, One tmux Session](multiple_ai_clis_tmux.md) |
| Two people are touching the same infrastructure file on different branches | [Git Workflows for Infrastructure](git_workflows.md) |
| You want PRs, CI status, and reviews without leaving the shell | [GitHub CLI](github_cli.md) |

## The Articles

1. **[SSH Mastery](ssh_mastery.md)** — `~/.ssh/config` aliases, jump hosts via `ProxyJump`, and local port forwarding
2. **[Terminal Multiplexing with tmux](tmux.md)** — sessions that outlive a dropped connection
3. **[FZF Mastery](fzf_advanced.md)** — one filtering primitive wired into file, process, and branch selection
4. **[Multiple AI CLIs, One tmux Session](multiple_ai_clis_tmux.md)** — splitting panes across more than one AI CLI to cross-check an answer
5. **[Git Workflows for Infrastructure](git_workflows.md)** — the feature-branch workflow built for IaC, and re-validating YAML after a merge conflict
6. **[GitHub CLI](github_cli.md)** — PRs, CI runs, and reviews from the shell, with `gh api` piping straight into `jq`

More Efficiency topics — shell productivity, `direnv`, and remote editing — are still on the way.

---

Start with **[SSH Mastery](ssh_mastery.md)** if you're still typing full connection strings, or jump straight to **[Terminal Multiplexing with tmux](tmux.md)** if a dropped session just cost you work.

After Efficiency, the **Mastery** tier covers turning what you just did by hand into something that never has to happen manually again — automation, Git internals, and CI-driven ops work. *(Coming soon)*
