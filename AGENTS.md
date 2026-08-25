# AGENTS.md

## Project overview

This repository contains the authoritative Altium Designer sources, reusable component
libraries, multi-board integration, generated review documents, and manufacturing outputs for
SensWear V1R1 hardware. Hardware edits can affect safety, manufacturability, firmware pin maps,
drivers, test fixtures, and shipped boards.

Read `README.md` before modifying a board. Use a compatible Altium Designer version and open
`SensWear_V1R1.DsnWrk` for system-level work.

## Repository map

- `SensWear-V1R1-Main/`: nRF54L15 main board, power, memory, IMU, RGB, and daughter interface.
- `SensWear-V1R1-Debug/` and `SensWear-V1R2-Debug/`: SWD/UART/debug boards.
- `SensWear-V1R1-Haptic/`: haptic daughter board.
- `SensWear-V1R1-PPG/`: optical PPG daughter board.
- `SensWear-V1R1-Temperature/`: temperature daughter board.
- `SensWear-V1R1-Touch/`: touch daughter board.
- `SensWear-V1R1-Panel/`: sensor-panel and panel manufacturing project.
- `SensWear_V1R1/`: Altium multi-board integration project.
- `SensWearComponentsLibrary/`: shared schematic and PCB libraries.
- `Templates/`: Altium templates, rules, logos, and pad/via templates.
- `Assets/` and `Resources/`: supporting/reference assets; not automatically authoritative.

Within each board, `.PrjPcb`, `.SchDoc`, `.PcbDoc`, harnesses, rules, libraries, and `.OutJob`
files are sources. `Output/`, `Project Outputs for .../`, generated PDFs, Gerbers, drill files,
BOMs, pick-and-place files, reports, and fabrication archives are derived snapshots.

## Editing rules

- Modify Altium source documents, not generated PDFs/Gerbers/BOMs alone.
- Use the shared component library where appropriate. A library change can affect several
  boards; identify every consumer before editing symbols, footprints, models, or pin mappings.
- Preserve designators, net names, harness names, differential-pair names, classes, layer-stack
  intent, variants, and output-job paths unless the change explicitly requires migration.
- Do not bulk rewrite Altium files with generic text formatters. Many are structured or binary
  and should be changed through Altium.
- Do not rename/move projects or output directories without updating workspace, project,
  multi-board, output-job, and documentation references.
- Never treat files in `History/`, `__Previews/`, or project logs as design sources.

## Required electrical review

Before accepting a schematic or PCB change, check as applicable:

- absolute maximum ratings, voltage domains, current and thermal margins;
- power-up/down sequencing, battery charging/protection, and regulator stability;
- MCU pin function, drive strength, pull state, boot/debug behavior, and level compatibility;
- I2C/SPI/UART addresses, pull-ups, bus loading, chip selects, and interrupt polarity;
- decoupling, grounding, return paths, analog/optical isolation, and sensitive routing;
- connector pinout, mating orientation, daughter-board compatibility, and mechanical clearance;
- footprint land pattern, pin-1 orientation, package revision, 3D/mechanical fit, and assembly;
- controlled impedance, stack-up, copper weight, vias, creepage/clearance, and fabrication limits.

Do not guess component ratings, footprints, or manufacturer part data. Use the approved
datasheet/library source and record assumptions.

## Validation in Altium

For every affected project:

1. Compile/validate the project and resolve new errors.
2. Run electrical rule checks for schematic changes.
3. Run design rule checks for PCB changes.
4. Review connectivity and cross-probes between schematic and PCB.
5. Inspect 2D/3D placement, board outline, connectors, and clearances.
6. For system changes, update and validate the multi-board project.

There is no trustworthy headless test command in this repository. Do not claim ERC/DRC or
manufacturing validation unless it was actually run in Altium and the reports were inspected.

## Manufacturing outputs

Regenerate affected output jobs only after source validation. Before release:

- inspect Gerbers and drill files in an independent viewer;
- compare BOM designators/quantities/MPNs with the schematic and approved alternates;
- inspect pick-and-place origin, rotation, side, and units;
- inspect assembly drawings, paste/mask, panel rails, tooling holes, fiducials, and V-score/tab
  details;
- verify the fabrication archive belongs to the intended revision.

Generated manufacturing files are revision-specific. Do not assume the newest dated archive is
approved, do not overwrite a known release casually, and do not place an order or send files to
a manufacturer without explicit authorization.

## Cross-repository coordination

Hardware is the source of truth for physical pins, buses, addresses, sensors, and electrical
capability. When these change:

- update firmware board DTS/pinctrl, shield overlays, Kconfig, drivers, and bring-up tests;
- update documentation describing available modules and hardware revisions;
- update SDK/mobile behavior only if the public BLE capability or data semantics changes;
- identify backward compatibility between board revisions.

Conversely, confirm firmware assumptions against schematics before changing sensor addresses,
interrupt polarity, power control, or daughter-board detection.

## Security and safety

- Do not add confidential vendor files, credentials, serial-number lists, or unlicensed models.
- Treat battery, charging, skin-contact, optical, thermal, and haptic changes as safety-relevant.
- Preserve unrelated working-tree changes and large generated artifacts.
- Do not delete old manufacturing releases unless explicitly requested and recovery is assured.

## Completion checklist

- Authoritative sources—not only outputs—were updated.
- Affected projects compile and relevant ERC/DRC checks were run, or the limitation is stated.
- Shared-library and multi-board impacts were reviewed.
- Required outputs were regenerated and independently inspected when release artifacts changed.
- Firmware pin/bus/driver implications are documented and synchronized when in scope.
- The change summary names affected board revisions and validation performed.
