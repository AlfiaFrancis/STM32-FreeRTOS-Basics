# STM32 FreeRTOS Basics

A hands-on learning repository focused on understanding Real-Time Operating Systems (RTOS) using STM32 microcontrollers, Embedded C, FreeRTOS, and CMSIS-RTOS APIs.

This repository documents my step-by-step learning journey, starting with basic task creation and scheduling and progressing toward advanced RTOS concepts through practical embedded firmware projects.

## Hardware and Development Tools

- **Microcontroller Board:** STM32 Nucleo-F103RB
- **IDE:** STM32CubeIDE
- **Programming Language:** Embedded C
- **RTOS:** FreeRTOS
- **RTOS API:** CMSIS-RTOS2
- **Hardware Abstraction:** STM32 HAL
- **Communication Interface:** UART / USART

## Project Structure

Each folder contains an individual project with its source code and a dedicated README explaining the implementation and concepts covered.

| Project | Concepts Covered |
|---|---|
| Task 01 – LED Blinking | GPIO control, task creation, periodic delays |
| Task 02 – Multiple Tasks | Multiple task execution, task priorities, `osDelay()`, UART transmission, task suspension and resumption |
| Upcoming Projects | Additional RTOS concepts and practical implementations |

## RTOS Learning Roadmap

### 1. RTOS Fundamentals
- What is an RTOS and why it is used
- Bare-metal programming vs RTOS-based programming
- Tasks, threads and the scheduler
- Real-time systems and deterministic behaviour

### 2. CMSIS-RTOS2 Basics
- Creating tasks using `osThreadNew()`
- Task attributes and priorities
- Task states and transitions
- Task delays using `osDelay()`
- Suspending and resuming tasks
- Starting and managing the RTOS kernel

### 3. Task Scheduling and Timing
- Preemptive scheduling
- Equal-priority task behaviour
- Time slicing and CPU utilisation
- Periodic task execution
- Tick rate and timing accuracy
- Stack allocation and task stack usage

### 4. Task Communication and Synchronisation
- Queues
- Binary and counting semaphores
- Mutexes and shared resource protection
- Event flags
- Thread flags
- Race conditions and priority inversion

### 5. Interrupts and RTOS Integration
- Interrupt Service Routines (ISRs)
- Interrupt priorities and RTOS restrictions
- Communicating between interrupts and tasks
- UART communication with RTOS tasks
- DMA integration with RTOS-based applications

### 6. Advanced RTOS Concepts
- Software timers
- Memory management
- Static and dynamic task allocation
- Watchdog integration
- Deadlocks and debugging
- Real-time system design and performance optimisation

## Learning Approach

Each project is developed to understand the underlying concept through practical implementation, testing, debugging and documentation. New topics will be added progressively as I gain experience with FreeRTOS and CMSIS-RTOS2.

## Goal

To build a strong foundation in RTOS-based embedded firmware development and apply these concepts to real-world microcontroller applications.

