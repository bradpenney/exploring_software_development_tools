---
date: "2026-06-21 12:00"
title: "Seeing API Traffic: curl -v and the Network Tab"
description: "You already curl APIs — now watch what's really on the wire. Use curl -v and browser DevTools to inspect headers, status codes, and the request lifecycle."
---

# Seeing API Traffic: curl -v and the Network Tab

!!! tip "Part of a Learning Path"
    This article is part of the [How APIs Actually Work](https://bradpenney.io/pathways/how-apis-work) pathway on [bradpenney.io](https://bradpenney.io) — a guided sequence through the topic. It also stands on its own.

You `curl` APIs every day. You read the JSON, you check the status code, you move on. But when something's wrong — a `401` you can't explain, a request that "works in `curl` but not the browser," a redirect you didn't expect — the body of the response tells you almost nothing. The answer is in the parts you usually don't look at: the headers, the handshake, the redirects, the preflight.

This article isn't about learning `curl` — you've got that. It's about learning to **see**: turning `curl` and your browser's DevTools into x-ray glasses for HTTP, so the invisible parts of a request become visible and debuggable.

If the *concepts* underneath (methods, headers, status families) are fuzzy, the [Anatomy of an HTTP Request and Response](https://cs.bradpenney.io/efficiency/web/anatomy_of_request_response/) on the CS site is the companion read. Here we make those parts observable on real traffic.

## Quick Start: The Flags That Reveal Everything

Three `curl` flags turn a silent request into a full transcript. Memorize these and you'll diagnose 90% of API issues without leaving the terminal.

```bash title="The diagnostic curl flags" linenums="1"
curl -v https://api.example.com/orders     # (1)!
curl -i https://api.example.com/orders     # (2)!
curl -s -o /dev/null -w '%{http_code}\n' \
     https://api.example.com/orders        # (3)!
```

1. `-v` (verbose) shows the **entire** conversation: DNS, TLS handshake, request headers sent (`>`), response headers received (`<`), then the body.
2. `-i` includes the response headers above the body — lighter than `-v` when you only care about the response side.
3. `-w` (write-out) prints just the bits you ask for — here only the status code — perfect for scripts and health checks.

## Reading a Verbose Request, Line by Line

The power of `-v` is that it labels who said what. Lines starting with `*` are `curl`'s own notes (connection, TLS), `>` is what `curl` **sent**, and `<` is what the server **returned**.

```bash title="Annotated output of curl -v" linenums="1"
$ curl -v https://api.example.com/orders
*   Trying 203.0.113.10:443...              # (1)!
* SSL connection using TLSv1.3 / ...        # (2)!
* Server certificate: CN=api.example.com    # (3)!
> GET /orders HTTP/1.1                       # (4)!
> Host: api.example.com
> Authorization: Bearer abc123              # (5)!
>
< HTTP/1.1 401 Unauthorized                  # (6)!
< WWW-Authenticate: Bearer error="invalid_token"  # (7)!
< Content-Type: application/json
<
{"error":"token expired"}                    # (8)!
```

1. DNS already resolved the host to this IP, and `curl` is connecting to port 443.
2. The TLS handshake succeeded and negotiated TLS 1.3 — if TLS were broken, it would fail *here*, before any HTTP.
3. The certificate the server presented — useful for spotting hostname mismatches or who [terminates TLS](https://networking.bradpenney.io/essentials/tls/https_for_apis/).
4. The request line `curl` sent: method, path, HTTP version.
5. The headers `curl` sent — confirm your `Authorization` actually went out as intended.
6. The status line: `401`, an [authentication failure](https://cs.bradpenney.io/efficiency/web/authentication_vs_authorization/).
7. The response header that explains *why* — `invalid_token`. This line is invisible without `-v`/`-i`, and it's the actual answer.
8. Only now, the body — which by itself ("token expired") you might have missed the cause of.

The lesson: the body said "token expired," but the *headers* (`WWW-Authenticate`, the `401` status) told the real story. Most API debugging lives above the body.

## Inspecting Specific Layers

Different problems hide in different layers. Match the flag to the question.

<div class="grid cards" markdown>

-   :material-key-outline: __Confirm what you're sending__

    ---

    **Why it matters:** half of "auth doesn't work" is a header that never left.

    ```bash title="Send and show an auth header" linenums="1"
    curl -v -H "Authorization: Bearer $TOKEN" \
         https://api.example.com/me
    ```

    Check the `>` lines: did your header actually go out, spelled correctly?

-   :material-sign-direction: __Follow redirects__

    ---

    **Why it matters:** a silent `301`/`302` can drop your headers.

    ```bash title="Follow and trace redirects" linenums="1"
    curl -v -L https://example.com/api
    ```

    `-L` follows redirects; `-v` shows each hop, including the [`http`→`https` upgrade](https://cs.bradpenney.io/efficiency/web/anatomy_of_request_response/).

-   :material-send: __Send a real POST__

    ---

    **Why it matters:** reproduce a write the way the app does it.

    ```bash title="POST JSON with the right content type" linenums="1"
    curl -v -X POST https://api.example.com/orders \
      -H "Content-Type: application/json" \
      -d '{"product_id":7,"quantity":2}'
    ```

    Forgetting `Content-Type` is a top cause of a rejected-but-valid body.

-   :material-clock-outline: __Measure the timing__

    ---

    **Why it matters:** separate slow DNS/TLS from a slow app.

    ```bash title="Break down where the time goes" linenums="1"
    curl -s -o /dev/null \
      -w 'dns:%{time_namelookup} connect:%{time_connect} total:%{time_total}\n' \
      https://api.example.com/orders
    ```

    Tells you if latency is in name resolution, the connection, or the server.

</div>

## The Browser's Network Tab: Seeing What curl Can't

`curl` is perfect for the request *you* construct. But some problems only exist in a real browser — most famously [CORS](https://networking.bradpenney.io/efficiency/http/cors_explained/) and the automatic requests browsers make on their own. For those, open **DevTools → Network** (F12 in Chrome/Firefox).

What the Network tab shows that `curl` won't:

- **Every request the page made**, including ones your code didn't explicitly write — analytics, redirects, and crucially **`OPTIONS` preflight** requests the browser sends before cross-origin calls.
- **CORS failures**, with the blocked request marked and the missing `Access-Control-Allow-*` header named in the console.
- **The full request/response per row** — click any request to see its headers, payload, status, timing, and response, the GUI equivalent of `curl -v`.

!!! tip "The 'works in curl, fails in the browser' workflow"

    When an API works in `curl` but fails in the browser, it's almost always [CORS](https://networking.bradpenney.io/efficiency/http/cors_explained/) — a browser-only rule `curl` doesn't enforce. Reproduce it in the Network tab: look for a red request, check the Console for the CORS message, and look for a preceding `OPTIONS` request. That tells you instantly it's a server CORS-header problem, not a `curl`-reproducible bug. Right-click any request → **Copy as cURL** to turn a browser request back into a terminal command you *can* iterate on.

## Why This Matters for Platform Work

- **It collapses guesswork during incidents.** "The API is broken" becomes a precise question: did DNS resolve? did TLS negotiate? what status and headers came back? Each is one flag away, so you bisect instead of speculate.
- **It distinguishes layers fast.** `curl -v` failing at the TLS line is a certificate problem; a clean handshake but a `401` is an auth problem; a `502` is a [gateway/backend](https://networking.bradpenney.io/efficiency/api_gateways/reverse_proxies_and_gateways/) problem. Same tool, different layer, instantly narrowed.
- **It bridges the front-end/back-end divide.** When a front-end dev reports a failure you can't reproduce in `curl`, the Network tab is the shared ground where you see *their* browser's view — preflights, CORS, dropped credentials — and fix the right thing.

## Common Scenarios

=== ":material-lock-alert: Mysterious 401"

    The body just says "unauthorized." Run `curl -v` and read two things: the `>` lines (did your `Authorization` header actually send, and is it spelled right?) and the `<` `WWW-Authenticate` header (the server's reason — expired, malformed, wrong scheme). The cause is almost always in those header lines, not the body.

=== ":material-transfer: Headers vanish after a redirect"

    Auth works against the final URL but fails against the public one. `curl -v -L` reveals a `301`/`302` in between — and `curl` (like browsers) may drop the `Authorization` header when redirected to a different host. Fix the client to call the canonical URL directly, or confirm the redirect target is the same host.

=== ":material-web-cancel: Browser-only failure"

    `curl` returns `200`, the app shows an error. Open DevTools → Network, find the red request, read the Console. A CORS message plus a preceding `OPTIONS` confirms a [server-side CORS config](https://networking.bradpenney.io/efficiency/http/cors_explained/) issue — invisible to `curl` by design.

## Practice Problems

??? question "Practice Problem 1: Did the Header Send?"

    A script gets `401` from an API, but the engineer swears the token is correct. Which `curl` flag confirms whether the `Authorization` header actually left the client, and where in the output do you look?

    ??? tip "Solution"

        Use `curl -v`. In the output, look at the lines beginning with `>` — these are exactly what `curl` **sent**. If `> Authorization: Bearer ...` isn't there (or the variable expanded to empty, e.g. `> Authorization: Bearer`), the header never went out — the bug is client-side construction, not the token's validity. This separates "wrong token" from "token never sent," which look identical from the response body alone.

??? question "Practice Problem 2: TLS or Auth?"

    Two failing requests: one errors before any HTTP status appears; the other returns `403`. Using `curl -v`, how do you tell a TLS/certificate problem from an authorization problem?

    ??? tip "Solution"

        Watch *where* `curl -v` stops. A **TLS/certificate** problem fails during the handshake — you'll see `*` lines about SSL and an error *before* any `>` request lines or `<` status line ever appear (no HTTP happened). An **authorization** problem completes the handshake and the request fully, then returns `< HTTP/1.1 403 Forbidden` — the connection was fine, the server just refused the action. TLS fails before HTTP; `403` is HTTP working and saying "no."

??? question "Practice Problem 3: Reproducing a Browser Request"

    A front-end developer's request fails in the browser but you can't reproduce it with the `curl` command you typed by hand. What DevTools feature gets you an exact, runnable reproduction, and why does it matter?

    ??? tip "Solution"

        In DevTools → Network, right-click the failing request and choose **Copy as cURL**. This generates a `curl` command with the *exact* headers, cookies, and body the browser actually sent — which often differ from what you typed (extra headers, auth cookies, content types). Running it reproduces the real request in the terminal where you can iterate with `-v`. If it now *succeeds* in `curl` but failed in the browser, that's a strong signal the issue is browser-only — typically CORS — rather than something wrong with the request itself.

## Key Takeaways

| Concept | What It Means |
| :--- | :--- |
| `curl -v` | Full transcript: DNS, TLS, sent headers (`>`), received headers (`<`), body |
| `curl -i` | Response headers above the body — lighter than `-v` |
| `curl -w` | Print just what you ask for (status code, timings) — script-friendly |
| **Read the headers** | Most API failures are explained above the body, not in it |
| **Network tab** | Sees browser-only traffic: preflights, CORS, dropped credentials |
| **Copy as cURL** | Turn a real browser request into a terminal command you can iterate on |

You already speak `curl` — this is about listening as well as talking. The headers, the handshake, the redirects, the preflights: they're all right there the moment you add `-v` or open the Network tab. Stop reading only the body, start reading the whole conversation, and the APIs you've been using on faith become systems you can actually see.

## Further Reading

### Related Tools & Concepts

- **JQ: Parsing API Responses and Logs** *(coming soon)* — once you can *see* the JSON body, `jq` slices it apart.

### Computer Science Fundamentals

- **[Anatomy of an HTTP Request and Response (cs.bradpenney.io)](https://cs.bradpenney.io/efficiency/web/anatomy_of_request_response/)** — the methods, headers, and status families you're inspecting.
- **[Authentication vs Authorization (cs.bradpenney.io)](https://cs.bradpenney.io/efficiency/web/authentication_vs_authorization/)** — what `401` vs `403` in the output mean.

### Networking Deep Dives

- **[CORS Explained (networking.bradpenney.io)](https://networking.bradpenney.io/efficiency/http/cors_explained/)** — the browser-only failures the Network tab reveals.
- **[HTTPS for APIs (networking.bradpenney.io)](https://networking.bradpenney.io/essentials/tls/https_for_apis/)** — the TLS handshake `curl -v` shows you.

### External Resources

- [curl documentation](https://curl.se/docs/manpage.html) — every flag, authoritative.
- [Everything curl (free book)](https://everything.curl.dev/) — the definitive guide by curl's author.
- [Chrome DevTools: Network features](https://developer.chrome.com/docs/devtools/network) — the Network tab in depth.
