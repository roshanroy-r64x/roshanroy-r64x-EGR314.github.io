---
title: Message Compliance Verification
---

# Message Compliance Verification  



The purpose of this assignment is to verify that messages transmitted and received within the Human Interface / UI Subsystem comply with the expected communication format and interface requirements. Ensuring proper message formatting and communication timing allows reliable integration with the other subsystems in the team project.

This subsystem is responsible for receiving user inputs, displaying system feedback, and communicating system status using UART.



## Subsystem Overview

The Human Interface / UI Subsystem consists of:

- ESP32-S3-WROOM-1 microcontroller  
- SSD1306 OLED display (I2C interface)  
- Four push button inputs  
- Status indicator LED  
- UART communication interface for system messaging  

The ESP32-S3 processes user input and communicates with the rest of the system using UART messages.


## Communication Interfaces

| Interface | ESP32 Signal | GPIO Pin | Purpose |
|----------|-------------|---------|--------|
| UART TX | UART_DOWN | TXD0 (GPIO43) | Sends messages to other subsystems |
| UART RX | UART_UP | RXD0 (GPIO44) | Receives messages from other subsystems |
| I2C SCL | SCL | GPIO9 | Clock signal for OLED display |
| I2C SDA | SDA | GPIO8 | Data signal for OLED display |
| Button 1 | SW1 | GPIO10 | User input |
| Button 2 | SW2 | GPIO11 | User input |
| Button 3 | SW3 | GPIO12 | User input |
| Button 4 | SW4 | GPIO13 | User input |
| Status LED | STATUS_LED | GPIO21 | Visual system feedback |

All signals operate at **3.3V logic levels** compatible with the ESP32-S3.


## UART Message Timing Diagram

The following diagram illustrates the structure and timing of UART communication between the Human Interface / UI Subsystem and the rest of the system.

The ESP32 transmits messages using:
**UART_DOWN (TXD0 / GPIO43)**


and receives messages using:
**UART_UP (RXD0 / GPIO44)**



### Message Format

UART communication uses the following message structure along with a signal transmission and reception example (Generated using CHATGPT-5):

![alt text](<Message Structure.png>)

When a valid message is received:

1. The start byte is detected.
2. The message type is identified.
3. The data payload is processed.
4. The checksum is verified.
5. The message is accepted if the checksum is correct.

If the checksum or message format is invalid, the message is discarded.

This message structure ensures reliable communication between subsystems while preventing corrupted data from affecting system operation.