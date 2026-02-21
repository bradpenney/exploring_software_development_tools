# Local Testing with Docker and Podman

You've written a complex shell script or a Python automation tool. You're ready to test it, but you don't want to run it on your laptop and mess up your local files, and you definitely don't want to test it in production. **This is why we use containers for local development.**

Docker and Podman allow you to create isolated, reproducible environments that mimic your production servers. For SREs, this is the ultimate "sandbox" for testing scripts, infrastructure changes, and automation before they ever touch real systems.

## Why Container Testing?

<div class="grid cards" markdown>

-   :material-shield-home: **Isolation**

    ---

    **Why it matters:** Run your "destructive" scripts safely. If a script runs `rm -rf /`, it only deletes files inside the container, not on your laptop.

-   :material-sync: **Reproducibility**

    ---

    **Why it matters:** "It works on my machine" is no longer an excuse. If it works in the container, it will work on the server running the same image.

-   :material-layers-triple: **Multi-service Testing**

    ---

    **Why it matters:** Use Docker Compose to spin up a database, a cache, and your script all at once to test their interactions.

</div>

## Quick Start: The SRE Sandbox

Need a clean Ubuntu environment to test a script?

```bash title="Start a Sandbox" linenums="1"
# Start a container and enter its shell
docker run --rm -it -v $(pwd):/work -w /work ubuntu:22.04 bash

# -it: Interactive terminal
# --rm: Delete the container when you exit
# -v: Mount your current directory into /work
# -w: Start in the /work directory
```

## Why This Matters for Platform Work

Platform engineers often write code that interacts with the OS, the network, or other services. Testing these interactions requires an environment that is "clean" yet "realistic."

### Common Scenarios

=== ":material-script-text: Testing Shell Scripts"

    Testing a script that installs packages and modifies `/etc`?
    - Use a `Dockerfile` to create a mirror of your production OS.
    - Run the script inside the container.
    - Check the exit code and file state.

=== ":material-database: Local DB Migrations"

    Testing a database schema change?
    - Use `docker-compose` to start the exact version of Postgres/MySQL you use in production.
    - Run your migration script against the local container.
    - If it fails, just `docker-compose down -v` and start over with a fresh DB.

=== ":material-api: Mocking Cloud APIs"

    Use tools like **LocalStack** inside Docker to simulate AWS services locally. Test your S3 upload script or your SQS consumer without spending a cent on cloud costs.

## Podman: The Daemonless Alternative

Many SREs prefer **Podman** over Docker.
- **Rootless**: It doesn't require `sudo` to run containers, making it more secure.
- **No Daemon**: It doesn't have a background process that can fail; it's a simple fork/exec model.
- **Compatible**: Most `docker` commands can be aliased to `podman` (e.g., `alias docker=podman`).

## Practice Problems

??? question "Practice Problem 1: Volumes and Persistence"

    You are running a script in a container that writes logs to `/var/log/myapp.log`. When the container exits, the log file is gone. How do you make the logs persist on your local laptop?

    ??? tip "Answer"

        Use a **Volume Mount** (`-v`). 
        ```bash
        docker run -v $(pwd)/logs:/var/log ubuntu ...
        ```
        This maps your local `./logs` directory to the container's `/var/log`. Anything the script writes there will be saved on your laptop.

??? question "Practice Problem 2: Networking"

    You have two containers: `app` and `db`. How do they talk to each other if they are in separate containers?

    ??? tip "Answer"

        Use a **Docker Network** or **Docker Compose**. In a Compose file, services can reach each other by their service name (e.g., the app can connect to `postgres://db:5432`).

## Key Takeaways

| Action | Command |
|:-------|:--------|
| **Run & Enter** | `docker run -it <image> bash` |
| **Mount Directory**| `-v <local>:<remote>` |
| **Clean Up** | `--rm` |
| **Check Logs** | `docker logs <container_id>` |
| **Multi-container**| `docker-compose up` |

## Further Reading

### Official Documentation
- [Docker Get Started](https://docs.docker.com/get-started/) - Official introductory guide.
- [Podman Documentation](https://podman.io/docs/) - For those moving to rootless containers.

### Related Tools & Alternatives
- [LocalStack](https://localstack.cloud/) - Fully functional local AWS cloud stack.
- [Kind (Kubernetes in Docker)](https://kind.sigs.k8s.io/) - Run a full K8s cluster inside Docker.

### Deep Dives
- [Namespace and Cgroups](https://cs.bradpenney.io/building_blocks/computational_thinking/) - The Linux kernels features that make containers possible.
- [Immutable Infrastructure](https://cs.bradpenney.io/building_blocks/how_parsers_work/) - Why containers are the foundation of modern deployment patterns.
