# Optional jack insertion detector

The comparator has been replaced by Q1, a JSCJ MMBT3904 NPN transistor (LCSC C20526). All detector parts are on the top side. The jack position, enclosure opening, microphone placement, and existing audio connections are unchanged.

## Circuit and removal

J1 contact SW4 (the inherited footprint's pin 4) connects directly to JACK_BIAS_SENSE. R6 (47 kohm, 0402) connects that signal to Q1's base; R8 (1 Mohm) connects base to ground. Q1's emitter is grounded. Its collector connects to MIC_PRESENT, with R7 (100 kohm) pulling it to VCC_3V3. MIC_PRESENT reaches J3 pin 20 / XIAO GPIO48.

**Remove or omit R6 to disconnect the complete detector from the jack.** Since the 2026-10-18 relayout, R6 is the 0402 at board coordinates X=114.6 mm, Y=126.1 mm (rotation 90), the left-hand resistor of the R6/R8/R7 row on the right side below Q1. Removing it leaves MIC_PRESENT HIGH; firmware must then ignore plug detection.

SW6 is unused. The earlier comparator design's ring-loading resistor, comparator U3, reference divider and its capacitor were removed in October 2026. The designators R9, R10 and C13 have since been reused for the I2C pull-ups and the AVDD 10 uF bulk capacitor; they are not part of the detector.

## Contact assumption

This implementation follows the user's reference: contact 4 is normally closed to the tip and opens on insertion. It uses the footprint contact named SW4; this name comes from the inherited LongWave footprint. It is **not** an assertion that the manufacturer's differently numbered family drawing and the inherited pad names match. Actual continuity has not been measured. Confirm that SW4 connects to the biased tip with no plug and opens with a plug. If this assumption is wrong, remove R6 and leave the audio circuit in use.

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

Three physical leads, three symbol pins, three electrical pads; no duplicate or mechanical pads. Symbol and footprint queries, disposable schematic/PCB renders, and placed pad readback verified the mapping. Since the 2026-10-18 relayout, Q1 sits at X=116.05 mm, Y=122.72 mm, rotation 90 degrees on F.Cu.

## Verification — 13 October 2026

Saved schematic ERC: zero errors and warnings. Wire, component, orphan, and short checks: zero findings. Exported netlist confirms the detector connections and unchanged existing audio/host pin connections, apart from SW4 and SW6.

Live PCB ground zones refilled and board saved before DRC: zero errors, zero unconnected items, zero schematic parity differences. Two pre-existing footprint-library mismatch warnings remain for U1 and J3. Layout and schematic renders inspected. Jack switch behavior remains subject to prototype confirmation.

## Relayout — 18 October 2026

R6, R8, Q1 and R7 now sit beside J1 SW4 on the right side, which shortens JACK_BIAS_SENSE. The circuit is unchanged. MIC_PRESENT is still on J3 pin 20 / XIAO GPIO48. The I2S and I2C pins on J3 were reassigned in this relayout; see the release README. Routed board results: DRC zero errors, zero unconnected items and zero schematic-parity differences; ERC zero errors and warnings. Jack switch behavior still needs confirmation on a prototype.
