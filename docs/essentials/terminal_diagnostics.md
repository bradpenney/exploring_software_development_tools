# Terminal Diagnostics: The SRE's Vital Signs

The system is slow. Users are reporting timeouts. You SSH into the server and the prompt feels sluggish. **What do you look for first?**

Terminal diagnostic tools are the "stethoscopes" of the platform engineer. They allow you to observe the health of the CPU, memory, disk, and network in real-time. Mastering these tools is the difference between guessing what's wrong and knowing exactly where the bottleneck lies.

## Quick Start: The "First 60 Seconds" Checklist

When you land on a server during an incident, run these in order:

1.  **Check load and CPU**: `htop` (or `top`)
2.  **Check memory**: `free -h`
3.  **Check disk space**: `df -h`
4.  **Check disk I/O**: `iostat -xz 1` (requires `sysstat`)
5.  **Check network connections**: `ss -tulpn`
6.  **Check kernel logs**: `dmesg -T | tail -n 50`

## Essential Diagnostic Tools

<div class="grid cards" markdown>

-   :material-speedometer: **htop / top**

    ---

    **Why it matters:** Real-time view of process activity. `htop` is preferred for its color-coded bars and ability to scroll/kill processes interactively.

    **Key insight:** Look for the "Load Average." If it's significantly higher than the number of CPU cores, your system is oversaturated.

-   :material-memory: **free -h**

    ---

    **Why it matters:** Shows total, used, and available memory in human-readable format.

    **Key insight:** Don't panic if "free" is low but "available" is high. Linux uses unused RAM for caching (buff/cache).

-   :material-harddisk: **df -h and du -sh**

    ---

    **Why it matters:** `df` shows partition usage; `du` finds which specific directory is eating your space.

    **Key insight:** A 100% full disk often causes silent failures in databases and logging agents.

</div>

## Why Diagnostics Matter for Platform Work

Diagnostics are the foundation of **Incident Response**. You cannot fix what you cannot measure. In a distributed system, being able to quickly rule out "the server is full" or "the process is OOMing" saves precious time.

### Common Scenarios

=== ":material-fire: High CPU Usage"

    Identify the process hogging the CPU:
    - Run `htop`.
    - Press `P` to sort by CPU usage.
    - If it's a Java or Python app, it might be an infinite loop or heavy GC.
    - Use `strace -p <pid>` to see what system calls the process is making in real-time.

=== ":material-lan-check: Port Conflicts"

    Why won't your new Nginx container start?
    - `ss -tulpn | grep :80`
    - This shows you exactly which process ID (PID) is already listening on port 80.
    - `ss` is the modern replacement for the older `netstat`.

=== ":material-file-alert: Ghost Files"

    `df` says the disk is full, but `du` can't find the files?
    - A process might be holding a deleted file open.
    - Use `lsof +L1` to find deleted files that are still consuming space because a process hasn't closed them.

## Key Diagnostic Commands

| Command | Purpose | Modern Alternative |
|:--------|:--------|:-------------------|
| `top` | CPU/Process monitor | `htop`, `btop` |
| `netstat`| Network connections | `ss` |
| `ifconfig`| Interface config | `ip addr` |
| `find` | Finding files | `fd` |
| `du` | Disk usage per dir | `dust` |
| `tail -f`| Follow logs | `multitail` |

## Practice Problems

??? question "Practice Problem 1: Identifying Memory Pressure"

    You run `free -h` and see that `free` is 100MB, but `available` is 4GB. Should you be worried about an Out-of-Memory (OOM) event?

    ??? tip "Answer"

        **No.** Linux is designed to use almost all available RAM for disk caching to improve performance. The `available` column is the one that matters—it tells you how much memory can be reclaimed for new processes without causing the system to swap.

??? question "Practice Problem 2: Finding a Process"

    You need to find the PID of a running process named `sidekiq` so you can kill it. What's the fastest command?

    ??? tip "Answer"

        ```bash
        pgrep -af sidekiq
        ```
        `pgrep` finds the PID, and `-af` shows the full command line so you can be sure you're targeting the right instance.

## Further Reading

### Official Documentation
- [The Brendan Gregg Blog](https://www.brendangregg.com/linuxperf.html) - The gold standard for Linux performance analysis.
- `man proc` - Understand the `/proc` filesystem where all this data comes from.

### Related Tools & Alternatives
- [btop](https://github.com/aristocratos/btop) - A high-performance, beautiful system monitor.
- [Glances](https://nicolargo.github.io/glances/) - An all-in-one cross-platform monitoring tool.

### Deep Dives
- [Operating System Scheduling](https://cs.bradpenney.io/building_blocks/computational_thinking/) - How the OS decides which process gets CPU time.
- [The Virtual Filesystem](https://cs.bradpenney.io/building_blocks/how_parsers_work/) - How Linux presents system state as files in `/proc`.
