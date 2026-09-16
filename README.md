# Operating Systems Laboratory

Python-based OS lab exercises, run on Ubuntu (or WSL running Ubuntu).

## Requirements
- Ubuntu 24.04 (or WSL) + VS Code + Python 3
- `os.fork()` in Program 1 only works on Linux/Unix — it will **not** run on
  native Windows Python.

## Repository layout

```
.
├── OS-First-Program/
│   └── program1.py
├── OS-Second-Program/
│   └── program2.py
└── README.md
```

---

## Program 1 — Parent and Child Process (`fork()`)

Demonstrates `fork()`, PID, PPID, and `wait()`.

**Run:**
```bash
mkdir -p ~/OS-First-Program
cd ~/OS-First-Program
# copy program1.py here
python3 program1.py
```

**Expected output:**
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

**Code walkthrough:**

| Code | Meaning |
|---|---|
| `import os` | Imports Python's operating-system module. |
| `os.getpid()` | Returns the PID of the current process. |
| `pid = os.fork()` | Creates a new child process. |
| `if pid == 0:` | This block runs in the **child** process. |
| `os.getppid()` | Returns the Parent Process ID (PPID). |
| `else:` | This block runs in the **parent** process. |
| `os.wait()` | Parent waits until the child finishes. |

**Common errors:**

| Problem | Cause | Fix |
|---|---|---|
| Bash syntax error after typing `os.fork()` | Python code typed at the `$` prompt | Put it inside `program1.py` instead |
| `python3: can't open file` | Terminal in the wrong folder | `cd ~/OS-First-Program`, then `python3 program1.py` |
| `AttributeError: os has no attribute fork` | Running native Windows Python | Run it from Ubuntu/WSL |
| `IndentationError` | Spaces under `if`/`else` don't match | Copy the indentation exactly |
| Different PID numbers each run | Normal OS behavior | Check `Child PPID == Parent PID`, not the exact numbers |

**Oral questions:**
- **What is PID?** A unique Process ID assigned by the operating system.
- **What does `fork()` do?** It creates a child process.
- **Why is Child PPID equal to Parent PID?** Because that parent process created the child.

---

## Program 2 — FCFS and SJF Scheduling

Simulates **FCFS** (First Come, First Served) and **non-preemptive SJF**
(Shortest Job First) CPU scheduling on the same set of processes, printing
the input table, execution intervals, and process sequence for each.

**Run:**
```bash
mkdir -p ~/OS-Second-Program
cd ~/OS-Second-Program
# copy program2.py here
python3 program2.py
```

**Input data:**

| PID | AT (Arrival Time) | BT (Burst Time) |
|-----|--------------------|------------------|
| P1  | 0                  | 7                |
| P2  | 2                  | 4                |
| P3  | 4                  | 1                |
| P4  | 5                  | 4                |

**Expected output:**
```
INPUT PROCESSES
PID AT BT
P1    0    7
P2    2    4
P3    4    1
P4    5    4

FCFS SCHEDULING
Process Start End
P1        0       7
P2        7       11
P3        11      12
P4        12      16
Sequence: P1 -> P2 -> P3 -> P4

SJF SCHEDULING
Process Start End
P1        0       7
P3        7       8
P2        8       12
P4        12      16
Sequence: P1 -> P3 -> P2 -> P4
```

**How it works:**
- **FCFS**: sorts all processes by arrival time (tie-break: PID), then runs
  them strictly in that order.
- **SJF (non-preemptive)**: at each decision point, builds the *ready set*
  (arrived but unfinished processes), picks the one with the smallest burst
  time (tie-break: arrival time, then PID), and lets it run to completion
  before choosing again. If nothing has arrived yet, the CPU goes `IDLE`
  until the next arrival.

**Common mistake:** don't sort every job by burst time up front — SJF must
only choose among processes that have *already arrived* at the current time.

**Note:** only scheduling order and execution intervals are covered here.
Waiting time, turnaround time, response time, Priority Scheduling, and Round
Robin are left for later programs.

---

## Pushing to GitHub

```bash
cd ~/<parent-folder-containing-both-programs>
git init
git add .
git commit -m "OS Lab: Program 1 (fork) and Program 2 (FCFS/SJF)"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```
