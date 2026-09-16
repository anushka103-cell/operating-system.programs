# Operating Systems Lab — Program 1: Parent and Child Process

Demonstrates `fork()`, PID, PPID, and `wait()` in Python on Linux.

## Objective
Create and run a short Python program that shows a parent process, a child
process, their PIDs, PPIDs, `fork()`, and `wait()`.

## Requirements
- Ubuntu (or WSL running Ubuntu) — `os.fork()` only exists on Linux/Unix,
  **not** on native Windows Python.
- Python 3

## Setup and run (Ubuntu terminal)

```bash
# 1. Create and enter the project folder
mkdir -p ~/OS-First-Program
cd ~/OS-First-Program

# 2. Clone this repo here (or just save program1.py into this folder)
git clone <your-repo-url> .

# 3. Run it
python3 program1.py
```

## Expected output

```
Before Fork
Current PID: 2500

Child Process
Child PID : 2501
Parent PID: 2500

Parent Process
Parent PID: 2500
Child PID : 2501
```

Your actual PID numbers will differ each run — that's normal. What matters:

1. Parent PID and Child PID are different.
2. **Child PPID equals Parent PID.**
3. Parent output prints *after* child output, because `os.wait()` makes the
   parent pause until the child finishes.

## Code walkthrough

| Code | Meaning |
|---|---|
| `import os` | Imports Python's operating-system module. |
| `os.getpid()` | Returns the PID of the current process. |
| `pid = os.fork()` | Creates a new child process. |
| `if pid == 0:` | This block runs in the **child** process. |
| `os.getppid()` | Returns the Parent Process ID (PPID). |
| `else:` | This block runs in the **parent** process. |
| `os.wait()` | Parent waits until the child finishes. |

## Common errors

| Problem | Cause | Fix |
|---|---|---|
| Bash syntax error after typing `os.fork()` | Python code typed at the `$` prompt | Put it inside `program1.py` instead |
| `python3: can't open file` | Terminal in the wrong folder | `cd ~/OS-First-Program`, then `python3 program1.py` |
| `AttributeError: os has no attribute fork` | Running native Windows Python | Run it from Ubuntu/WSL |
| `IndentationError` | Spaces under `if`/`else` don't match | Copy the indentation exactly |
| Different PID numbers each run | Normal OS behavior | Check `Child PPID == Parent PID`, not the exact numbers |

## Oral questions

- **What is PID?** A unique Process ID assigned by the operating system.
- **What does `fork()` do?** It creates a child process.
- **Why is Child PPID equal to Parent PID?** Because that parent process created the child.
