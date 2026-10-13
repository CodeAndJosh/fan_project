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

# Firmware feasability

To toggle between different speed modes, use a finite state machine.

Reference speed is set based on current state

| Current State | Next State | Reference RPM }
| --- | --- | --- |
| Off | Low | 0 RPM |
| Low | Med | 900 RPM |
| Med | High | 1200 RPM |
| High | Off | 1800 RPM |

# Control feasability
