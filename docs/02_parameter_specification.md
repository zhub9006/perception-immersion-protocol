# 02 — Experiment Parameter Specification

> **Document status:** Canonical reference | **Version:** 1.0.0 | **Last updated:** 2026

This document is the single source of truth for all experiment parameters, fidelity level definitions, and immersion threshold values used in the Perception-Immersion Protocol (PIP).

---

## 1. Fidelity Level Definitions

The protocol uses three fidelity levels as the primary independent variable. Each level is a **pre-specified bundle** of hardware and rendering parameters:

| Level | Code | Description |
|---|---|---|
| Low Fidelity | `LF` | Parameters below the literature-defined immersion threshold on all dimensions |
| Medium Fidelity | `MF` | Parameters at or near the immersion threshold — the critical zone of interest |
| High Fidelity | `HF` | Parameters well above the immersion threshold on all dimensions |

---

## 2. Visual Parameters

### 2.1 Frame Rate

| Level | Value | Notes |
|---|---|---|
| LF | 30 fps | Well below threshold; motion judder clearly perceptible |
| MF | 60 fps | At-threshold; comfortable for most users |
| HF | 90 fps | Above threshold; recommended for all current HMDs |

**Literature threshold:** ≥60 fps (Oculus Best Practices, 2014; Jerald, 2015)
**Measurement:** Reported by Unity `Time.deltaTime` and verified with FPSVR overlay

### 2.2 Head-Tracking Latency (Motion-to-Photon)

| Level | Value | Notes |
|---|---|---|
| LF | 80 ms | Artificially inflated via `WaitForEndOfFrame` delay injection |
| MF | 40 ms | Moderate; within acceptable range |
| HF | <20 ms | Native HMD latency; no artificial delay |

**Literature threshold:** <20 ms (Moss et al., 2011; Jerald, 2015)
**Measurement:** High-speed camera (240 fps) measuring LED flash-to-display latency

### 2.3 Field of View (Horizontal)

| Level | Value | Notes |
|---|---|---|
| LF | 60° | Simulated via Unity camera FoV restriction + edge vignette |
| MF | 90° | Near-threshold; standard for most consumer HMDs |
| HF | 110° | Maximum for current HMDs (Meta Quest 3: ~110°) |

**Literature threshold:** ≥90° (Slater, 2003; Bowman & McMahan, 2007)
**Measurement:** Rendered via Unity Camera.fieldOfView

### 2.4 Texture / Rendering Resolution

| Level | Value | Notes |
|---|---|---|
| LF | 512×512 textures, 0.5× render scale | Visibly pixelated |
| MF | 1024×1024 textures, 1.0× render scale | Standard |
| HF | 2048×2048 textures, 1.4× render scale | Supersampled |

**Literature threshold:** ≥1024×1024 textures (Bowman, 2004)

### 2.5 Anti-Aliasing

| Level | Value |
|---|---|
| LF | None (0× MSAA) |
| MF | 2× MSAA |
| HF | 4× MSAA + TAA |

---

## 3. Audio Parameters

### 3.1 Spatial Audio Mode

| Level | Value | Notes |
|---|---|---|
| LF | No audio | Silent virtual environment |
| MF | Stereo (panned) | Left/right panning only; no elevation cues |
| HF | HRTF binaural | Full 3D spatialisation via Steam Audio / Resonance Audio |

**Literature threshold:** HRTF binaural (Loomis et al., 1992; Hendrix & Barfield, 1996)

### 3.2 Ambient Soundscape

| Level | Value |
|---|---|
| LF | None |
| MF | Mono ambient loop (forest birdsong) |
| HF | Spatialised multi-source ambient (directional birdsong, wind, water) |

---

## 4. Haptic Parameters

### 4.1 Controller Haptic Feedback

| Level | Value | Notes |
|---|---|---|
| LF | None | Haptics disabled |
| MF | Simple vibrotactile pulse | Single 50ms buzz on object contact |
| HF | Parametric vibrotactile | Amplitude and frequency modulated by contact force |

**Literature threshold:** Any controller vibration (Burkhardt et al., 2004)

---

## 5. Tracking Parameters

### 5.1 Degrees of Freedom (DoF)

| Level | Value | Notes |
|---|---|---|
| LF | 3-DoF (rotation only) | Simulated by disabling positional tracking |
| MF | 6-DoF | Standard; full head position + rotation |
| HF | 6-DoF + hand tracking | Full body tracking via controller + optional hand tracking |

### 5.2 Controller Tracking Precision

| Level | Value |
|---|---|
| LF | Artificially jittered (+/- 5mm Gaussian noise injected) |
| MF | Native controller tracking |
| HF | Native tracking + predictive smoothing |

---

## 6. Full Fidelity Level Summary Matrix

| Parameter | Low Fidelity (LF) | Medium Fidelity (MF) | High Fidelity (HF) | Threshold |
|---|---|---|---|---|
| Frame Rate | 30 fps | 60 fps | 90 fps | **≥60 fps** |
| Latency (M2P) | 80 ms | 40 ms | <20 ms | **<20 ms** |
| Field of View | 60° | 90° | 110° | **≥90°** |
| Render Scale | 0.5× | 1.0× | 1.4× | **≥1.0×** |
| Texture Resolution | 512×512 | 1024×1024 | 2048×2048 | **≥1024×1024** |
| Anti-Aliasing | None | 2× MSAA | 4× MSAA+TAA | **≥2× MSAA** |
| Spatial Audio | None | Stereo | HRTF Binaural | **HRTF** |
| Haptics | None | Simple vibration | Parametric vibration | **Any haptics** |
| Tracking DoF | 3-DoF | 6-DoF | 6-DoF + hands | **6-DoF** |
| Tracking Noise | +/-5mm jitter | Native | Native + smoothed | **Native** |

---

## 7. Outcome Measure Thresholds

### 7.1 Igroup Presence Questionnaire (IPQ)

The IPQ (Schubert et al., 2001) has three subscales (scored 1–7):

| Subscale | Abbreviation | Items | Threshold (criterion) |
|---|---|---|---|
| General Presence | GP | 1 item (Item 1) | **≥4.0 / 7.0** |
| Spatial Presence | SP | 5 items | **≥4.2 / 7.0** |
| Involvement | INV | 4 items | **≥3.8 / 7.0** |
| Realness | REAL | 4 items | **≥3.5 / 7.0** |

> **Immersion threshold definition:** A fidelity level is deemed to have crossed the immersion threshold when the mean IPQ-GP score ≥ 4.0 AND mean IPQ-SP score ≥ 4.2 in a sample of N ≥ 20 participants.

### 7.2 Simulator Sickness Questionnaire (SSQ)

Kennedy et al. (1993) SSQ. Scored as weighted sum across 3 subscales:

| Subscale | Abbreviation | Clinical Concern Threshold |
|---|---|---|
| Nausea | N | ≥20 |
| Oculomotor | O | ≥20 |
| Disorientation | D | ≥20 |
| **Total Score** | **TS** | **≥15 (stop criterion)** |

> **Safety rule:** If any participant's SSQ Total Score ≥15 at the post-block check, the session is terminated immediately and the participant is moved to a recovery area.

### 7.3 Custom PIP Immersion Scale (CIS-10)

A 10-item Likert scale (1–7) developed for this protocol to capture dimensions not fully covered by IPQ:

| Item | Dimension |
|---|---|
| CIS-01 | Perceived depth of virtual space |
| CIS-02 | Believability of virtual object physics |
| CIS-03 | Sense of bodily presence in virtual space |
| CIS-04 | Naturalness of head movement response |
| CIS-05 | Quality of spatial audio integration |
| CIS-06 | Sense of agency over virtual hands |
| CIS-07 | Ease of attention capture by virtual stimuli |
| CIS-08 | Degree of forgetting about physical surroundings |
| CIS-09 | Perceived smoothness of visual motion |
| CIS-10 | Overall quality of the virtual experience |

**CIS-10 Threshold:** Mean score ≥5.0 / 7.0 = full immersion criterion for this protocol.

---

## 8. Technical Hardware Specifications (Reference)

### 8.1 Primary HMD (Tested Configuration)

| Specification | Value |
|---|---|
| Device | Meta Quest 3 |
| Display | Pancake lens LCD, 2064×2208 per eye |
| Refresh Rate | 72 / 90 / 120 Hz (configurable) |
| Native FoV | ~110° horizontal / ~96° vertical |
| Native Latency | ~12–18 ms (manufacturer spec) |
| Tracking | Inside-out 6-DoF, 4 cameras |
| Audio | Integrated spatial audio + 3.5mm jack |

### 8.2 Secondary HMD (Cross-Validation)

| Specification | Value |
|---|---|
| Device | Valve Index |
| Display | IPS LCD, 1440×1600 per eye |
| Refresh Rate | 80 / 90 / 120 / 144 Hz |
| Native FoV | ~130° horizontal (wide mode) |
| Native Latency | ~7 ms (manufacturer spec) |
| Tracking | SteamVR Lighthouse 2.0 |
| Audio | Off-ear speakers with HRTF support |

### 8.3 Computing Requirements

| Component | Minimum | Recommended |
|---|---|---|
| CPU | Intel i7-9700K / AMD Ryzen 7 3700X | Intel i9-12900K / AMD Ryzen 9 5900X |
| GPU | NVIDIA RTX 2070 / AMD RX 6700 XT | NVIDIA RTX 4080 / AMD RX 7900 XTX |
| RAM | 16 GB DDR4 | 32 GB DDR5 |
| Storage | 100 GB SSD | 500 GB NVMe SSD |
| OS | Windows 10 64-bit | Windows 11 64-bit |

---

## 9. Parameter Versioning

All parameter changes must be logged in `CHANGELOG.md` with:
- Version bump (semantic versioning)
- Rationale for change
- Which published studies prompted the revision
- Whether historical data remains comparable

---

## 10. References

- Bowman, D. A. (2004). 3D User Interfaces. *Addison-Wesley Professional*.
- Burkhardt, J.-M., et al. (2004). Haptic feedback in VR. *Presence*, 13(3).
- Hendrix, C., & Barfield, W. (1996). Presence within virtual environments. *Presence*, 5(3), 274–289.
- Jerald, J. (2015). *The VR Book*. ACM Books.
- Kennedy, R. S., et al. (1993). Simulator sickness questionnaire. *Int. J. Aviation Psychology*, 3(3).
- Loomis, J. M., et al. (1992). Noncontact haptic perception. *Ecological Psychology*, 4(2).
- Moss, J. D., et al. (2011). Motion sickness and visual surroundings. *Human Factors*, 53(3).
- Oculus Best Practices. (2014). *Oculus VR Best Practices Guide*. Oculus VR LLC.
- Schubert, T., et al. (2001). The experience of presence. *Presence*, 10(3), 266–281.
- Slater, M. (2003). A note on presence terminology. *Presence Connect*, 3(3).
