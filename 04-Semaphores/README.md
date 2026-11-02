# STM32 FreeRTOS – Semaphores

## Overview

This project demonstrates the use of **Semaphores in RTOS** using STM32 and CMSIS-RTOS2 APIs.

The objective is to understand how multiple tasks coordinate their execution using semaphores, how tasks wait for and release a semaphore, and how task priorities affect scheduling.

The project uses three tasks with different execution priorities and a binary semaphore to control access to a simulated shared resource.

## Hardware & Software

- **Microcontroller:** STM32 Nucleo-F103RB
- **IDE:** STM32CubeIDE
- **RTOS:** FreeRTOS
- **API:** CMSIS-RTOS2
- **Communication:** USART2 UART (115200 baud)
- **Language:** Embedded C

## Concepts Covered

- Introduction to RTOS Semaphores
- Binary Semaphores
- Counting Semaphores
- Semaphore creation and initialization
- Semaphore Acquire and Release
- Task synchronization
- Task priorities and scheduling
- Semaphore contention
- Introduction to Priority Inversion

## Semaphore Configuration

### 1. Binary Semaphore

```c
myBinarySem01Handle =
    osSemaphoreNew(1, 1, &myBinarySem01_attributes);
```

Configuration:

- Maximum count: 1
- Initial count: 1

The binary semaphore is initially available.

Tasks acquire the semaphore before accessing the simulated shared resource and release it after completing their work.

**APIs used:**

- `osSemaphoreNew()` – Creates a semaphore.
- `osSemaphoreAcquire()` – Acquires or waits for a semaphore.
- `osSemaphoreRelease()` – Releases a semaphore.

### 2. Counting Semaphore

```c
myCountingSem01Handle =
    osSemaphoreNew(2, 2, &myCountingSem01_attributes);
```

Configuration:

- Maximum count: 2
- Initial count: 2

This creates a counting semaphore with two available tokens.

It is included to understand how counting semaphores represent multiple available resources. It is created but not actively used by the current tasks.

## Task Configuration

Three tasks are created with the following priorities:

| Task | Priority | Function |
|---|---|---|
| LPT | Normal | Acquires the binary semaphore and simulates resource usage |
| MPT | Normal | Executes a busy-wait loop without acquiring the semaphore |
| HPT | Above Normal | Acquires and releases the binary semaphore |

### Task Execution

**LPT – Low Priority Task**
- Attempts to acquire the binary semaphore.
- Accesses the simulated shared resource.
- Executes a busy-wait loop.
- Releases the semaphore.
- Enters a 500 ms delay.

**MPT – Medium Priority Task**
- Executes independently without acquiring the semaphore.
- Performs a busy-wait loop.
- Enters a 500 ms delay.

**HPT – High Priority Task**
- Attempts to acquire the binary semaphore.
- Waits indefinitely if the semaphore is unavailable.
- Releases the semaphore after acquiring it.
- Enters a 200 ms delay.

## UART Output

USART2 is configured at 115200 baud to monitor task execution.

The `printf()` function is redirected to UART using `__io_putchar()` and `HAL_UART_Transmit()`.

UART messages help observe:

- Which task is executing.
- When a task attempts to acquire a semaphore.
- When a task accesses the simulated resource.
- When a semaphore is released.
- When a task enters its delay period.

## Priority Inversion

Priority inversion is an important RTOS scheduling problem that occurs when a high-priority task is indirectly delayed by a lower-priority task holding a shared resource.

This project introduces semaphore contention between HPT and LPT and provides a starting point for understanding priority inversion.

**Important:** The current configuration uses a binary semaphore, with LPT and MPT both at Normal priority. It does not yet demonstrate the complete classic priority inversion scenario.

A classic demonstration requires three distinct priorities:

- HPT – High priority
- MPT – Medium priority
- LPT – Low priority

When LPT holds a resource, HPT waits for it, and MPT preempts LPT, the high-priority task experiences indirect delay.

A mutex with priority inheritance can help prevent this situation.

## Learning Outcome

Through this project, I explored:

- How binary and counting semaphores are created.
- How tasks synchronize using semaphore acquire and release operations.
- How a task becomes blocked while waiting for a semaphore.
- How task priorities influence RTOS scheduling.
- How semaphore contention can delay task execution.
- The difference between semaphores and mutexes, including priority inheritance.

## Future Improvements

- Implement a complete priority inversion demonstration.
- Compare binary semaphore and mutex behavior.
- Implement priority inheritance using a mutex.
- Explore semaphore signaling from interrupts using ISR-safe APIs.
- Implement counting semaphore-based resource management.

---

**Project:** STM32 FreeRTOS Basics  
**Topic:** Semaphores  
**Framework:** CMSIS-RTOS2
