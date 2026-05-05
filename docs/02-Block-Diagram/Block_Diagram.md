---
title: Block Diagram
---

# Individual Block Diagram

![text](Block.png)

## Block Diagram Description

The Human Interface / UI subsystem provides the primary interaction point between the user and the EVScope system. The central component of the subsystem is the ESP32-S3-WROOM-1 microcontroller module, which manages user inputs, display output, and communication with other subsystems.

Power for the subsystem is supplied through a 9–12 V barrel jack input and regulated down to 3.3 V using a switching buck regulator. This regulated 3.3 V rail powers the ESP32 module, OLED display, and all digital interface components. The use of a switching regulator ensures high efficiency and minimizes heat dissipation compared to linear regulation.

User interaction is handled through four pushbutton inputs connected to ESP32 GPIO pins. These buttons provide directional and control inputs (up, down, select, back) that allow the user to interact with the system. A status LED connected to a GPIO pin provides immediate visual feedback for system states such as activity, enable/disable status, or error conditions.

System information is displayed on an SSD1306 OLED display connected to the ESP32 via the I2C interface. This allows efficient communication using only two pins while providing real-time feedback such as WiFi status, motor direction, and system errors.

Communication with other subsystems is handled through UART, which connects the ESP32 to both upstream and downstream ribbon connectors. This enables structured message passing between the WiFi subsystem and motor control subsystem. The use of standardized ribbon connectors ensures consistent integration across the team design.

Programming and debugging are supported through a UART programming header connected to an external USB-to-UART adapter. This allows firmware to be uploaded and debug messages to be monitored during development. Additional GPIO, power, and communication lines are exposed through expansion headers to support testing and future subsystem integration.

---

## Design Decision Process and Requirements Justification

The block diagram was developed through an iterative design process focused on meeting the subsystem’s core functional requirements: user interaction, real-time system feedback, reliable communication, and safe operation.

The ESP32-S3-WROOM-1 was selected as the central controller because it integrates sufficient processing capability, built-in wireless support, and multiple communication interfaces (UART and I2C). This decision reduced overall system complexity while meeting all control and communication requirements.

A switching buck regulator was chosen instead of a linear regulator due to the large difference between the input voltage (9–12 V) and required output voltage (3.3 V). This decision was made to improve power efficiency and reduce thermal losses, which is critical for stable system operation.

The OLED display was selected using the I2C interface to minimize pin usage while still providing clear and reliable user feedback. This satisfies the requirement for real-time system visibility without overloading the microcontroller’s available GPIO resources.

UART communication was chosen for subsystem integration because it provides a simple, reliable, and widely supported method for serial data transfer. The use of upstream and downstream ribbon connectors ensures that the subsystem can be easily integrated into the larger system while maintaining consistent signal routing.

Pushbuttons and a status LED were included to meet user interface requirements by providing both input and immediate feedback. These components ensure that the user can control the system and understand its current state without relying solely on the display.

Overall, the block diagram satisfies product requirements by clearly defining how power is distributed, how user input is processed, how system feedback is presented, and how communication occurs between subsystems. Each component was selected and connected with the goal of maximizing reliability, minimizing complexity, and ensuring compatibility with the overall system design.

