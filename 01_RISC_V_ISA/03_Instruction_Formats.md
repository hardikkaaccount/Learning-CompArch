# 1.3 Instruction Formats: The Binary DNA

## How Hardware Reads

We write `ADD x1, x2, x3`.
The computer sees: `0000000 00011 00010 000 00001 0110011`

This 32-bit chunk tells the electrical gates exactly what to do. RISC-V is brilliant because these formats are extremely regular. This makes the hardware decoder simple.

## The 6 Core Formats

You don't need to memorize the binary, but you MUST understand the **types**.

### 1. R-Type (Register)

_Usage: Math between two registers._
`ADD x1, x2, x3` (x1 = x2 + x3)

- **Fields**: [Source Reg 1] [Source Reg 2] [Dest Reg] [Opcode]
- The most "pure" RISC instruction.

### 2. I-Type (Immediate)

_Usage: Math with a small constant numbers (Immediate)._
`ADDI x1, x2, 10` (x1 = x2 + 10)

- **Fields**: [12-bit Constant] [Source Reg] [Dest Reg] [Opcode]
- Note: We don't need a register for the number "10". It's embedded in the instruction itself!

### 3. S-Type (Store)

_Usage: Saving data from register to RAM._
`SW x2, 10(x1)` (Store Word: Mem[x1 + 10] = x2)

- **Fields**: [High Constant] [Source Data] [Source Addr] [Low Constant] [Opcode]
- Weirdness: The immediate constant is split in two! This keeps register fields in the same place as R-Type (Hardware simplification).

### 4. B-Type (Branch)

_Usage: If/Else conditional jumps._
`BEQ x1, x2, label` (Branch if EQual: if x1 == x2, goto label)

- **Concept**: The "label" is actually a relative offset number. "Jump forward 4 lines".

### 5. U-Type (Upper Immediate)

_Usage: Loading big numbers._
`LUI x1, 0x12345` (Load Upper Immediate)

- loads the top 20 bits of a register. Why? Because I-Type only fits 12 bits. To load a big 32-bit number, you need two instructions (LUI + ADDI).

### 6. J-Type (Jump)

_Usage: Function calls and unconditional jumps._
`JAL x1, label` (Jump And Link)

- Jumps to label, and saves the current address into x1 (usually `ra`) so we can return later.

## Exercise: The "ADD" Decoding

Let's decode `ADD t0, t1, t2`.
(Assume `t0`=x5, `t1`=x6, `t2`=x7)

1. **Opcode** for Integer Math: `0110011`
2. **Function3/7**: Codes that say "This is ADD, not SUB".
3. **Rest**:
   - rd (Dest): 00101 (5)
   - rs1 (Source1): 00110 (6)
   - rs2 (Source2): 00111 (7)

**Hardware View**: The bits `00110` flow into the "Read Address" port of the Register File. The values flow out to the ALU. The opcode tells the ALU "Do Addition". The result flows back to Register `00101`.

---
## Navigation
[< Previous](./02_Registers_and_Memory.md) | [Home](../README.md) | [Next >](./04_Lab_Assembly_Basics.md)
