# MYSHELL

## Overview
This repository contains small, focused C programs that demonstrate key building blocks of a Unix-like shell:

- **Pipelines & descriptor duplication** (using `pipe`, `dup/dup2`, `fork`, `exec`)
- **Signal handling** (e.g., gracefully handling `SIGINT`/Ctrl-C, restoring terminal behavior)

These are educational, single-file demos you can compile and run individually to understand how shells wire processes together and deal with signals. Files currently in the repo:

```
pipe_dup_implementation.c
signal_handling.c
```

## Features
- Create one or more pipes and connect child processes’ stdin/stdout with `dup2`.
- Execute external programs via `fork` + `exec*`.
- Handle user-generated signals (e.g., Ctrl-C) without killing the parent process abruptly.
- Clear, minimal code focusing on the core OS primitives.

## Prerequisites
- GCC or Clang
- A POSIX-like environment (Linux/macOS/WSL)

## Build
Each demo is standalone. For example:

```bash
# Pipeline/dup demo
gcc -Wall -Wextra -O2 pipe_dup_implementation.c -o pipe_demo

# Signal-handling demo
gcc -Wall -Wextra -O2 signal_handling.c -o sig_demo
```

## Run
### Pipeline/dup demo
```bash
./pipe_demo
```

### Signal-handling demo
```bash
./sig_demo
```

## How it works (high level)
- **Pipes & dup:** Create a unidirectional channel (`pipe(fd)`), `fork()` children, and call `dup2(fd[0 or 1], STDIN_FILENO/STDOUT_FILENO)` to rewire standard streams before `exec`.
- **Signals:** Use `sigaction` (or `signal`) to install handlers.

## Learning notes
- Always `close` unused pipe ends to avoid deadlocks.
- After `dup2`, you can `close` the original FDs — the stdio descriptors remain redirected.
- Avoid non-async-signal-safe calls inside handlers.

## License
Add a license if you intend others to reuse these snippets.

## Roadmap ideas
- Support multiple pipes (`cmd1 | cmd2 | cmd3`).
- Foreground/background job control (`SIGTSTP`, `SIGCHLD`).
