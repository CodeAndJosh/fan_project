# Table Fan with a Closed-Loop PI Speed Controller

## Description

A three-mode office fan powered by a wall outlet. It is controlled using a closed loop motor control. A single pushbutton cycles through Off, Low, Medium, and Hgh modes.

### DISCLAIMER

This is not a certified commercial product. I only intend this to be in my portfolio

## Objective

Build a small table fan with a PI Controller to control the motor's speed

## Preliminary Requirements

| Category | Requirement |
| --- | --- |
| Input Voltage | 120VAC, 60Hz |
| Internal Bus | 5V |
| Motor | 3-phase BLDC, Hall sensors, ~40W |
| User Interface | One button |
| Control | Outer speed PI + Inner current/torque PI |
| Protection | Overcurrent, locked rotor, overheating, undervoltage |
| Cost | $50 |

- Preformance:
  - Steady-state Error: 50RPM
  - Settling Time: <2s
  - Controller Frequency: 5kHz
    - Current Loop Controller: 500Hz
    - Speed Loop Controller: 50Hz
  - PWM Frequency: 20kHz
- Speed: 900 - 1,800 RPM
  - Low: 900 RPM
  - Medium: 1,200 RPM
  - High: 1,800 RPM

## Architecture

AC Input -> Isolated AC/DC Supply -> Motor Driver -> Motor
Outlet --> Button --> PI Controller --> PWM --> Motor Driver --> DC Motor --> Speed Sensor --> Controller

## Hardware

### Major Components

| Component | Part Number | Purpose |
| --- | --- | --- |
| Motor | TBD | Fan actuation |
| Microcontroller | TBD | Control and mode logistics |
| Motor driver | TBD | Motor power stage |
| Speed sensor | TBD | Closed-loop feedback |
| Protection components | TBD | Fuse, surge, and fault protection |

### Electrical Design (WIP)

## Control-System Design

The controller compares data from the sensor with the active mode's speed setpoint:

/[
e[k] = \omega_{\text{ref}}[k] - /omega[k]/]

A discrete PI controller calculates the motor command:

\[u[k] = K<sub>p e[k] + K<sub>i T<sub>s \sum e[k]/]

## Firmware (WIP)

Features:

- Button state machine
- Speed measurements
- Periodic control interrupt
- PI controller
- PWM generation
- Fault detection
- Telemetry or debugging interface

### Operating States

| Current State | Next State |
| --- | --- |
| Off | Low |
| Low | Medium |
| Medium | High |
| High | Off |

## Verification and Resuts (WIP)

### Test Summary (WIP)

## Safety

This product contains hazardous AC mains voltage

### Implemented Precautions (WIP)

## Repository Structure

hardware/      Schematics, PCB files, and BOM

firmware/      Embedded source code

simulation/    Plant and controller models

mechanical/    Enclosure and assembly files

tests/         Test procedures and recorded data

docs/          Images, Plots, and design notes
