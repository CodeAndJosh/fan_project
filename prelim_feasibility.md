# Major assumptions

This document will consider realistic worst-case scenarios (maximum power consumption, maximum power losses, etc.)

# Electrical/Power feasibility
 
## 12V Power Supply

Voltage from US wall outlet: 120V

Isolated AC-DC power stage that converts 120VAC to 170VDC to 12VDC

## MCU

A draw of 90mA + 10mA from VBUS is a good estimate for current draw of RPi Pico according to official datasheet

So, RPi Pico draw approx. 0.5W

GPIOs also output 3.3V, so our motor driver should be able to read 3.3V

## DC-DC Converter

Buck converter with duty cycle between 0.45 and 0.5 should be sufficient enough to generate 5V, assuming efficiency is 0.9

## Motor Driver

Three options:

1. Integrated BLDC controller & driver
- Pros
  - Simplifies hardware
  - Lower integration risk
- Cons
  - Limited flexibility
  - Less control over optimization
2. Three-phase gate driver
- Pros
  - Maximum design flexibility; allows for custom control
  - High-power potential
  - Better optimization
- Cons
  -  More engineering effort
  -  Higher implementation risk
  -  Higher development cost
3. External BLDC driver module
- Pros
  - Fastest implmentation
  - Convenient for testng
- Cons
  - Less customization
  - Higher unit costs
 
Possible action: Use external BLDC drive module for proof of concept prototyping, then use three-phase gate driver

# Motor feasibility

## Motor power calculations

The mechanical power of rotating system is:

$$ P_{\mathrm{mech}} = \tau \omega $$

with angular velocity being:

$$ \omega_{\mathrm{max}} = \frac{2 \pi n}{60} $$

Our operating points are between 900 and 1800 RPM

So if n =  1800, then our max angular velocity should be:

$$ \omega_{\mathrm{max}} = 188.5 $$

The electrical power required for our motor is:

$$ P_{\mathrm{elec}} = \frac{P_{\mathrm{mech}}}{\eta_{\mathrm{motor}} \eta_{\mathrm{drive}}} = \frac{\tau \omega}{\eta_{\mathrm{motor}} \eta_{\mathrm{drive}}} $$

We will assume efficiencies of motor and drive are 0.8 and 0.9 respectively to account for worst-case scenario

To characterize our torque:

$$ \tau = \frac{P_{\mathrm{elec}} \eta_{\mathrm{motor}} \eta_{\mathrm{drive}}}{\omega} $$

Given varying scenarios for electrical power based on power rating
(we are assuming all available power is going through the motor

| Power Rating | Total Electrical Power | Max Torque |
| --- | --- | --- |
| 12V 5A | 60W | 0.229 N*m |
| 12V 4A | 48W | 0.183 N*m |
| 12V 3A | 36W | 0.138 N*m |
| 12V 2A | 24W | 0.092 N*m |
| 12V 1A | 12W | 0.046 N*m |
| 5V 3A | 15W | 0.057 N*m |

## Motor comparisons

Types of motors:

- Inrunner motors
  - High RPM potential
  - Compact size and lightweight design
  - Low torque output

Assessment: For our use case, inrunner motor's high speed, lightweight design isn't valuable and its high power usage is problematic
  
- Outrunner motors
  - High torque output
  - Larger diameter
  - Efficient cooling

Assessment:

- Star connection
  - Lower current draw
  - Higher torque at lower speeds
  - Improved protection against overheating

Assessment:

- Delta connection
  -  Higher current flow
  -  suited for high-speed performance
  -  Less protection from overheating

Assessment:

Source: https://arshon.com/blog/types-of-bldc-motors-and-controllers-a-complete-guide-for-engineers/

# Firmware feasability

To toggle between different speed modes, use a finite state machine.

Reference speed is set based on current state

| Current State | Next State | RPM Setpoint |
| --- | --- | --- |
| Off | Low | 0 RPM |
| Low | Med | 900 RPM |
| Med | High | 1200 RPM |
| High | Off | 1800 RPM |

# Control feasability

Closed-loop control requires feedback data, we will use a speed sensor to provide that feedback data

Types of speed sensors:

- Hall effect based sensor
  - Cost-effective
  - Reliable commutation signals
  - Fast response time
  - Great low-speed performance
  - Temperature sensitive
  - Sensitive to EMI
  - Requires power supply

Assessment: If we need a sensor, a Hall effect based sensor is a good option as an accurate and cost-effective solution. We could account for temperature using a heat sink

- Optical encoder
  - Precise angular resolution
  - Immune to EMI
  - Minimal noise distortion
  - Higher current consumption

Assessment: Despite its reliable accuracy, its high current consumption makes it not ideal for a sensor. Additionally, its high cost makes it not ideal for a first prototype

- Magnetic resolver
  - High reliability in noisy environments
  - Temperature resistive
  - Compact and cost effective
  - Less precise
  - Sensitive to EMI
  - Requires careful mechanical alignment

Assessment: The lack of reliable accuracy in a magnetic encoder is worrying yet manageable. However its high cost makes it not ideal for a first prototype

- Sensorless back-EMF
  - Lower cost and simpler construction
  - More compact PCB design
  - Higher reliability
  - Poor startup performance
  - Advanced algorithms required for precision

Assessment: Despites its disadvantages, a sensorless controller allows for a simple, compact, low-cost solution. Due to its poor startup performance, an extra open-loop sequence should be added. Depending on value of back EMF @ 900 RPM, we should either increase low-mode RPM or use a sensor

Source: https://arshon.com/blog/types-of-bldc-motors-and-controllers-a-complete-guide-for-engineers/

Types of controller algoriths:

- Field-Oriented Control
  -

Assessment:

- Extended Kalman Filter
  -

Assessment:

- Sliding Mode Observers
  -

Assessment:
