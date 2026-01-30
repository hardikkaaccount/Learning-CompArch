# 4.3 Concurrency Issues: Data Races

With great power comes great bugs.

## The Race Condition

Task A and Task B both want to increment a global counter `cnt`.
`cnt++` looks like one line of C code.
But in Assembly (RISC-V), it is THREE instructions:

1.  `LW  t0, cnt` (Load)
2.  `ADDI t0, t0, 1` (Add)
3.  `SW  t0, cnt` (Store)

**The Disaster Scenario**:

1.  Task A Loads `cnt` (Value: 5).
2.  **INTERRUPT!** Context Switch to Task B.
3.  Task B Loads `cnt` (Value: 5).
4.  Task B Adds 1 -> 6. Stores 6.
5.  Task B finishes. Switch back to Task A.
6.  Task A (still has 5 in `t0`) Adds 1 -> 6. Stores 6.

**Result**: We incremented twice, but the value is 6 instead of 7. **Data Corruption.**

## The Solution: Mutex (Mutual Exclusion)

We need a "Lock".

```c
take_lock(mutex); // If locked, sleep until free.
cnt++;
give_lock(mutex);
```

## The Deadlock (The Standoff)

- Task A holds Lock 1, wants Lock 2.
- Task B holds Lock 2, wants Lock 1.
- **Result**: Both freeze forever.

**Research Insight**: Formal Verification is a field dedicated to mathematically proving that a system CANNOT deadlock.
