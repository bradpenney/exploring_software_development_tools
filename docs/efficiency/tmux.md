---
date: "2026-08-05 10:30"
title: "Terminal Multiplexing with tmux"
description: "tmux keeps a session alive on the server even when your connection breaks — sessions, windows, and panes for incident war rooms, connection resilience, and shared troubleshooting."
---

# Terminal Multiplexing with tmux

<!-- PATHWAY_ROADMAP:START -->
<div class="pathway-pills" markdown>
:material-map-marker-path: <span class="pathway-pills__label">Part of a pathway:</span> [Debugging With Nothing But a Terminal](https://bradpenney.io/pathways/nothing-but-a-terminal){: .pathway-pill }
</div>

??? abstract ":material-map-legend: Consult the map"

    <div class="grid cards" markdown>

    -   :material-console: __Debugging With Nothing But a Terminal__ — step 13 of 20

        ---

        ← [Vim Survival Mode](https://tools.bradpenney.io/essentials/vim_survival_mode/) · **you are here** · [FZF Mastery](https://tools.bradpenney.io/efficiency/fzf_advanced/) →

        [Start the pathway →](https://bradpenney.io/pathways/nothing-but-a-terminal)

    </div>
<!-- PATHWAY_ROADMAP:END -->

Your SSH connection just dropped during a long-running database migration. Without a multiplexer, your session is gone, and the process might be in an unknown state. With `tmux`, you just reconnect and pick up exactly where you left off. **This is why `tmux` is a non-negotiable tool for SREs.**

`tmux` (Terminal Multiplexer) lets you manage multiple terminal sessions from a single window. More importantly, it keeps those sessions alive on the server even if your connection breaks.

## Installation

=== ":material-linux: Linux"

    ```bash title="Install tmux on Linux" linenums="1"
    # Debian/Ubuntu
    sudo apt-get update && sudo apt-get install tmux

    # RHEL/CentOS/Fedora
    sudo dnf install tmux
    ```

=== ":material-apple: macOS"

    ```bash title="Install tmux on macOS" linenums="1"
    brew install tmux
    ```

=== ":material-microsoft-windows: Windows"

    `tmux` is a Unix tool with no native Windows build. Use it inside WSL:

    ```bash title="Install tmux inside WSL" linenums="1"
    sudo apt-get update && sudo apt-get install tmux
    ```

## Quick Start: The 3-Command Survival Guide

If you're new to `tmux`, these three commands provide 90% of the value:

1.  **Start a session**: `tmux new -s my-work`
2.  **Detach (Leave it running)**: Press `Ctrl+b` then `d`
3.  **Attach (Get back to it)**: `tmux attach -t my-work`

## How Tmux Works

`tmux` sits between your terminal emulator and the shell. It manages a **Server** that hosts multiple **Sessions**. Each session can have multiple **Windows**, and each window can be split into multiple **Panes**.

```mermaid
graph TD
    Server[Tmux Server] --> SessionA[Session: Incident-123]
    Server --> SessionB[Session: Maintenance]

    SessionA --> Win1[Window 1: Logs]
    SessionA --> Win2[Window 2: Editor]

    Win1 --> Pane1[Pane: tail -f access.log]
    Win1 --> Pane2[Pane: htop]

    style Server fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style SessionA fill:#d69e2e,stroke:#cbd5e0,stroke-width:2px,color:#000
    style SessionB fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style Win1 fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style Win2 fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style Pane1 fill:#2f855a,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style Pane2 fill:#2f855a,stroke:#cbd5e0,stroke-width:2px,color:#fff
```

## The Prefix Key

Most `tmux` commands are triggered by a **Prefix**. By default, this is `Ctrl+b`. You press the prefix, release it, and then press the command key.

<div class="grid cards" markdown>

-   :material-window-maximize: **Window Management**

    ---

    **Why it matters:** Keep different tasks (logging, editing, monitoring) in separate "tabs" within one SSH session.

    - `Prefix` + `c`: Create a new window
    - `Prefix` + `n` / `p`: Next/Previous window
    - `Prefix` + `0-9`: Jump to specific window

-   :material-columns: **Pane Management**

    ---

    **Why it matters:** Watch logs and run commands side-by-side in the same view.

    - `Prefix` + `"`: Split horizontally
    - `Prefix` + `%`: Split vertically
    - `Prefix` + `Arrow Keys`: Move between panes
    - `Prefix` + `z`: Zoom (maximize) the current pane

-   :material-content-save: **Session Management**

    ---

    **Why it matters:** Persist your work across reboots or connection drops.

    - `tmux ls`: List running sessions
    - `tmux attach`: Reconnect to the last session
    - `Prefix` + `d`: Detach (disconnect without killing)

</div>

## Why Tmux Matters for Platform Work

For an SRE, `tmux` is about **resilience and multitasking**.

### Common Scenarios

=== ":material-wifi-off: Connection Resilience"

    When performing risky operations (like upgrading a kernel or migrating a database), always run them inside a `tmux` session. If your VPN drops or your Wi-Fi flutters, the operation continues safely on the server — reconnect and you're looking at the same terminal, mid-command.

    ```bash title="Protect a Risky Operation from a Dropped Connection" linenums="1"
    tmux new -s migration
    # run the risky command here
    # connection drops? reconnect from any terminal:
    tmux attach -t migration
    ```

=== ":material-monitor-dashboard: Incident War Room"

    Split your screen into four quadrants — logs, resource usage, cluster state, and a free shell for troubleshooting:

    - **Top Left**: `tail -f` the error logs
    - **Top Right**: `htop` for system resources
    - **Bottom Left**: `kubectl get pods -w`
    - **Bottom Right**: A shell for active troubleshooting

    ```bash title="Start the Layout" linenums="1"
    tmux new -s incident-123
    tmux split-window -h
    tmux split-window -v
    ```

    That's the first two cuts; see [Multiple AI CLIs, One tmux Session](multiple_ai_clis_tmux.md) for the pane-splitting mechanics that take you the rest of the way to four. A common variant once you're past initial triage: swap one quadrant for an AI CLI to sanity-check a hypothesis while the other three keep watching the system.

=== ":material-account-group: Shared Troubleshooting"

    Two people can attach to the same `tmux` session on a server. This is a "poor man's screen sharing" that is incredibly effective for remote pair-debugging without any special software.

    ```bash title="Attach to a Shared Session" linenums="1"
    tmux attach -t shared-debug
    ```

    Both people run this against the same session name — whatever either of you types, the other sees in real time.

## Practice Problems

??? question "Practice Problem 1: The Panic Button"

    You're inside `tmux` and everything is frozen. How do you kill the entire session and get back to your normal shell?

    ??? tip "Answer"

        Type `exit` in every pane, or use the command `Prefix` + `:kill-session` followed by `Enter`. If you're completely stuck, you can run `tmux kill-server` from *outside* `tmux` in another terminal.

??? question "Practice Problem 2: Finding Your Way"

    You have 10 windows open in a session and forgot which one has your editor. What's the fastest way to see a list of all windows and pick one?

    ??? tip "Answer"

        Press `Prefix` + `w`. This opens an interactive, searchable list of all windows and panes. You can use the arrow keys to navigate and `Enter` to switch to the selected one.

## Key Takeaways

| Action | Command / Shortcut |
|:-------|:-------------------|
| **Prefix** | `Ctrl+b` (Default) |
| **New Session** | `tmux new -s <name>` |
| **Detach** | `Prefix` + `d` |
| **Attach** | `tmux attach -t <name>` |
| **Split Vertically** | `Prefix` + `%` |
| **Split Horizontally** | `Prefix` + `"` |
| **Zoom Pane** | `Prefix` + `z` |

## What's Next

If you're following the [Debugging With Nothing But a Terminal](https://bradpenney.io/pathways/nothing-but-a-terminal) pathway, the next step is **[FZF Mastery](fzf_advanced.md)** — panes and windows solve where things run, fzf solves picking what to run them on, without memorizing exact pod, branch, or file names.

## Further Reading

### Official Documentation
- [Tmux Wiki](https://github.com/tmux/tmux/wiki) - The official source for documentation and FAQs.
- `man tmux` - The comprehensive manual page.

### Related Tools & Alternatives
- [tmate](https://tmate.io/) - Instant terminal sharing based on tmux.
- [tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect) - Plugin to persist sessions across system reboots.

### Deep Dives
- [The Tao of Tmux](https://leanpub.com/the-tao-of-tmux/read) - A philosophical and technical deep dive into tmux workflows.
