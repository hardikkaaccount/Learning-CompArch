# 1.4 Lab: Assembly Basics

## Your First Bare Metal Code

We will write a simple program: **Fibonacci Sequence**.
`0, 1, 1, 2, 3, 5, 8...`

### The Setup

Since we don't have a physical board yet, we use **[Venus Simulator](https://venus.cs.berkeley.edu/)**.

1. Open Venus in your browser.
2. Go to the "Editor" tab.

### The Code

Copy/Paste this into the simulator. Read the comments!

```asm
# Fibonacci Generator in RISC-V Assembly
# Target: Calculate the 7th Fibonacci number
# x1 (ra) = 0
# t0 = n (counter)
# t1 = Current number (Fib n)
# t2 = Previous number (Fib n-1)
# t3 = Temp storage

.data
    # This section is for static data (variables)
    result_msg: .string "Final Fibonacci Value: "

.text
    # This section is for instructions
    .globl main

main:
    # Initialize values
    li t0, 7        # Load Immediate: Calculate 7th fib number
    li t1, 1        # Current = 1
    li t2, 0        # Prev = 0
    li t4, 1        # Loop counter i = 1

loop:
    beq t4, t0, end # If i == 7, go to end (Branch Equal)

    # Logic: next = current + prev
    #        prev = current
    #        current = next

    add t3, t1, t2  # t3 = current + prev
    mv  t2, t1      # prev = current (Move is pseudo for ADDI t2, t1, 0)
    mv  t1, t3      # current = new_sum

    addi t4, t4, 1  # i++
    j loop          # Jump back to start of loop

end:
    # t1 now holds the answer (13)
    # Let's verify by just inspecting usage, or ecall to print (OS specific)

    # Venus specific instruction to print integer in a0
    mv a0, t1       # Move result to argument register a0
    li a7, 1        # System call code for "Print Int" is 1
    ecall           # Wake up OS to do the print

    # Exit program
    li a7, 10       # Exit code
    ecall
```

### Explanation of Steps

1. **Directives**: `.text` means "Code lives here". `.globl` means "Accessible from outside".
2. **Loads**: `li` (Load Immediate) is actually a "Pseudo-instruction". The assembler turns it into `addi x, x0, val`.
3. **The Loop**: We use a label `loop:` to mark a spot. `j loop` jumps there.
4. **Conditional**: `beq` (Branch if Equal) is our `if` statement.
5. **System Calls (`ecall`)**: This is how we ask the "OS" (Simulator) to print. In bare metal, we wouldn't use this; we'd write to a specific memory address to blink a light.

### Challenges

1. Modify the code to calculate the 10th number.
2. Step through line-by-line using the "Simulator" tab. Watch the `t` registers change.
3. **Hard Mode**: Write a program that sums numbers from 1 to 10 (`1 + 2 + ... + 10`).

## 🔬 Compare with C (Godbolt)

Want to cheat? Write the code in C and see what the compiler does.

1. Open **[Godbolt](https://godbolt.org/)**.
2. Type:
   ```c
   int fib(int n) {
       int a=0, b=1, sum=0;
       for(int i=1; i<n; i++) {
           sum = a+b; a=b; b=sum; // Look familiar?
       }
       return a;
   }
   ```
3. See the generated assembly. It will look surprisingly similar to yours!

---

_> Next Module: The hardware pipeline that executes this code._

---

## Navigation

[< Previous](./03_Instruction_Formats.md) | [Home](../README.md) | [Next >](../02_Microarchitecture/README.md)
