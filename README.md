# Hornet Keyboard — Split Wireless Communication and Charging Mechanical Keyboard PCB

The Hornet Keyboard is a fully custom, split mechanical keyboard featuring integrated **Bluetooth**, **Qi wireless charging**, and **USB-C programmability & Connectivity**.  
This project represents a complete end-to-end embedded hardware design — from schematic capture to multi-layer PCB layout — optimized for both performance and manufacturability.

---

## Project Overview

| Feature | Description |
|----------|-------------|
| **Form Factor** | Compact Split ergonomic layout key matrix |
| **Microcontroller** | Industry standard NRF52840 QIAA semi-BGA |
| **MCU Interface** | USB-C (DFU bootloader + HID communication) and STLinkV3 SWD |
| **Wireless** | On-board Bluetooth LE trace antenna with matched RF trace and controlled impedance |
| **Power** | Dual power paths: 3.7 V LiPo battery or USB VBUS (5 V) with power switching (USB PRIORITY)|
| **Charging Options** | USB-C Power Delivery (5V @ 1000mA) or Qi wireless charging receiver (5V @ 800mA)|
| **Firmware** | DFU-based update over USB; designed for ZMK and QMK-style firmware structure |
| **Sensors** | On-board voltage monitoring and NTC-based battery temperature protection |
| **Other** | Rotary encoders, Jumpers to probe and enable/disable submodules | standardized M3 mounting holes |
---
## Schematics
<img width="859" height="1203" alt="image" src="https://github.com/user-attachments/assets/9b0e48ad-b14c-4282-ab08-93ebe4c5fa27" />
<img width="1636" height="1127" alt="image" src="https://github.com/user-attachments/assets/bae0fe3d-1988-4345-a292-614d781cf3fa" />
<img width="1649" height="1148" alt="image" src="https://github.com/user-attachments/assets/54c0852c-de8f-48f4-a0a0-6bc903e4a211" />

## Layout
<img width="2227" height="1220" alt="image" src="https://github.com/user-attachments/assets/42f013ce-a481-48cb-8560-fc459172c081" />
3D model (ish)
<img width="1602" height="971" alt="image" src="https://github.com/user-attachments/assets/d7f261e9-df3e-43f1-b0ab-6ca462b41832" />

Individual Layers:
L1: FCu
<img width="1508" height="931" alt="image" src="https://github.com/user-attachments/assets/7216d7f5-6415-4088-bf94-4c4108307263" />
L2: In1
<img width="1470" height="901" alt="image" src="https://github.com/user-attachments/assets/6a0afaa1-d6dc-4ee3-8f4d-2edb4d8d4e13" />
L3: In2
<img width="1465" height="870" alt="image" src="https://github.com/user-attachments/assets/990aaa9a-13c6-4469-8bd1-3c5013f1db05" />
L4: BCu
<img width="1454" height="877" alt="image" src="https://github.com/user-attachments/assets/48371024-7ca0-43fc-a1c4-cb593ec50d68" />


## System Architecture

The system integrates three major electrical domains:

1. **Digital Domain (nRF52840)**
   - Industry standard MCU with built-in BLE 5.4
   - High-speed interrupt-based matrix scanning and battery management.
   - Dedicated power pin for reading battery charge.

2. **Power Management Domain**
   - Dual-source power path with **ideal diode OR-ing** between USB VBUS and LiPo battery.
   - **Li-Ion charge controller** managing both USB and Qi charge inputs.
   - **Dynamic load sharing** ensures stable operation while charging.
   - **Overcurrent and thermal protection** via FET-based switching and NTC feedback.

3. **Wireless RF Domain**
   - Custom-routed **50 Ω matched trace** for the Bluetooth antenna.
   - π-matching network for impedance tuning based on antenna manufacturer data.
   - Keepout and ground clearance zone designed per RF layout guidelines.

---

## Electrical Design Highlights

- **4-layer PCB** for clean separation of signal, power, and RF planes.
- **Differential USB data routing** (D+ / D–) with 90 Ω differential impedance.
- **Common-mode choke** and ESD suppression on USB-C connector for EMI resilience.
- **Reverse-current blocking MOSFET** for power path control between USB and battery.
- **Qi receiver integration** including rectification, voltage regulation, and charge negotiation.
- **Seamless transition** between wireless power and USB without brownout.

---

## Design Challenges and Solutions

| Challenge | Solution |
|------------|-----------|
| RF isolation near digital lines | Created a separate RF ground island with via stitching for return path control. |
| Simultaneous charge and operate stability | Implemented a load-sharing circuit with MOSFET-controlled current path separation. |
| USB-C CC detection and DFU mode switching | Used CC pull-down resistors with firmware-based detection for mode selection. |
| Battery voltage droop during transmit bursts | Added low-ESR ceramic capacitors and ferrite bead isolation near the antenna supply rail. |
| Routing dense key matrix | Used hierarchical sheets and labeled nets to efficiently organize over 60 switches across the split halves. |

---

## Tools and Workflow

- **KiCad 8.0** — schematic capture and PCB design  
- **JLCPCB / LCSC** — fabrication and component sourcing  
- **SolidWorks** — mechanical fit and case alignment  
- **STM32CubeIDE** — firmware prototyping (I²C, SPI, GPIO, BLE config)  
- **DFU-util** — firmware flashing over USB-C

---

## Firmware Architecture (Planned)

- **FreeRTOS** scheduler for concurrent matrix scanning and BLE communication.
- **Low-power sleep mode** using MCU deep-sleep and battery voltage triggers.
- **DFU bootloader** supporting over-the-air firmware updates (future).

---

## Key Learning Outcomes

- Advanced PCB design for mixed-signal and RF systems.
- Real-world power management and LiPo protection circuit design.
- High-speed differential pair routing and controlled impedance design.
- Integration of wireless charging standards into a compact PCB layout.
- Designing for manufacturability and testability at the hobbyist scale.

---

## Gallery

*(Add PCB renders, block diagrams, or assembly photos here)*

---

## Repository Structure
