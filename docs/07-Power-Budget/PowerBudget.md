---
title: Power Budget
tags:
- tag1
- tag2
---

# Power Budget
**Team 304 – Human Interface / UI Subsystem**

## Overview

This page estimates the electrical power required by the Human Interface / UI Subsystem.  
The subsystem is powered from a 9–12 V input that is converted to 3.3 V using a switching buck regulator (AP63203). The regulated 3.3 V rail powers the following components:

- ESP32-S3-WROOM-1 microcontroller module
- SSD1306 OLED display
- Status indicator LED
- User input buttons
- Expansion interface

The goal of this power budget is to verify that the subsystem can operate safely within the limits of the power regulator and overall system power supply.

---

# 3.3 V Rail Power Budget

| Component | Voltage | Typical Current | Peak Current | Typical Power | Peak Power |
|---|---|---|---|---|---|
| ESP32-S3-WROOM-1 | 3.3 V | 36 mA | 355 mA | 0.12 W | 1.17 W |
| SSD1306 OLED Display | 3.3 V | 20 mA | 25 mA | 0.07 W | 0.08 W |
| Status LED | 3.3 V | 4 mA | 4 mA | 0.01 W | 0.01 W |
| Push Buttons (4x) | 3.3 V | ~0 mA | 1.3 mA | ~0 W | 0.004 W |
| Expansion Header | 3.3 V | 0 mA | 0 mA* | 0 W | 0 W |
| **Total** | 3.3 V | ≈60 mA | ≈385 mA | ≈0.20 W | ≈1.27 W |

\*Expansion header load depends on any future external peripherals and is not included in the baseline subsystem power budget.

---

# Estimated Input Power (12 V Source)

The AP63203 buck regulator is used to convert 9–12 V to 3.3 V.  
Assuming an approximate 88% regulator efficiency, the estimated input current from a 12 V supply is:

| Case | Output Power | Input Power | Input Current @ 12 V |
|---|---|---|---|
| Typical operation | 0.20 W | 0.23 W | 19 mA |
| Worst case peak | 1.27 W | 1.44 W | 120 mA |

---

# Conclusions

The Human Interface / UI Subsystem requires approximately 60 mA during typical operation and may reach up to 385 mA in a worst-case scenario when the ESP32 is operating at peak load.

This current level is within the expected operating range of the ESP32-S3 power requirements and the AP63203 switching regulator. The subsystem therefore operates safely within the available power limits.

Because the OLED display and status LED have relatively low power consumption, the ESP32 microcontroller represents the largest contributor to subsystem power usage. If wireless features are disabled or used intermittently, the typical power consumption will remain closer to the lower estimate.

---

# Datasheet References

- ESP32-S3-WROOM-1 Datasheet – Espressif Systems  
- AP63203 Buck Converter Datasheet – Diodes Incorporated  
- SSD1306 OLED Display Module Documentation  
- AP2012EC LED Datasheet