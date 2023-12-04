#  Coffee machine

<div align="center">
<img width=600px src="https://github.com/GuilleAQ/cafe_machine/blob/main/resources/figures/1.jpg" alt="explode"></a> 
</div>

<h3 align="center">  Coffee machine </h3>


<div align="center">
<img width=100px src="https://img.shields.io/badge/status-finished-brightgreen" alt="explode"></a>
<img width=100px src="https://img.shields.io/badge/license-Apache-orange" alt="explode"></a>
</div>


## Table of Contents
- [Table of Contents](#table-of-contents)
- [Hardware circuit](#Hardware-circuit)
- [Software](#Software)
- [Video demo](#Video-demo)


## Hardware circuit
## Components

1. Liquid Crystal Display (LCD)
2. DHT Sensor (Type: DHTTYPE: 11)
3. LEDs
4. Ultrasonic Sensor (HC-SR04)
5. Button
6. Joystick
7. Arduino UNO

## Pin Connections

- **LCD**:
  - RS Pin: Digital Pin 10
  - E Pin: Digital Pin 12
  - D4 Pin: Digital Pin 6
  - D5 Pin: Digital Pin 5
  - D6 Pin: Digital Pin 4
  - D7 Pin: Digital Pin 3

- **DHT Sensor (DHTTYPE: 11)**:
  - Data Pin: Digital Pin 9

- **LEDs**:
  - LED 1: Digital Pin 13
  - LED 2: Digital Pin 11

- **Ultrasonic Sensor (HC-SR04)**:
  - Echo Pin: Digital Pin 8
  - Trigger Pin: Digital Pin 7

- **Button**:
  - Button Pin: Digital Pin 2

- **Joystick**:
  - vrx Pin: Analog Pin 0
  - vry Pin: Analog Pin 1
  - sw Pin: Analog Pin 2


<div align="center">
<img width=600px src="https://github.com/GuilleAQ/cafe_machine/blob/main/resources/figures/circucito.jpg" alt="explode"></a> 
</div>

## Software

### Threads

The project utilizes the concept of threads to achieve multitasking-like behavior on the Arduino platform. This is accomplished using libraries such as `Thread.h`, `StaticThreadController.h`, and `ThreadController.h`. The Thread libraries provide functionalities to start, stop, and set execution intervals for these threads.

- **Implementation**: 
  - Threads are used to define independent tasks that can run concurrently with the main program loop.
  - Each thread is associated with specific functions or tasks and is managed to ensure timely execution.
      - `led_thread`: Manages LED operations.
      - `lcd_thread`: Controls the LCD display.
      - `distance_thread`: Handles distance measurement tasks.
      - `joystick_thread`: Manages joystick input processing.
      - `button_thread`: Dedicated to button interaction handling.
      - `dht_thread`: Reads and processes data from the DHT sensor.

### Interrupts

Interrupts are employed to handle immediate tasks, such as responding to sensor inputs or user actions, without waiting for the current loop iteration to complete. In this case I have used the interrupts to detect that a button is pressed for X time.

- **Implementation**:
  - External interrupts are likely attached to specific Arduino pins (pin 2 used in this case), allowing for immediate response to events like button presses.
  - These interrupts temporarily halt the main program execution to execute a specific interrupt service routine (ISR). `button_callback()` which handles the logic to go state ADMIN or restart SERVICE state
  - After the ISR execution, the normal program flow resumes.

### Watchdog Timer

The watchdog timer is used as a safety mechanism to reset the system in case of software faults or non-responsive behavior.

- **Implementation**:
Implemented using the `avr/wdt.h` library. The watchdog timer is set to a predefined interval. If the program fails to 'kick' (reset) the timer within this interval, the Arduino is automatically reset. This feature ensures that the Arduino can recover from errors and does not remain stuck in an infinite loop.
  - Using `wdt_enable(WDTO_8S)`.
  - This setup ensures that if the program becomes unresponsive for 8 seconds, the system will reset.
  - It acts as a critical safety feature to prevent system lock-ups.

### Avoidance of `delay` Function

An important aspect of this project's design is the avoidance of the `delay` function commonly used in Arduino sketches. Here's why:

- **Non-Blocking Code**: The `delay` function halts the execution of the entire program for a specified period. This blocking nature can be detrimental in applications requiring responsive or concurrent task handling.
- **Efficiency**: By avoiding `delay`, the program can continuously monitor inputs, update outputs, and perform computations without unnecessary pauses.
- **Thread Utilization**: The use of threads allows tasks to be scheduled and run independently without needing `delay`. This approach leads to more efficient and responsive code, especially important in systems managing multiple sensors or user inputs.
- **Real-Time Responsiveness**: In applications like sensor monitoring or user interface management, real-time responsiveness is crucial. Avoiding `delay` ensures the program remains responsive to sensor changes or user inputs at all times.


## Video demo

[video demo](https://urjc-my.sharepoint.com/personal/g_alcocer_2020_alumnos_urjc_es/_layouts/15/stream.aspx?id=%2Fpersonal%2Fg%5Falcocer%5F2020%5Falumnos%5Furjc%5Fes%2FDocuments%2Fvideosgit%2Fdemo%2Emp4&referrer=StreamWebApp%2EWeb&referrerScenario=AddressBarCopied%2Eview)
