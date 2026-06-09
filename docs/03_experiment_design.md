# 03 — Experiment Design

## 3.1 Overview

The PIP uses a **3 (Fidelity Level: Low / Medium / High) × 2 (Task Complexity: Simple / Complex) within-subjects repeated measures design**.

- **Independent Variables:** Fidelity Level (3), Task Complexity (2)
- **Primary Dependent Variable:** IPQ General Presence score (GP, 1–7)
- **Secondary DVs:** PIP custom scale score, SSQ Total Severity, mean reaction time (ms), head movement entropy (bits)
- **Covariates:** VR experience (0–4), age, IPD, order (counterbalance group)

---

## 3.2 Session Timeline

```
Session Timeline (≈75 minutes)
─────────────────────────────────────────────────────────────────────
 0:00  Arrival & informed consent
 0:10  Baseline SSQ administration
 0:15  HMD fitting, IPD calibration, guardian setup
 0:20  Practice block (5 trials, medium fidelity, not analysed)
 0:25  ─────────────────────────────────────────────────────────────
        BLOCK 1 (20 trials, ~10 min)
        → Post-block: IPQ + PIP scale + SSQ
 0:40  REST (2 min mandatory)
 0:42  ─────────────────────────────────────────────────────────────
        BLOCK 2 (20 trials, ~10 min)
        → Post-block: IPQ + PIP scale + SSQ
 0:57  REST (2 min mandatory)
 0:59  ─────────────────────────────────────────────────────────────
        BLOCK 3 (20 trials, ~10 min)
        → Post-block: IPQ + PIP scale + SSQ
 1:14  HMD removal, 5 min recovery
 1:19  Post-session debrief + final SSQ
 1:25  End
─────────────────────────────────────────────────────────────────────
```

---

## 3.3 Trial Structure (Target Detection Task)

Each trial follows this sequence:

```
Trial Sequence
──────────────────────────────────────────────────────────
  [0 ms]       Fixation cross appears (500 ms)
  [500 ms]     Environment loads / fidelity applied
  [1000 ms]    ISI begins (random 800–2000 ms uniform)
  [1800–3000]  Stimulus appears (target or distractor)
  [+200 ms]    Stimulus disappears
  [+1500 ms]   Response window closes
               → RT recorded (if response made)
               → Feedback (500 ms): ✓ or ✗
  [+500 ms]    Inter-trial interval (500 ms blank)
──────────────────────────────────────────────────────────
  Total trial duration: ~4000–5500 ms
```

---

## 3.4 Block Structure

```
Block Structure (20 trials)
──────────────────────────────────────────────────────────
  Trials 1–5:   Warm-up (data collected but flagged)
  Trials 6–20:  Core measurement trials
  Post-block:   IPQ (14 items, ~3 min)
                PIP Scale (10 items, ~2 min)
                SSQ (16 items, ~3 min)
──────────────────────────────────────────────────────────
```

---

## 3.5 Fidelity Conditions

### Condition 1: Low Immersion (Sub-Threshold)
- Designed to produce presence scores **below threshold**
- Frame rate: 30 fps, FOV: 60°, latency: 80 ms, texture: 512, no audio, no haptics
- Expected IPQ GP: ~2.8 ± 0.8
- Expected PIP scale: ~28 ± 8

### Condition 2: Medium Immersion (At-Threshold)
- Designed to produce presence scores **at or near threshold**
- Frame rate: 60 fps, FOV: 90°, latency: 40 ms, texture: 1024, stereo audio, vibration haptics
- Expected IPQ GP: ~4.1 ± 0.7
- Expected PIP scale: ~43 ± 7

### Condition 3: High Immersion (Above-Threshold)
- Designed to produce presence scores **clearly above threshold**
- Frame rate: 90 fps, FOV: 110°, latency: <20 ms, texture: 2048, HRTF audio, modulated haptics
- Expected IPQ GP: ~5.2 ± 0.6
- Expected PIP scale: ~54 ± 6

---

## 3.6 Power Analysis

Sample size was determined using G*Power 3.1 for a **repeated-measures ANOVA**:

| Parameter | Value |
|---|---|
| Effect size (f) | 0.40 (medium-large; based on Schubert et al., 2001) |
| α (two-tailed) | 0.05 |
| Power (1-β) | 0.80 |
| Number of groups | 1 (within-subjects) |
| Number of measurements | 3 |
| Correlation among measures | 0.50 |
| **Required N** | **N = 24** |
| **Target N (with 25% attrition)** | **N = 30** |

---

## 3.7 Randomisation

- **Block order:** Counterbalanced via Latin Square (see `docs/02_parameter_specification.md` §2.7)
- **Trial stimuli:** Target vs. distractor randomised within each block, constrained so no more than 3 consecutive targets
- **ISI:** Sampled uniformly from [800, 2000] ms per trial using Unity's `Random.Range()`
- **Seed:** Participant ID numeric portion used as random seed for reproducibility

---

## 3.8 Data Quality Checks

The following automated checks are applied during data collection:

| Check | Criterion | Action |
|---|---|---|
| Missing response rate | > 30% of trials | Flag session for review |
| Extremely fast RT | < 100 ms | Code as anticipation, exclude from RT analysis |
| Extremely slow RT | > 1400 ms | Code as miss |
| SSQ Total Severity | > 20 | Pause session, administer recovery protocol |
| Head tracker dropout | > 5% frames | Flag trial |
| Incomplete questionnaire | Any item missing | Prompt re-completion before proceeding |
