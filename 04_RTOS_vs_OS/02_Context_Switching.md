# 4.2 Context Switching: The Magic Trick

How does a CPU with only **one** set of registers run **ten** tasks?
It uses a "Save Game" mechanic called **Context Switching**.

## The Procedure

Imagine Task A is running. The **SysTick Timer** fires (an interrupt).

1.  **Pause**: Hardware interrupts execution.
2.  **Save Context**: The OS pushes ALL of Task A's registers (`x1`-`x31`) onto Task A's **Stack**.
3.  **Decide**: The Scheduler picks Task B.
4.  **Restore Context**: The OS pops ALL of Task B's registers from Task B's **Stack**.
5.  **Resume**: The OS executes `mret` (Return).

**Illusion**: To Task A, it felt like time stopped for a split second. To Task B, it feels like it just woke up.

## The Cost

Context Switching is not free.

- It takes CPU cycles to save/restore 32 registers.
- It thrashes the **Cache** (Task A's data is replaced by Task B's data).

**Research Area**: "Hardware Threading" (Barrel Processors) where the CPU has _multiple_ physical register sets to switch instantly (0 cycles).

---
## Navigation
[< Previous](./01_SuperLoops_vs_RTOS.md) | [Home](../README.md) | [Next >](./03_Concurrency_Issues.md)
