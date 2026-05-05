---
title: Message Compliance Verification
---

# Message Compliance Verification

The purpose of this page is to verify that messages transmitted and received by the Human Interface / UI Subsystem follow the team communication format. This subsystem uses the ESP32 to receive WiFi status messages from Dylan's WiFi subsystem, send motor direction commands to Quinn's motor subsystem, and display system status on the OLED.

## Subsystem Overview

The Human Interface / UI Subsystem consists of:

- ESP32 microcontroller
- SSD1306 OLED display using I2C
- Four pushbutton inputs
- Status LED
- UART communication interface

The ESP32 receives communication messages, validates them, updates the OLED display, and controls whether the user is allowed to operate the system.

## Communication Interfaces

| Interface | ESP32 Signal | GPIO Pin | Purpose |
|----------|-------------|----------|---------|
| UART TX | UART1 TX | GPIO17 | Sends motor direction commands to Quinn's motor subsystem |
| UART RX | UART1 RX | GPIO16 | Receives WiFi status and error messages |
| I2C SCL | SCL | GPIO22 | Clock signal for OLED display |
| I2C SDA | SDA | GPIO21 | Data signal for OLED display |
| Button 1 | UP | GPIO19 | User input |
| Button 2 | DOWN | GPIO23 | User input |
| Button 3 | LED ON | GPIO5 | User input |
| Button 4 | LED OFF | GPIO18 | User input |
| Status LED | LED | GPIO2 | Visual system feedback |

All signals operate at 3.3V logic levels compatible with the ESP32.

## UART Message Format

The team communication protocol uses a simple framed message format:

AZ[Sender ID][Receiver ID][Payload]BY

The message begins with `AZ` and ends with `BY`. The sender and receiver IDs identify which subsystem is communicating, and the payload contains the command or status.

## Message Data Definitions

### WiFi Connected Message

| Field | Value |
|------|------|
| Full Message | AZDRYESYB |
| Sender | D, Dylan WiFi subsystem |
| Receiver | R, Roshan OLED/UI subsystem |
| Payload | YES |
| Meaning | WiFi is connected and system operation is allowed |

System behavior:
- OLED displays WiFi connected status
- User inputs and outputs are enabled

---

### WiFi Disconnected Message

| Field | Value |
|------|------|
| Full Message | AZDRNOYB |
| Sender | D, Dylan WiFi subsystem |
| Receiver | R, Roshan OLED/UI subsystem |
| Payload | NO |
| Meaning | WiFi is not connected and system operation is disabled |

System behavior:
- OLED displays WiFi disconnected status
- Outputs are disabled
- User commands are ignored

---

### Motor Forward Command

| Field | Value |
|------|------|
| Example Message | AZRQFORWARDBY |
| Sender | R, Roshan OLED/UI subsystem |
| Receiver | Q, Quinn motor subsystem |
| Payload | FORWARD |
| Meaning | Command motors to move forward |

System behavior:
- Sent when user presses forward button
- Only transmitted if WiFi is connected

---

### Motor Reverse Command

| Field | Value |
|------|------|
| Example Message | AZRQREVERSEBY |
| Sender | R, Roshan OLED/UI subsystem |
| Receiver | Q, Quinn motor subsystem |
| Payload | REVERSE |
| Meaning | Command motors to move in reverse |

System behavior:
- Sent when user presses reverse button
- Only transmitted if WiFi is connected

---

### Error Message

| Field | Value |
|------|------|
| Example Message | Any message containing ERROR |
| Payload | ERROR |
| Meaning | A subsystem has detected an error |

System behavior:
- OLED displays ERROR
- System ignores the message
- Outputs remain disabled or unchanged

---

## Message Validation Process

When a UART message is received:

1. The ESP32 reads the incoming UART data  
2. The system checks if the message contains ERROR  
3. The system checks if the message matches known valid messages  
4. If the message is AZDRYESYB, WiFi is set to connected  
5. If the message is AZDRNOYB, WiFi is set to disconnected  
6. If the message is invalid, OLED displays INVALID  
7. The OLED updates to reflect the current system state  

---

## System Behavior Summary

| Received Message | OLED Output | System Action |
|----------------|------------|--------------|
| AZDRYESYB | WiFi Connected | Enable operation |
| AZDRNOYB | WiFi Disconnected | Disable operation |
| Contains ERROR | ERROR | Ignore message |
| Invalid message | INVALID | Ignore message |

---

## Compliance Summary

This communication format satisfies the team API requirements by defining a clear structure for all messages. Each message contains a sender, receiver, and payload, allowing the system to interpret commands consistently.

The subsystem does not respond to arbitrary UART data. Instead, it validates all messages before taking action. This ensures reliable communication between the WiFi subsystem, motor subsystem, and user interface subsystem.

This design improves system safety, prevents unintended behavior, and ensures that the user receives accurate and real-time feedback through the OLED display.