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

## Power Budget Analysis and Conclusions

The power budget was developed by first identifying all components powered by the 3.3 V rail and extracting their typical and peak current values from their respective datasheets. For each component, power consumption was calculated using the relationship:

P = V × I

Using this method, both typical and worst-case power values were determined for each component. These values were then summed to obtain the total subsystem power consumption under normal operation and peak load conditions. This approach ensures that the design accounts not only for average usage but also for transient conditions, such as WiFi transmission bursts from the ESP32, which significantly increase current draw.

The ESP32-S3-WROOM-1 was identified as the dominant contributor to power consumption, particularly under peak conditions where current can reach up to 355 mA. In contrast, the OLED display, LED, and pushbuttons contribute relatively small and predictable loads. This highlights that system power design must primarily accommodate the microcontroller’s dynamic behavior rather than static peripheral loads.

To determine input power requirements, the total output power was adjusted based on the efficiency of the AP63203 buck regulator. By assuming an efficiency of approximately 88%, the input power and current were calculated for both typical and worst-case scenarios. This step is critical because it ensures that the upstream power source and regulator can safely supply sufficient energy without overheating or entering unstable operating regions.

From this analysis, it can be concluded that the subsystem operates well within safe limits of both the regulator and the expected power supply. The peak current requirement of approximately 385 mA on the 3.3 V rail is within the capabilities of the AP63203, and the estimated input current of 120 mA at 12 V is relatively low, indicating efficient power conversion.

Additionally, the power budget confirms that there is margin available for minor expansion, such as additional sensors or interface components, as long as their power consumption remains within reasonable limits. However, any high-power additions, particularly those that operate concurrently with the ESP32 at peak load, would require reevaluation of the power budget.

Overall, the power budget provides confidence that the subsystem is both efficient and stable under expected operating conditions, while also highlighting the importance of designing around worst-case scenarios in embedded systems.

---

# Datasheet References

- ESP32-S3-WROOM-1 Datasheet – Espressif Systems  
- AP63203 Buck Converter Datasheet – Diodes Incorporated  
- SSD1306 OLED Display Module Documentation  
- AP2012EC LED Datasheet