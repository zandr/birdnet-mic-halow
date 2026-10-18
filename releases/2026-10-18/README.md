# BirdNET microphone HAT - prototype release 2026-10-18

Source revision: `0738ede6865a6e5e90c3dbb153b48d1290b74f9a` on branch `relayout`. The board and schematic are identical to `cf38929`, the routing commit. KiCad 10.0.7.

This package replaces the 2026-10-17 package. It contains a complete relayout that applies the 2026-10-18 design-review fixes. It is published for prototype review and quotation. **Production/order approval is still INCOMPLETE:** ADC sourcing, the JLCPCB placement preview, a Gerber viewer check and the final order choices remain outstanding. Publishing this package is not a fabrication order.

## ⚠ Firmware: J3 GPIO assignment changed

The I2S and I2C signals on J3 were permuted so that I2S drops straight down from the ADC. **This no longer matches the LongWave DAC hat's pin configuration.**

| Hat J3 pin | Signal | XIAO ESP32-S3 GPIO | Before 2026-10-18 |
| --- | --- | --- | --- |
| 8 | I2S_SDOUT | GPIO42 | I2C_SCL |
| 10 | I2S_BCLK | GPIO41 | I2C_SDA |
| 12 | I2S_WS | GPIO40 | ADC_RST |
| 14 | I2C_SCL | GPIO39 | I2S_WS |
| 16 | I2C_SDA | GPIO38 | I2S_SDOUT |
| 18 | ADC_RST (SHDNZ) | GPIO47 | I2S_BCLK |
| 20 | MIC_PRESENT | GPIO48 | unchanged |

Power and ground are unchanged: pin 2 is VIN, pins 4, 6 and 28 are GND, and pins 24 and 26 are VCC_3V3. All other pins are not connected. None of the reassigned GPIOs is a strapping pin; the ESP32-S3 GPIO matrix routes any of them to the I2S and I2C peripherals.

ADC_RST now has a **100 k pull-down** (R3), so the ADC stays in shutdown until firmware drives GPIO47 high after IOVDD is stable (TLV320ADC5140 datasheet §10). The previous design pulled it up.

## Included files

- `Fabrication-birdnet-mic-hat.zip`: the 11 Gerber layers, PTH/NPTH drill files and Gerber job file. Use this archive for PCB fabrication, not the whole release archive.
- `Schematic-birdnet-mic-hat.pdf`: the complete schematic.
- `Assembly-placement.pdf`: top and bottom locator drawings with pin-1 markers and DNP outlines. The bottom page is viewed from below.
- `PCB-fabrication.pdf`: a supplemental copper/fab plot.
- `BOM-birdnet-mic-hat.csv` and `CPL-birdnet-mic-hat.csv`: mono build with exactly 31 fitted parts, 30 on top and J3 on the bottom.
- `DNP.csv`: C3, C11 and R2, which are left off the fitted BOM and CPL.
- `Sourcing.csv`: selected procurement parts, URLs and verification dates.
- `Source-0738ede.zip`: the exact committed project, libraries and design notes, with older release archives excluded.
- `evidence/`: fresh check results, raw exporter outputs, design notes and the export review.
- `manifest.json` and `SHA256SUMS`: file sizes, provenance and integrity hashes.

## Changes since 2026-10-17

All footprints were re-placed and all tracks re-routed. The board outline, J1 and J3 are unchanged. The design-review fixes:

1. **I2C pull-ups:** R9 and R10, 4.7 k to VCC_3V3.
2. **SHDNZ:** R3 is now a 100 k pull-down; it was a 10 k pull-up.
3. **ESD protection:** D1 and D2, ESD9B5.0ST5G 5 V bidirectional TVS diodes in SOD-923, on JACK_TIP and JACK_RING at J1.
4. **PDM_CLK series resistor:** R11, 33 Ω at U1 pin 11. Fit 0 Ω to bypass it. Check the edges on a scope.
5. **U1 exposed pad:** the full-pad paste was removed, leaving the four window apertures. **The four thermal vias under U1 must be filled and capped (plugged) at fab.**
6. **AVSS return:** a top-copper ground bar runs from U1 pin 4 to the VREF, MICBIAS, AREG and AVDD capacitors (C5, C4, C1, C9) with no vias. U1 pad 4's zone connection is solid, because it is wired directly to that bar.
7. **Supply filtering:** added C13 (10 µF on AVDD) and C14 (10 µF on VCC_3V3/IOVDD), per TI Fig 172. FB1 (BLM15PX601SN1D ferrite) now separates the MK1 supply: AVDD → FB1 → MIC_VDD → MK1 and C12.
8. **Jack detector:** Q1, R6, R7 and R8 moved next to J1 SW4, which shortens JACK_BIAS_SENSE.
9. **Board:** the edge-clearance via is gone, and the copper finish is set to ENIG.

There are 45 vias in total: 0.45 mm pads with 0.25 mm drills.

## Stereo option

The board is assembled mono by default. C3, C11 and R2 are placed on the PCB but not fitted; they complete the second analog channel, from the jack ring to ADC IN2. For stereo, fit C3 and C11 (1 µF 0603) and R2 (2.2 k 0402), and enable channel 2 in firmware. Without R2, the ring has no microphone bias.

## Fabrication / quotation contract

| Item | Specification / state |
| --- | --- |
| Fabricator target | JLCPCB; no service or order submitted |
| Layers, top to bottom | F.Cu / In1.Cu (GND plane) / In2.Cu (power and signals) / B.Cu |
| Material / thickness | FR-4, nominal 1.6 mm |
| Outline | Unchanged from 2026-10-17: 17.791 x 22.689 mm envelope (Edge.Cuts centreline), with the notch and rounded corners shown in the Gerber |
| Copper | Saved stackup: nominal 35 µm on all four layers. Confirm what the vendor offers before ordering |
| Dielectric stackup | Saved stackup: three nominal 0.48 mm FR-4 dielectrics. Vendor stackup acceptance still required |
| Minimum features | 0.15 mm track / 0.127 mm clearance; 0.45/0.25 mm vias; 0.2 mm copper-to-edge |
| Via treatment | **Fill and cap (plug) the four thermal vias under U1.** Tent or plug the rest as the vendor allows |
| Impedance | No controlled-impedance specification |
| Finish | **ENIG** (set in the source and recorded in the Gerber job file) |
| Mask / legend | Green mask and white legend proposed. There is no reference text on the silkscreen; use the assembly drawing |
| Assembly | Both sides; only J3 is on the bottom. Confirm the service supports these parts, the board size and any panelization |
| Stencil | Both paste layers included. U1's exposed pad uses four window apertures; assembler to review |
| Quantity / panel | Not specified; needs the vendor's tooling/panel review and a quote |
| Drill | Separate metric PTH and NPTH files; NPTH holds the two 1.1 mm jack locating holes |

## Checks

- **DRC:** run fresh on the live board after a zone refill and save. Zero errors, zero unconnected items, zero schematic-parity violations. Two library-mismatch warnings remain for U1 and J3. The footprint instances on the board are authoritative; do not refresh them from the library without re-checking and re-exporting.
- **ERC:** zero errors and zero warnings.
- **JLCPCB manufacturing preflight:** READY, no issues reported. This narrow automated verdict does not establish production or assembly readiness.
- **Fabrication files:** checked by structural cross-check, not GerbView; see `evidence/viewer-review.md`.
- **BOM and CPL:** the fitted designator sets match, with 31 parts and no duplicates. Both were checked against the full schematic inventory (`evidence/source-inventory.csv`), which has 34 symbols: 31 fitted and 3 DNP. Use the BOM and CPL in the package root, not the raw exporter files under `evidence/`.

## Open items before ordering

- **CPL orientation is PREVIEW_REQUIRED.** Coordinates are native KiCad pad-box midpoints in mm (X as exported, Y inverted). No correction rule matched, so the native rotations were kept unchanged. Inspect every part in JLCPCB Component Placements, especially U1, U2, Q1, MK1, J1, D1/D2 and the bottom-side J3.
- **U1 sourcing:** TLV320ADC5140IRTWR has no verified LCSC listing, so its BOM LCSC field is intentionally blank. Arrange assembler sourcing or consignment.
- **New parts:** D1/D2 (C111566) and FB1 (C160977) are JLCPCB Extended parts. C13/C14 (C19702), R9/R10 (C25900) and R11 (C25105) are Basic parts. Stock was checked on 2026-10-18 through the Konnect JLCPCB lookup. The other parts' stock was last checked on 2026-10-14.
- **TPS7A20 output capacitance:** VCC_3V3 now carries 10 µF (C14) plus local decoupling. The TPS7A20 datasheet's maximum stable C_OUT has not been checked against this; confirm before ordering.
- **Jack switch:** the jack-switch mapping and insertion detection still need a continuity test with a real jack and a prototype test. See `evidence/source-JACK-DETECTION.md`.
- **PDM_CLK:** scope the clock edges at MK1 to confirm the 33 Ω value for R11.
- **Mechanical:** the physical enclosure, the XIAO stack and acoustic clearance were not re-verified. MK1 did not move. Keep the top-port microphone opening clear and keep the exact J1 and J3 part versions.

Before ordering, also confirm quantity, finish, stackup and copper, the assembly service, and any vendor panelization or tooling requirements. Complete the placement preview and the vendor's DFM and stencil review.

The vendor format references carried forward from the October 14 preparation are:
- https://jlcpcb.com/help/article/bill-of-materials-for-pcb-assembly
- https://jlcpcb.com/help/article/pick-place-file-for-pcb-assembly
- https://jlcpcb.com/capabilities/pcb-capabilities
- https://jlcpcb.com/capabilities/pcb-assembly-capabilities

This package does not imply a fabrication order, a parts purchase or assembly approval.
