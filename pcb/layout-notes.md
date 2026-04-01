# PCB Layout Notes — Single-Sided Toner Transfer

## Design rules (use these in KiCad / EasyEDA)

| Rule                | Value      | Reason                                         |
|---------------------|------------|------------------------------------------------|
| Minimum trace width | 0.8 mm     | Thinner traces lift during ironing             |
| Preferred trace width (power) | 1.5 mm | Reduces voltage drop and etch risk         |
| Minimum clearance   | 0.6 mm     | Reliable with laser printer toner             |
| Via drill           | 0.8 mm     | Smallest bit most hobbyists have              |
| Via annular ring    | 0.4 mm     | Total pad = 1.6 mm diameter                   |
| Copper layer        | **Bottom only** | One-sided board                          |
| Board dimensions    | 50 × 30 mm | Fits two copies on A4 for toner transfer       |
| Corner radius       | 2 mm       | Optional, reduces sharp-corner cracking        |

> **No top copper layer.**  All traces and pads are on the **bottom (B.Cu)** layer.  
> Silk screen markings are fine on the top, but not required for function.

---

## Component placement guidelines

1. **USB connector (J1)** — mount flush with the left edge of the board.  The wide pads (≥ 2 mm wide) survive ironing well.
2. **CH340G (U1)** — centre-left area.  Keep the crystal (Y1) within 5 mm of pins 7 and 8 with short, direct traces.
3. **AMS1117-3.3 (U2)** — top-right corner, tab facing the board edge for heat dissipation.
4. **Electrolytic caps (C1, C2)** — standing radial, place near their respective supply pins.
5. **ESP-12 header (J2)** — right-hand side, 2 mm pitch.  Verify the module polarity before soldering.
6. **Buttons (SW1, SW2)** — bottom edge of the board, labelled PROG and RESET.
7. **LED1 + R6** — top edge, visible when board is inserted into a USB port.
8. **SMD passives** — fill gaps between the ICs; orient all 0805 pads parallel to the nearest trace to ease routing.

---

## Toner transfer — step by step

### What you need
- Laser printer (any brand)
- Glossy magazine paper, photo paper, or dedicated PCB transfer paper
- Clothes iron or laminator
- Single-sided copper-clad board (1.6 mm FR4 or phenolic)
- Ferric chloride or ammonium persulphate etchant
- Acetone or IPA for toner removal
- Drill press or hand drill with 0.8 mm and 1.0 mm bits

### Steps

1. **Print** the bottom copper layer mirrored on glossy paper at 1:1 scale (100 % — no scaling).  
   Print two copies in case one tears.

2. **Clean** the copper surface with fine steel wool or 400-grit sandpaper, then wipe with acetone.  Do not touch the copper with bare fingers afterward.

3. **Align** the printed side face-down on the copper.  Tape one edge so it cannot shift.

4. **Iron** at cotton/linen setting (≈ 180 °C) for 3–5 minutes with firm, circular pressure.  Do not slide the iron — press and lift.  
   *Laminator alternative:* pass through 5–6 times at maximum temperature.

5. **Soak** in warm water for 5 minutes, then gently rub the paper away.  The toner stays on the copper.

6. **Touch up** any broken traces with a permanent marker (Staedtler Lumocolor or similar etch-resist pen).

7. **Etch** in ferric chloride at 40–50 °C, agitating gently, until all bare copper is removed (5–15 minutes).  
   *Safety: wear gloves and eye protection.  Dispose of spent etchant per local regulations.*

8. **Remove toner** with acetone, then rinse.

9. **Drill** all through-hole pads:  
   - 0.8 mm — IC pads, crystal, SMD via holes  
   - 1.0 mm — resistors, capacitors, USB connector, buttons  
   - 1.5 mm — ESP-12 header (2 mm pitch, slightly larger clearance)

10. **Tin** the copper surface with a thin coat of solder to prevent oxidation.

11. **Solder** SMD components first (0805 passives, U1 CH340G, U2 AMS1117), then through-hole parts.

12. **Inspect** under magnification for solder bridges, especially the CH340G SOP-16 pins.

---

## Jumper wires (if needed)

A true single-sided layout of this circuit can be achieved without jumpers.  
If your etching result has one or two broken traces, a short 0-Ω 0805 resistor or a trimmed component lead soldered across the gap is the quickest fix.

---

## Layer export (KiCad)

When exporting Gerbers:

| File          | Layer           |
|---------------|-----------------|
| `.gtl`        | *unused* (no top copper) |
| `.gbl`        | Bottom copper (B.Cu) — **the only copper layer** |
| `.gbs`        | Bottom soldermask (optional for home etching) |
| `.gto`        | Top silkscreen (optional, not etched) |
| `.drl`        | Drill file — NC Drill format |
| `.gko`        | Board outline (Edge.Cuts) |
