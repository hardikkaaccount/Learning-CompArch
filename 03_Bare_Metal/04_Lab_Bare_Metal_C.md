# 3.4 Lab: Bare Metal C (Write a Driver)

## Goal

We will write a "Driver" for a fake Serial Port (UART) in our simulator.
This is exactly how you would do it on physical hardware.

## The Hardware Spec (Hypothetical)

- **Device**: UART (Universal Asynchronous Receiver-Transmitter).
- **Base Address**: `0x10000000`
- **Register Offset 0x00**: Data Register (Write char here to transmit).
- **Register Offset 0x05**: Status Register (Bit 0 is "Tx Ready").

## The Driver Code `uart.c`

```c
#include <stdint.h>

// 1. Define the Hardware Layout
// We use a struct to model the memory layout of the device
typedef struct {
    volatile uint8_t DR;     // 0x00: Data Register
    volatile uint8_t _pad1[4]; // Skip 0x01-0x04
    volatile uint8_t SR;     // 0x05: Status Register
} UART_Device;

// 2. Map the pointer to the Base Address
#define UART_BASE 0x10000000
UART_Device* uart = (UART_Device*) UART_BASE;

// 3. Bit Definitions
#define TX_READY_BIT (1 << 0) // Bit 0

// 4. The Driver Function
void uart_putc(char c) {
    // Wait until hardware is ready (Polling)
    // While (SR & 1) == 0 ... wait.
    while ((uart->SR & TX_READY_BIT) == 0);

    // Send the character
    uart->DR = c;
}

void uart_puts(const char* str) {
    while (*str) {
        uart_putc(*str++);
    }
}

// 5. App
void main() {
    uart_puts("Hello Bare Metal World!\n");
    // We can't return from main in bare metal! nowhere to go.
    while(1);
}
```

## Exercise

1.  Study the pointer logic. Why `volatile`? (See Lesson 3.1).
2.  Why the `while(1)` at the end?
3.  **Thought Experiment**: If `uart_puts` takes 1ms per character, and you are printing a 1000 char log, the CPU is blocked for 1 second. How would you fix this using Interrupts? (Ring Buffers).

## 🛠️ Tools to Try

Since you might not have a physical board, use these:

- **[Wokwi Simulator](https://wokwi.com/projects/new/esp32-c3)**: Simulate an ESP32-C3 (RISC-V) right in your browser. You can write the C code above and see it run!
- **[PlatformIO](https://platformio.org/)**: The best VS Code extension for embedded development. Use this when you get real hardware.

---

## Navigation

[< Previous](./03_The_Boot_Process.md) | [Home](../README.md) | [Next >](../04_RTOS_vs_OS/README.md)
