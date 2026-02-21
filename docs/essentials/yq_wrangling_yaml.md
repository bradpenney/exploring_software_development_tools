# YQ: Wrangling YAML Configs

Kubernetes manifests, Ansible playbooks, GitHub Actions, Docker Compose—in modern platform engineering, **YAML is everywhere.** But YAML's indentation-sensitive nature makes it notoriously difficult to edit with standard text tools like `sed` or `awk`.

`yq` is a portable command-line YAML processor. It's essentially `jq` for YAML, allowing you to slice, dice, and transform configuration files with precision and safety.

## Quick Start: Get Productive in 5 Minutes

`yq` syntax is intentionally similar to `jq`. If you know one, you're halfway to knowing the other.

```bash title="Common YQ Operations" linenums="1"
# Read a specific value from a K8s manifest
yq '.metadata.name' pod.yaml

# Update a value in-place
yq -i '.spec.replicas = 3' deployment.yaml

# Convert YAML to JSON (perfect for piping to jq)
yq -o=json '.' config.yaml

# Merge two YAML files
yq eval-all 'select(fileIndex == 0) * select(fileIndex == 1)' base.yaml overlay.yaml
```

## Why YQ Matters for Platform Work

YAML is the backbone of Infrastructure as Code (IaC). One wrong indentation can break a production deployment. `yq` removes this friction by treating YAML as a structured data format rather than a text file.

### Common Scenarios

=== ":material-kubernetes: K8s Manifest Auditing"

    Find all containers in a Deployment that don't have resource limits defined:

    ```bash title="Audit Resource Limits" linenums="1"
    yq '.spec.template.spec.containers[] | select(has("resources") | not) | .name' deployment.yaml
    ```

=== ":material-github: CI/CD Pipeline Updates"

    Update the image tag across multiple GitHub Action workflow files:

    ```bash title="Update Image Tag" linenums="1"
    yq -i '.jobs.build.steps[] | select(.uses == "docker/build-push-action*") | .with.tags = "v2.1.0"' .github/workflows/*.yml
    ```

=== ":material-sync: Config Transformation"

    Extract values from a legacy config and format them for a new system:

    ```bash title="Extract and Format" linenums="1"
    yq '.database | {host: .addr, port: .port}' old-config.yaml
    ```

## Core Functionality

<div class="grid cards" markdown>

-   :material-pencil: **In-place Editing (`-i`)**

    ---

    **Why it matters:** Allows you to modify files directly without temporary files or redirects.

    ``` bash title="Update Config" linenums="1"
    yq -i '.debug = true' config.yaml
    ```

    **Key insight:** Always verify your filter *without* `-i` first!

-   :material-file-tree: **Multi-document Handling**

    ---

    **Why it matters:** Kubernetes files often contain multiple documents separated by `---`.

    ``` bash title="Read All Documents" linenums="1"
    yq '.. | select(has("kind")) | .kind' multi.yaml
    ```

    **Key insight:** `yq` handles the stream of documents automatically.

-   :material-swap-horizontal: **Format Conversion (`-o`)**

    ---

    **Why it matters:** Sometimes you need JSON for a tool that doesn't speak YAML.

    ``` bash title="YAML to JSON" linenums="1"
    yq -o=json '.' service.yaml
    ```

    **Key insight:** Useful for interoperability between different CLI tools.

</div>

## Practice Problems

??? question "Practice Problem 1: Navigating Lists"

    In a YAML file like `{"items": [{"name": "a", "val": 1}, {"name": "b", "val": 2}]}`, how do you get the value (`val`) for the item named "b"?

    ??? tip "Answer"

        ```bash
        yq '.items[] | select(.name == "b") | .val' file.yaml
        ```
        This iterates through the list, filters for the specific name, and then selects the desired field.

??? question "Practice Problem 2: Adding a Field"

    How would you add a `labels` object with `app: my-app` to the `metadata` of a YAML file?

    ??? tip "Answer"

        ```bash
        yq '.metadata.labels.app = "my-app"' file.yaml
        ```
        `yq` will automatically create the parent objects (`labels`) if they don't exist.

## Key Takeaways

| Feature | command/Filter |
|:--------|:---------------|
| **Read** | `yq '.path.to.key' file.yaml` |
| **Write** | `yq -i '.key = "value"' file.yaml` |
| **Filter** | `select(.key == "match")` |
| **Convert** | `-o=json` (to JSON), `-o=xml` (to XML) |
| **Delete** | `del(.key.to.remove)` |

## Further Reading

### Official Documentation
- [YQ Documentation](https://mikefarah.gitbook.io/yq/) - Comprehensive guide for the most popular `yq` implementation (mikefarah).
- [YQ GitHub Repository](https://github.com/mikefarah/yq) - Source code and community discussions.

### Related Tools & Alternatives
- [jq](https://stedolan.github.io/jq/) - The JSON processor that inspired `yq`.
- [yamllint](https://yamllint.readthedocs.io/) - For validating YAML syntax and style.

### Deep Dives
- [YAML Specification](https://yaml.org/spec/1.2.2/) - For when you really need to understand why your indentation is broken.
- [Parsing and Serialization](https://cs.bradpenney.io/building_blocks/how_parsers_work/) - Theoretical background on how structured data is handled.
