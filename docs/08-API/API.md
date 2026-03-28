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

## Message Data Definitions

The following tables define the byte-level structure of the UART messages used by the Human Interface / UI Subsystem. Each table includes the variable name, C data type, valid range recognized by the code, and an example valid message.



### Message 1: UI Button Event Message
**Purpose:** Sent from the Human Interface / UI Subsystem to another subsystem when a button is pressed or released.  
**Total number of bytes:** 3

|                | Byte 1 | Byte 2 | Byte 3 |
|----------------|--------|--------|--------|
| Variable Name  | `message_type` | `button_id` | `button_state` |
| Variable Type  | `uint8_t` | `uint8_t` | `uint8_t` |
| Min Value      | 1 | 1 | 0 |
| Max Value      | 1 | 4 | 1 |
| Example        | 1 | 2 | 1 |

**Notes:**
- `message_type = 1` means "button event"
- `button_id` identifies which pushbutton generated the event
- `button_state = 0` means released, `1` means pressed

### Message 2: OLED Display Update Message
**Purpose:** Received by the Human Interface / UI Subsystem to update the OLED display.  
**Total number of bytes:** 4

|                | Byte 1 | Byte 2 | Byte 3 | Byte 4 |
|----------------|--------|--------|--------|--------|
| Variable Name  | `message_type` | `screen_id` | `status_code` | `value` |
| Variable Type  | `uint8_t` | `uint8_t` | `uint8_t` | `int8_t` |
| Min Value      | 2 | 0 | 0 | -100 |
| Max Value      | 2 | 9 | 9 | 100 |
| Example        | 2 | 1 | 3 | 25 |

**Notes:**
- `message_type = 2` identifies an OLED display update message
- `screen_id` selects which screen layout is displayed
- `status_code` determines the type of message shown
- `value` is a signed numeric value displayed on the screen

### Message 3: Status LED Control Message
**Purpose:** Received by the Human Interface / UI Subsystem to control the status LED.  
**Total number of bytes:** 3

|                | Byte 1 | Byte 2 | Byte 3 |
|----------------|--------|--------|--------|
| Variable Name  | `message_type` | `led_mode` | `led_state` |
| Variable Type  | `uint8_t` | `uint8_t` | `uint8_t` |
| Min Value      | 3 | 0 | 0 |
| Max Value      | 3 | 2 | 1 |
| Example        | 3 | 1 | 1 |

**Notes:**
- `message_type = 3` identifies an LED control message
- `led_mode` determines the LED behavior  
  - `0` = solid  
  - `1` = blinking  
  - `2` = error indication
- `led_state` determines whether the LED is on or off  
  - `0` = off  
  - `1` = on  

### Message 4: Heartbeat / UI Alive Message
**Purpose:** Sent periodically from the Human Interface / UI Subsystem to indicate that the subsystem is operating correctly.  
**Total number of bytes:** 3

|                | Byte 1 | Byte 2 | Byte 3 |
|----------------|--------|--------|--------|
| Variable Name  | `message_type` | `ui_status` | `error_code` |
| Variable Type  | `uint8_t` | `uint8_t` | `uint8_t` |
| Min Value      | 4 | 0 | 0 |
| Max Value      | 4 | 1 | 9 |
| Example        | 4 | 1 | 0 |

**Notes:**
- `message_type = 4` identifies a heartbeat message
- `ui_status` indicates subsystem health  
  - `0` = subsystem fault  
  - `1` = subsystem operating normally
- `error_code` reports any subsystem error condition  
  - `0` = no error  
  - `1–9` = specific subsystem error codes



When a valid message is received:

1. The start byte is detected.
2. The message type is identified.
3. The data payload is processed.
4. The checksum is verified.
5. The message is accepted if the checksum is correct.

If the checksum or message format is invalid, the message is discarded.

This message structure ensures reliable communication between subsystems while preventing corrupted data from affecting system operation.