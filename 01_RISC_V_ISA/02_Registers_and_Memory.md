# 1.2 Registers & Memory: The Workbench

## The Analogy: The Carpenter's Workshop

To understand a CPU, imagine a carpenter at a workbench.

1.  **The Workbench (Registers)**:
    - Small table right in front of the carpenter.
    - **Speed**: Instant access.
    - **Capacity**: Tiny. Only fits 32 tools/items.
    - **Rule**: All work (sawing, nailing) happens HERE.

2.  **The Warehouse (RAM/Memory)**:
    - Huge shelving unit across the room.
    - **Speed**: Slow. Carpenter has to walk over to get stuff.
    - **Capacity**: Massive (Billions of items).
    - **Rule**: Storage only. You don't saw wood while it's sitting on the shelf.

## The RISC-V Registers

RISC-V has **32 General Purpose Registers** (x0 - x31). Each is 32-bits (or 64-bits) wide.
You _must_ memorize the special ones.

| Register           | Name    | Role                                                                 | "The Carpenter" Analogy                      |
| :----------------- | :------ | :------------------------------------------------------------------- | :------------------------------------------- |
| **x0**             | `zero`  | **Hardwired Zero**. Always holds 0. Writing to it does nothing.      | Highly efficient trash can / An empty hand.  |
| **x1**             | `ra`    | **Return Address**. Where to go back to after a function call.       | A sticky note saying "Go back to bench 4".   |
| **x2**             | `sp`    | **Stack Pointer**. Points to the current bottom of the stack memory. | The bookmark in your current project manual. |
| **x5-x7, x28-x31** | `t0-t6` | **Temporaries**. Scratch space. Can be overwritten by others.        | Scrap paper.                                 |
| **x10-x17**        | `a0-a7` | **Arguments/Return Values**. Function inputs and outputs.            | The "Inbox" and "Outbox" trays.              |

### The "Zero" Register (x0)

Why have a register that is always 0?
Because it simplifies the hardware!

- Want to clear a register? `ADD x1, x0, x0` (x1 = 0 + 0)
- Want to move a value? `ADD x1, x2, x0` (x1 = x2 + 0)
  RISC principles: Don't make a "MOVE" instruction if "ADD 0" works.

## Memory Addressing

Memory is just a giant array of bytes. `byte[] memory`.
Every byte has an address (Index).

- **Word**: 4 bytes (32 bits). This is the standard chunk size for RISC-V 32.
- **Byte Addressable**: Each address points to 1 byte.
- **Endianness**: RISC-V is Little-Endian (Least significant byte at the lowest address).

### Key Concept: The Stack

The Stack is a region of RAM used for function local variables.

- It grows **downwards** (from high address to low address).
- `sp` (Stack Pointer) keeps track of the bottom.
- When you enter a function: Decrement `sp` (make space).
- When you leave a function: Increment `sp` (clean up).

---
## Navigation
[< Previous](./01_Mental_Model.md) | [Home](../README.md) | [Next >](./03_Instruction_Formats.md)
