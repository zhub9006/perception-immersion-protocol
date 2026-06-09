# 02 — Parameter Specification

This document defines every controllable parameter in the Perception-Immersion Protocol, its valid range, the threshold value derived from literature and/or empirical benchmarks, and the rationale for its inclusion.

---

## 2.1 Display Fidelity Parameters

### FPS — Frame Rate

| Property | Value |
|---|---|
| Parameter key | `frame_rate` |
| Data type | Integer |
| Valid range | 24 – 144 fps |
| **Low condition** | **30 fps** |
| **Medium condition** | **60 fps** |
| **High condition** | **90 fps** |
| **Immersion threshold** | **≥60 fps** |
| Rationale | Below 60 fps, temporal aliasing is perceptible; 90 fps eliminates VOR mismatch artefacts |
| Source | Jerald (2015); Moss et al. (2011) |
| UXF setting key | `"display_fps"` |

### FOV — Field of View

| Property | Value |
|---|---|
| Parameter key | `field_of_view` |
| Data type | Float (degrees) |
| Valid range | 40° – 180° |
| **Low condition** | **60°** |
| **Medium condition** | **90°** |
| **High condition** | **110°** |
| **Immersion threshold** | **≥90°** |
| Rationale | Below 80° creates noticeable "porthole" effect breaking presence |
| Source | Slater (2003); Bowman (2004) |
| UXF setting key | `"camera_fov"` |

### Latency — Motion-to-Photon

| Property | Value |
|---|---|
| Parameter key | `tracking_latency_ms` |
| Data type | Integer (ms) |
| Valid range | 0 – 200 ms |
| **Low condition** | **80 ms** (simulated) |
| **Medium condition** | **40 ms** |
| **High condition** | **<20 ms** (native) |
| **Immersion threshold** | **<20 ms** |
| Rationale | VOR operates at ~7 ms; >20 ms lag produces perceptible swimming |
| Source | Moss et al. (2011) |
| UXF setting key | `"render_latency_ms"` |

### Resolution — Texture Quality

| Property | Value |
|---|---|
| Parameter key | `texture_resolution` |
| Data type | String enum |
| Valid values | `"512"`, `"1024"`, `"2048"` |
| **Low condition** | **512×512** |
| **Medium condition** | **1024×1024** |
| **High condition** | **2048×2048** |
| **Immersion threshold** | **≥1024×1024** |
| Source | Bowman (2004) |
| UXF setting key | `"texture_quality"` |

---

## 2.2 Audio Parameters

### Spatial Audio Mode

| Property | Value |
|---|---|
| Parameter key | `audio_mode` |
| Data type | String enum |
| Valid values | `"none"`, `"stereo"`, `"hrtf"` |
| **Low condition** | **`"none"`** |
| **Medium condition** | **`"stereo"`** |
| **High condition** | **`"hrtf"`** |
| **Immersion threshold** | **`"hrtf"`** |
| Source | Loomis (1992); Hendrix & Barfield (1996) |
| UXF setting key | `"audio_spatialization"` |

---

## 2.3 Presence Measurement Thresholds

### IPQ Thresholds

| Subscale | Min | **Threshold** | Max |
|---|---|---|---|
| General Presence (GP) | 1 | **≥4.0** | 7 |
| Spatial Presence (SP) | 6 | **≥24** | 42 |
| Involvement (INV) | 4 | **≥16** | 28 |
| Experienced Realism (REAL) | 4 | **≥16** | 28 |

### SSQ Safety Thresholds

| Subscale | Safe | Caution | Stop |
|---|---|---|---|
| Nausea (N) | 0–7.5 | 7.5–15 | **>15** |
| Oculomotor (O) | 0–7.5 | 7.5–15 | **>15** |
| Disorientation (D) | 0–7.5 | 7.5–15 | **>20** |
| Total Severity (TS) | 0–15 | 15–20 | **>20** |

### Custom PIP Scale Threshold

| Metric | Value |
|---|---|
| Scale range | 1–70 (10 items × 7-point) |
| **Immersion threshold** | **≥42 (60%)** |
| High immersion | ≥56 (80%) |
| Low immersion | <28 (40%) |

---

## 2.4 Task Parameters

### Target Detection Task (TDT)

```json
{
  "task_type": "target_detection",
  "stimulus_duration_ms": 200,
  "isi_min_ms": 800,
  "isi_max_ms": 2000,
  "target_probability": 0.25,
  "n_trials_per_block": 20,
  "response_window_ms": 1500,
  "feedback_duration_ms": 500
}
```

---

## 2.5 Counterbalancing Schema

Latin Square design (3 conditions × 3 groups):

| Group | Block 1 | Block 2 | Block 3 |
|---|---|---|---|
| A | Low | Medium | High |
| B | Medium | High | Low |
| C | High | Low | Medium |

Assignment: `group = (ppid_int % 3)`
