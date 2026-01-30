# Module 4: The Manager (RTOS vs OS)

## Introduction

We now have hardware (Mod 1), a CPU engine (Mod 2), and basic drivers (Mod 3).
But what if we want to run _two_ things at once? Blink an LED **AND** read a sensor?
If the sensor reading hangs, does the LED stop blinking?

This module introduces the **Operating System (OS)**, specifically focusing on **Real-Time Operating Systems (RTOS)** which are crucial for embedded/robotics research.

## Learning Objectives

1.  **Concurrency**: The illusion of doing multiple things at once.
2.  **Scheduling**: How to decide who gets the CPU.
3.  **Context Switching**: The magic trick of saving/restoring registers.
4.  **Synchronization**: preventing chaos when two tasks access the same variable.

## Prerequisites

- Module 3 (Interrupts).
- Understanding that the CPU has only one set of registers.

---

_Proceed to `01_SuperLoops_vs_RTOS.md`._

---
## Navigation
[< Previous](../03_Bare_Metal/04_Lab_Bare_Metal_C.md) | [Home](../README.md) | [Next >](./01_SuperLoops_vs_RTOS.md)
