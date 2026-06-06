Here's the full calibration analysis based on your data:

---

### Raw Data & Averages

| Run | Rev 180° | Rev 360° | Fwd 180° | Fwd 360° |
|-----|----------|----------|----------|----------|
| 1 | -1049 | -2101 | 1080 | 2100 |
| 2 | -1061 | -2113 | 1042 | 2113 |
| 3 | -1051 | -2116 | 1080 | 2135 |
| 4 | -1029 | -2108 | 1065 | 2132 |
| 5 | -1069 | -2121 | 1074 | 2147 |
| **Avg** | **-1051.8** | **-2111.8** | **1068.2** | **2125.4** |

---

### Ticks Per Revolution (TPR)

| Direction | From 180° (×2) | From 360° direct | Best Estimate |
|-----------|---------------|-----------------|---------------|
| Forward | 2 × 1068.2 = **2136.4** | **2125.4** | **(2136.4 + 2125.4) / 2 = 2130.9** |
| Reverse | 2 × 1051.8 = **2103.6** | **2111.8** | **(2103.6 + 2111.8) / 2 = 2107.7** |
| **Combined** | | | **(2130.9 + 2107.7) / 2 = 2119.3** |

> Use **TPR = 2119.3 ticks/revolution** as your unified constant, or split by direction for higher accuracy.

---

### Linearity Check

| Direction | 2 × 180° avg | 360° avg | Difference | Error % |
|-----------|-------------|----------|------------|---------|
| Forward | 2136.4 | 2125.4 | 11.0 | **0.52%** |
| Reverse | 2103.6 | 2111.8 | 8.2 | **0.39%** |

✅ Both under 1% — encoder is highly linear, a single scale factor is sufficient.

---

### Forward/Reverse Asymmetry

| | TPR |
|---|---|
| Forward | 2130.9 |
| Reverse | 2107.7 |
| Difference | **23.2 ticks (1.09%)** |

> This ~1% asymmetry is worth correcting if you need precise bidirectional motion. Use separate TPR constants per direction.

---

### Distance Calibration Formulas (in terms of D)

**Wheel circumference:**
```
C = π × D
```

**Distance per tick (unified):**
```
dist_per_tick = (π × D) / 2119.3
              ≈ 0.001484 × D   (mm if D is in mm)
```

**Distance per tick — Forward only:**
```
dist_per_tick_fwd = (π × D) / 2130.9 ≈ 0.001475 × D
```

**Distance per tick — Reverse only:**
```
dist_per_tick_rev = (π × D) / 2107.7 ≈ 0.001491 × D
```

---

### Ready-to-Use Formula

```
distance = (encoder_ticks / 2119.3) × π × D

# Or equivalently:
distance = encoder_ticks × 0.001484 × D
```

For directional accuracy:
```
if moving forward:  distance = encoder_ticks × 0.001475 × D
if moving reverse:  distance = encoder_ticks × 0.001491 × D
```

---

### Standard Deviation (Noise Floor)

| Column | Std Dev |
|--------|---------|
| Rev 180° | 14.5 ticks |
| Rev 360° | 7.4 ticks |
| Fwd 180° | 16.4 ticks |
| Fwd 360° | 19.0 ticks |

> Per revolution, your encoder noise is roughly **±7–19 ticks**, which translates to a position uncertainty of roughly **±(0.004 to 0.009) × D** per rotation. Over many rotations this accumulates — pairing with an IMU or external reference is recommended for long runs.

---

Plug in your actual `D` (in mm) and you'll have production-ready calibration constants. Want me to also generate a code snippet (Python/C++) that implements this with the directional split?
