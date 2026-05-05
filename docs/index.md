---
title: Welcome
tags:
- tag1
- tag2
---

<center>
<font size="6">Roshan Roy Datasheet</font><br>
as part of<br>
<font size="8">EVScope</font><br>
for<br>
<font size="5">Team 304</font><br>

**Submission: May 04, 2026**
</center>

## Introduction

This datasheet documents the design and implementation of the Human Interface / User Interface (UI) Subsystem for the EVScope project. The purpose of this subsystem is to provide a clear and interactive interface between the user and the embedded system. It enables users to monitor system status, interact with system functions, and receive feedback through visual indicators and display output.

This document contains the hardware design decisions, component selection, schematic details, and supporting analysis required to implement the subsystem. It also includes supporting engineering documentation such as the bill of materials, power budget, and component justifications. These materials provide a complete overview of how the subsystem operates and how it integrates into the overall EVScope system.

---

### Project Summary

EVScope is an embedded system designed to integrate sensing, processing, and user interaction within a single platform. The system combines multiple subsystems that work together to collect information, process it, and communicate useful results to the user.

The Human Interface / UI Subsystem is responsible for presenting system information and enabling user interaction with the device. This subsystem uses an ESP32-S3 microcontroller, an OLED display, and user input buttons to provide real-time system feedback and control. The subsystem communicates with the rest of the system through standard interfaces while maintaining a stable power and control architecture.

More detailed information about the overall system architecture, subsystem interactions, and project goals can be found in the team's main report:

[Team 304 Project Report](https://github.com/EGR314-S-2026-304/EGR314-S-2026-304.github.io)

---

### My Contribution

My primary responsibility for the EVScope project is the design and implementation of the Human Interface / UI Subsystem. This includes the development of the hardware required to manage user interaction and system feedback.

The major responsibilities for this subsystem include:

- Designing the power regulation circuitry that converts the system input voltage to the regulated 3.3 V supply required by the interface electronics.
- Integrating the ESP32-S3-WROOM-1 microcontroller module to control display output, user input processing, and system communication.
- Implementing the OLED display interface using the I2C communication protocol.
- Designing and integrating user input buttons and a status indicator LED for system feedback.
- Creating expansion and programming interfaces including UART programming headers and GPIO expansion headers.
- Developing supporting documentation including the schematic design, component selection, bill of materials, and power budget analysis.

For additional subsystem documentation, please see the following sections:

- **Component Selection** – justification of key electronic components
- **Microcontroller Selection** – explanation of the controller used in the subsystem
- **Schematic Design** – detailed circuit implementation
- **Power Budget** – analysis of subsystem power consumption
- **Bill of Materials (BOM)** – complete list of required components

These sections collectively describe the full hardware design process used to create the Human Interface / UI Subsystem and demonstrate how it supports the overall EVScope system.
