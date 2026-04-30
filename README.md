# Bondtech INDX — Integration Reference

The Bondtech INDX is a modular, mechanical tool-changing platform for FDM/FFF 3D printers.
Unlike filament-swapping systems, INDX physically swaps entire tool assemblies — nozzle,
heat break, and filament path — enabling true multi-material, multi-nozzle, and
multi-application printing.

This repository is the **integration reference** for hardware designers and printer builders
who want to incorporate INDX into their own platforms.

---

## What is INDX?

Each INDX tool is a self-contained print head that docks and undocks from the toolhead
carriage via a Y-axis approach. The carriage carries a motorised latch — the
**Dynamic Dual Drive** — that locks and releases tools mechanically without additional
wiring per tool.

Key properties:

- Multiple tools on a single carriage (number depends on printer size)
- Tool swap is fully mechanical; no per-tool electronics or connectors to mate
- The latch is driven by the main extruder motor; locking and unlocking are extruder moves
- Each tool has its own XY and Z offset, calibrated once and stored in firmware

---

## Repository contents

| Path | Description |
|------|-------------|
| `CAD/` | Reference STEP geometry for integration |
| `docs/` | Removed for now |

---

## CAD

`CAD/INDX_simplified_1.1.step` contains a simplified assembly of the INDX system
suitable for clearance checks, dock geometry layout, and integration modelling.

---
