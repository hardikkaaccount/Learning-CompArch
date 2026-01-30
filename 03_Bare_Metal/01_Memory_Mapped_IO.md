# 3.1 Memory Mapped I/O: The Magic Trick

## How do computers touch the real world?

Software is just data. How does "data" turn on a physical lightbulb?
Does the CPU have a special instruction like `TURN_ON_LIGHT`? **NO.**

To the CPU, **everything looks like RAM.**
Hardware designers perform a magic trick: They wire physical devices to appear at specific Memory Addresses.

## The Address Map

Imagine your Memory (RAM) addresses go from `0` to `1,000,000`.

- Addresses `0` to `900,000`: Real RAM (Data storage).
- Address `900,001`: **Not RAM**. It's a wire connected to an LED.
- Address `900,002`: **Not RAM**. It's a wire connected to a Motor.

This is **Memory Mapped I/O**.

### The C Code

To turn on the LED at address `900,001` (0xDBBA0):

```c
// 1. Create a pointer to that specific integer address
// "volatile" tells the compiler: "Don't optimize this!
// Even if I don't read it back, the WRITE is important (side effect)."
volatile int* led_ptr = (volatile int*) 0xDBBA0;

// 2. Write to it just like a variable
*led_ptr = 1; // LED Turns ON!
*led_ptr = 0; // LED Turns OFF!
```

## Control Registers vs Data Registers

Devices usually have multiple addresses (Registers):

1.  **Control Register (CR)**: Settings. "Set speed to 100".
2.  **Status Register (SR)**: Read-only. "Are we done yet?"
3.  **Data Register (DR)**: The actual payload.

**Research Note**: In AI Accelerators, you send the "Concept" of a Matrix Multiplication by writing the _address_ of your matrix to the accelerator's Control Register via MMIO.

---
## Navigation
[< Previous](./README.md) | [Home](../README.md) | [Next >](./02_Interrupts_and_Polling.md)
