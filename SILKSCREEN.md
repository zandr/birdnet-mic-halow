# Silkscreen policy

All 34 component reference fields are hidden on the assembled PCB (26 before the 2026-10-18 relayout). Board identification text and orientation graphics remain. Use the schematic, BOM, and fabrication/assembly views to identify components.

Visible filled pin-1 dots on F.SilkS:

| Component | Pin-1 pad location (mm) | Dot bounds (mm) | Diameter |
|---|---|---|---|
| U1, TLV320ADC5140 | X 111.6685, Y 123.35 | X 112.10–112.40, Y 124.45–124.75 | 0.30 mm |
| MK1, MSM261DGT003 | X 102.885, Y 111.56 | X 103.45–103.85, Y 112.95–113.35 | 0.40 mm |

In the current top view, both dots identify the lower-right pin-1 corner. They are outside the package bodies so they remain visible after assembly. U1's original four equal corner graphics did not distinguish pin 1; MK1's original tiny circle lies inside its package outline. These original footprint graphics remain, with the external dots providing visible orientation.

The added dots are board graphics. If either component moves or rotates, move its dot with it.

Verified 14 October 2026: every reference field hidden; exactly two added pin-1 polygons with the stated dimensions; existing track/via geometry and pad connections/geometry unchanged. Saved-board DRC reports zero errors, zero unconnected items, zero schematic parity differences, and no silkscreen violations. Two existing library mismatch warnings remain for U1 and J3.

The separate board-text labels C12 and J1 have also been removed. Hiding reference fields alone did not affect these manually placed labels. The only remaining standalone front silkscreen text is MIC and MIC HAT.

MIC and MIC HAT have also been removed at the user's request. No standalone front silkscreen text remains; orientation marks and component outlines remain.

Rechecked 18 October 2026 after the relayout (commit cf38929), which added D1, D2, R9, R10, R11, C13, C14 and FB1. All 34 reference fields are hidden. U1 and MK1 did not move, so both pin-1 dots stay at the positions in the table above. There is no standalone board text. Saved-board DRC shows no silkscreen violations; the only warnings are the two library mismatches for U1 and J3.
