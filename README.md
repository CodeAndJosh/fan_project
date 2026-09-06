# Table Fan with a Closed-Loop PI Speed Controller

## Objective

Build a small table fan with a PI Controller to control the motor's speed

## Requirements

| Category | Requirement |
| --- | --- |
| Input Voltage | 120VAC, 60Hz |
| Internal Bus | 5V |
| Motor | 3-phase BLDC, Hall sensors, ~40W |
| User Interface | One button |
| Speed Modes | Off -> Low -> Medium -> High -> Off |
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

## Hardware

- AC-DC Converter
- BLDC
- MCU

## Architecture

Outlet --> Button --> PI Controller --> PWM --> Motor Driver --> DC Motor --> Speed Sensor --> Controller
