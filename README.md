Since KiCad has no package management, I'm trying out a monorepo approach.

# Voron PMU

![PMU](assets/voron-pmu-1.0.0.png)

## The problem

Loss of power. Smaller printers can be powered through a UPS, however, this hardly works for more power hungry ones, especially with chamber heaters. They would require massive and expensive UPSes that make no sense for a casual printer hobbyist.

Very ofthen, though, power loss is short-lived: 10 minutes (when one, for example, accidentally disengages the wrong circuit breaker).

There are a few sides of this problem:

1. Printer loses calibration and positional data;
2. Printhead may drop down, ramming hot nozzle into the part ruining it;
3. Part cools down and pops of the buildplate.

## Solution

The solution is to split the the power lines: essential parts run on a simple UPS (Raspberry Pi, controller boards, motors), while heaters get the power directly from the mains. When power loss occurs, this PMU board detects it, and trigger Klipper macroses which pause the print, park the printhead to a safe position and waits for the power to be restored. Once power is back, another set of macroses unsure proper printing temperatures and resumes the print.

## Other features

Soft on/off: once plugged into the mains, printer will start in "Standby" mode, waiting for user to press the Power On button. This is more relevant to printfarms, where restoring power to a large amount of devices at once may trigger a huge mains inrush current, tripping the main breaker.

Power Off button can also be connected, acting also as an Emergency Stop.
