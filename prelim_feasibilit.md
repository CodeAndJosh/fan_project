# Motor feasibility

The mechanical power of rotating system is:

$$ P_{\mathrm{mech}} = \tau \omega $$

with angular velocity being:

$$ \omega_{\mathrm{max}} = \frac{2 \pi n}{60} $$

Our operating points are between 900 and 1800 RPM

So if n =  1800, then our max angular velocity is

$$ \omega_{\mathrm{max}} = 188.5 $$

The electrical power required for our motor is:

$$ P_{\mathrm{elec}} = \frac{P_{\mathrm{mech}}}{\eta_{\mathrm{motor}} \eta_{\mathrm{drive}}} = \frac{\tau \omega}{\eta_{\mathrm{motor}} \eta_{\mathrm{drive}}} $$

We will assume efficiencies of motor and drive are 0.8 and 0.9 respectively to account for worst-case scenario

To characterize our torque:

$$ \tau = \frac{P_{\mathrm{elec}} \eta_{\mathrm{motor}} \eta_{\mathrm{drive}}}{\omega} $$

Given varying scenarios for electrical power based on power rating

| Power Rating | Total Electrical Power | Max Torque |
| --- | --- | --- |
| 12V 5A | 60W | 0.229 N*m |
| 12V 4A | 48W | 0.183 N*m |
| 12V 3A | 36W | 0.138 N*m |
| 12V 2A | 24W | 0.917 N*m |
| 5V 3A | 15W | 0.057 N*m |
