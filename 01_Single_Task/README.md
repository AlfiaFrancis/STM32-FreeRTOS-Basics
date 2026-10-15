# STM32 FreeRTOS Basics — LED Blinking

## Project Overview

This is my first beginner project while learning FreeRTOS on an STM32 microcontroller using STM32CubeIDE.

The project blinks the onboard LED connected to GPIO pin PA5 using a FreeRTOS task.

## Hardware Used

- STM32 Nucleo-F103RB development board

## Software and Technologies

- STM32CubeIDE
- STM32 HAL Library
- CMSIS-RTOS2 API
- FreeRTOS
- Embedded C

## Working Principle

1. Initialize the STM32 HAL library.
2. Configure the system clock and GPIO pin PA5.
3. Initialize the FreeRTOS kernel.
4. Create a task using  osThreadNew().
5. Start the scheduler using osKernelStart().
6. Set PA5 HIGH and wait for 2 seconds using osDelay(2000).
7. Set PA5 LOW and wait for another 2 seconds.
8. Repeat the sequence continuously.

## FreeRTOS Concepts Practiced

- Task creation
- Task function and infinite loop
- Task attributes and stack size
- Task delay using osDelay()
- RTOS kernel initialization
- Scheduler startup
- GPIO control using STM32 HAL

## Expected Result

The onboard LED turns ON for approximately 2 seconds and OFF for approximately 2 seconds, repeating continuously.

## Learning Progress

This is my starting point in learning FreeRTOS. I plan to extend this repository with more STM32 RTOS projects as I learn tasks, scheduling, queues, semaphores, mutexes, and interrupt handling.

## Build and Run

1. Open the STM32CubeIDE project.
2. Connect the STM32 Nucleo-F103RB board through USB.
3. Build and flash the firmware.
4. Observe the onboard LED.
