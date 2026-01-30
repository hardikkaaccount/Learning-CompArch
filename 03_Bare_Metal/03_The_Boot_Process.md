# 3.3 The Boot Process: The First Breath

## The Reset Vector

When you power on a chip, there is chaos in the registers. Random 1s and 0s.
The hardware has one guarantee: **The Program Counter (PC) is set to a specific fixed number.**

- On many RISC-V chips: `0x1000` or `0x80000000`.

The cpu _blindly_ fetches the instruction at that address.

## The Bootloader Stages

1.  **Stage 0 (Mask ROM)**: "Baked in" code on the silicon. Unchangeable. It checks "Is there code in Flash memory?"
2.  **Stage 1 (Flash Boot)**: Sets up the Stack Pointer (`sp`). (Remember, C needs a stack, but the stack pointer is random at boot!).
    - Zeros out the `.bss` section (all those global variables = 0).
    - Copies data from Flash to RAM (if needed).
3.  **Jump to Main**: Finally, it calls `main()`.

## Linker Scripts (`.ld`)

How does the GCC compiler know that "Flash is at 0x80000000"?
It doesn't. You must tell it.
A **Linker Script** is a map:

```ld
MEMORY {
    FLASH (rx) : ORIGIN = 0x80000000, LENGTH = 128K
    RAM (rwx)  : ORIGIN = 0x20000000, LENGTH = 16K
}
SECTIONS {
    .text : { *(.text) } > FLASH   /* Code goes to Flash */
    .data : { *(.data) } > RAM     /* Variables go to RAM */
}
```

**Research Insight**: If you are designing a new AI chip, YOU write this map. You decide where memory lives.

---
## Navigation
[< Previous](./02_Interrupts_and_Polling.md) | [Home](../README.md) | [Next >](./04_Lab_Bare_Metal_C.md)
