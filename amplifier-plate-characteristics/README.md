# Pentode Amplifier with a Live Plate-Characteristics Display

The [screen-resistor amplifier](../amplifier/README.md) with the instrument
a datasheet can't print: a **plate-characteristics plot that moves**.

## The two datasheet plots (docs/6AU6A.pdf)

- **Page 3, "Average Plate Characteristics"**: Ip(Vp) for Ec1 = 0…−5 V at a
  *fixed* screen, Ec2 = 150 V. Moving between these curves is what the
  signal on the grid does.
- **Page 4, top**: Ip(Vp) for Ec2 = 150/125/100/75/50 V at Ec1 = 0. The
  whole family **compresses downward as the screen voltage falls**.

These two plots are often misread as alternatives. In a real RC stage with
an **unbypassed screen resistor they happen simultaneously**: rising plate
current sags Vp through R_L, rising screen current sags Vg2 through R_g2,
and the sagging screen slides your operating curve down the page-4 family —
*while the load line, set only by B+ and R_L, never moves.*

## What the display shows

- **Faint green reference family** — Ec1 = 0…−5 V at Vg2 = 150 V (the
  page-3 plot), pinned static.
- **Bright amber LIVE curve** — the tube's characteristic at the
  instantaneous (Vg1, Vg2), recomputed every frame from the same calibrated
  model the circuit solver uses. It swings with the grid (page-3 motion)
  and compresses with the screen (page-4 motion). At the demo defaults
  Vg2 swings ~50 V per cycle — the curve visibly breathes between the
  family's upper curves and its floor.
- **Static white LOAD LINE** — from (B+, 0) to (0, B+/R_L), clipped to the
  plot; redrawn only when you move the B+ or R_L sliders, deliberately
  frozen during the signal.
- **Operating-point dot** — at (Vp, (B+−Vp)/R_L), always on the load line,
  riding it at the intersection with the breathing curve.
- **Live Vg2 readout** — the number doing the compressing.

Axes: 0–500 V at 50 V/div, 0–4 mA at 0.5 mA/div (our RC-stage current
scale; the datasheet's 20 mA axis belongs to a fixed 150 V screen supply).

## Controls

Sliders as in the parent project: heater, B+ (300 V), R_L (100k, live
bands), R_g2 (470k, live bands), signal amplitude (default 2 Vpk), DC
offset (default −2 V). **Screen bypass capacitor: checkbox, default OFF**
— check it and the screen freezes at its average: the live curve stops
compressing and only slides between the green Ec1 lines (pure page-3
motion). **The suppressor is permanently connected in this build** (no
tetrode toggle). Scope, meters, and glass toggle inherited.

## Experiments

1. **Watch the compression**: defaults (bypass off, A = 2 V). The amber
   curve dives ~50 V worth of Vg2 every cycle; the dot slides along the
   unmoving white line.
2. **Freeze it**: check the bypass box — Vg2 pins, the curve stops
   breathing vertically and only steps with the grid, gain jumps (~4× →
   ~30×). This is what the capacitor is *for*.
3. **Move the load line** (the only way it moves): drag B+ or R_L — the
   white line pivots, the dot finds the new intersection, the curves are
   unaffected. Field lines vs circuit lines, cleanly separated.
4. **Deeper sag**: raise R_g2 toward 1M or raise the drive — the amber
   curve spends its lows nearer the family's floor.

## Files & running

- `plate_curves_sim.blend` — open, Text Editor → Run Script once
- `plate_curves_sim.py` — or `blender -P plate_curves_sim.py`
- Sidebar (N) → **Plate Curves** tab → Run / Pause
- `shots/` — bench, the max-sag/min-sag compression pair, and the
  bypassed (pinned) comparison

Engine notes: same verified 6AU6 electron simulation and algebraic
coupled-load-line solver as the parent; the display curves are drawn from
the solver's own live-calibrated companion model (the reference family with
a pinned space-charge term so it is truly static). Stateful sim: Reset,
don't scrub; one tube project per Blender session.
