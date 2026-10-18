# Export review — 2026-10-18

GerbView was **not** used for this package. Opening `Fabrication-birdnet-mic-hat.zip` in KiCad GerbView (or the JLCPCB Gerber viewer) is still outstanding.

What was checked instead:

- **Gerber file functions.** The 11 Gerber layers are F.Cu (L1), In1.Cu (L2), In2.Cu (L3), B.Cu (L4), both masks, both pastes, both legends and the profile. Their headers match the job file. The job file records a 1.6 mm board and an ENIG finish.
- **Drill files.** PTH has one 0.25 mm tool with 45 hits, matching the board's 45 vias; there are no plated component holes. NPTH has one 1.1 mm tool with 2 hits, the J1 locating holes.
- **Paste.** F.Paste has 4 regions, the U1 exposed-pad windows; pad 25 has no full paste. B.Paste has 30 flashes, one for each J3 pad.
- **Silkscreen.** F.Silkscreen contains footprint outlines and the U1 and MK1 pin-1 dots, with no reference text. B.Silkscreen holds only a few outline draws.
- **Second export.** Konnect's live-snapshot JLCPCB export and kicad-cli's saved-file export produce identical Gerbers apart from timestamps.
- **Rendered PDFs.** The schematic PDF and both assembly-locator pages were rendered to PNG and inspected. Labels are readable, fitted and DNP parts are distinguishable, and the pin-1 markers sit on the correct pads. The bottom page is mirrored, as viewed from below.
- **Layout render.** An independent render of the routed layout from the saved board was inspected during routing.

This is a structural check, not vendor DFM or stencil approval. Before ordering, U1's exposed-pad paste and the plugged thermal vias still need assembler review.
