# STM32 FreeRTOS – Multiple Tasks

## Overview

This project explores multitasking using FreeRTOS on the STM32 Nucleo-F103RB development board with STM32CubeIDE.

## Features

- Created four tasks with different priorities.
- Controlled GPIO pins PA5 and PA6.
- Used `osDelay()` for periodic task execution.
- Transmitted messages using USART2.
- Suspended and resumed Task 3 from Task 4.

## Task Description

| Task | Priority | Function |
|---|---|---|
| Task 1 | Low | PA5 ON/OFF with 1-second delays |
| Task 2 | Below Normal | PA6 ON/OFF with 500 ms delays |
| Task 3 | Normal | Sends a UART message every 200 ms |
| Task 4 | Above Normal | Suspends Task 3, waits 5 seconds, then resumes it |

## Tools and Hardware

- Board: STM32 Nucleo-F103RB
- IDE: STM32CubeIDE
- Language: Embedded C
- RTOS: FreeRTOS
- APIs: STM32 HAL and CMSIS-RTOS2
- UART: USART2 at 115200 baud

## Learning Outcomes

- Task creation and priority configuration.
- Task delays and scheduler behaviour.
- UART transmission from a task.
- Task suspension and resumption.




