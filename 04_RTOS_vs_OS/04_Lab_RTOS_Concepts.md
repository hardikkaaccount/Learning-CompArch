# 4.4 Lab: RTOS Concepts (The Scheduler)

## Goal

We will simulate a Round-Robin Scheduler in pseudocode/C thinking.

## Concept: The Task Control Block (TCB)

The OS needs to track every task. It uses a struct:

```c
struct TCB {
    uint32_t *stack_pointer; // Where was this task's stack top?
    uint32_t priority;
    enum State state; // READY, BLOCKED, RUNNING
};
```

## Exercise: Pen & Paper Trace

Assume we have 3 Tasks and 1 CPU.

- **Quantum**: 100ms (Time slice).

**Timeline**:

1.  **T=0ns**: OS starts Task 1.
2.  **T=50ms**: Task 1 calls `sleep(200)`.
    - _scheduler action_: Move Task 1 to BLOCKED list. Pick Task 2.
3.  **T=150ms**: Task 2 has run for 100ms (Quantum expired).
    - _scheduler action_: Preempt Task 2. Move to READY list. Pick Task 3.
4.  **T=250ms**: Task 1 wakes up? (Is it T=250 yet?)
    - _scheduler action_: Move Task 1 to READY list.
    - Does Task 1 preempt Task 3? (Only if Task 1 priority > Task 3).

## Challenge

Write a Python script `scheduler_sim.py` that manages a list of `Task` objects and prints a timeline log.

- `Task(name, duration, priority)`
- `run_scheduler(tasks)`

This is exactly how simulator tools for research (like Gem5) work at a high level.

### Solution: Python Simulator

```python
from collections import deque

# Task states
READY = "READY"
RUNNING = "RUNNING"
BLOCKED = "BLOCKED"


class Task:
    def __init__(self, name, duration, priority):
        self.name = name
        self.remaining = duration      # total run time left (ms)
        self.priority = priority
        self.state = READY
        self.wake_time = None           # used if sleeping

    def __repr__(self):
        return f"{self.name}({self.state})"


def run_scheduler(tasks, quantum=100):
    time = 0
    ready = deque(tasks)
    blocked = []
    current = None

    print("=== Scheduler Simulation Start ===")

    while ready or blocked or current:
        # Wake up blocked tasks
        for task in blocked[:]:
            if task.wake_time <= time:
                task.state = READY
                blocked.remove(task)
                ready.append(task)
                print(f"T={time}ms: {task.name} wakes up → READY")

        # Pick a task if none running
        if current is None and ready:
            current = ready.popleft()
            current.state = RUNNING
            print(f"T={time}ms: {current.name} starts RUNNING")

        if current is None:
            time += 10
            continue

        # Simulate execution
        run_time = min(quantum, current.remaining)
        time += run_time
        current.remaining -= run_time

        # Simulated behavior: Task1 sleeps at 50ms
        if current.name == "Task1" and current.remaining > 0 and time >= 50:
            current.state = BLOCKED
            current.wake_time = time + 200
            blocked.append(current)
            print(f"T={time}ms: {current.name} calls sleep(200) → BLOCKED")
            current = None
            continue

        # Task finished
        if current.remaining <= 0:
            print(f"T={time}ms: {current.name} finishes")
            current = None
            continue

        # Quantum expired → round-robin
        print(f"T={time}ms: Quantum expired for {current.name}")
        current.state = READY
        ready.append(current)
        current = None

    print("=== Scheduler Simulation End ===")


if __name__ == "__main__":
    tasks = [
        Task("Task1", duration=300, priority=1),
        Task("Task2", duration=300, priority=1),
        Task("Task3", duration=300, priority=1),
    ]

    run_scheduler(tasks)
```

---

## Navigation

[< Previous](./03_Concurrency_Issues.md) | [Home](../README.md) | [**Finish Course 🎓**](../Course_Completed.md)
