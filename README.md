Analog Line Follower

A line follower where the steering loop has no microcontroller in it. Ten reflective sensors go in, two motor PWM signals come out, and everything in between is op-amps, resistors and one comparator.

Status: schematic complete in KiCad. Not simulated or fabricated yet. This repo will be updated as that happens.

Why analog

Most line followers read sensors with an ADC, run a PID loop in code and write PWM. I wanted to see how far the same idea goes when the loop is built from circuits you can probe with a scope. There’s no sampling delay or loop rate to tune around, and every signal in the controller is a voltage you can look at. It also makes the PID terms very direct: Kp, Ki and Kd are trimmer pots on the board.

How it works

Sensing. Ten QRE1113 reflectance sensors, each buffered by an op-amp so the later stages don’t load them.
Position error. The right-side signals are inverted around a 2.5 V reference, then all ten are summed with weighted resistors (100k down to 20k). Sensors farther from center pull harder. A pot sets overall gain and another trims the center offset.
PID. Separate P, I and D op-amp stages feed a summer. The integral stage is built but left unpopulated by default, so the first version runs as PD.
Speed and steering. A base-speed pot is added to the PID output for one motor and subtracted for the other.
PWM. A triangle oscillator around 5 kHz is compared against each motor’s duty voltage by an LM339.
Drive. Two AO3400A MOSFETs switch the motors low-side, with flyback diodes.
Power. An AP63205 steps 12 V down to 5 V. A buffered divider provides the 2.5 V reference.

Test points are placed on the key signals (error, P, I, D, PID output, triangle, duty, PWM) so each stage can be checked on its own.

Parts

10 × QRE1113GR sensors
7 × MCP6004 quad op-amps
1 × LM339 comparator
1 × AP63205 buck regulator
2 × AO3400A MOSFETs, 2 × SS14 diodes
Trimmers for Kp, Ki, Kd, gain, offset and base speed

What I want to check next

Simulate the PID response and the triangle/PWM stage.
Confirm the LM339 input range is comfortable with the triangle peak.
Check PWM edge speed at the gates.
Add reverse or braking, and handling for junctions and dashed lines, if the base design works well.
