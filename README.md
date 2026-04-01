# ESP12 Programmer

A **minimal, compact USB-to-serial programmer** for the ESP-12E/F module.  
Single-sided copper PCB — designed specifically for the **toner-transfer** DIY etching method.

---

## Features

- USB to UART via **CH340G** (widely available, cheap)
- **AMS1117-3.3** LDO supplies clean 3.3 V to the module
- **PROG** button holds GPIO0 LOW during reset to enter flash mode
- **RESET** button for hardware reset
- All resistors, capacitors and ICs fit on one side; all copper traces on the **bottom layer only**
- Board footprint ≈ **50 × 30 mm** — small enough to print two copies on one A4 sheet
- Minimum trace width **0.8 mm**, minimum clearance **0.6 mm** — friendly for home etching

---

## Quick-start

1. Plug in via USB — the OS will enumerate a serial port (CH340 driver required on Windows/macOS).
2. Seat the ESP-12E/F module on the 2 × 8 pin header.
3. Hold **PROG**, tap **RESET**, release **PROG** — the module is now in bootloader mode.
4. Flash with `esptool.py` or your IDE of choice.
5. Tap **RESET** to run the new firmware.

```
esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash 0x0 firmware.bin
```

---

## Schematic overview

```
USB (5 V)
  │
  ├── 5 V ──┬── [C1 10 µF] ──┐
  │         │                GND
  │      [AMS1117-3.3]
  │         │ OUT
  │         ├── [C2 10 µF] ──┐
  │         │                GND
  │        3.3 V ─────────────────────────────────────────────┐
  │                                                            │
  ├── D+ ──────────── CH340G D+                        ESP-12E │
  └── D- ──────────── CH340G D-                    ┌──────────┤
                       │                           │ VCC ◄────┤ 3.3 V
              [C3 100 nF] to GND                   │ GND      │
                       │                           │          │
                CH340G TXD ──────────────────────► │ RXD      │
                CH340G RXD ◄────────────────────── │ TXD      │
                       │                           │          │
                       │         RESET ── [R3 10k]─┤ RST      │
                       │           └── [SW2] ──────┤ (button) │
                       │                           │          │
                       │         PROG  ── [R1 10k]─┤ GPIO0    │
                       │           └── [SW1] ──────┤ (button) │
                       │                           │          │
                       │                  [R2 10k]─┤ EN/CH_PD │
                       │                           │          │
                       │               GND──[R4 10k]─ GPIO15  │
                       │                           │          │
                       │                  [R5 10k]─┤ GPIO2    │
                       └───────────────────────────┘ GND      │
                                                    └──────────┘
```

> **GPIO15** must be pulled **LOW** to boot from internal flash.  
> **EN (CH_PD)** and **GPIO2** must be pulled **HIGH** for normal operation.

---

## Bill of materials

See [BOM.md](BOM.md) for the full, sourcing-ready parts list.

---

## PCB / toner-transfer tips

See [pcb/layout-notes.md](pcb/layout-notes.md) for single-sided layout rules and step-by-step toner-transfer instructions.

---

## Schematic detail

See [schematic/schematic.md](schematic/schematic.md) for the complete annotated schematic with pin numbers and net names.

---

## Licence

Released into the public domain — do whatever you like with it.
