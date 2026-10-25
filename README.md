# BankOS Platform — Banking & ATM Transaction System (OS mini-project)

Mini-project for Assignment No. 3 / Practical No. 9 (Operating Systems).
Selected problem statement: **Problem 5 — Banking and ATM Transaction Processing System.**

A bank's ATM network, mobile app, online banking portal and branch counters generate concurrent
transaction requests (deposits, withdrawals, transfers, balance inquiries, fraud/security checks).
This visualizer demonstrates, on that single scenario, all four required Operating Systems concepts:

| Module | OS Concept | What it shows |
| --- | --- | --- |
| CPU Scheduling | Scheduling algorithms | Preemptive Priority Scheduling (+ FCFS, SJF, Priority NP, Round Robin) over a transaction queue, with an animated Gantt chart and computed Waiting Time / Turnaround Time |
| Producer–Consumer | Process synchronization | Four channels (producers) and configurable teller threads (consumers) around a shared bounded buffer, modelled with mutex + `empty`/`full` semaphores |
| Banker's Algorithm | Deadlock avoidance | Editable Allocation/Max matrices, derived Need and Available, a step-safe safety check, safe-sequence discovery, and an interactive resource-request grant/deny simulator |
| Memory Management | Memory allocation | First Fit vs. Best Fit allocation of session memory across free partitions, with fragmentation reporting |

The accompanying case-study report (Assignment No. 3) covers the full problem analysis, pseudocode,
and worked examples for all four modules — the numbers in this app reproduce that report's examples
exactly (see **Testing** below).

## Run it

No build step needed — it's a single static HTML file that loads React, ReactDOM and Babel Standalone
from a CDN and compiles its own JSX in the browser.

1. Clone this repository.
2. Open `bankos-platform.html` directly in any modern browser (double-click it, or `open bankos-platform.html` / drag it into a browser tab).
3. An internet connection is needed on first load (for the CDN scripts and Google Fonts); no server or `npm install` required.

## Project structure

```
.
├── bankos-platform.html   # the entire mini-project (self-contained)
└── README.md
```

## Modules in detail

### 1. CPU Scheduling
Edit the transaction table (ID, name, arrival time, burst time, priority — 1 = highest) or load the
example dataset, pick an algorithm (FCFS, SJF, Priority Non-Preemptive, Priority Preemptive, Round
Robin), and press Play to watch the Gantt chart animate. Average Waiting Time and Turnaround Time
are computed live, with a full per-transaction results table.

### 2. Producer–Consumer
Configure the bounded-buffer capacity, number of teller (consumer) threads, and producer/consumer
speed, then press Start. Four channels randomly produce transaction requests into the shared buffer;
teller threads dequeue and "process" them. The buffer visual and the `full`/`empty` counters make the
semaphore state visible, and the event log shows when a producer or consumer blocks.

### 3. Banker's Algorithm
Edit the Allocation and Max matrices (Need and Available are derived automatically), then **Run
Safety Check** to execute the safety algorithm and reveal a safe sequence or an unsafe-state warning.
The **Simulate a Resource Request** panel lets you pick a transaction and a request vector; it checks
the request against Need and Available, tentatively grants it, re-runs the safety algorithm, and
commits the grant only if the resulting state stays safe.

### 4. Memory Management
Edit the list of free memory partitions and the list of session memory requests, then compare First
Fit and Best Fit side by side — each shows a colored memory bar, an allocation log, and a summary of
total free memory, largest free block, and any requests that couldn't be served.

## Testing

The three worked examples below are reproduced in both the report and this app, and were cross-checked
with an independent Node.js script against the app's own logic:

- **CPU Scheduling** (5 transactions, Preemptive Priority): Avg WT = 4.2 ms, Avg TAT = 7.2 ms.
- **Banker's Algorithm** (5 transactions × 3 resource types, Available = (3,3,2)): safe sequence
  ⟨T2, T4, T5, T1, T3⟩.
- **Memory Management** (blocks `[100,500,200,300,600]`, requests `[212,417,112,426]`): First Fit
  leaves one request unserved; Best Fit serves all four.

Load the example datasets in-app (buttons on each module) to reproduce these results directly.

## Author

Submitted by: Viraj Mohadikar - 24003158 and Rohini Shende - 24003173
Class: 5th SEM, Section B — Operating Systems
Subject Teacher: Dr. Pallavi Wankhede


## Platform features

- One shared **Workload** (type, channel, arrival, burst, priority, memory, resource claims) feeds every module.
- **Live bank** runs the whole pipeline tick by tick: channels, bounded buffer, Banker's Algorithm gate, memory allocation, CPU. It has play, pause, step back/forward, speed and a scrubber.
- **Sync lab** has a Protected / Unprotected switch that shows lost and doubled transactions without semaphores.
- **Report and tests** runs 7 self-tests in the browser against the real engine.
- Three scenarios, light/dark theme, and settings saved in the browser.
