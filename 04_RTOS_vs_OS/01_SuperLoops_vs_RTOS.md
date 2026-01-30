# 4.1 SuperLoops vs RTOS: The Evolution of Loops

## 1. The Super Loop (Beginner)

The simplest way to write firmware.

```c
void main() {
    while(1) {
        check_buttons();   // 1ms
        update_sensors();  // 5ms
        blink_led();       // 0.1ms
    }
}
```

**Pros**: Dead simple.
**Cons**:

- **Latency**: If `update_sensors()` freezes for 1 second, the buttons stop working.
- **Timing**: It is impossible to say "Run `blink_led` exactly every 10ms". The timing depends on how long the other functions take.

## 2. The Interrupt-Driven Loop (Intermediate)

We use a hardware timer to fire an interrupt every 1ms.

- **Main**: Does background work.
- **ISR**: Updates "Critical" things.
  **Cons**: If the ISR gets too complex, it blocks the main loop too much.

## 3. The Real-Time Operating System (RTOS) (Advanced)

We blindly create "Tasks" (like threads).

- Task A: "Check Buttons" (High Priority).
- Task B: "Process Video" (Low Priority).

The **Scheduler** (The Boss) pauses Task B instantly when Task A needs to run.
**Guarantee**: "Task A will ALWAYS wake up within 10 microseconds of a button press, no matter what Task B is doing."

### Research Key: Hard Real-Time

- **General OS (Windows/Linux)**: "I'll try my best." (Soft Real-Time).
- **RTOS (FreeRTOS/Zephyr)**: "I promise, or I will die trying." (Hard Real-Time).
  _Critical for:_ Airbags, Pacemakers, Drone Flight Controllers.

---
## Navigation
[< Previous](./README.md) | [Home](../README.md) | [Next >](./02_Context_Switching.md)
