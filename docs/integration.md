# Integration overview

This document covers the key constraints and geometry a printer designer must
account for when integrating INDX.

---

## Docking axis

INDX tools dock and undock along the **Y axis**. The toolhead carriage approaches
each dock from the front (positive Y toward the dock) and the tool seats at a
fixed `dock_y` coordinate.

Three Y positions matter for every tool:

| Position | Description |
|----------|-------------|
| `dock_y` | Tool fully seated — this is where the tool lives when not in use |
| `trigger_y` | `dock_y + 5 mm` — the point at which the latch engages or disengages |
| `clearance_y` | Printer-specific — the Y coordinate from which the carriage can travel in X without clipping any docked tool |

`clearance_y` is the most important dimension to establish early. It must clear the
bodies of **all** docked tools across the full X range of the carriage.

---

## Per-tool X position

Each tool has a fixed X coordinate (`tool.x`) that aligns the latch with the tool
body. The carriage moves to `(tool.x, clearance_y)` before any Y approach, so
the X positions of adjacent tools must allow for this lateral travel.

---

## Z offsets

Every tool will have a slightly different nozzle height due to manufacturing
tolerances in the tool body and tip. Each tool stores an individual Z offset
that is applied after pickup, keeping the nozzle at the correct print height
regardless of which tool is active.

A single machine-wide **global Z offset** is also maintained. It captures the
Z reference shared by all tools and persists across reboots.

---

## The latch mechanism

The latch is driven by the extruder motor on the carriage. Locking and unlocking
are extruder moves — no dedicated actuator is needed per tool.

- **Locking**: a forward extrude move engages the gear mesh and clamps the tool
- **Unlocking**: requires a short wiggle sequence (Y + E moves) to ensure the
  activation gear comes into mesh with the motor before the carriage can pull away;
  see the firmware documentation for details

Because the extruder motor is also used for filament drive, the latch must be
fully locked before any print move and fully open before any pickup.

---

## Clearance envelope

When designing docks and the surrounding structure, account for:

1. **Peel distance** — after seating or picking up a tool, the carriage makes a
   short relative Y move (~10 mm) before sprinting to `clearance_y`. The dock
   structure must not obstruct this exit path.
2. **Z hop** — firmware raises the nozzle during the toolchange approach. The
   dock height and any structure above the tool must allow for this vertical
   travel.
3. **Carriage width** — the carriage must not foul adjacent docked tools during
   the lateral X move to each tool's X position.
