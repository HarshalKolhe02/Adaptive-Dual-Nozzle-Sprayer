# Adaptive Dual Nozzle Sprayer
This repository includes THE Raspberry Pi–based adaptive dual-nozzle spraying system for precision agricultural drones with selective nozzle control.

## Overview 
The agriculture problem statement of the NIDAR competition required precise spraying over clustered plants. Using a large nozzle for small plants leads to over-spraying and reduced precision, while a small nozzle with large plants fails to spray adequately. To address this limitation, we introduced a dual-nozzle spray system capable of dynamically switching between wide-area and precision spraying.

## System Architecture
The Adaptive Dual Nozzle Spraying System has a control layer developed using Raspberry Pi, a custom controller PCB design, and an electro-fluidic spraying system. The power source is a LiPo battery that uses a buck converter to regulate the power to the electronics.

The Raspberry Pi runs the spray logic and acts as an interface to the controller PCB, which powers the pump and two solenoid valves. The pump sucks water from the tank and supplies it through separate solenoid valves to a precision spray nozzle and a wide spray nozzle.

It is possible by selective activation of the solenoid valves to switch dynamically from precision spraying to wide-area spraying. This is done according to the requirements for the NIDAR competition by the clustering algorithms.


<img width="2000" height="1414" alt="Flowchart" src="https://github.com/user-attachments/assets/289c8207-1a72-4d14-bf2a-c75017d25bc1" />

## Implementation

### 1. Hardware setup
The hardware components include a 12 V diaphragm pump, two solenoid valves, and polyurethane tubing. The fluid from the tank enters the pump to be pressurized. The pressurized fluid is then divided into two paths where it passes through two solenoid valves. Depending on the control logic algorithm, the final solenoid opens and the fluid passes through its nozzle to produce a sprayed mist.

![piping and instrumentation diagram](https://github.com/user-attachments/assets/a764e6fb-968c-441c-9c42-e402c2bd4a90)


### 2. Control Circuit.
The control circuit incorporates MOSFET-based switching to drive three independent relays, along with an onboard 5 V step-down regulator for the control electronics. It also includes a dedicated power distribution stage to safely supply power to the pump and solenoid valves.
The Circuit Diagram is shown below.

![Circuit](https://github.com/user-attachments/assets/3a910a49-87bd-4025-bb07-0368437b36f1)



### 3. PCB 
The control circuit was implemented on a custom PCB to integrate the MOSFET switching circuits, relay drivers, 5 V step-down regulator, and power distribution for the pump and solenoid valves. The PCB was fabricated and tested to verify correct voltage levels, relay operation, and load switching under operating conditions.

The Final PCB layout is shown below

<img width="588" height="729" alt="pcb" src="https://github.com/user-attachments/assets/e19c4eba-5113-4577-8bf9-3e59919ecb74" />


### 4. Algorithm 
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

![aaooiravat1](https://github.com/user-attachments/assets/b41c3420-04d3-4853-ae0c-0ab647af5c63)
![aairavat2](https://github.com/user-attachments/assets/8c334e61-2815-4965-a4c0-8ba24926c53e)


