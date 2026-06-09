# 🧠 Perception-Immersion Protocol (PIP)
### A Reproducible VR Cognitive Psychology Experiment Framework — v2.0

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Unity: 6+](https://img.shields.io/badge/Unity-6-blue.svg)](https://unity.com)
[![Python: 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://python.org)
[![Status: Active](https://img.shields.io/badge/Status-Active-brightgreen.svg)]()

---

## Overview

The **Perception-Immersion Protocol (PIP)** is a fully reproducible, open-source testing framework for studying the relationship between VR simulation fidelity and perceptual immersion thresholds in cognitive psychology research.

**Version 2.0** was built by synthesising a systematic GitHub landscape survey (June 2026) of open-source VR perception experiments, integrating their parameter configurations and empirical benchmark values into a unified, reproducible protocol.

### Source Repositories Surveyed

| Repository | Domain | Key Contribution |
|---|---|---|
| [adriagare/Simulation-of-Human-Arm-Movement-in-a-Virtual-Environment](https://github.com/adriagare/Simulation-of-Human-Arm-Movement-in-a-Virtual-Environment) | Visuomotor perturbation / body ownership | **Primary empirical benchmark** — 4-threshold detection model (kinematic ~17.6°, verbal ~53.6°), 9 kinematic metrics, exact permutation testing, N=10 pilot cohort |
| [lrlcardoso/VRXPerceptStudy_rev1](https://github.com/lrlcardoso/VRXPerceptStudy_rev1) | Crossmodal perception | Unity VR crossmodal experimental design patterns |
| [LordKrishnan/PHD-Project2-RealismStudy](https://github.com/LordKrishnan/PHD-Project2-RealismStudy_Experiment_Codes.) | Stereoscopic realism / focus cues | 2-IFC paradigm, Thurstonian scaling, correct focus cues → no realism decay with depth separation (N=15) |
| [imatv/ziliLab-VR-OVI](https://github.com/imatv/ziliLab-VR-OVI) | Oculo-vestibular integration | GVS protocol for tilt perception bias in VR |

### Local Environment
- **Filesystem scanned:** `/data` (CLI: `ls`, `cat` only; no VR-specific config files found locally)
- **Relevant local context:** Research methodology, data science, and grant writing coursework materials

---

## 🎯 Research Questions Addressed

1. **What is the minimum VR fidelity threshold** at which participants report subjective immersion (presence) significantly above baseline?
2. **How do individual differences** (age, VR experience, simulator sickness susceptibility) modulate immersion thresholds?
3. **Which rendering parameters** (frame rate, FoV, spatial audio, haptic latency) are the strongest drivers of crossing the immersion threshold?
4. **Is the immersion threshold continuous or step-like** (phase transition)?
5. **What is the gap between implicit motor detection and explicit verbal awareness** of perceptual mismatch in VR? *(Empirical pilot: ~36° — motor system reacts at ~17.6°, conscious report at ~53.6°)*

---

## 📁 Repository Structure

```
perception-immersion-protocol/
├── README.md                                        ← This file (v2.0)
├── CHANGELOG.md                                     ← Version history
├── LICENSE                                          ← MIT License
│
├── docs/
│   ├── 01_theoretical_background.md                ← Presence & immersion theory
│   ├── 02_parameter_specification.md               ← All experiment parameters & thresholds
│   ├── 03_experiment_design.md                     ← Full trial/block structure
│   ├── 04_data_collection_guide.md                 ← Hardware setup & data pipeline
│   ├── 05_analysis_guide.md                        ← Statistical analysis protocol
│   └── 06_visuomotor_threshold_module.md           ← ★ Empirical visuomotor sub-protocol
│
├── config/
│   ├── session_settings.json                       ← Master session-level parameters
│   ├── block_high_immersion.json                   ← High-fidelity block config
│   ├── block_medium_immersion.json                 ← Medium-fidelity block config
│   ├── block_low_immersion.json                    ← Low-fidelity block config
│   ├── visuomotor_probe_config.json                ← ★ Visuomotor perturbation parameters
│   └── participant_profile_schema.json             ← Participant metadata schema
│
├── scales/
│   ├── IPQ_igroup_presence.md                      ← Igroup Presence Questionnaire (14 items)
│   ├── SSQ_simulator_sickness.md                   ← Simulator Sickness Questionnaire (16 items)
│   ├── CIS_custom_immersion_scale.md               ← PIP 10-item Cognitive Immersion Scale
│   └── VEQ_verbal_event_taxonomy.md                ← ★ Verbal event annotation taxonomy
│
├── scripts/
│   ├── pip_analysis.py                             ← Python analysis pipeline
│   ├── pip_analysis.R                              ← R analysis script
│   ├── visuomotor_probe_analysis.py                ← ★ Visuomotor threshold analysis
│   └── aggregate_thresholds.py                     ← ★ Cross-participant threshold aggregation
│
└── data/
    └── benchmark_thresholds.json                   ← ★ Empirical threshold benchmarks
```

---

## 🚀 Quick Start

### Prerequisites
- Unity 6 with XR Interaction Toolkit and OpenXR
- VR headset: Meta Quest, Valve Index, HTC Vive, or OpenXR-compatible device
- OptiTrack / optical tracking system (optional — for visuomotor module)
- Python 3.10+ (`pip install pandas numpy matplotlib`)
- R ≥ 4.2 (`install.packages(c("lme4","emmeans","pwr","ggplot2"))`)

### Setup & Run

```bash
# 1. Clone
git clone https://github.com/zhub9006/perception-immersion-protocol.git
cd perception-immersion-protocol

# 2. Standard fidelity experiment
python scripts/pip_analysis.py --data data_output/ --mode fidelity

# 3. Visuomotor threshold sub-protocol
python scripts/visuomotor_probe_analysis.py --all

# 4. Cross-participant aggregation
python scripts/aggregate_thresholds.py --per-participant data_analysis/per_participant/

# 5. Full R statistical analysis
Rscript scripts/pip_analysis.R --data data_output/
```

---

## 📊 Key Immersion Threshold Parameters

Full specifications in [`docs/02_parameter_specification.md`](docs/02_parameter_specification.md).

### Rendering / Hardware Thresholds (Literature Benchmarks)

| Parameter | Low Fidelity | Medium Fidelity | High Fidelity | Threshold (Source) |
|---|---|---|---|---|
| Frame Rate (fps) | 30 | 60 | 90 | **≥60 fps** (Jerald, 2015) |
| Field of View (°) | 60 | 90 | 110 | **≥90°** (Slater, 2003) |
| Head Tracking Latency (ms) | 80 | 40 | <20 | **<20 ms** (Moss et al., 2011) |
| Spatial Audio | None | Stereo | HRTF Binaural | **HRTF** (Loomis, 1992) |
| Texture Resolution | 512×512 | 1024×1024 | 2048×2048 | **≥1024** (Bowman, 2004) |
| IPQ Presence Score | < 3.5 | 3.5–4.5 | > 4.5 | **≥4.0/7.0** (Schubert, 2001) |
| SSQ Total Score | < 5 | 5–15 | < 5 | **< 15** (Kennedy, 1993) |

### Visuomotor Perturbation Thresholds (Empirical — Pilot N=10)

> Source: adriagare (2026); paradigm: cumulative monotonic rotation of displayed forearm in VR

| Threshold Type | Mean Angle | Detection Method |
|---|---|---|
| **Kinematic** | ~17.6° | Permutation test on 9 kinematics vs. baseline (α=0.05) |
| **Behavioural** | ~24–32° | Any annotated event (hesitation, surprise, verbalisation) |
| **Rest-correction** | ~32–40° | Permutation test on 4 rest metrics vs. baseline |
| **Verbal** | ~53.6° | First `user_said_strange` event annotation |
| **Implicit–Explicit gap** | **~36°** | Verbal − Kinematic threshold |

*Probe angle series: [0°, 8°, 16°, 24°, 32°, 40°, 48°, 56°, 64°, 72°, 80°] | 5 trials/level*

---

## 🧪 Experiment Modules

### Module A — Fidelity × Presence (Primary Protocol)
- **Design:** 3 (Fidelity) × 2 (Task Complexity) fully within-subjects
- **N target:** 30 (power: d=0.62, α=0.05, 1−β=0.80; Cummings & Bailenson, 2016)
- **Trials:** 20 × 6 conditions = 120 total
- **Duration:** ~75 minutes
- **Primary DV:** IPQ General Presence (1–7)

### Module B — Visuomotor Threshold (Secondary Protocol)
- **Design:** Cumulative perturbation (monotonically increasing forearm rotation angle)
- **N target:** 30 (pilot: 10)
- **Levels:** 11 × 5 trials = 55 probe trials per participant
- **Duration:** ~45 minutes
- **Primary DV:** 4 detection thresholds (kinematic, behavioural, rest-correction, verbal)

### Module C — Stereoscopic Realism (Tertiary Protocol)
- **Design:** 2-IFC task (correct vs. incorrect focus cues) across depth separation levels
- **N target:** 15–20 (reference: LordKrishnan PhD study, N=15)
- **Analysis:** Thurstonian scaling of relative realism scores
- **Duration:** ~30 minutes

---

## 📖 References

- Adrià Gasull Rectoret (2026). *Simulation of Human Arm Movement in a Virtual Environment*. Bachelor's Thesis, Universitat de Barcelona. [GitHub](https://github.com/adriagare/Simulation-of-Human-Arm-Movement-in-a-Virtual-Environment)
- Brookes, J. et al. (2020). Studying human behavior with virtual reality: The Unity Experiment Framework. *Behavior Research Methods*, 52, 455–463.
- Cummings, J. J., & Bailenson, J. N. (2016). How immersive is enough? A meta-analysis of the effect of immersive technology on user presence. *Media Psychology*, 19(2), 272–309.
- Hibbard, P. B. et al. (2017). Perceived realism of 3D space and depth separation in stereoscopic images. *Journal of Vision*.
- Hogan, N., & Sternad, D. (2009). Sensitivity of smoothness measures to movement duration, amplitude, and arrests. *Journal of Motor Behavior*, 41(6), 529–534.
- Jerald, J. (2015). *The VR Book: Human-Centered Design for Virtual Reality*. ACM Books.
- Kennedy, R. S. et al. (1993). Simulator sickness questionnaire. *International Journal of Aviation Psychology*, 3(3), 203–220.
- Moss, J. D. et al. (2011). Characteristics of head-mounted displays and their effects on simulator sickness. *Human Factors*, 53(3), 308–319.
- Schubert, T., Friedmann, F., & Regenbrecht, H. (2001). The experience of presence: Factor analytic insights. *Presence*, 10(3), 266–281.
- Slater, M. (2003). A note on presence terminology. *Presence Connect*, 3(3).

---

## 📜 License

MIT License. See [LICENSE](LICENSE).

## 🤝 Contributing

Issues and pull requests welcome. Please see [CONTRIBUTING.md](CONTRIBUTING.md).

---

*Protocol v2.0.0 | Built 2026 | GitHub landscape survey: 4 VR cognitive psychology repositories*
