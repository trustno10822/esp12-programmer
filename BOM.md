# Bill of Materials — ESP12 Programmer

Minimal parts list for a compact, single-sided toner-transfer PCB.  
All SMD resistors and capacitors are **0805** (hand-solderable).  
Through-hole parts are used where they improve reliability on home-etched boards.

| Ref  | Qty | Value / Part       | Package       | Description                                     |
|------|-----|--------------------|---------------|-------------------------------------------------|
| U1   | 1   | CH340G             | SOP-16        | USB-to-UART bridge (12 MHz crystal required)    |
| U2   | 1   | AMS1117-3.3        | SOT-223       | 3.3 V LDO regulator, 1 A                        |
| Y1   | 1   | 12 MHz crystal     | HC-49S or SMD | Clock for CH340G                                |
| J1   | 1   | USB Type-A female  | Through-hole  | USB host connector (fits toner-transfer pads)   |
| J2   | 1   | 2 × 8 pin 2 mm female header | Through-hole | ESP-12E/F socket                   |
| C1   | 1   | 10 µF / 16 V       | Radial 2.5 mm | Bulk cap on USB 5 V rail                        |
| C2   | 1   | 10 µF / 10 V       | Radial 2.5 mm | Bulk cap on 3.3 V rail                          |
| C3   | 1   | 100 nF             | 0805          | Bypass cap for CH340G VCC                       |
| C4   | 1   | 100 nF             | 0805          | Bypass cap for AMS1117 IN pin                   |
| C5   | 1   | 22 pF              | 0805          | Crystal load cap 1                              |
| C6   | 1   | 22 pF              | 0805          | Crystal load cap 2                              |
| R1   | 1   | 10 kΩ              | 0805          | GPIO0 pull-up to 3.3 V                          |
| R2   | 1   | 10 kΩ              | 0805          | EN (CH_PD) pull-up to 3.3 V                     |
| R3   | 1   | 10 kΩ              | 0805          | RST pull-up to 3.3 V                            |
| R4   | 1   | 10 kΩ              | 0805          | GPIO15 pull-down to GND                         |
| R5   | 1   | 10 kΩ              | 0805          | GPIO2 pull-up to 3.3 V                          |
| R6   | 1   | 470 Ω              | 0805          | Power LED current limiter                       |
| LED1 | 1   | Red LED            | 0805          | Power-on indicator                              |
| SW1  | 1   | Tactile push button | 6 × 6 mm THT | PROG — pulls GPIO0 LOW                          |
| SW2  | 1   | Tactile push button | 6 × 6 mm THT | RESET — pulls RST LOW                           |

## Notes

- **CH340G** is the preferred IC: widely available, cheap, and well-supported on all OSes.  
  If you want to skip the crystal, substitute **CH340C** (identical pinout, internal oscillator — omit Y1, C5, C6).
- **AMS1117-3.3** can supply up to 800 mA continuous (package limited). More than enough for ESP-12.
- **USB connector**: a right-angle Type-A female (e.g. Molex 67068) sits flush with the PCB edge and is easy to solder to wide toner-transfer pads.
- Resistors and capacitors are 0805 for ease of hand-soldering. 1206 also fits the same footprint.
