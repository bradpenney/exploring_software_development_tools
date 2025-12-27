# Makefiles

In the modern world of fancy IDEs and complex CI/CD pipelines, the humble **Makefile** remains one of the most powerful tools in a developer's workshop.

Invented in 1976, `make` is a build automation tool. It answers one simple question: **"Which files need to be updated?"**

## The Core Concept: Dependency

`make` doesn't just run scripts; it checks timestamps.

-   If `main.o` is older than `main.c`, then `main.c` must have changed.
-   Therefore, we must rebuild `main.o`.
-   If `main.c` hasn't changed, do nothing.

This "Lazy Evaluation" saves massive amounts of time in large projects.

## The Syntax

A Makefile is composed of **Rules**.

```makefile
target: dependencies
    command
```

**Important:** The indentation MUST be a **Tab character**, not spaces.

### Example

```makefile
# The final executable depends on the object file
app: main.o
    gcc -o app main.o

# The object file depends on the source code
main.o: main.c
    gcc -c main.c

# A "Phony" target to clean up
clean:
    rm *.o app
```

## How it Works

1.  You run `make app`.
2.  Make looks at `app`. It needs `main.o`.
3.  Make looks at `main.o`. It needs `main.c`.
4.  Make checks the timestamps.
    -   If `main.c` is newer than `main.o`, it runs the command `gcc -c main.c`.
    -   Then, it runs the command for `app`.

## Variables and Phony Targets

To make Makefiles readable, we use variables.

```makefile
CC = gcc
CFLAGS = -Wall -g

build: main.c
    $(CC) $(CFLAGS) -o app main.c
```

**Phony Targets** are commands that don't produce a file (like `clean`, `test`, or `deploy`). We mark them so `make` doesn't get confused if a file named "clean" accidentally exists.

```makefile
.PHONY: clean test

clean:
    rm -rf build/
```

## Practice Problems

??? question "Practice Problem 1: The Infinite Loop"

    What happens if you have a circular dependency?
    A: B
    B: A

    ??? tip "Solution"
        **Make will complain.**
        
        `make` detects the cycle and drops a dependency to break the loop, usually printing a warning: `make: Circular A <- B dependency dropped.`

??? question "Practice Problem 2: Always Run"

    If you want a command to run *every single time* you type `make`, regardless of file changes, what should you do?

    ??? tip "Solution"
        Make it a **Phony Target**. 
        
        Since a Phony Target (like `print-date`) doesn't correspond to a file, `make` assumes the "file" is always missing and therefore always runs the command to "build" it.

## Key Takeaways

-   **Target:** The file you want to create.
-   **Dependency:** The files the target relies on.
-   **Command:** The script to build the target.
-   **Lazy:** Only rebuilds what changed.

---

Makefiles are the universal adapter of the software world. Whether you are compiling C, generating a static website, or deploying to Kubernetes, a `Makefile` provides a standard, documented entry point for anyone joining your project.
