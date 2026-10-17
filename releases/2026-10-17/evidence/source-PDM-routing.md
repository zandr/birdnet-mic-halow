# PDM front-layer routing - 2026-10-16

Both PDM nets now run exclusively on F.Cu at their existing 0.15 mm width. PDM_DATA's four vias and PDM_CLK's three vias have been removed. PDM_DATA crosses beneath the jack body, avoiding all contact pads and locating holes. PDM_CLK takes a local detour around the microphone's ground pads and the optional analog-input components. No trace keepout was present in the jack footprint or board rule areas; its courtyard was not treated as a copper keepout.

The new routes initially isolated a piece of the front ground pour around C11. The clock path was adjusted to preserve its thermal connections, and one 0.45/0.25 mm GND stitching via was added at (106.900, 119.300) mm with a short connection to C11 ground. This joins that copper to the inner ground plane. Total via count decreases from 51 to 45; both PDM nets have zero vias.

Refilled and saved the live board before DRC: zero errors, zero unconnected items and zero schematic-parity violations. The existing U1/J3 footprint-library mismatch warnings remain. Readback confirms all footprint positions and pad nets are preserved, the seven removed vias are exactly the old PDM vias, and the only added via is GND. See the adjacent `verification.json` and `drc.json`.

The existing manufacturing ZIP remains an archive of its older documented revision and must be regenerated before manufacturing this routing.
