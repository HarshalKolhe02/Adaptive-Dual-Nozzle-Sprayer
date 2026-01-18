# Adaptive Dual Nozzle Sprayer
This repository includes THE Raspberry Pi–based adaptive dual-nozzle spraying system for precision agricultural drones with selective nozzle control.

## Overview 
The agriculture problem statement of the NIDAR competition required precise spraying over clustered plants. Using a large nozzle for small plants leads to over-spraying and reduced precision, while a small nozzle with large plants fails to spray adequately.To address this limitation, we introduced a dual-nozzle spray system capable of dynamically switching between wide-area and precision spraying.

## System Architecture
The Adaptive Dual Nozzle Spraying System has a control layer developed using Raspberry Pi, a custom controller PCB design, and an electro-fluidic spraying system. The power source is a LiPo battery that uses a buck converter to regulate the power to the electronics.

Raspberry Pi runs the spray logic and acts as an interface to the controller PCB that powers the pump and two solenoid valves. The pump suctions water from the tank and supplies the water through separate solenoid valves to a precision spray nozzle and a wide spray nozzle.

It is possible by selective activation of the solenoid valves to switch dynamically from precision spraying to wide-area spraying. This is done according to the requirements for the NIDAR competition by the clustering algorithms

Note--System Architecture image---

## Implementation

### 1. Hardware setup
The hardware components include a 12 V diaphragm pump, two solenoid valves, and polyurethane tubing. The fluid from the tank enters the pump to be pressurized. The pressurized fluid is then divided into two paths where it passes through two solenoid valves. Depending on the control logic algorithm, the final solenoid opens and the fluid passes through its nozzle to produce a sprayed mist.

---P&ID---

### Control Circuit.
The control circuit incorporates MOSFET-based switching to drive three independent relays, along with an onboard 5 V step-down regulator for the control electronics. It also includes a dedicated power distribution stage to safely supply power to the pump and solenoid valves.
The Circuit Diagram is shown below.

note ----Circuit Diagram---

### PCB 
The control circuit was implemented on a custom PCB to integrate the MOSFET switching circuits, relay drivers, 5 V step-down regulator, and power distribution for the pump and solenoid valves. The PCB was fabricated and tested to verify correct voltage levels, relay operation, and load switching under operating conditions.
The Final PCB layout is shown below

---pcb layout---

### Algorithm 
The control algorithm uses the lgpio factory library on the Raspberry Pi to control the pump and solenoid valves. To prevent pump lock due to pressure buildup, the small nozzle valve remains open by default and is closed only when the large nozzle is activated.

The main spraying logic is implemented in:
```
src/sprayingmaster.py
```

For manual operation, RC-based control is provided through:
```
src/manual_rc_trigger.py
```
In this mode, RC Channel 5 controls the pump, while RC Channel 7 selects the active nozzle. This script requires integration with the ArduCopter firmware to access RC channel inputs.

## Results
-- Photos --


