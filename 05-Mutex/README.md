# FreeRTOS Recursive Mutex – STM32

## Overview

This project demonstrates the implementation of a **Recursive Mutex using CMSIS-RTOS2 on STM32**.

A recursive mutex allows the same task to acquire the same mutex multiple times without blocking itself. It is useful when functions call other functions that also need access to a shared resource protected by the same mutex.

This project explores recursive mutex creation, acquisition, release, and nested function calls.

## Concepts Covered

- Introduction to Recursive Mutex
- Difference between Normal Mutex and Recursive Mutex
- Recursive Mutex creation using `osMutexNew()`
- `osMutexRecursive` attribute
- Mutex acquisition using `osMutexAcquire()`
- Mutex release using `osMutexRelease()`
- Nested function calls
- Mutex ownership and acquisition count

## Hardware and Software Requirements

**Hardware**
- STM32 Nucleo-F103RB development board
- USB connection for UART communication

**Software**
- STM32CubeIDE
- FreeRTOS
- CMSIS-RTOS2
- Serial terminal (PuTTY / Tera Term)

## Project Configuration

### Task Configuration

| Task | Priority | Function |
|---|---|---|
| LPT | Below Normal | Calls FuncA() |
| MPT | Normal | Periodic delay |
| HPT | Above Normal | Periodic delay |
| VHPT | High | Periodic delay |

### Mutex Creation

The project creates both a normal mutex and a recursive mutex.

**Normal Mutex:**

```c
osMutexId_t myMutex01Handle;

const osMutexAttr_t myMutex01_attributes = {
    .name = "myMutex01"
};
```

**Recursive Mutex:**

```c
osMutexId_t myRecursiveMutex01Handle;

const osMutexAttr_t myRecursiveMutex01_attributes = {
    .name = "myRecursiveMutex01",
    .attr_bits = osMutexRecursive
};
```

The `osMutexRecursive` attribute configures the mutex to allow repeated acquisition by its current owner.

## Implementation

### Function A

`FuncA()` acquires the normal mutex and then calls `FuncB()`.

```c
void FuncA(void)
{
    printf("Func A entered mutex\r\n");

    osMutexAcquire(myMutex01Handle, osWaitForever);

    printf("Func A acquired mutex\r\n");

    FuncB();

    osMutexRelease(myMutex01Handle);

    printf("Func A released mutex\r\n");
}
```

### Function B

`FuncB()` acquires and releases the recursive mutex.

```c
void FuncB(void)
{
    printf("Func B entered mutex\r\n");

    osMutexAcquire(myRecursiveMutex01Handle, osWaitForever);

    printf("Func B acquired mutex\r\n");

    osMutexRelease(myRecursiveMutex01Handle);

    printf("Func B released mutex\r\n");
}
```

**Note:** In the current implementation, `FuncA()` and `FuncB()` use two different mutexes. Therefore, this code demonstrates separate mutex acquisition in nested function calls, but does not yet demonstrate recursive acquisition of the same mutex.

To demonstrate actual recursive mutex behavior, both functions must acquire and release `myRecursiveMutex01Handle`.

## Expected Learning Outcome

After completing this project, the following concepts should be understood:

1. A normal mutex cannot be recursively acquired by its current owner.
2. A recursive mutex allows the same task to acquire it multiple times.
3. Each successful recursive acquisition requires a corresponding release.
4. The mutex becomes available to other tasks only after all acquisitions have been released.
5. Recursive mutexes are useful in nested function calls where multiple functions require the same protected resource.

## Project Structure

```text
FreeRTOS-Recursive-Mutex/
│
├── Core/
│   ├── Inc/
│   └── Src/
│       └── main.c
│
└── README.md
```

## Conclusion

This project provides practical experience with mutex handling in FreeRTOS and introduces recursive mutexes as a synchronization mechanism for nested function calls.

It builds on normal mutex concepts and helps understand how RTOS manages shared resources and mutex ownership.
