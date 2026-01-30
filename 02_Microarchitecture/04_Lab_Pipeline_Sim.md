# 2.4 Lab: Pipeline Simulation (Visualizing the Flow)

Since we cannot see electricity, we use a simulator to visualize the pipeline stages.

## Tool: RIPES

**Ripes** is a visual processor simulator for RISC-V. It shows data moving through wires!
_Download_: [GitHub - Ripes](https://github.com/mortbopet/Ripes) (It's a standalone AppImage/Exe).
_Note_: If you can't download it now, watch a video of it in action, but installing it is highly recommended.

## The Exercise

1.  Open Ripes.
2.  Select **Processor**: "5-stage Processor" (Standard).
3.  Paste the Assembly code from Module 1 (Fibonacci) or simply:
    ```asm
    addi t0, zero, 10
    addi t1, zero, 20
    add  t2, t0, t1
    ```
4.  **Step through clock cycles**.

### Observations to make:

1.  **Cycle 1**: instruction `addi t0...` is in **IF** stage.
2.  **Cycle 2**: `addi t0...` moves to **ID** stage. `addi t1...` enters **IF**.
3.  **Cycle 3**: Watch the registers!
4.  **Data Hazard**:
    - Change the code to:
      ```asm
      addi t0, zero, 10
      add  t1, t0, zero  # Uses t0 immediately!
      ```
    - Watch carefully. Does the processor "Stall"? Or do you see a "Forwarding" line light up (red wire) sending the 10 from EX stage back to ID?

## Research Question

If you have a customized ML accelerator (TPU), it doesn't have a 5-stage pipeline like this. It uses a **Systolic Array**.

- _Task_: Google "Systolic Array Animation" to see how it differs from the scalar pipeline you just watched. It flows like a wave!
