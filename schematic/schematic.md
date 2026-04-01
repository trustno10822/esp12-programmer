# ESP12 Programmer — Annotated Schematic

## Net list

| Net name  | Description                                  |
|-----------|----------------------------------------------|
| VUSB      | USB 5 V supply                               |
| +3V3      | 3.3 V regulated supply                       |
| GND       | Common ground                                |
| USB_D+    | USB data plus                                |
| USB_D-    | USB data minus                               |
| UART_TX   | CH340G TXD → ESP RXD                         |
| UART_RX   | ESP TXD → CH340G RXD                         |
| ESP_RST   | Module reset line (active LOW)               |
| ESP_GPIO0 | Boot-mode select (LOW = serial bootloader)   |
| ESP_EN    | Module enable / chip power-down (active HIGH)|
| ESP_GPIO2 | Must be HIGH at boot                         |
| ESP_GPIO15| Must be LOW at boot (boot from flash)        |

---

## Full schematic (ASCII)

```
                         VUSB
                           │
   J1 (USB Type-A)        [C4 100n]     [C1 10µ]
   ┌──────────────┐         │               │
   │ Pin 1  VBUS ─┼──────── ┼ ─────────────┼── VUSB
   │ Pin 2  D-   ─┼── USB_D-│               │
   │ Pin 3  D+   ─┼── USB_D+│           ┌──┴──┐
   │ Pin 4  GND  ─┼── GND   │           │  U2  │  AMS1117-3.3
   └──────────────┘         │      VUSB─┤ IN   │
                                        │  OUT ├── +3V3
                                        │  GND ├── GND
                                        └──────┘
                                        [C2 10µ] on +3V3
                                        [C4 100n] on VUSB (already above)

   ┌──────────────────────── CH340G (U1, SOP-16) ────────────────────────┐
   │                                                                      │
   │ Pin  1  GND  ──────────────────────────────────────────── GND       │
   │ Pin  2  TXD  ──────────────────────────────────────────── UART_TX   │
   │ Pin  3  RXD  ──────────────────────────────────────────── UART_RX   │
   │ Pin  4  V3   ── [C3 100n] ── GND  (3.3 V bypass, tie to +3V3)       │
   │ Pin  5  UD+  ──────────────────────────────────────────── USB_D+    │
   │ Pin  6  UD-  ──────────────────────────────────────────── USB_D-    │
   │ Pin  7  XI   ── Y1 (12 MHz) ── Pin 8 XO, with C5/C6 22pF to GND    │
   │ Pin  8  XO   ── Y1 (12 MHz) ── Pin 7 XI                             │
   │ Pin 16  VCC  ──────────────────────────────────────────── +3V3      │
   └──────────────────────────────────────────────────────────────────────┘

   Note: CH340G V3 pin (pin 4) must be tied to VCC (+3V3) via a 100 nF cap
   when operating at 3.3 V.  Connect V3 directly to +3V3 and put C3
   between V3 and GND.

   ┌──────────────────────── ESP-12E/F (J2, 2×8, 2 mm) ──────────────────┐
   │                                                                       │
   │  VCC     ────────────────────────────────── +3V3                     │
   │  GND     ────────────────────────────────── GND                      │
   │                                                                       │
   │  RXD     ────────────────────────────────── UART_TX                  │
   │  TXD     ────────────────────────────────── UART_RX                  │
   │                                                                       │
   │  RST     ──── R3 (10 k) ── +3V3                                      │
   │          ──── SW2 ─────── GND        (RESET button)                  │
   │                                                                       │
   │  GPIO0   ──── R1 (10 k) ── +3V3                                      │
   │          ──── SW1 ─────── GND        (PROG / flash button)           │
   │                                                                       │
   │  EN/CH_PD──── R2 (10 k) ── +3V3                                      │
   │                                                                       │
   │  GPIO15  ──── R4 (10 k) ── GND       (boot from flash, must be LOW)  │
   │                                                                       │
   │  GPIO2   ──── R5 (10 k) ── +3V3      (must be HIGH at boot)          │
   └───────────────────────────────────────────────────────────────────────┘

   Power LED:
   +3V3 ── R6 (470 Ω) ── LED1 (anode) ── LED1 (cathode) ── GND
```

---

## Boot-mode truth table

| GPIO15 | GPIO0 | GPIO2 | Mode                  |
|--------|-------|-------|-----------------------|
| LOW    | HIGH  | HIGH  | Boot from flash (normal run) |
| LOW    | LOW   | HIGH  | UART download (flash firmware) |

**To enter flash mode:** hold SW1 (PROG), momentarily press SW2 (RESET), release SW1.

---

## Signal notes

- **UART_TX / UART_RX** — direct 3.3 V CMOS levels.  No level shifter needed because both CH340G and ESP-12 operate at 3.3 V.
- **RST and GPIO0** — open-drain friendly: the 10 kΩ pull-ups limit current when buttons are held, and the lines can also be driven by external logic without conflict.
- **EN (CH_PD)** — if the module is not responding, check this pin is firmly at 3.3 V; a weak pull-up can cause intermittent issues.
- **Crystal (Y1)** — HC-49S through-hole or SMD equivalent both work.  Place as close as possible to U1 pins 7 and 8 to minimise parasitic capacitance.
