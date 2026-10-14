# BirdNET microphone HAT - prototype release 2026-10-14

Source revision: `945164dfe8b7c11e6e93898689d7ac028b0bccf1` (KiCad 10.0.7).
Build variant: mono, 23 fitted parts (22 top, J3 bottom); C3, C11, R2 not fitted.

**Release status: INCOMPLETE for production/order approval.** Local fabrication and assembly artifacts are generated and checked. ADC sourcing, assembler placement preview and final order options remain open. This is a prototype/quotation handoff, not an approved production order. Nothing has been uploaded or ordered.

## Files to use

- `Fabrication-birdnet-mic-hat.zip`: only the 11 Gerber layers, two drill files and Gerber job. This is the PCB fabrication input.
- `BOM-birdnet-mic-hat.csv`: assembly BOM with explicit references, quantities, selected MPNs and LCSC codes where verified.
- `CPL-birdnet-mic-hat.csv`: both-side placement file in millimetres; 23 fitted designators.
- `Assembly-placement.pdf`: top/bottom locator drawings with pad-1 markers. Bottom drawing is viewed from the bottom; CPL is kept in native KiCad export coordinates.
- `Schematic-birdnet-mic-hat.pdf`: complete one-sheet schematic.
- `PCB-fabrication.pdf`: native fabrication-layer reference plot.
- `DNP.csv`, `Sourcing.csv`: population exclusions and dated procurement references.
- `Source-945164d.zip`: exact committed KiCad project, libraries and design notes.
- `evidence/`: raw exports/checks for traceability; do not use its native BOM/CPL for assembly.
- `manifest.json`, `SHA256SUMS`: accepted file inventory, sizes and hashes.

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

## Verification and deliberate limitations

- Direct DRC: zero errors, zero unrouted items, zero schematic-parity violations. Two `lib_footprint_mismatch` warnings remain for U1/J3. Retained board instances are authoritative for this prototype export; do not refresh footprints from the library without rechecking and re-exporting.
- ERC: zero errors and zero warnings.
- Konnect manufacturing preflight: READY, no reported issues, with direct DRC evidence. This narrow result does not establish assembly, sourcing or order readiness.
- GerbView inspection: all 11 Gerber layers and both drill outputs load. Top copper/mask/silk, bottom copper/mask/paste, top paste, both inner layers and hole/outline registration inspected. No obvious export registration or outline defect observed. The ZIP importer skips the Gerber job as expected; that file opens separately. Bottom silk is intentionally empty. No removed C12/J1/MIC/MIC HAT text reintroduced.
- Schematic and both assembly locator pages rendered and visually checked. Native fabrication plot is supplemental.
- BOM and CPL reference sets match exactly: 23 fitted, no duplicates; C3/C11/R2 excluded. Both sides accounted for.
- Exporter defect: native BOM/CPL included DNP parts even when exclusion was requested. Release CSVs were derived from the explicit schematic DNP inventory, removing C3/C11/R2 and expanding references. Raw originals are retained only as evidence.
- Passive procurement details were added to the release BOM only; source circuit and library files were not changed. Check effective MLCC capacitance under operating bias, particularly the LDO input/output capacitors, during final component approval.
- CPL uses native pad-box midpoints, millimetres, absolute X and inverted Y, native rotations and top/bottom names. No empirical rotation corrections were applied. **PREVIEW_REQUIRED**: inspect all 23 parts in JLCPCB Component Placements, particularly U1, U2, Q1, MK1, J1 and bottom J3, comparing physical pin 1 with the supplied assembly drawings. Native export success does not validate vendor model orientation.
- U1 exact MPN TLV320ADC5140IRTWR is confirmed by TI, but a current exact LCSC listing could not be verified. Its LCSC field is deliberately blank. Arrange assembler global sourcing/consignment or resolve an exact catalogue match before approving assembly. TLV320ADC5140IRTWT / C1852018 is a TI-confirmed different reel size of the same device and is a sourcing option only; LCSC listed it out of stock at lookup. No substitution is selected here.
- Other LCSC catalogue matches are documented with URLs; this is not confirmation of JLCPCB assembly-library stock, basic/extended category, price or feeder availability.
- Jack switch mapping and insertion detector require a real-jack prototype continuity test; R6 can be removed to isolate detection. Enable MICBIAS and allow settling before reading GPIO48; inserted is HIGH, debounce in firmware. See source JACK-DETECTION.md.
- Enclosure/XIAO stack height and microphone acoustic clearance were not newly physically verified for this release. Keep the top-port microphone opening clear; do not change J1 or J3 height/version without mechanical review.

## Final order review

Resolve ADC sourcing; approve all vendor part matches and rotations; confirm thickness/copper/finish, assembly service/panelization and quantities; review vendor Gerber/assembly previews and DFM/stencil feedback; then authorize the prototype order. The archive is reproducible source-linked handoff material, not automatic order approval.

## Vendor authorities checked 2026-10-14

- https://jlcpcb.com/help/article/bill-of-materials-for-pcb-assembly
- https://jlcpcb.com/help/article/pick-place-file-for-pcb-assembly
- https://jlcpcb.com/help/article/advice-for-bom-and-cpl-files-preparation
- https://jlcpcb.com/capabilities/pcb-capabilities
- https://jlcpcb.com/capabilities/pcb-assembly-capabilities
- https://www.ti.com/product/TLV320ADC5140/part-details/TLV320ADC5140IRTWR
- Exact procurement catalogue URLs are in Sourcing.csv. Availability is rechecked when ordering.
