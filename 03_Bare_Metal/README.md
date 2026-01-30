# Module 3: No Safety Net (Bare Metal)

## Introduction

In the previous modules, we lived in a clean, simulated world.
Now we enter the **Real World**.

In Bare Metal programming, there is no Operating System (No Linux, No Windows).

- `printf()`? Doesn't exist (unless you write it).
- File Systems? Don't exist.
- Memory Protection? Nope. You can overwrite your own code and crash the CPU instantly.

## Learning Objectives

1.  **Memory Mapped I/O (MMIO)**: How to make electricity move using pointers.
2.  **Interrupts**: How hardware talks back to software.
3.  **Booting**: The first instruction that runs on power-up.
4.  **Drivers**: Writing code to control a specific piece of silicon.

## Prerequisites

- C Pointers (Understanding `*ptr = 0x1234`).
- Hexadecimal notation.

---

_Proceed to `01_Memory_Mapped_IO.md`._
