# 2.3 Memory Hierarchy: The Speed Gap

## The Logic vs Memory Gap

Over the last 40 years:

- **CPUs** got 1000x faster.
- **RAM** got larger, but only slightly faster (latency-wise).

If a CPU has to wait for RAM every time it needs data, it would spend 99% of its time waiting.
(Like a Ferrari stuck in traffic).

## The Hierarchy

1.  **Registers**: Instant. (0 cycles latency).
2.  **L1 Cache (Level 1)**: Small (32KB). Inside the core. Fast (1-4 cycles).
3.  **L2 Cache**: Bigger (256KB). Shared. Slower (10-20 cycles).
4.  **Main Memory (DRAM)**: Huge (16GB). Very Slow (100-300 cycles!).
5.  **Disk/SSD**: Glacial. (Millions of cycles).

## Key Concept: Locality

Why do Caches work? Because programs are predictable.

1.  **Temporal Locality** (Time):
    - If you used a variable `x` now, you'll probably use it again soon.
    - _Action_: Keep `x` in L1 Cache.

2.  **Spatial Locality** (Space):
    - If you accessed `array[0]`, you'll probably access `array[1]` next.
    - _Action_: When fetching `array[0]`, fetch the whole "Cache Line" (64 bytes) so `array[1..15]` are already there!

## The Cache Miss

- **Hit**: Data is in cache. Fast!
- **Miss**: Data not in cache. CPU must stall and fetch from RAM.
- **Miss Penalty**: The time wasted waiting.

**ML Research Note**: Matrix Multiplication is heavily optimized to maximize "Spatial Locality" (Blocking/Tiling) to avoid cache misses.

## 🎓 Deep Dive Resource

- **[Onur Mutlu: ETH Zurich Lectures](https://www.youtube.com/@OnurMutluLectures)**: If you want to research DRAM/Flash memory, this channel is the Gold Standard.
  - _Start with_: "Memory Systems and Memory Hierarchy".

---

## Navigation

[< Previous](./02_Hazards_and_Stalls.md) | [Home](../README.md) | [Next >](./04_Lab_Pipeline_Sim.md)
