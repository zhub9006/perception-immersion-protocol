# 🧠 Perception-Immersion Protocol (PIP)
### A Reproducible VR Cognitive Psychology Experiment Framework

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Unity: 2018.4+](https://img.shields.io/badge/Unity-2018.4%2B-blue.svg)](https://unity.com)
[![Framework: UXF](https://img.shields.io/badge/Framework-UXF%202.x-purple.svg)](https://github.com/immersivecognition/unity-experiment-framework)

---

## Overview

The **Perception-Immersion Protocol (PIP)** is a fully reproducible, open-source testing framework for studying the relationship between VR simulation fidelity and perceptual immersion thresholds in cognitive psychology research.

It is built on top of the [Unity Experiment Framework (UXF)](https://github.com/immersivecognition/unity-experiment-framework) — a peer-reviewed, MIT-licensed C# framework developed by the Immersive Cognition Group at the University of Leeds (Brookes et al., *Behavior Research Methods*, 2019) — and integrates established psychophysics scales (IPQ, SUS, SSQ) with parametric stimulus control.

---

## 🎯 Research Questions Addressed

1. **What is the minimum VR fidelity threshold** at which participants report subjective immersion (presence) significantly above baseline?
2. **How do individual differences** (age, VR experience, susceptibility to simulator sickness) modulate immersion thresholds?
3. **Which rendering parameters** (frame rate, field-of-view, spatial audio, haptic latency) contribute most to crossing the immersion threshold?
4. **Is the immersion threshold continuous or step-like** (phase transition)?

---

## 📁 Repository Structure

```
perception-immersion-protocol/
├── README.md                          ← This file
├── docs/
│   ├── 01_theoretical_background.md  ← Presence & immersion theory
│   ├── 02_parameter_specification.md ← All experiment parameters & thresholds
│   ├── 03_experiment_design.md        ← Full trial/block structure
│   ├── 04_data_collection_guide.md    ← UXF setup & data pipeline
│   └── 05_analysis_guide.md           ← Statistical analysis protocol
├── config/
│   ├── session_settings.json          ← UXF session-level parameters
│   ├── block_high_immersion.json      ← High-fidelity block config
│   ├── block_medium_immersion.json    ← Medium-fidelity block config
│   ├── block_low_immersion.json       ← Low-fidelity block config
│   └── participant_profile_schema.json← Participant metadata schema
├── scales/
│   ├── IPQ_igroup_presence.md         ← Igroup Presence Questionnaire
│   ├── SSQ_simulator_sickness.md      ← Simulator Sickness Questionnaire
│   └── custom_immersion_scale.md      ← PIP custom 10-item scale
├── scripts/
│   ├── pip_analysis.R                 ← R analysis script
│   └── pip_analysis.py                ← Python analysis script
└── CHANGELOG.md
```

---

## 🚀 Quick Start

### Prerequisites
- Unity 2018.4 LTS or later (tested up to Unity 6)
- UXF v2.4.5+ (`UXF.unitypackage` from [releases](https://github.com/immersivecognition/unity-experiment-framework/releases/latest))
- A VR headset supported by OpenVR/SteamVR (Meta Quest, Valve Index, HTC Vive)
- R ≥ 4.2 or Python ≥ 3.9 for analysis

### Setup Steps

1. **Clone this repository**
   ```bash
   git clone https://github.com/zhub9006/perception-immersion-protocol.git
   cd perception-immersion-protocol
   ```

2. **Import UXF into your Unity project**
   - Drag `UXF.unitypackage` into your Unity Assets panel
   - Run the UXF Setup Wizard (UXF → UXF Wizard)

3. **Configure the experiment**
   - Copy `config/session_settings.json` to your Unity `StreamingAssets/` folder
   - Set `experimentName` to `"pip_experiment"` in the UXF Rig inspector
   - Set `settingsSearchPattern` to `"*.json"`

4. **Run a pilot session**
   - Press Play in Unity
   - Enter Participant ID, select settings profile (`high_immersion`, `medium_immersion`, or `low_immersion`)
   - Data outputs to `data_output/<experiment>/<ppid>/<session>/`

5. **Analyse results**
   - `Rscript scripts/pip_analysis.R --data data_output/`
   - or: `python scripts/pip_analysis.py --data data_output/`

---

## 📊 Key Immersion Threshold Parameters

See [`docs/02_parameter_specification.md`](docs/02_parameter_specification.md) for full specifications. Summary:

| Parameter | Low Fidelity | Medium Fidelity | High Fidelity | Threshold (literature) |
|---|---|---|---|---|
| Frame Rate (fps) | 30 | 60 | 90 | **≥60 fps** (Jerald, 2015) |
| Field of View (°) | 60 | 90 | 110 | **≥90°** (Slater, 2003) |
| Head Tracking Latency (ms) | 80 | 40 | <20 | **<20 ms** (Moss et al., 2011) |
| Spatial Audio | None | Stereo | HRTF Binaural | **HRTF** (Loomis, 1992) |
| Haptic Feedback | None | Simple vibration | Full 6-DOF | **Controller vibration** |
| Texture Resolution | 512×512 | 1024×1024 | 2048×2048 | **≥1024** (Bowman, 2004) |
| IPQ Presence Score | < 3.5 | 3.5 – 4.5 | > 4.5 | **≥4.0/7.0** (Schubert, 2001) |
| SSQ Total Score | < 5 | 5 – 15 | < 5 | **< 15** (Kennedy, 1993) |

---

## 🧪 Experiment Overview

- **Design:** 3 (Fidelity Level) × 2 (Task Complexity) within-subjects
- **Participants:** N = 30 target (power analysis: d=0.5, α=0.05, β=0.80)
- **Trials per condition:** 20 trials × 6 conditions = 120 trials total
- **Session duration:** ~75 minutes (including consent, calibration, breaks)
- **Primary DV:** IPQ General Presence subscale (1–7 Likert)
- **Secondary DVs:** SSQ total, reaction time, head movement entropy

---

## 📖 References

- Brookes, J., Warburton, M., Alghadier, M., Mon-Williams, M., & Mushtaq, F. (2020). Studying human behavior with virtual reality: The Unity Experiment Framework. *Behavior Research Methods*, 52, 455–463.
- Schubert, T., Friedmann, F., & Regenbrecht, H. (2001). The experience of presence: Factor analytic insights. *Presence*, 10(3), 266–281.
- Slater, M. (2003). A note on presence terminology. *Presence Connect*, 3(3).
- Kennedy, R. S., Lane, N. E., Berbaum, K. S., & Lilienthal, M. G. (1993). Simulator sickness questionnaire. *The International Journal of Aviation Psychology*, 3(3), 203–220.
- Jerald, J. (2015). *The VR Book: Human-Centered Design for Virtual Reality*. ACM Books.

---

## 📜 License

MIT License. See [LICENSE](LICENSE).

## 🤝 Contributing

Issues and pull requests welcome. Please read the contributing guidelines before submitting.
