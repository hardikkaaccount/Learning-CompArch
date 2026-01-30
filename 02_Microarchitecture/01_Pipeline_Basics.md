# 2.1 Pipeline Basics: The Laundry Analogy

## Throughput vs Latency

Imagine you have 4 loads of laundry to do.
Each load takes:

1. **Wash** (30 mins)
2. **Dry** (30 mins)
3. **Fold** (30 mins)
   **Total per load**: 90 mins.

### Scenario A: Sequential Execution (No Pipeline)

You put Load 1 in. Wait 90 mins. Finish.
Then put Load 2 in. Wait 90 mins. Finish.

- **Time for 4 loads**: 4 \* 90 = **360 mins**.

### Scenario B: Pipelined Execution

1. **T=0**: Load 1 enters Wash.
2. **T=30**: Load 1 moves to Dry. **Load 2 enters Wash.**
3. **T=60**: Load 1 to Fold. Load 2 to Dry. **Load 3 to Wash.**

- **Time for 4 loads**: First one takes 90. The rest pop out every 30 mins!
- Total time: ~180 mins. **2x Speedup!**

## The CPU Pipeline

A standard RISC-V CPU breaks execution into 5 stages (The 5-Stage Pipeline):

1.  **IF (Instruction Fetch)**: Get the bits from memory.
2.  **ID (Instruction Decode)**: Read registers, figure out "What is this?"
3.  **EX (Execute)**: Use the ALU (Math) or calculate address.
4.  **MEM (Memory)**: Read/Write from RAM (if needed).
5.  **WB (Write Back)**: Save result into Register.

### The ideal world

In a perfect world, we finish 1 instruction EVERY SINGLE CLOCK CYCLE (CPI = 1).
Even though each instruction takes 5 cycles to finish, we have 5 of them in flight at once.

## Key Research Concept: IPC (Instructions Per Cycle)

- High IPC = Efficient Architecture.
- Low IPC = Stalls/Bubbles (Factory stopped).

Your goal in research is often keeping the pipeline full.
