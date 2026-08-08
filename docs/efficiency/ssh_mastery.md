---
date: "2026-08-05 09:00"
title: "SSH Mastery: Config, Tunnels, and Jump Hosts"
description: "Beyond ssh user@host — the ~/.ssh/config aliases, port forwarding, and ProxyJump tunneling that turn SSH from a login prompt into your primary interface for remote platform work."
---

# SSH Mastery: Beyond Simple Connections

<!-- PATHWAY_ROADMAP:START -->
<div class="pathway-pills" markdown>
:material-map-marker-path: <span class="pathway-pills__label">Part of a pathway:</span> [Debugging With Nothing But a Terminal](https://bradpenney.io/pathways/nothing-but-a-terminal){: .pathway-pill }
</div>

??? abstract ":material-map-legend: Consult the map"

    <div class="grid cards" markdown>

    -   :material-console: __Debugging With Nothing But a Terminal__ — step 1 of 20

        ---

        ← *(first step)* · **you are here** · [Terminal Diagnostics](https://tools.bradpenney.io/essentials/terminal_diagnostics/) →

        [Start the pathway →](https://bradpenney.io/pathways/nothing-but-a-terminal)

    </div>
<!-- PATHWAY_ROADMAP:END -->

You're juggling 50 different servers across three environments. You're tired of typing `ssh -i ~/.ssh/prod-key.pem ec2-user@10.0.4.122`. You need to access a private database that's only reachable from a jump host. **This is why you need to master SSH.**

SSH (Secure Shell) is the primary interface for remote platform work. While basic usage is simple, mastering the SSH configuration and tunneling capabilities will save you hours of typing and enable complex remote workflows.

## Installation

Every major platform ships an SSH client, but it's not always enabled by default.

=== ":material-linux: Linux"

    ```bash title="Install OpenSSH Client" linenums="1"
    # Debian/Ubuntu
    sudo apt-get update && sudo apt-get install openssh-client

    # RHEL/CentOS/Fedora
    sudo dnf install openssh-clients
    ```

=== ":material-apple: macOS"

    OpenSSH ships with macOS out of the box — no install needed. Verify with:

    ```bash title="Check for SSH" linenums="1"
    ssh -V
    ```

=== ":material-microsoft-windows: Windows"

    Modern Windows 10/11 ships OpenSSH as an optional feature, but it's often not enabled:

    ```powershell title="Enable OpenSSH Client on Windows" linenums="1"
    Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH.Client*'
    Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
    ```

    Alternatively, use the client bundled with [Git for Windows](https://gitforwindows.org/) or WSL.

## The Basics: Direct and Jump-Host Connections

`ssh user@hostname` connects you to `hostname` as `user`, prompting for a password or falling back to a key your agent already knows about. Point at a specific key file with `-i` when it isn't:

```bash title="A Basic SSH Connection" linenums="1"
ssh ec2-user@10.0.4.122
ssh -i ~/.ssh/prod-key.pem ec2-user@10.0.4.122
```

Some servers aren't reachable directly — a private database that only accepts connections from inside the network, say. You reach it through a **jump host** (also called a bastion host): one hardened, publicly reachable machine that every connection into the private network has to pass through first. `-J` chains the two hops into a single command instead of SSHing into the jump host and then SSHing again from there by hand:

```bash title="Connecting Through a Jump Host" linenums="1"
ssh -J ec2-user@jump.example.com ec2-user@10.0.4.122
```

Everything below exists to stop you from typing either of those in full, every single time.

## Setting Up Key-Based Auth

Set this up once, and `ssh user@host` just logs you in — no password prompt, ever. It's also strictly more secure: a password is guessable and phishable, where a key pair proves you hold something a server can verify without ever seeing it. That verification is a cryptographic signature, not a shared secret; see [SSH: Signatures as Login](https://cs.bradpenney.io/efficiency/security/public_key_cryptography/#ssh-signatures-as-login) on the Computer Science site for the mechanism underneath. Setting one up is three commands:

```bash title="Set Up Key-Based Auth" linenums="1"
ssh-keygen -t ed25519 -C "your_email@example.com"  # (1)!
ssh-add ~/.ssh/id_ed25519                           # (2)!
ssh-copy-id user@host                               # (3)!
```

1.  Generates a new key pair. `ed25519` is the modern default — smaller and faster than RSA, with no meaningful security tradeoff.
2.  Loads the key into your running agent, so you unlock it once per session instead of on every connection.
3.  Copies your **public** key to the server's `~/.ssh/authorized_keys` — the one-time step that makes password-free login possible from then on. It's safe to run over an untrusted network: the public key was never a secret.

![Running ssh-keygen to generate a new ed25519 key pair, showing the save location and fingerprint](../images/terminal/ssh_keygen.gif)

That's the connection sorted — no more prompts. If you want even more control over how each host connects — a short name instead of an IP, a specific key per server, jump hosts folded in — that's what the config file is for.

## The Secret Weapon: `~/.ssh/config`

The SSH config file turns both patterns above — direct and jump-host — into named aliases with their own settings, so `ssh prod-db` or `ssh internal-svc` is all you ever type.

```bash title="Example SSH Config" linenums="1"
# Global settings
Host *
    ServerAliveInterval 60
    AddKeysToAgent yes

# A specific server
Host prod-db
    HostName 10.0.4.122
    User ec2-user
    IdentityFile ~/.ssh/prod-key.pem

# Connecting through a jump host
Host internal-svc
    HostName 192.168.1.50
    ProxyJump jump-host  # (1)!
```

1.  Automatically tunnels your connection through another host — the config-file equivalent of the `-J` flag from the last section.

That `internal-svc` entry is doing real work: your client connects to the jump host first, then rides that connection to the private host on the other side, all in one `ssh internal-svc` command.

```mermaid
graph LR
    You[Your Laptop] -->|ssh internal-svc| Jump[Jump Host]
    Jump -->|tunnel| Target[internal-svc]

    style You fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style Jump fill:#d69e2e,stroke:#cbd5e0,stroke-width:2px,color:#fff
    style Target fill:#2f855a,stroke:#cbd5e0,stroke-width:2px,color:#fff
```

"Tunnel" here means exactly what it sounds like: your second connection, to `internal-svc`, doesn't open its own path to the private subnet — it rides inside the first connection, to the jump host, which is the only one that actually needed network access to both sides. None of this requires a manual two-step login; `ProxyJump` folds the hop into the one command. (`-L`, `-R`, and `-D` forwarding work the same way for non-SSH traffic — see [SSH Tunnels Explained](https://networking.bradpenney.io/essentials/tunneling/ssh_tunnels/) on the Networking site.) `IdentityFile` in the `prod-db` entry points at the key you set up in the last section, so each alias remembers which key goes with which host.

## Why SSH Mastery Matters for Platform Work

SREs are "network navigators." You often need to bridge the gap between your local environment and a restricted remote VPC.

### Common Scenarios

=== ":material-pipe: Local Port Forwarding"

    Access a remote service (like a database or web UI) as if it were running on your own laptop.
    ```bash title="Tunnel to Remote DB" linenums="1"
    ssh -L 5432:localhost:5432 prod-db
    ```
    Now, point your local DB client to `localhost:5432`. SSH tunnels the traffic securely to the remote database.

=== ":material-forward: Agent Forwarding"

    You need to use your local Git keys on a remote server to clone a repo.

    - **Don't** copy your private keys to the server.
    - **Do** use `ssh -A user@host`.
    - Your local SSH agent "lends" its keys to the remote session temporarily.

=== ":material-connection: Persistent Connections (ControlMaster)"

    Tired of the 2-second delay every time you run a command over SSH?
    ```bash title="Speed up SSH" linenums="1"
    Host *
        ControlMaster auto
        ControlPath ~/.ssh/sockets/%r@%h:%p
        ControlPersist 10m
    ```
    The first connection stays open in the background. Subsequent connections happen instantly.

## Essential SSH Shortcuts

Everything above covers connecting. These three come up less often, but each has saved someone a genuinely bad afternoon: recovering from a session that's stopped responding, moving a file without standing up separate FTP access, or letting a colleague reach something running on your own machine.

<div class="grid cards" markdown>

-   :material-keyboard-esc: **The Escape Sequence (`~.`)**

    ---

    **Why it matters:** A frozen remote server won't respond to `exit` — the terminal isn't listening anymore. This closes the connection from your side instead, no response required from the far end.

    ```text title="Force-Close a Frozen SSH Session" linenums="1"
    <Enter>
    ~.
    ```

    Press `Enter` first, to guarantee you're at the start of a fresh line, then type `~` immediately followed by `.`. Nothing echoes to the screen — that's expected, not a sign it didn't work.

-   :material-file-sync: **SCP and SFTP**

    ---

    **Why it matters:** File transfer using the same credentials and `~/.ssh/config` aliases as your SSH session — no separate FTP setup, no new auth to manage.

    ```bash title="Copy a File, Two Ways" linenums="1"
    scp local-script.sh user@host:/tmp/
    sftp user@host
    ```

    `scp` is a one-line copy for a single file; `sftp` opens an interactive session for browsing and moving several.

-   :material-monitor-share: **Remote Port Forwarding (`-R`)**

    ---

    **Why it matters:** Let someone else reach a service running on *your* laptop, through a server they already have access to. Use with caution — you're the one exposing a port here.

    ```bash title="Expose a Local Server Through a Remote Host" linenums="1"
    ssh -R 9000:localhost:3000 shared-host
    ```

    This is one of four SSH forwarding flags — see [SSH Tunnels Explained](https://networking.bradpenney.io/essentials/tunneling/ssh_tunnels/) on the Networking site for the other three, and the one idea underneath all of them.

</div>

## Practice Problems

??? question "Practice Problem 1: ProxyJump vs. SSH Tunnels"

    You need to reach a server in a private subnet. You can SSH into a jump host in the public subnet. What is the modern, cleanest way to configure this in your `~/.ssh/config`?

    ??? tip "Answer"

        Use `ProxyJump` — it's the config-file version of the `-J` flag, so you stop typing the jump host's address every time:
        ```bash
        Host private-server
            HostName 10.0.1.5
            ProxyJump jump-host
        ```
        This is much simpler and safer than manual `-L` or `-W` tunneling.

??? question "Practice Problem 2: Security"

    Is it safe to use SSH Agent Forwarding (`-A`) when connecting to a server you don't fully trust?

    ??? tip "Answer"

        **No.** While your private key is never copied to the server, anyone with root access on that remote server can "talk" to your local agent and use your identities as long as you are connected. Only use `-A` on trusted infrastructure.

## Key Takeaways

| Feature | Flag / Setting | Purpose |
|:--------|:---------------|:--------|
| **Local Tunnel** | `-L` | Access remote service locally |
| **Jump Host** | `-J` or `ProxyJump`| Connect through an intermediary host |
| **Agent Forwarding**| `-A` | Use local keys on remote host |
| **Config File** | `~/.ssh/config` | Alias and simplify connections |
| **Kill Session** | `Enter ~ .` | Emergency disconnect |

## What's Next

If you're following the [Debugging With Nothing But a Terminal](https://bradpenney.io/pathways/nothing-but-a-terminal) pathway, the next step is **[Terminal Diagnostics](../essentials/terminal_diagnostics.md)** — once you can reach the box without thinking about it, the next question during an incident is what to check first once you're there.

## Further Reading

### Official Documentation
- [OpenSSH Official Site](https://www.openssh.com/) - The home of the project.
- `man ssh_config` - Detailed documentation of every possible config option.

### Related Tools & Alternatives
- [Mosh](https://mosh.org/) - Better than SSH for roaming and intermittent connections.
- [Ansible](https://www.ansible.com/) - Uses SSH for automated configuration management.

### Deep Dives
- [Public-Key Cryptography: The Theory Under TLS](https://cs.bradpenney.io/efficiency/security/public_key_cryptography/) - The theory behind how SSH keys keep you secure.
