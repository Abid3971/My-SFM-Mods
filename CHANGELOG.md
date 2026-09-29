# Changelog

## V4
- Graph and vibrator share one clock: Tease climbs 0-8s, holds to 12s, is quiet
  for 8s, and the graph follows it exactly.
- Graph is always drawn, flat on the bottom at 0 or off; it belongs to the
  vibrator only.
- Piston card rebuilt as a machine control: detent rail, LOW/MED/HIGH carriage,
  stroke bar pumping at the toy's real rate, carriage drops to OFF when off.
- PISTON chip names the toy's real speed; VIBRATOR chip names the sound band.
- Vibrator volume follows strength inside each band, faded, with hysteresis on
  the band change so fast dragging cannot pop.
- Sync both: piston follows the vibrator's pattern and phase, quantised.
- Per-toy pattern clocks; the piston no longer restarts the vibrator's pattern.
- One-screen page: FULL button, auto-fullscreen on Connect, address bar
  collapses on scroll.
- Cards are title + control only.
- Version metadata: BepInPlugin 4.0.0, assembly 4.0.0.0.

### Earlier builds

## 2.2.2
- Removed the per-card status rows; the chips carry both toys' values.

## 2.2.1
- Sync button renamed to "Sync both".
- Graph always drawn instead of hidden.
- Fixed popping when dragging fast (band hysteresis + distance-scaled fade).

## 2.2.0
- Removed the cyan piston trace (one pink vibrator graph).
- Piston rail recoloured to the site's pink palette; fullscreen control.

## 2.1.2
- Audio curve configurable ([Audio] LowBandFloor / HighBandFloor / FadeSeconds).

## 2.1.1
- Volume scales with strength inside each band, faded so 50/51 does not click.

## 2.1.0
- Per-toy pattern clocks; the graph reads each toy's real output.

## 2.0.5
- Baseline: phone-served page, pairing, both toys, four patterns, ecstasy.
