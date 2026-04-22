# Test Cases — SJF vs Priority Scheduling Simulator
## C4 | Operating Systems Course Project

---

## Scenario A — Basic Mixed Workload

### Description
A normal workload with 5 processes having different arrival times, burst times, and priorities.  
This scenario verifies that both algorithms work correctly on a standard input.

### Input Data

| PID | Arrival Time | Burst Time | Priority |
|-----|-------------|------------|----------|
| P1  | 0           | 6          | 3        |
| P2  | 1           | 4          | 1        |
| P3  | 2           | 8          | 4        |
| P4  | 3           | 2          | 2        |
| P5  | 4           | 5          | 5        |

### Expected SJF Results

| PID | AT | BT | CT | TAT | WT | RT |
|-----|----|----|----|-----|----|----|
| P1  | 0  | 6  | 13 | 13  | 7  | 0  |
| P2  | 1  | 4  | 7  | 6   | 2  | 0  |
| P3  | 2  | 8  | 25 | 23  | 15 | 13 |
| P4  | 3  | 2  | 5  | 2   | 0  | 0  |
| P5  | 4  | 5  | 18 | 14  | 9  | 9  |

**SJF Averages:** Avg WT = 6.6 | Avg TAT = 11.6 | Avg RT = 4.4

### Expected Priority Results

| PID | AT | BT | Priority | CT | TAT | WT | RT |
|-----|----|----|----------|----|-----|----|----|
| P1  | 0  | 6  | 3        | 11 | 11  | 5  | 0  |
| P2  | 1  | 4  | 1        | 5  | 4   | 0  | 0  |
| P3  | 2  | 8  | 4        | 23 | 21  | 13 | 13 |
| P4  | 3  | 2  | 2        | 7  | 4   | 2  | 2  |
| P5  | 4  | 5  | 5        | 25 | 21  | 16 | 16 |

**Priority Averages:** Avg WT = 7.2 | Avg TAT = 12.2 | Avg RT = 6.2

### Analysis
SJF produced lower average waiting time (6.6 vs 7.2) and lower average turnaround time (11.6 vs 12.2).  
Priority scheduling gave P2 (priority=1) the fastest response time since it had the highest priority.

---

## Scenario B — Conflict Between Burst Time and Priority

### Description
This scenario creates a direct conflict: P1 has high priority (1) but a long burst time (10),  
while P2 has low priority (5) but a very short burst time (2).  
This reveals how the two algorithms make opposite decisions.

### Input Data

| PID | Arrival Time | Burst Time | Priority |
|-----|-------------|------------|----------|
| P1  | 0           | 10         | 1        |
| P2  | 1           | 2          | 5        |
| P3  | 2           | 3          | 4        |
| P4  | 3           | 1          | 3        |
| P5  | 4           | 4          | 2        |

### Key Observation — SJF
SJF will preempt P1 at time=1 when P2 arrives (burst=2 < remaining=9).  
Short processes P2, P4, P3 finish quickly before P1 completes.

### Key Observation — Priority
Priority will NOT preempt P1 because P1 has priority=1 (highest).  
P1 runs to completion first, then processes are served by priority order.

### Analysis
- **SJF** favors short jobs: P2 (burst=2) finishes very quickly even though its priority is low.
- **Priority** favors urgent jobs: P1 (priority=1) runs to completion uninterrupted even though it's long.
- This is the core trade-off: **efficiency vs urgency**.
- SJF gives better average WT and TAT in this scenario.
- Priority ensures the most important process (P1) is served first without interruption.

---

## Scenario C — Starvation Risk

### Description
4 short processes with high priority (1) arrive at time=0 alongside 1 long process with low priority (5).  
This demonstrates how Priority scheduling can cause starvation for low-priority processes.

### Input Data

| PID | Arrival Time | Burst Time | Priority |
|-----|-------------|------------|----------|
| P1  | 0           | 1          | 1        |
| P2  | 0           | 1          | 1        |
| P3  | 0           | 1          | 1        |
| P4  | 0           | 1          | 1        |
| P5  | 0           | 8          | 5        |

### Key Observation — SJF
SJF will run P1, P2, P3, P4 first (burst=1 each), then P5.  
P5 waits 4 units before starting. Fair to short jobs.

### Key Observation — Priority
Priority will run P1, P2, P3, P4 first (priority=1), then P5.  
P5 (priority=5) is starved until all high-priority processes finish.  
**Starvation observed:** P5 waited 4 units with no chance of running earlier.

### Analysis
In this scenario, both algorithms produce similar results because SJF also favors short jobs.  
However, in a continuous system with new high-priority/short jobs arriving constantly,  
Priority scheduling causes indefinite starvation for P5, while SJF would eventually run P5  
once no shorter jobs remain.

---

## Scenario D — Input Validation Test

### Description
This scenario tests the simulator's ability to reject invalid input data safely.  
No simulation is run — this tests the validation layer only.

### Invalid Input Attempts

| Test | PID | Arrival Time | Burst Time | Priority | Expected Result |
|------|-----|-------------|------------|----------|-----------------|
| 1    | P1  | -1          | 5          | 2        | REJECTED — negative arrival time |
| 2    | P1  | 0           | 0          | 2        | REJECTED — burst time must be ≥ 1 |
| 3    | P1  | 0           | 5          | 15       | REJECTED — priority must be 1–10 |
| 4    | P1  | 0           | 5          | 2        | ACCEPTED |
| 5    | P1  | 2           | 3          | 1        | REJECTED — duplicate Process ID |
| 6    |     | 0           | 5          | 2        | REJECTED — missing Process ID |
| 7    | P2  | abc         | 5          | 2        | REJECTED — non-numeric arrival time |

### Validation Rules Implemented
- Process ID: required, must be unique, max 5 characters
- Arrival Time: required, must be a number ≥ 0
- Burst Time: required, must be a number ≥ 1
- Priority: required, must be a number between 1 and 10
- All fields must be filled before adding a process

---

## Summary Comparison Table

| Metric | Scenario A SJF | Scenario A Priority | Scenario B SJF | Scenario B Priority |
|--------|---------------|--------------------|-----------------|--------------------|
| Avg WT | 6.6           | 7.2                | lower           | higher             |
| Avg TAT| 11.6          | 12.2               | lower           | higher             |
| Avg RT | 4.4           | 6.2                | higher          | lower (for P1)     |
| Fairness | Medium      | Lower (starvation risk) | Higher    | Lower              |

---

## Conclusion

**SJF** consistently produces lower average waiting time and turnaround time because it always  
minimizes the time short processes spend waiting. However, it can delay long processes indefinitely.

**Priority Scheduling** ensures that the most urgent/important processes are served first,  
which is critical in real-time systems. However, it risks starvation for low-priority processes.

**Recommended for general workloads:** SJF — better overall metrics.  
**Recommended for time-critical systems:** Priority — ensures urgent tasks are handled immediately.
