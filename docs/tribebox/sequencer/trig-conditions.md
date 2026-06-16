# TribeBox Trig Conditions

Status: **defined**

Trig Conditions define whether a sequencer step is allowed to play on a given pass.

They are separate from Probability, Retrig, Fill, and Parameter Locks.

---

## Evaluation Order

For each active step, playback is resolved in this order:

```text
1. Condition
2. Probability
3. Trigger playback
4. Retrig / ratchet generation
5. Parameter Locks and playback modifiers
```

If the condition fails, the step does not play and no retrig events are generated.

If the condition passes but probability fails, the step does not play and no retrig events are generated.

---

## Step Fields

Each step may contain:

```json
{
  "condition": "ALWAYS",
  "probability": 100,
  "retrig": {
    "enabled": false,
    "count": 2,
    "decay": 0,
    "length": "short"
  }
}
```

Default values:

```text
condition: ALWAYS
probability: 100
retrig.enabled: false
```

---

## Supported Conditions for v0.1

The initial supported condition set is intentionally small but expressive.

```text
ALWAYS
FILL
NOT_FILL
FIRST
NOT_FIRST
PRE
NOT_PRE
1:2
2:2
1:3
2:3
3:3
1:4
2:4
3:4
4:4
1:8
2:8
3:8
4:8
5:8
6:8
7:8
8:8
```

---

## Condition Definitions

### ALWAYS

The step is always eligible to play.

```text
condition: ALWAYS
```

This is the default state.

---

### FILL

The step is eligible only while Fill is active.

```text
condition: FILL
```

Useful for rolls, crashes, alternate hits, and one-cycle live changes.

---

### NOT_FILL

The step is eligible only while Fill is not active.

```text
condition: NOT_FILL
```

Useful for replacing a normal step with a Fill step.

Example:

```text
Step 16 normal hat → NOT_FILL
Step 16 crash      → FILL
```

---

### FIRST

The step is eligible only on the first cycle after the Pattern or Variation starts.

```text
condition: FIRST
```

Useful for pickups, one-time impacts, and transition markers.

---

### NOT_FIRST

The step is eligible on every cycle except the first one after Pattern or Variation start.

```text
condition: NOT_FIRST
```

Useful for introducing a part after the initial loop.

---

### PRE

The step is eligible only if the previous conditional trig on the same track played.

```text
condition: PRE
```

This allows simple dependent behavior without a complex generative engine.

---

### NOT_PRE

The step is eligible only if the previous conditional trig on the same track did not play.

```text
condition: NOT_PRE
```

Useful for alternate hits and call/response behavior.

---

### Ratio Conditions

Ratio conditions define cycle-based playback.

Example:

```text
condition: 1:4
```

The step is eligible on the first cycle out of four.

```text
1:4 = first cycle out of four
2:4 = second cycle out of four
3:4 = third cycle out of four
4:4 = fourth cycle out of four
```

Supported ratio groups for v0.1:

```text
1:2, 2:2
1:3, 2:3, 3:3
1:4, 2:4, 3:4, 4:4
1:8, 2:8, 3:8, 4:8, 5:8, 6:8, 7:8, 8:8
```

---

## Probability

Probability is separate from trig condition.

```text
probability: 0–100%
```

Default:

```text
probability: 100%
```

Probability is evaluated after condition eligibility.

Example:

```text
condition: 1:4
probability: 70%
```

The step is eligible only on the first cycle out of four. On that eligible cycle, it has a 70% chance of playing.

For v0.1, probability is recalculated on every eligible pass. A deterministic seed system is not required initially.

---

## Fill Behavior

Fill is a performance state used by `FILL` and `NOT_FILL` conditions.

Fill button modes:

```text
momentary: active while held
latched: active until the end of the current pattern cycle
```

The Fill state should be readable by the sequencer during condition evaluation.

Fill should not create a separate Pattern, duplicate steps, or modify saved step data.

---

## Retrig / Ratchet Interaction

Retrig is evaluated after condition and probability.

```text
condition passes
→ probability passes
→ trigger plays
→ retrig events are generated
```

If condition or probability fails, no retrig events happen.

Retrig fields:

```text
retrig_enabled: true / false
retrig_count: 2 / 3 / 4 / 6 / 8 / 12 / 16
retrig_decay: 0–100%
retrig_length: gate / full / short
```

Retrig should be stored as a playback modifier on the step, not as hidden sequencer steps.

---

## Microtiming and Swing Interaction

Timing resolution order:

```text
grid position
→ swing offset
→ microtiming offset
→ retrig offsets
```

Trig Conditions do not change the step timing; they only determine eligibility.

---

## Pattern Cycle Counter

Ratio conditions require a cycle counter.

The cycle counter is based on the Pattern master length, not the individual track length.

```text
cycle = completed master pattern loop
```

This keeps ratio conditions predictable even when tracks use independent lengths for polymeter.

---

## Pattern and Variation Launch

`FIRST` and `NOT_FIRST` reset when a Pattern or Variation is launched.

Variation changes are quantized to the Pattern boundary by default, so the first cycle of a new Variation is clear and predictable.

---

## Suggested UI Labels

The internal condition names may be mapped to compact UI labels:

```text
ALWAYS   → --
FILL     → FILL
NOT_FILL → !FILL
FIRST    → FIRST
NOT_FIRST→ !FIRST
PRE      → PRE
NOT_PRE  → !PRE
1:4      → 1:4
```

The UI should avoid exposing implementation details such as cycle counters unless in an advanced/debug view.

---

## Final Decision

TribeBox supports a compact Elektron-inspired condition system with Fill, First-cycle logic, previous-trigger dependency, ratio conditions, and independent probability. Conditions are evaluated before probability, and retrig events are generated only after both condition and probability pass.
