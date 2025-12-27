# Semantic Versioning (SemVer)

In the software world, version numbers are not just marketing (like "iPhone 15" or "Windows 95"). They are a **contract**.

**Semantic Versioning** (SemVer) is a standard specification (Tom Preston-Werner, 2013) that gives meaning to the underlying numbers. It allows automated tools (like `npm` or `pip`) to upgrade your software safely.

## The Format: X.Y.Z

A SemVer number has three parts: **Major.Minor.Patch**.

### 1. MAJOR (X)
Increment this when you make **incompatible API changes**.
-   *Meaning:* "I broke something. You will need to change your code to upgrade."
-   *Example:* `1.0.0` -> `2.0.0`.

### 2. MINOR (Y)
Increment this when you add **functionality in a backward-compatible manner**.
-   *Meaning:* "I added a new feature, but your old code will still work."
-   *Example:* `1.1.0` -> `1.2.0`.

### 3. PATCH (Z)
Increment this when you make **backward-compatible bug fixes**.
-   *Meaning:* "I fixed a bug. It works exactly the same, just better."
-   *Example:* `1.1.1` -> `1.1.2`.

## The Rules of the Game

1.  **Start at 0.1.0:** During initial development, anything goes. The rules don't apply until `1.0.0`.
2.  **Reset Lower Numbers:** If you increment Major, reset Minor and Patch to 0. (`1.4.2` -> `2.0.0`).
3.  **Never Change a Tag:** Once a version is released (tagged), it is immutable. If you made a mistake, you must release a new version.

## Dependency Hell

Why does this matter? Imagine your project depends on a library called `request-lib`.

In your `package.json` (or `requirements.txt`), you might see:
`"request-lib": "^1.2.0"`

The caret (`^`) means: *"Install the latest version that does not change the Major number."*
-   It **will** auto-install `1.2.1` (Patch).
-   It **will** auto-install `1.3.0` (Minor).
-   It **will NOT** install `2.0.0` (Major).

This prevents your app from breaking automatically when a library updates.

## Practice Problems

??? question "Practice Problem 1: The Upgrade"

    You are the maintainer of a library. You rename a function from `getUser()` to `fetchUser()`, removing the old function entirely. 
    
    Current version: `1.4.2`. What should the new version be?

    ??? tip "Solution"
        **2.0.0**.
        
        Removing a function is a **Breaking Change**. Code that used `getUser()` will crash. You must increment the Major version.

??? question "Practice Problem 2: The New Feature"

    You add a new optional parameter to an existing function. Old code calling it without the parameter still works fine.
    
    Current version: `2.4.1`. What should the new version be?

    ??? tip "Solution"
        **2.5.0**.
        
        This is new functionality (a new parameter) but it is backward-compatible. Increment Minor and reset Patch to 0.

## Key Takeaways

| Change Type | Increment | Example |
| :--- | :--- | :--- |
| **Breaking** | MAJOR | `2.0.0` |
| **Feature** | MINOR | `1.2.0` |
| **Fix** | PATCH | `1.1.1` |

---

Semantic Versioning brings order to the chaos of dependency management. It allows developers to trust updates, knowing that a simple "bug fix" release won't suddenly delete the API they rely on.
