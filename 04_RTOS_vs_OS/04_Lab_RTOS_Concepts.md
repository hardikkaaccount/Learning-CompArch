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
