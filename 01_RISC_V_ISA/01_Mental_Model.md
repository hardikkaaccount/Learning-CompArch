# 1.1 The Mental Model: RISC vs CISC

## The Great Translation

Computers don't run C or Python. They run **Machine Code**.
Every fancy feature you use (Objects, Classes, Lambdas) must be crushed down into simple, stupid commands.

### The Philosophy: RISC vs CISC

Imagine you are designing a robot kitchen. You have two choices for the command list you give the robot.

#### 1. CISC (Complex Instruction Set Computer) - The "Master Chef" approach

_Examples: x86 (Intel/AMD)_
You give high-level, complex commands.

- **Command**: `MAKE_SANDWICH`
- **Hardware**: The robot arm is incredibly complex. It has to know what a sandwich is, where the bread is, how to slice it.
- **Pros**: Code size is small (one command!).
- **Cons**: Hardware is nightmareishly complex to build and power-hungry.

#### 2. RISC (Reduced Instruction Set Computer) - The "Subway Artist" approach

_Examples: RISC-V, ARM (Apple M1/M2)_
You give tiny, atomic commands.

- **Commands**:
  1. `GRAB_BREAD`
  2. `ADD_CHEESE`
  3. `ADD_HAM`
  4. `CLOSE_BREAD`
- **Hardware**: The robot arm is simple. It just knows how to "grab" and "move".
- **Pros**: Hardware is simple, fast, and power-efficient.
- **Cons**: Code size is larger (4 commands instead of 1).

**Research Reality**: RISC won. Modern x86 processors actually translate their complex commands into RISC-like micro-ops internally! RISC-V is the purest form of this philosophy.

## The RISC-V "Contract"

RISC-V is a **Load/Store Architecture**. This is the most critical concept to learn.

- **ALU (Math) operations** only happen in **Registers** (CPU hands).
- **Memory (RAM)** is only accessed via **Load** and **Store** instructions.

You CANNOT do `ADD [MemoryAddress], 5`.
You MUST do:

1. `LOAD` value from Memory to Register.
2. `ADD` 5 to Register.
3. `STORE` Register back to Memory.

This simplicity allows the hardware designers (us) to optimize the hell out of the CPU.
