> **Superseded 2026-10-18:** the board was fully re-placed and re-routed (commit cf38929). The via and track details below describe the old layout only.

# Routing cleanup - 2026-10-16

Cleaned the latest manually edited board through Konnect and KiCad's native editor. Footprints and pad connectivity were preserved.

- Replaced short zigzags in the VCC_3V3 branches with continuous horizontal/vertical/45-degree runs. The routed inner supply is on In2.Cu; In1.Cu remains the ground-plane layer.
- Moved the C7 supply via from (107.000, 124.600) to (106.930, 125.270) mm, giving C7 a straight connection. Removed the redundant supply via at (105.850, 125.000) and its stub.
- Simplified detector, DREG, AVDD, microphone and I2S paths; normalized uneven diagonals and beveled the long AVDD/I2S inner-layer right-angle corners. Existing widths were retained.
- Repaired the baseline ADC_RST disconnect and removed its dangling MIC_TIP_N spur.
- Refilled zones, saved, and checked the resulting board: zero DRC errors, zero unrouted items, zero schematic-parity violations. Existing U1/J3 footprint-library mismatch warnings remain.
- Compared the saved result with the pre-cleanup editor snapshot: all 26 footprint positions/orientations, pad positions/sizes and pad nets preserved; one via moved and one removed (52 to 51). Routed centreline length decreased from 312.859 to 310.679 mm. Segment count increased from 223 to 234 because explicit 45-degree corner transitions replace arbitrary-angle segments.

Evidence: `validation/routing-2026-10-16/drc.json` and `geometry-comparison.json`.

The archived manufacturing package under `releases/2026-10-14/` remains tied to its documented older source revision. Regenerate fabrication outputs from this board before manufacturing; do not use that older ZIP for the cleaned routing.
