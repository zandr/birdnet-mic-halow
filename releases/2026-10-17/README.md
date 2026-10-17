# BirdNET microphone HAT - prototype release 2026-10-17

Source revision: `d093773228fe31281557e0ddf73387ee0721a594`. Tag: `v2026.10.17`. KiCad 10.0.7.

This package supersedes the October 14 exports with the cleaned routing and zero-via PDM signal routes. It is published for prototype review and quotation. **Production/order approval remains INCOMPLETE:** ADC sourcing, JLCPCB placement preview and final order choices remain outstanding. GitHub publication is not a fabrication order.

## Included files

- `Fabrication-birdnet-mic-hat.zip`: the 11 current Gerber layers, PTH/NPTH drill files and Gerber job. Use this archive for PCB fabrication, not the entire release archive.
- `Schematic-birdnet-mic-hat.pdf`: complete schematic.
- `Assembly-placement.pdf`: regenerated top/bottom pad and reference locator drawings with pin-1 markers.
- `PCB-fabrication.pdf`: supplemental native fabrication-layer plot.
- `BOM-birdnet-mic-hat.csv` and `CPL-birdnet-mic-hat.csv`: mono build, exactly 23 fitted parts; 22 top and J3 bottom.
- `DNP.csv`: C3, C11, R2 omitted from the fitted BOM/CPL.
- `Sourcing.csv`: selected procurement parts, URLs and their original verification dates (2026-10-14). Availability was not revalidated for this release.
- `Source-d093773.zip`: exact committed project, libraries, design notes and validation history; older release archives excluded.
- `evidence/`: fresh checks and raw exporter outputs, plus design notes.
- `manifest.json` and `SHA256SUMS`: file sizes, provenance and integrity hashes.

## Changes since the previous package

- Simplified supply and signal routing, preserving the manual refinements.
- Aligned the C7 supply via and removed a redundant supply via.
- PDM_DATA: four signal vias to zero, 15.983 to 13.197 mm.
- PDM_CLK: three signal vias to zero, 20.413 to 17.107 mm.
- Both PDM signals run entirely on F.Cu. One GND stitch at C11 maintains plane connectivity; total board via count is 45.
- Footprint positions and pad nets remain unchanged.

## Fabrication / quotation contract

| Item | Specification / state |
| --- | --- |
| Fabricator target | JLCPCB; no service or order submitted |
| Layers, top to bottom | F.Cu / In1.Cu / In2.Cu / B.Cu |
| Material / thickness | FR-4, nominal 1.6 mm |
| Outline envelope | 17.791 x 22.689 mm (Edge.Cuts centreline); notch and rounded outline per Gerber |
| Copper | Saved stackup nominal 35 um on all four copper layers; confirm vendor offering before order |
| Dielectric stackup | Saved nominal three 0.48 mm FR-4 dielectrics; vendor stackup acceptance still required |
| Impedance | No controlled-impedance specification |
| Finish | Not selected in source; ENIG is a proposed quotation option, not an approved order selection |
| Mask / legend | Green mask and white legend proposed for quotation; source preserves no reference text on silk |
| Assembly | Both sides; J3 only on bottom. Confirm service supports these parts and board size/panelization |
| Stencil | Both paste layers included; assembler to review tooling and U1 exposed-pad stencil treatment |
| Quantity / panel | Not specified; vendor tooling/panel and quote required |
| Drill | Separate metric PTH and NPTH; two 1.1 mm jack locating holes in NPTH |

Do not assume JLCPCB defaults match the saved stackup. Vendor rules, stock and service availability must be confirmed against the final quote.

## Checks and remaining reviews

Fresh direct DRC after live refill/save: zero errors, zero unconnected items, zero schematic-parity violations. The two existing U1/J3 library-mismatch warnings are retained; board footprint instances are authoritative for this prototype. Do not refresh them without rechecking and re-exporting.

Fresh ERC: zero errors and zero warnings. Manufacturing preflight: READY with no reported issues. This narrow automated verdict does not establish production or assembly readiness.

The new 14-file fabrication inventory is nonempty and layer names match the requested four copper, two mask, two silk, two paste and one outline layers plus separate PTH/NPTH drills and job metadata. Gerber/drill visual inspection is recorded in `evidence/viewer-review.md`. Schematic and both assembly pages were rendered for inspection.

The native exporter still includes DNP parts. The final CPL was filtered using the fresh schematic DNP inventory; the previously curated procurement BOM was retained only after the complete fresh source inventory matched the previous one exactly. BOM and CPL fitted designator sets match: 23 parts, no duplicates. Use the root BOM/CPL, not the raw files under evidence.

CPL orientation remains **PREVIEW_REQUIRED**. Coordinates are millimetres, native pad-box midpoints, native X/inverted-Y and top/bottom layers, with native rotations. No guessed rotation corrections were applied. Inspect every component in JLCPCB Component Placements, particularly U1/U2/Q1/MK1/J1 and bottom J3, before authorizing assembly.

U1 is TLV320ADC5140IRTWR. Its exact LCSC listing remains unverified; the BOM LCSC field is intentionally blank. Resolve assembler sourcing or consignment before approval. Other catalogue part selections are unchanged from October 14 and do not establish current JLCPCB stock or service availability.

Jack-switch mapping and insertion detection still require a real-jack continuity/prototype test. R6 removal isolates detection. Enable MICBIAS, allow settling, then read GPIO48; inserted is HIGH. Physical enclosure/XIAO stack and acoustic clearance were not newly verified. Keep the top-port microphone opening clear and retain the exact J1/J3 versions.

Before ordering, confirm quantity, finish, stackup/copper, assembly service and any vendor panelization/tooling requirements; complete placement preview and vendor DFM/stencil review. The source remains four-layer FR-4, nominal 1.6 mm; no controlled impedance is specified.

Vendor format authorities carried forward from the October 14 preparation:
- https://jlcpcb.com/help/article/bill-of-materials-for-pcb-assembly
- https://jlcpcb.com/help/article/pick-place-file-for-pcb-assembly
- https://jlcpcb.com/capabilities/pcb-capabilities
- https://jlcpcb.com/capabilities/pcb-assembly-capabilities

No fabrication order, parts purchase or assembly approval is implied by this package.
