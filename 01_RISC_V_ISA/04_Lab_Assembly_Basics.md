# 1.4 Lab: Assembly Basics

## Your First Bare Metal Code

We will write a simple program: **Fibonacci Sequence**.
`0, 1, 1, 2, 3, 5, 8...`

### The Setup

Since we don't have a physical board yet, we use **[Venus Simulator](https://venus.cs61c.org/)**.

1. Open Venus in your browser.
2. Go to the "Editor" tab.

### The Code

Copy/Paste this into the simulator. Read the comments!

```asm
.data
    result_msg: .string "Final Fibonacci Value: "

.text
    .globl main
main:
    li t0, 7
    li t1, 1
    li t2, 0
    li t4, 1
loop:
    beq t4, t0, end
    add t3, t1, t2
    mv  t2, t1
    mv  t1, t3
    addi t4, t4, 1
    j loop
end:
    # 1. Print the string
    li a0, 4           # Venus ecall 4 = print string
    la a1, result_msg  # Load address into a1
    ecall

    # 2. Print the integer result
    li a0, 1           # Venus ecall 1 = print integer
    mv a1, t1          # Move result (t1) into a1
    ecall

    # 3. Terminate program
    li a0, 10          # Venus ecall 10 = exit
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

## 🔌 Bonus: Bare-Metal Style on Arduino

For those interested in how this low-level access looks in C/C++ on a microcontroller (like Arduino), here is an example of accessing hardware registers directly (memory-mapped I/O) instead of using high-level libraries.

```cpp
#include <Arduino.h>
#include <stdint.h>

// 1. Define the Hardware Layout
typedef struct {
    volatile uint8_t DR;       // 0x00: Data Register
    volatile uint8_t _pad1[4]; // 0x01–0x04
    volatile uint8_t SR;       // 0x05: Status Register
} UART_Device;

// 2. Base Address (example / hypothetical)
#define UART_BASE 0x10000000
#define TX_READY_BIT (1 << 0)

UART_Device* uart = (UART_Device*)UART_BASE;

// 3. Low-level UART functions
void uart_putc(char c) {
    // Wait until transmitter is ready
    while ((uart->SR & TX_READY_BIT) == 0);
    uart->DR = c;
}

void uart_puts(const char* str) {
    while (*str) {
        uart_putc(*str++);
    }
}

// 4. Arduino entry points
void setup() {
    uart_puts("Hello from Arduino bare-metal style!\n");
}

void loop() {
    // Nothing to do
    while (1);
}
```

---

_> Next Module: The hardware pipeline that executes this code._

---

## Navigation

[< Previous](./03_Instruction_Formats.md) | [Home](../README.md) | [Next >](../02_Microarchitecture/README.md)
