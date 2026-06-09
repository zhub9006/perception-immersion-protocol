# 03 — Experiment Design

> **Document status:** Canonical reference | **Version:** 1.0.0 | **Last updated:** 2026

---

## 1. Design Overview

| Element | Specification |
|---|---|
| **Design type** | 3 × 2 fully within-subjects factorial |
| **Factor A** | Fidelity Level: Low (LF), Medium (MF), High (HF) |
| **Factor B** | Task Complexity: Simple (S), Complex (C) |
| **N conditions** | 6 (LF-S, LF-C, MF-S, MF-C, HF-S, HF-C) |
| **Primary DV** | IPQ General Presence score (1–7) |
| **Secondary DVs** | IPQ-SP, IPQ-INV, IPQ-REAL, SSQ-TS, CIS-10, reaction time, head movement entropy |
| **Counterbalancing** | Latin square (6×6) across fidelity × task order |
| **Target N** | 30 participants (power analysis below) |
| **Session duration** | ~75 minutes total |

---

## 2. Power Analysis

Based on the meta-analytic effect size for VR fidelity on presence (d = 0.62; Cummings & Bailenson, 2016):

```
Input parameters:
  Effect size (Cohen's d):  0.62
  Alpha (α):                0.05 (one-tailed for H1)
  Power (1-β):             0.80
  Design:                   Repeated measures, ρ = 0.50

Required N = 24
Target N = 30 (25% buffer for attrition)
```

**Software:** `pwr` package in R; `statsmodels` in Python.

---

## 3. Participant Criteria

### 3.1 Inclusion Criteria
- Age 18–45 years
- Normal or corrected-to-normal vision (Snellen ≥20/40)
- Normal hearing (self-reported)
- Able to provide written informed consent
- Able to stand/sit comfortably for 75 minutes

### 3.2 Exclusion Criteria
- History of epilepsy or photosensitive seizures
- Active vestibular disorder or severe motion sickness history
- Pregnancy
- Use of vestibular-affecting medications (antihistamines, scopolamine) in past 24h
- Baseline SSQ Total Score ≥10 (assessed before any VR exposure)
- Claustrophobia or discomfort with HMD
- Prior participation in this specific protocol

### 3.3 VR Experience Classification

Collected via pre-screening questionnaire:

| Category | Definition |
|---|---|
| Novice | < 5 hours lifetime VR use |
| Intermediate | 5–50 hours lifetime VR use |
| Experienced | > 50 hours lifetime VR use |

---

## 4. Session Structure

### 4.1 Timeline

```
T+00:00  Arrival, written consent, demographic questionnaire
T+05:00  Baseline SSQ (pre-VR)
T+10:00  HMD fitting and IPD calibration
T+12:00  VR orientation trial (neutral condition, not analysed)
T+17:00  --- BLOCK 1 (Condition A) ---
           Pre-block SSQ
           20 trials (~8 min)
           Post-block IPQ + CIS-10
           Post-block SSQ
           2-min seated rest
T+32:00  --- BLOCK 2 (Condition B) ---
           (same structure)
T+47:00  --- BLOCK 3 (Condition C) ---
           (same structure)
T+62:00  --- BLOCK 4 (Condition D) ---
T+??:??  --- BLOCKS 5–6 (Conditions E, F) ---
           NOTE: Only 3 blocks per session recommended;
           schedule second session if 6 conditions needed
T+72:00  Final debrief + VR experience questionnaire
T+75:00  Participant dismissal
```

> **Recommendation:** Run 3 blocks per session and schedule a second session (same day or next day) for the remaining 3 conditions to prevent fatigue confounds.

### 4.2 Trial Structure (Per Block)

Each block contains 20 trials in the virtual forest environment:

```
Trial onset:
  1. Fixation cross (1,000 ms)
  2. Target stimulus appears (Simple: single object; Complex: 3-object array)
  3. Participant responds (Simple: "Does the object appear in front of you?" Y/N;
                           Complex: "Which object is closest?" 3-AFC)
  4. Response window: 3,000 ms max
  5. Feedback (correct/incorrect tone, 200 ms)
  6. Inter-trial interval: 1,000–1,500 ms (uniform random)
```

**Total block duration:** ~8 minutes (20 trials × ~22 s average)

---

## 5. Virtual Environment: The Standard Forest Scene

To hold Plausibility Illusion (Psi) constant across fidelity levels, all conditions use the **same virtual environment** (forest clearing) with only rendering quality varied.

### 5.1 Environment Specifications

| Element | Specification |
|---|---|
| Scene name | `PIP_Forest_v1` |
| Size | 50m × 50m playable area |
| Lighting | Dynamic directional sun + ambient probe |
| Geometry | ~15,000 triangles (LOD managed) |
| Objects | 12 interactable target objects (varied shape/colour) |
| Skybox | HDRI procedural sky |
| Physics | Unity Physics (gravity 9.81 m/s²) |
| Navigation | Stationary (participant does not locomote; head rotation only) |

### 5.2 Rendering Per Fidelity Level

| Element | LF | MF | HF |
|---|---|---|---|
| Post-processing | None | Bloom, colour grading | Full URP stack |
| Shadows | None | Hard shadows | Soft shadows + SSAO |
| Reflections | None | Baked probes | Real-time reflections |
| Grass / foliage | None | Static billboards | Animated GPU instanced |
| Water shader | Flat colour | Basic normal map | PBR with refraction |

---

## 6. Tasks

### 6.1 Simple Task (S)
**Name:** Single-Object Depth Judgement
**Instruction to participant:** *"A single object will appear somewhere in the virtual forest. Press the green button if the object appears to be in front of you (within arm's reach), or the red button if it appears further away."*
**Cognitive load:** Low (single discrimination, no working memory demand)
**Stimuli:** 12 objects × 2 distances (near: 0.5m; far: 3.0m) = 24 unique stimuli, sampled randomly

### 6.2 Complex Task (C)
**Name:** Multi-Object Distance Ranking
**Instruction to participant:** *"Three objects will appear in different locations in the virtual forest. Using the trackpad, select the object that appears closest to you."*
**Cognitive load:** High (spatial comparison, working memory for 3 locations)
**Stimuli:** 12 objects arranged in triplets at pseudo-random distances (0.5–5.0m), 20 unique arrangements

---

## 7. Randomisation and Counterbalancing

### 7.1 Condition Order

A 6×6 Latin square ensures each condition appears in each position equally often across the N=30 sample (using 5 complete squares).

```
Latin Square (first 6 participants):
  P01: LF-S, MF-S, HF-S, LF-C, MF-C, HF-C
  P02: MF-S, HF-S, LF-C, MF-C, HF-C, LF-S
  P03: HF-S, LF-C, MF-C, HF-C, LF-S, MF-S
  P04: LF-C, MF-C, HF-C, LF-S, MF-S, HF-S
  P05: MF-C, HF-C, LF-S, MF-S, HF-S, LF-C
  P06: HF-C, LF-S, MF-S, HF-S, LF-C, MF-C
```

This sequence repeats (with random perturbation) for P07–P30.

### 7.2 Stimulus Randomisation

Within each block, trial order is randomised using a seeded pseudorandom sequence. Seed = `participant_id * 100 + block_number`. This ensures reproducibility while preventing order effects.

---

## 8. Data Recording

### 8.1 UXF Automatic Recording

The UXF framework automatically records per-trial:
- `ppid`: Participant ID
- `session`: Session number
- `block_num`: Block number (1–6)
- `trial_num`: Trial number within block
- `condition`: Fidelity level + task code (e.g., `HF-C`)
- `response`: Button pressed
- `correct`: Boolean
- `reaction_time_ms`: Time from stimulus onset to response
- `frame_rate_mean`: Mean fps during trial
- `frame_rate_min`: Minimum fps during trial

### 8.2 Manual / Questionnaire Recording

Completed via UXF's built-in questionnaire component:
- `ipq_gp`: IPQ General Presence (1 item)
- `ipq_sp_01` through `ipq_sp_05`: IPQ Spatial Presence items
- `ipq_inv_01` through `ipq_inv_04`: IPQ Involvement items
- `ipq_real_01` through `ipq_real_04`: IPQ Realness items
- `cis_01` through `cis_10`: Custom Immersion Scale items
- `ssq_01` through `ssq_16`: SSQ items

### 8.3 Head Movement Entropy

Head rotation data is logged at 90 Hz via `Camera.transform.rotation`. Post-hoc entropy is computed as Shannon entropy of the angular velocity distribution — a proxy for engagement/exploration.

---

## 9. Stopping Rules

1. **SSQ stop criterion:** Any post-block SSQ Total Score ≥15 → terminate session, log reason.
2. **Technical failure:** Frame rate drops below 25 fps for >5 consecutive seconds → abort block, re-run.
3. **Participant withdrawal:** Participant may withdraw at any time without consequence.
4. **Data exclusion:** Blocks with >20% missing responses excluded from analysis.

---

## 10. Ethical Considerations

- Protocol approved under institutional ethics framework (reference to be added)
- Informed consent covers VR exposure risks, data storage, right to withdraw
- Data stored pseudonymously (participant ID only; name-to-ID mapping held separately)
- SSQ monitoring throughout protects against simulator sickness harm
- Debrief document provided to all participants post-session

---

## 11. References

- Cummings, J. J., & Bailenson, J. N. (2016). How immersive is enough? A meta-analysis of the effect of immersive technology on user presence. *Media Psychology*, 19(2), 272–309.
- Kennedy, R. S., et al. (1993). SSQ. *Int. J. Aviation Psychology*, 3(3).
- Schubert, T., et al. (2001). IPQ. *Presence*, 10(3).
