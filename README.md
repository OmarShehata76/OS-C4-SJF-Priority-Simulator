# SJF vs Priority Scheduling Simulator

## Project Overview
This project is part of the Operating Systems course (C4 — SJF vs Priority Comparison Project).  
It implements and compares two CPU scheduling algorithms:
- **SJF — Shortest Job First (Preemptive / SRTF)**
- **Priority Scheduling (Preemptive)**

Both algorithms are simulated on the same workload, with full Gantt charts, per-process metrics, and a detailed comparison analysis.

---

## Team Members

| No. | Student Name | Student ID | Contribution Area |
|-----|-------------|------------|-------------------|
| 1   |             |            |                   |
| 2   |             |            |                   |
| 3   |             |            |                   |
| 4   |             |            |                   |
| 5   |             |            |                   |

---

## Algorithms Implemented

### SJF — Shortest Job First (Preemptive)
- Always selects the process with the shortest **remaining** burst time from the ready queue.
- Preempts the current process if a new process arrives with a shorter remaining burst time.
- Tie-breaking rule: earlier arrival time first, then by Process ID alphabetically.

### Priority Scheduling (Preemptive)
- Always selects the process with the **highest priority** (lowest priority number = highest priority).
- Preempts the current process if a new process arrives with a higher priority.
- Tie-breaking rule: earlier arrival time first, then by Process ID alphabetically.

---

## Features
- Dynamic process input at runtime (add as many processes as needed)
- Full input validation (rejects negative arrival time, zero/negative burst time, duplicate IDs, invalid priority values)
- Separate Gantt Charts for both algorithms
- Per-process metrics: WT, TAT, RT, CT
- Average WT, Average TAT, Average RT for both algorithms
- Head-to-Head comparison table
- Auto-generated conclusion and recommendation
- 4 pre-loaded test scenarios (A, B, C, D)

---

## Metrics Calculated

| Metric | Formula | Description |
|--------|---------|-------------|
| CT (Completion Time) | — | Time when process finishes execution |
| TAT (Turnaround Time) | CT - Arrival Time | Total time from arrival to completion |
| WT (Waiting Time) | TAT - Burst Time | Time spent waiting in the ready queue |
| RT (Response Time) | First CPU time - Arrival Time | Time from arrival to first execution |

---

## How to Run

### Option 1 — Direct (Recommended)
1. Download `sjf_priority_simulator.html`
2. Double-click to open in any browser (Chrome or Edge recommended)
3. No installation required

### Option 2 — From Repository
1. Clone the repository:
   ```
   git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
   ```
2. Open `src/sjf_priority_simulator.html` in your browser
3. Done!

---

## How to Use the Simulator

1. **Add Processes** — Enter Process ID, Arrival Time, Burst Time, and Priority, then click **+ Add**
2. **Or Load a Scenario** — Click any of the 4 scenario buttons to auto-load test data
3. **Run Simulation** — Click the green **▶ Run Simulation** button
4. **View Results** — Gantt Charts, metrics tables, comparison, and conclusion appear automatically

---

## Test Scenarios

| Scenario | Description |
|----------|-------------|
| A — Basic Mixed | Normal workload with different arrival and burst times |
| B — Conflict | High-priority long process vs low-priority short process |
| C — Starvation | Workload where low-priority process may wait a very long time |
| D — Validation | Invalid input data to demonstrate input validation behavior |

Full documented test scenarios with inputs and outputs are in the `/test-cases/` folder.

---

## Repository Structure

```
project-root/
  src/
    sjf_priority_simulator.html   ← Main application file
  screenshots/
    scenario_a_gantt.png
    scenario_a_results.png
    scenario_b_gantt.png
    scenario_b_results.png
    scenario_c_gantt.png
    scenario_c_results.png
    scenario_d_validation.png
  test-cases/
    test_cases.md                 ← All documented test scenarios
  README.md
  .gitignore
```

---

## Priority Rule
> **Lower number = Higher priority**  
> Example: Priority 1 is more important than Priority 5.

---

## Technology Used
- **Language:** HTML5 + CSS3 + JavaScript (Vanilla)
- **GUI:** Browser-based interface (no installation required)
- **Version Control:** Git + GitHub
