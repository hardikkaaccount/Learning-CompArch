# Zero to Research: Electronics & Computer Architecture

## Course Syllabus

Welcome to your comprehensive roadmap. We are building your understanding from the ground up.
Each module contains structured reading, visual references, and hands-on labs.

# [Click Here to Start Course](./01_RISC_V_ISA/README.md)

---

### Module 1: The Language of Machinery (RISC-V ISA)

_Goal: Understand how software talks to hardware._

- **[01_Mental_Model.md](./01_RISC_V_ISA/01_Mental_Model.md)**: CISC vs RISC, The Translation Layer.
- **[02_Registers_and_Memory.md](./01_RISC_V_ISA/02_Registers_and_Memory.md)**: The CPU's workspace vs Storage.
- **[03_Instruction_Formats.md](./01_RISC_V_ISA/03_Instruction_Formats.md)**: Decoding the binary patterns.
- **[04_Lab_Assembly_Basics.md](./01_RISC_V_ISA/04_Lab_Assembly_Basics.md)**: Writing your first assembly program.

### Module 2: The Factory Floor (Microarchitecture)

_Goal: Understand how the CPU executes instructions fast._

- **[01_Pipeline_Basics.md](./02_Microarchitecture/01_Pipeline_Basics.md)**: Fetch, Decode, Execute.
- **[02_Hazards_and_Stalls.md](./02_Microarchitecture/02_Hazards_and_Stalls.md)**: When the factory gets stuck.
- **[03_Memory_Hierarchy.md](./02_Microarchitecture/03_Memory_Hierarchy.md)**: Caches, RAM, and Speed.
- **[04_Lab_Pipeline_Sim.md](./02_Microarchitecture/04_Lab_Pipeline_Sim.md)**: Visualizing instruction flow.

### Module 3: No Safety Net (Bare Metal Programming)

_Goal: Control hardware directly without an OS._

- **[01_Memory_Mapped_IO.md](./03_Bare_Metal/01_Memory_Mapped_IO.md)**: Talking to the outside world.
- **[02_Interrupts_and_Polling.md](./03_Bare_Metal/02_Interrupts_and_Polling.md)**: Handling events.
- **[03_The_Boot_Process.md](./03_Bare_Metal/03_The_Boot_Process.md)**: What happens when power hits?
- **[04_Lab_Bare_Metal_C.md](./03_Bare_Metal/04_Lab_Bare_Metal_C.md)**: Writing a driver from scratch.

### Module 4: The Manager (RTOS vs OS)

_Goal: Managing time and resources._

- **[01_SuperLoops_vs_RTOS.md](./04_RTOS_vs_OS/01_SuperLoops_vs_RTOS.md)**: Why we need schedulers.
- **[02_Context_Switching.md](./04_RTOS_vs_OS/02_Context_Switching.md)**: Saving and restoring state.
- **[03_Concurrency_Issues.md](./04_RTOS_vs_OS/03_Concurrency_Issues.md)**: Deadlocks and Race Conditions.
- **[04_Lab_RTOS_Concepts.md](./04_RTOS_vs_OS/04_Lab_RTOS_Concepts.md)**: Designing a simple scheduler. 

---

_> "You can't understand the cloud if you don't understand the transistor."_
