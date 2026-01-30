# 2.2 Hazards & Stalls: Chaos in the Factory

Pipelining is great until dependencies happen. These are called **Hazards**.

## 1. Data Hazards (The "Wet Shirt" Problem)

Imagine:

1.  **Instr A**: `ADD t0, t1, t2` (Computes `t0`)
2.  **Instr B**: `ADD t3, t0, t5` (Needs `t0`)

In the pipeline:

- Cycle 3: Instr A is in **Execute** stage (calculating `t0`).
- Cycle 3: Instr B is in **Decode** stage (trying to READ `t0`).

**Problem**: `t0` hasn't been written back to the register file yet! It's still inside the ALU. Instr B reads old garbage data.

### Solutions:

1.  **Stall (The Bubble)**: Hardware pauses Instr B for 2 cycles. Performance drops.
2.  **Forwarding (Bypassing)**: Hardware wires the output of the ALU _directly_ back to the input of the next cycle. "Here, take the result before I even save it!"

## 2. Control Hazards (The "Fork in the Road")

Imagine:

1.  **Instr A**: `BEQ t0, t1, Label` (If Equal, Jump).
2.  **Instr B**: The instruction right after A.

**Problem**:

- Cycle 2: We are decoding the Branch. We don't know yet if we should take the jump!
- But the pipeline _must_ fetch the next instruction in Cycle 2. Which one? The one after A? Or the one at Label?

### Solutions:

1.  **Stall**: Wait until we know. (Terrible for performance).
2.  **Branch Prediction**: Guess!
    - "I guess we won't jump." (Static)
    - "Last time we jumped, so let's jump again." (Dynamic)
    - If wrong? **Flush**. Throw away the work we did on the wrong path (Wasted energy).

**Research Area**: Modern Branch Predictors use AI/Perceptrons to guess correctly 99% of the time.

---
## Navigation
[< Previous](./01_Pipeline_Basics.md) | [Home](../README.md) | [Next >](./03_Memory_Hierarchy.md)
