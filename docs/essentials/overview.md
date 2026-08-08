---
date: "2026-08-05 12:15"
title: "Dev Tools Essentials: Git, Data, and Terminal Diagnostics"
description: "The daily tools that separate a working professional from someone still fighting their environment — Git fundamentals, HTTP debugging, JSON/YAML wrangling, regex, and reading a struggling server."
---

# Essentials

You already manage production systems. Essentials covers the tools you reach for constantly but never formally learned end to end — Git past "commit and push," seeing what's actually on the wire when an API call doesn't do what you expected, and the data and diagnostic tools an incident actually runs on.

<div class="grid cards two-col" markdown>

-   :material-git: **[Git Series](git/git_basics.md)**

    ---

    **[Git for Beginners](git/git_basics.md)** and **[Git Clone, Push & Pull](git/git_collaboration.md)** — the three states, your first repository, and working with remotes as a team, not just solo.

-   :material-web: **[Web Tools](web/inspecting_http_traffic.md)**

    ---

    **[How to Inspect HTTP Traffic](web/inspecting_http_traffic.md)** — `curl -v` and browser DevTools, for the headers and status codes a plain response body doesn't show you.

-   :material-code-json: **Data on the Command Line**

    ---

    **[jq: Parsing API Responses and Logs](jq_parsing_json.md)**, **[yq: Wrangling YAML Configs](yq_wrangling_yaml.md)**, and **[Regular Expressions for SREs](regex_for_sres.md)** — filtering JSON, editing YAML safely, and the six regex characters that solve most log-searching problems.

-   :material-console: **Surviving the Terminal**

    ---

    **[Terminal Diagnostics](terminal_diagnostics.md)** and **[Vim Survival Mode](vim_survival_mode.md)** — the first-60-seconds incident checklist, and enough Vim to fix a config and get out alive.

</div>

---

Start with **[Git for Beginners](git/git_basics.md)** if Git itself is still shaky, jump to **[How to Inspect HTTP Traffic](web/inspecting_http_traffic.md)** if you're debugging an API right now, or start with **[Terminal Diagnostics](terminal_diagnostics.md)** if you just landed on a struggling server and need to know what to check first.
