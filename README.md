# Exoskeleton Motor Power & Protection PCB

A 48V motor power distribution and protection board designed in Altium Designer for the McMaster exoskeleton team.

<p align="center">
<img width="1288" height="1278" alt="Screenshot 2026-11-20 183755" src="https://github.com/user-attachments/assets/dba3c857-bc7c-4476-96fc-f9c4da913e54" />
</p>
<p align="center"><i>3D render of the board</i></p>

## Overview

This board sits between the main high-voltage supply and the exoskeleton's motor. It controls when power reaches the motor, protects the system from faults, and reports what the motor is drawing. It does not make decisions on its own. A low-voltage control board enables it and reads its measurements, so all of the logic stays in software.

## Key Specs

- 48V / 25A motor power path
- Two-layer PCB
- Electronic switching stage with inrush limiting, overvoltage protection, and undervoltage lockout
- Series mechanical relay that provides a hard disconnect
- Continuous current and voltage monitoring over I2C

## How It Works

**Two-stage power switching.** A gate driver (LTC4366-1) drives a high-power MOSFET to bring up the 48V rail in a controlled way, limiting inrush current and shutting down on overvoltage or undervoltage. A relay in series with this stage can fully isolate the motor during shutdown or fault conditions.

**Monitoring.** An INA228 power monitor measures motor current across a shunt resistor, along with bus voltage, and sends the data to the control board over I2C. The control board uses it for diagnostics and fault detection.

**Software control.** A GPIO signal from the control board turns the MOSFET stage on and off. If the measurements look wrong, for example an overcurrent or an unexpected voltage drop, the control board can disable the MOSFET stage or open the relay.

**Layered protection.** Hardware protection in the gate driver, a mechanical disconnect, and software monitoring together mean no single failure leaves the motor unprotected.

## Interfaces

- **Power in:** 48V from the system battery through the team's power bus board
- **Power out:** a dedicated output connector to the motor
- **Control connector:** 5V supply for the onboard logic, I2C lines for measurements, and the enable signal

## My Contributions

- Designed the schematic and PCB layout in Altium Designer
- Selected components for the switching, protection, and sensing circuits

## Tools

`Altium Designer`

## Note

This is a team project. Full schematics, layout files, and firmware aren't published to respect the team's design.
