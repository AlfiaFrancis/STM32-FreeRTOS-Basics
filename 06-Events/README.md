# FreeRTOS Event Flags – Button and UART Communication

## Overview

This project demonstrates the use of **CMSIS-RTOS2 Event Flags** in an STM32 microcontroller using FreeRTOS.

Two independent events are used to notify tasks:

- **Button Event:** Generated when the onboard push button is pressed.
- **UART Event:** Generated when the STM32 receives five bytes of UART data.

The corresponding RTOS tasks wait for their event flags and execute their operations when notified.

## Hardware and Software

### Hardware
- STM32 Nucleo-F103RB
- Onboard push button (PC13)
- Onboard LED (PA5)
- USB connection for UART communication

### Software
- STM32CubeIDE
- STM32 HAL Library
- FreeRTOS
- CMSIS-RTOS2 API
- PuTTY (Serial Terminal)

## Concepts Covered

- FreeRTOS tasks and task scheduling
- Event Flags
- Bitwise operators and event masks
- Interrupt-based GPIO input
- UART interrupt-based reception
- Communication between ISRs and RTOS tasks
- UART transmission using `printf()`

## Event Flag Configuration

Two event flags are defined:

```c
#define Button_Task (1 << 3)
#define Uart_Task   (1 << 4)
```

Each flag represents a different event.

| Event | Bit position | Purpose |
|---|---:|---|
| Button_Task | Bit 3 | Indicates a button press |
| Uart_Task | Bit 4 | Indicates UART data reception |

Both flags belong to the same event flags object:

```c
osEventFlagsId_t myEvent01Handle;
```

## Project Implementation

### 1. Button Event

The onboard push button is configured on PC13 using falling-edge external interrupt mode.

When the button is pressed, the HAL GPIO callback sets the corresponding event flag:

```c
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
    osEventFlagsSet(myEvent01Handle, Button_Task);
}
```

The ButtonTask waits for this event:

```c
osEventFlagsWait(myEvent01Handle,
                 Button_Task,
                 osFlagsWaitAll,
                 osWaitForever);
```

When the event occurs, the task resumes.

### 2. UART Event

USART2 is configured with the following parameters:

| Parameter | Configuration |
|---|---|
| Baud Rate | 115200 |
| Word Length | 8 bits |
| Stop Bits | 1 |
| Parity | None |
| Mode | TX and RX |
| Flow Control | None |

UART reception is performed using interrupt mode.

The STM32 starts receiving five bytes using:

```c
HAL_UART_Receive_IT(&huart2, Rxdata, 5);
```

When five bytes are received, the HAL receive-complete callback executes:

```c
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
    if (huart->Instance == USART2)
    {
        osEventFlagsSet(myEvent01Handle, Uart_Task);

        HAL_UART_Receive_IT(&huart2, Rxdata, 5);
    }
}
```

The callback performs two operations:

1. Sets the UART event flag to notify the UART task.
2. Restarts UART reception for the next five bytes.

### 3. UART Task

The UART task waits for the UART event flag:

```c
osEventFlagsWait(myEvent01Handle,
                 Uart_Task,
                 osFlagsWaitAll,
                 osWaitForever);
```

When five bytes are received, the task resumes and executes:

```c
printf("Data received: %.5s\r\n", Rxdata);

HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5);

osDelay(500);
```

The task:
- Prints the received five characters through USART2.
- Toggles the onboard LED connected to PA5.
- Delays for 500 ms before continuing.

The `%.5s` format specifier limits the printed output to five characters because the receive buffer is not necessarily null-terminated.

## UART Communication Testing

PuTTY is used as the serial terminal.

### Configuration

- COM Port: STM32 ST-LINK Virtual COM Port
- Baud Rate: 115200
- Data Bits: 8
- Stop Bits: 1
- Parity: None
- Flow Control: None

### Testing Procedure

1. Connect the STM32 Nucleo board to the PC through USB.
2. Open PuTTY and configure the serial connection.
3. Flash and run the STM32 program.
4. Type five characters in PuTTY.
5. The STM32 receives the five bytes through USART2.
6. The UART interrupt callback sets the UART event flag.
7. The UART task wakes up, prints the received data, and toggles the onboard LED.

Example:

```text
UART test started
Data received: HELLO
Data received: WORLD
```

## LED Behaviour

The onboard LED is connected to PA5.

In the current implementation, the LED toggles whenever the UART task processes a received message.

The button task currently waits for the button event, but its LED toggle operation is commented out.

## Key Learnings

- Event flags allow tasks to wait for specific events without continuously checking conditions.
- Multiple event flags can be maintained in a single event flags object.
- ISRs can notify RTOS tasks using event flags.
- UART reception can be handled using interrupt mode instead of continuously polling the UART peripheral.
- A task can remain blocked until its required event occurs.
- Event flags provide event notification; they do not carry the actual UART data. The received data remains in the UART buffer.

## Conclusion

This project demonstrates how FreeRTOS Event Flags can be used to coordinate independent events generated by GPIO and UART interrupts.

It also demonstrates how interrupt-based UART reception can be combined with RTOS task scheduling to handle incoming serial data without continuously polling the UART peripheral.
