---
name: logix-ton-one-per-timer
description: >
  Studio 5000 / CompactLogix / ControlLogix rule: one TON (or TOF/RTO) per TIMER tag.
  A TON whose rung is false resets that timer every scan (EN=0, ACC=0).
  Use when writing or editing Logix AOI/program L5X, pulse generators,
  holdTmr, rampTmr, OffTmr, TON, TOF, RTO, TIMER, ladder rungs, or
  electrode/HV/analog pulse timing. Use when the user runs /logix-ton.
license: MIT
metadata:
  author: Andrea Zaccagnino
  short-description: One TON/TOF/RTO per Logix TIMER tag
---

# One TON per TIMER tag

A Logix TIMER tag (CompactLogix, ControlLogix, Studio 5000) may have **one** TON, TOF, or RTO. A second instruction on the same tag, even on another rung or in another state, **resets** the timer whenever its rung is false.

## Do this

- One TON per TIMER. Extra states: keep that TON enabled, load a new `.PRE`, `RES` on `.DN`.
- TON rung: only `XIC`/`XIO` then `TON(tmr,?,?)`. No `EQ`, `OTL`, `MOVE` on that rung (L5X import often leaves TON EnableIn false).
- Before shipping pulse/timer logic, watch the **instance** `.EN` and `.ACC` with the enable on. `.EN` = 0 means the TON is not running or is being reset.

## Proven pattern

```
XIC(enable)TON(holdTmr,?,?);
```

State rungs only OTL/OTU the output. On `.DN`, MOVE next PRE, RES, change state.

Same for TOF and RTO: one instruction per TIMER tag.

## How it failed (field, 2026-11-05/06)

An AOI used two `TON(holdTmr)` (ON and OFF) and two `TON(rampTmr)` (up and down). The false TON cleared ACC every scan. State stuck at 1; the analog ramp never finished. Cost ~4 hours of machine time. Fix: one TON enabled by the pulse enable, PRE/RES on state change.
