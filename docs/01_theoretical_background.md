# 01 — Theoretical Background: Presence, Immersion & Perception

## 1.1 Defining the Core Constructs

### Immersion (Technological)
Immersion refers to the **objective, measurable properties** of the VR display system that determine how completely it replaces real-world sensory input. It is a property of the technology, not the user. Key dimensions (Slater & Wilbur, 1997):
- **Inclusiveness** — extent to which the outside world is shut out
- **Extensiveness** — range of sensory modalities stimulated
- **Surrounding** — degree to which the display subtends the visual field
- **Vivid** — resolution, colour depth, frame rate quality

### Presence (Psychological)
Presence is the **subjective sense of "being there"** — the user's psychological response to the immersive technology. Presence is mediated by immersion but modulated by individual differences, task demands, and cognitive load (Witmer & Singer, 1998).

### The Immersion-Presence Relationship
```
[Technological Immersion] ──────────────► [Subjective Presence]
        │                                          ▲
        │   (fidelity parameters)         (individual differences,
        │                                  task, cognitive load)
        └──────────────────────────────────────────┘
```

The **immersion threshold** is the minimum level of technological fidelity at which a statistically reliable increase in presence is observed. Below this threshold, changes in fidelity parameters do not produce detectable changes in presence scores.

---

## 1.2 Theoretical Models

### Slater's Plausibility-Place Illusion Model (2009)
Slater distinguishes two orthogonal illusions:
- **Place Illusion (PI):** The sense of *being* in the virtual environment (spatial presence)
- **Plausibility Illusion (Psi):** The sense that events in the VE are *really happening*

Both are necessary for full immersive presence. PI is primarily driven by display fidelity (immersion), while Psi is driven by narrative coherence.

### Witmer & Singer's Presence Questionnaire Model (1998)
Presence is determined by four factors:
1. **Control factors** — sense of control over environment
2. **Sensory factors** — vividness and consistency of sensory information
3. **Distraction factors** — interface awareness and isolation
4. **Realism factors** — scene realism and consistency

### Cognitive Load Theory (Sweller, 1988) applied to VR
High extraneous cognitive load (from poor rendering, latency, or sickness) competes with germane cognitive load, reducing the mental bandwidth available for presence formation.

---

## 1.3 Psychophysical Basis of Immersion Thresholds

### Frame Rate Threshold
The human visual system's flicker fusion frequency ranges from ~45–75 Hz depending on peripheral vs. foveal vision and luminance. Below 60 fps, temporal aliasing becomes perceptible, breaking presence. Literature consensus: **≥60 fps** for comfortable VR; **≥90 fps** for demanding applications (Jerald, 2015).

### Latency (Motion-to-Photon) Threshold
Head-tracking latency above ~20 ms produces perceptible lag between head movement and display update, causing the "swimming" effect and contributing to simulator sickness. The vestibulo-ocular reflex (VOR) operates at ~7 ms, making latency below 20 ms imperceptible. **Threshold: <20 ms** (Moss et al., 2011).

### Field-of-View Threshold
Human binocular visual field extends ~200° horizontally. VR headsets typically offer 90–120°. Below ~80°, the "window on the world" effect (Lombard & Ditton, 1997) reduces presence. **Threshold: ≥90°** horizontal FOV.

### Spatial Audio Threshold
Head-Related Transfer Functions (HRTFs) enable 3D sound localisation. Simple stereo audio lacks elevation cues. The transition from stereo to HRTF binaural audio produces a statistically significant increase in presence scores (Loomis, 1992). **Threshold: HRTF binaural**.

### Resolution Threshold
Texture and rendering resolution affects the sense of detail and realism. Below 1024×1024 textures, pixelation is clearly visible at arm's length in current consumer headsets. **Threshold: ≥1024×1024**.

---

## 1.4 Measurement Instruments

### Igroup Presence Questionnaire (IPQ)
A validated 14-item scale measuring presence on three subscales:
- **SP** — Spatial Presence (6 items, 1–7)
- **INV** — Involvement (4 items, 1–7)
- **REAL** — Experienced Realism (4 items, 1–7)
- Plus one general presence item (GP)

**Immersion threshold operationalisation:** GP score ≥ 4.0 on a 7-point scale.

### Simulator Sickness Questionnaire (SSQ)
Kennedy et al. (1993) 16-item checklist measuring:
- **N** — Nausea subscale
- **O** — Oculomotor subscale
- **D** — Disorientation subscale
- **TS** — Total Severity

**Safety threshold:** TS < 15 (mild discomfort); TS > 20 triggers mandatory break.

### Custom PIP Immersion Scale
A 10-item Likert scale (1–7) designed for this protocol, measuring immediate post-trial immersion ratings. See `scales/custom_immersion_scale.md`.

---

## 1.5 Key Literature

| Citation | Contribution |
|---|---|
| Slater & Wilbur (1997) | Immersion/presence distinction |
| Witmer & Singer (1998) | Presence questionnaire & model |
| Schubert et al. (2001) | IPQ validation |
| Slater (2003) | Presence terminology clarification |
| Bowman & McMahan (2007) | Fidelity-performance relationships |
| Jerald (2015) | VR display thresholds |
| Brookes et al. (2020) | UXF framework |
| Skarbez et al. (2017) | Survey of presence factors |
