# 3.2 Interrupts: Don't Call Us, We'll Call You

## Polling vs Interrupts

You are the CPU. You want to know if a button was pressed.

### Method 1: Polling (The Nag)

"Is button pressed? No."
"Is button pressed? No."
"Is button pressed? No."
...
**Pros**: Simple.
**Cons**: Wastes 100% of CPU time doing nothing useful.

### Method 2: Interrupts (The Doorbell)

"I'm going to do math. Tap me on the shoulder when the button is pressed."
**Pros**: CPU sleeps or works on other things.
**Cons**: Complex logic. "Wait, I was in the middle of something!"

## The Interrupt Service Routine (ISR)

When the hardware "Interrupt Request" (IRQ) line goes HIGH:

1.  **Hardware**: Stops the current instruction stream.
2.  **Hardware**: Saves the PC (Program Counter) into a special register (`mepc` in RISC-V).
3.  **Hardware**: Jumps to a specific address defined in the **Vector Table**.
4.  **Software (ISR)**: The function at that address runs.
    - Ack the interrupt ("I heard you!").
    - Do the work (Read the button).
    - Restore registers.
    - `mret` (Return from Machine Mode Exception).
5.  **Hardware**: Jumps back to where it left off.

## The Hard Part: Concurrency

What if an interrupt happens _while you are handling another interrupt_?

- **Nested Interrupts**: Allowed? Risky.
- **Race Conditions**: If the Main Loop and the ISR both try to update `counter++`, you get data corruption.
- **Critical Sections**: Disabling interrupts briefly to update shared data safely.

---
## Navigation
[< Previous](./01_Memory_Mapped_IO.md) | [Home](../README.md) | [Next >](./03_The_Boot_Process.md)
