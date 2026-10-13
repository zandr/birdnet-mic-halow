# Optional jack insertion detector

The comparator has been replaced by Q1, a JSCJ MMBT3904 NPN transistor (LCSC C20526). All detector parts are on the top side. The jack position, enclosure opening, microphone placement, and existing audio connections are unchanged.

## Circuit and removal

J1 contact SW4 (the inherited footprint's pin 4) connects through **R10, 0 ohm, 0603**, to JACK_BIAS_SENSE. R6 (47 kohm) connects that signal to Q1's base; R8 (1 Mohm) connects base to ground. Q1's emitter is grounded. Its collector connects to MIC_PRESENT, with R7 (100 kohm) pulling it to VCC_3V3. MIC_PRESENT reaches J3 pin 20 / XIAO GPIO48.

**Remove or omit R10 to disconnect the complete detector from the jack.** R10 is the vertical 0603 resistor at board coordinates X=117.0 mm, Y=122.0 mm, on the right edge above the analog regulator. Removing it leaves MIC_PRESENT HIGH; firmware must then ignore plug detection.

SW6 is unused. The existing ring-loading resistor R9 and comparator U3, reference divider, and C13 have been removed.

## Contact assumption

This implementation follows the user's reference: contact 4 is normally closed to the tip and opens on insertion. It uses the footprint contact named SW4; this name comes from the inherited LongWave footprint. It is **not** an assertion that the manufacturer's differently numbered family drawing and the inherited pad names match. Actual continuity has not been measured. Confirm that SW4 connects to the biased tip with no plug and opens with a plug. If this assumption is wrong, remove R10 and leave the audio circuit in use.

## Firmware

Enable MICBIAS and allow it to settle before reading GPIO48. Configure the GPIO as an input with internal pull-up and pull-down disabled. With the assumed switch mapping, no plug turns Q1 on and produces LOW; an inserted plug opens the contact and produces HIGH. Debounce in firmware; 20 ms is an initial value to validate on the prototype. Disabled mic bias also produces HIGH and must not be interpreted as plug insertion.

A 2.5 V or 2.75 V mic-bias setting is a straightforward starting point. The transistor avoids requiring mic bias to exceed the GPIO's 2.475 V HIGH threshold. With 1.375 V bias, a conservative 0.95 V base-emitter assumption, 1% resistor tolerances, and the existing 2.2 kohm tip-bias resistor, estimated available base current exceeds 7 microamps; the output pull-up needs about 33 microamps at 3.3 V. This is a design calculation, not measured low-current transistor saturation across temperature. Validate LOW/HIGH margins on the prototype at the selected bias and intended temperature range.

The bias-sense branch loads the tip only while its NC switch is closed. At 2.75 V and an assumed 0.7 V base-emitter voltage, branch current is about 42 microamps, including the existing tip-bias resistor. Opening the switch removes that load from the microphone. R2 can remain unpopulated for mono; detection no longer depends on a ring-bias threshold.

## Transistor pin map

Manufacturer: JSCJ / Jiangsu Changjing. Part: MMBT3904, gain range 100–300, SOT-23. Datasheet revision 2.1, pages 1 and 3:
https://datasheet.lcsc.com/datasheet/pdf/9335632bcfa83df8351353eab379720e.pdf?productCode=C20526

Symbol: Transistor_BJT:MMBT3904. Footprint: Package_TO_SOT_SMD:SOT-23. Coordinates below are footprint-local in KiCad's front/component-side orientation.

| Physical lead | Function | Symbol pin/type | Footprint pad | X/Y mm |
|---|---|---|---|---|
| 1 | Base | 1 B / input | 1 | -0.9375 / -0.95 |
| 2 | Emitter | 2 E / passive | 2 | -0.9375 / +0.95 |
| 3 | Collector | 3 C / passive | 3 | +0.9375 / 0 |

Three physical leads, three symbol pins, three electrical pads; no duplicate or mechanical pads. Symbol and footprint queries, disposable schematic/PCB renders, and placed pad readback verified the mapping. Q1's actual board orientation is 0 degrees on F.Cu.

## Verification — 13 October 2026

Saved schematic ERC: zero errors and warnings. Wire, component, orphan, and short checks: zero findings. Exported netlist confirms the detector connections and unchanged existing audio/host pin connections, apart from SW4 and SW6.

Live PCB ground zones refilled and board saved before DRC: zero errors, zero unconnected items, zero schematic parity differences. Two pre-existing footprint-library mismatch warnings remain for U1 and J3. Layout and schematic renders inspected. Jack switch behavior remains subject to prototype confirmation.
