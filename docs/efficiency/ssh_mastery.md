# SSH Mastery: Beyond Simple Connections

You're juggling 50 different servers across three environments. You're tired of typing `ssh -i ~/.ssh/prod-key.pem ec2-user@10.0.4.122`. You need to access a private database that's only reachable from a jump host. **This is why you need to master SSH.**

SSH (Secure Shell) is the primary interface for remote platform work. While basic usage is simple, mastering the SSH configuration and tunneling capabilities will save you hours of typing and enable complex remote workflows.

## The Secret Weapon: `~/.ssh/config`

Stop typing long commands. The SSH config file allows you to define aliases and settings for your frequently used hosts.

```ssh title="Example SSH Config" linenums="1"
# Global settings
Host *
    ServerAliveInterval 60
    AddKeysToAgent yes

# A specific server
Host prod-db
    HostName 10.0.4.122
    User ec2-user
    IdentityFile ~/.ssh/prod-key.pem

# Connecting through a Bastion
Host internal-svc
    HostName 192.168.1.50
    ProxyJump bastion-host  # (1)!
```

1.  Automatically tunnels your connection through another host.

## Quick Start: SSH Key Management

1.  **Generate a modern key**: `ssh-keygen -t ed25519 -C "your_email@example.com"`
2.  **Use the Agent**: `ssh-add ~/.ssh/id_ed25519` (Stops you from typing your passphrase every time).
3.  **Copy to Server**: `ssh-copy-id user@host` (No more password prompts).

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
    ```ssh title="Speed up SSH"
    Host *
        ControlMaster auto
        ControlPath ~/.ssh/sockets/%r@%h:%p
        ControlPersist 10m
    ```
    The first connection stays open in the background. Subsequent connections happen instantly.

## Essential SSH Shortcuts

<div class="grid cards" markdown>

-   :material-keyboard-esc: **The Escape Sequence (`~.`)**

    ---

    **Why it matters:** If a remote server freezes, you can't type `exit`. Press `Enter`, then `~`, then `.` to kill the connection immediately.

-   :material-file-sync: **SCP and SFTP**

    ---

    **Why it matters:** Fast file transfers using the same credentials as your SSH session.

    ``` bash title="Copy File"
    scp local-script.sh user@host:/tmp/
    ```

-   :material-monitor-share: **Remote Port Forwarding (`-R`)**

    ---

    **Why it matters:** Let someone else access a service running on *your* laptop. Use with caution!

</div>

## Practice Problems

??? question "Practice Problem 1: ProxyJump vs. SSH Tunnels"

    You need to reach a server in a private subnet. You can SSH into a Bastion host in the public subnet. What is the modern, cleanest way to configure this in your `~/.ssh/config`?

    ??? tip "Answer"

        Use `ProxyJump`. 
        ```ssh
        Host private-server
            HostName 10.0.1.5
            ProxyJump bastion-host
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
| **Jump Host** | `-J` or `ProxyJump`| Connect through a bastion |
| **Agent Forwarding**| `-A` | Use local keys on remote host |
| **Config File** | `~/.ssh/config` | Alias and simplify connections |
| **Kill Session** | `Enter ~ .` | Emergency disconnect |

## Further Reading

### Official Documentation
- [OpenSSH Official Site](https://www.openssh.com/) - The home of the project.
- `man ssh_config` - Detailed documentation of every possible config option.

### Related Tools & Alternatives
- [Mosh](https://mosh.org/) - Better than SSH for roaming and intermittent connections.
- [Ansible](https://www.ansible.com/) - Uses SSH for automated configuration management.

### Deep Dives
- [Public Key Infrastructure](https://cs.bradpenney.io/building_blocks/computational_thinking/) - The theory behind how SSH keys keep you secure.
- [Network Layer Abstractions](https://cs.bradpenney.io/building_blocks/how_parsers_work/) - Understanding how port forwarding works at the transport layer.
