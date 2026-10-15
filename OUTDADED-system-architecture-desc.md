## 12V Power Supply

- 12V Power Supply provides DC power for the entire system
- Power Supply will intake power from AC wall outlet, goes through a series of protectors, rectifiers, and DC-DC converter to obtain our 12V
  - Alternatively, uses a flyback converter instead of rectifiers and DC-DC converter
- Likely the power supply will be premanufactured
- If we do not find a power supply within budget, power supply could be substituted with batteries that supply 12V

## Switch-Mode DC-DC Converter

- DC-DC Converter provides 5V from 12V obtained from power supply
  - Primary purpose is to power MCU requiring 5V
  - PWM obtained by PWM generator
  - Buck Converter topology
- Protection may be needed if power provided by power supply exceeds power ratings
- Have plans to design my own buck converter once I have a working prototype

## Microcontroller Unit + Button

- MCU acts as the control unit between button input, sensor and motor driver
- A cascaded PI controller will be encoded into the microcontroller
- To toggle the different modes, code will use a finite-state machine

## Motor Driver + Speed Sensor
- Motor driver drives our motor with the control system in our microcontroller
- Speed sensor detects motor speed via Hall Effect

## Motor
- Motor is the primary mechanism behind the fan
- Type of motor is BLDC motor
  - BLDC motor suitable for this type of project due to low power and high efficiency 
