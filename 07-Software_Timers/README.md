# STM32 FreeRTOS – Software Timers (CMSIS-RTOS)

## Overview

This project demonstrates the implementation of **Software Timers in FreeRTOS using the CMSIS-RTOS API** on the STM32 Nucleo-F103RB board.

Software timers allow an application to execute callback functions after a specified time interval without creating a separate task for every time-based operation.

This project demonstrates two types of software timers:

- **Periodic Timer:** Executes its callback repeatedly at a fixed interval.
- **One-Shot Timer:** Executes its callback only once after the configured timeout.

## Hardware and Software Requirements

### Hardware
- STM32 Nucleo-F103RB
- USB connection
- PC for serial monitoring

### Software
- STM32CubeIDE
- STM32 HAL Library
- FreeRTOS
- CMSIS-RTOS API
- PuTTY / Tera Term (UART monitoring)

## Hardware Configuration

| Peripheral | Configuration | Purpose |
|---|---|---|
| PA5 | GPIO Output | Onboard LED control |
| PC13 | GPIO Input, Pull-up | User button input |
| USART2 | 115200 baud rate | UART message transmission |

## Software Timer Configuration

Two software timers are created using `osTimerNew()`.

### 1. Periodic Timer

```c
PeriodicTimerHandle = osTimerNew(
    PeriodicCallback,
    osTimerPeriodic,
    NULL,
    &PeriodicTimer_attributes
);
```

- Timer type: `osTimerPeriodic`
- Period: 1000 ms
- Callback: `PeriodicCallback()`

The timer initially starts when `StartDefaultTask` begins execution.

It executes the callback every 1 second, printing a periodic status message through UART.

### 2. One-Shot Timer

```c
Oneshot_TimerHandle = osTimerNew(
    OneshotCallback,
    osTimerOnce,
    NULL,
    &Oneshot_Timer_attributes
);
```

- Timer type: `osTimerOnce`
- Timeout: 5000 ms
- Callback: `OneshotCallback()`

The one-shot timer starts when the user button (PC13) is pressed.

After 5 seconds, its callback executes once, turns OFF the LED, prints a UART message, and restarts the periodic timer.

## Working Principle

### Initial Operation

1. The RTOS scheduler starts.
2. `StartDefaultTask` begins execution.
3. The periodic timer starts with a 1000 ms period.
4. Every 1 second, `PeriodicCallback()` executes.
5. The callback prints a message through UART.

### Button Press Operation

When the user presses the onboard button connected to PC13:

1. The GPIO input becomes LOW because the input is configured with a pull-up.
2. The LED connected to PA5 turns ON.
3. The one-shot timer starts with a 5000 ms timeout.
4. The periodic timer is stopped using `osTimerStop()` if it is running.
5. After 5 seconds, `OneshotCallback()` executes.
6. The LED turns OFF.
7. A one-shot completion message is printed through UART.
8. The periodic timer restarts with a 1000 ms period.

## Important CMSIS-RTOS Timer APIs

| API | Description |
|---|---|
| `osTimerNew()` | Creates a software timer |
| `osTimerStart()` | Starts a timer with a specified period |
| `osTimerStop()` | Stops a running timer |
| `osTimerIsRunning()` | Checks whether a timer is running |
| `osTimerDelete()` | Deletes a timer |

### Timer Types

| Timer Type | Behaviour |
|---|---|
| `osTimerOnce` | Executes callback only once |
| `osTimerPeriodic` | Executes callback repeatedly |

## Concepts Demonstrated

- FreeRTOS software timer implementation.
- Difference between periodic and one-shot timers.
- Timer creation and initialization.
- Starting and stopping timers dynamically.
- Checking timer status using `osTimerIsRunning()`.
- Timer callback functions.
- Interaction between GPIO inputs, LED outputs, and software timers.
- UART debugging using `printf()` and `__io_putchar()`.

## Expected UART Output

Example output during operation:

```text
Periodic callback done
Periodic callback done
Periodic callback done

###ONESHOT###

Periodic callback done
Periodic callback done
```

The periodic messages stop while the periodic timer is inactive. After the one-shot timer expires, periodic messages resume.

## Learning Outcome

Through this project, I learned how FreeRTOS software timers can be used to perform time-based operations without creating additional application tasks.

I also explored how periodic and one-shot timers can be controlled dynamically based on external GPIO events.

**Next Learning Topics:**
- Multiple software timers.
- Timer callbacks and timer service task.
- Timer command queues.
- Using software timers with task notifications and event flags.
