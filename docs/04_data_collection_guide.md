# 04 — Data Collection Guide (UXF Setup & Pipeline)

## 4.1 UXF Architecture Overview

The PIP is built on **UXF 2.4.5** (Unity Experiment Framework by Brookes et al., 2020). UXF provides:
- **Session/Block/Trial** hierarchy for experiment structure
- **Cascading settings system** — JSON files applied at Session, Block, or Trial level
- **Automatic data output** — CSV trial results, JSON settings snapshot, tracker CSVs
- **Events system** — `OnSessionBegin`, `OnTrialBegin`, `OnTrialEnd` Unity Events

### Key UXF Components Required
```
[UXF_Rig]
├── Session (MonoBehaviour)
│   ├── experimentName: "pip_experiment"
│   ├── settingsMode: 0 (load from file)
│   ├── settingsSearchPattern: "*.json"
│   └── storeSessionSettings: true
└── [UXF_DataHandling]
    ├── File Saver (active: true)
    │   ├── storagePath: "data_output"
    │   └── sortDataIntoFolders: true
    ├── Download or Copy (active: false)
    └── Web AWS DynamoDB (active: false)
```

---

## 4.2 Directory Setup

### Unity Project Structure
```
Assets/
├── StreamingAssets/
│   ├── session_settings.json        ← Copy from config/
│   ├── block_high_immersion.json    ← Copy from config/
│   ├── block_medium_immersion.json  ← Copy from config/
│   └── block_low_immersion.json     ← Copy from config/
├── UXF/                             ← UXF package contents
├── PIP/
│   ├── Scripts/
│   │   ├── PIPExperimentBuilder.cs  ← Session setup
│   │   ├── PIPSceneManipulator.cs   ← Fidelity control
│   │   ├── PIPQuestionnaireManager.cs
│   │   └── PIPSafetyMonitor.cs
│   └── Prefabs/
│       ├── TargetObject.prefab
│       └── DistractorObject.prefab
└── Scenes/
    ├── PIP_MainScene.unity
    └── PIP_ForestEnvironment.unity
```

### Output Directory Structure
```
data_output/
└── pip_experiment/
    └── <ppid>/
        └── <session_num>/
            ├── trial_results.csv       ← Main results file
            ├── session_info/
            │   ├── participant_details.csv
            │   ├── log.csv
            │   └── settings.json       ← Settings snapshot
            ├── trackers/
            │   ├── head_movement_T001.csv
            │   ├── head_movement_T002.csv
            │   └── ...
            └── other/
                ├── ipq_block_1.json
                ├── ssq_block_1.json
                ├── pip_scale_block_1.json
                └── ...
```

---

## 4.3 Trial Results CSV Schema

The `trial_results.csv` file contains one row per trial with these columns:

| Column | Type | Description |
|---|---|---|
| `trial_num` | int | Sequential trial number (1-based) |
| `block_num` | int | Block number (1–3) |
| `trial_num_in_block` | int | Trial within block (1–20) |
| `start_time` | float | Unix timestamp of trial start |
| `end_time` | float | Unix timestamp of trial end |
| `trial_duration` | float | Duration in seconds |
| `fidelity_level` | string | `"low"`, `"medium"`, `"high"` |
| `counterbalance_group` | string | `"A"`, `"B"`, `"C"` |
| `is_target` | bool | Whether target was shown |
| `response_made` | bool | Whether participant responded |
| `response_correct` | bool | Correct response |
| `reaction_time_ms` | float | RT in ms (NaN if no response) |
| `display_fps` | int | Configured frame rate |
| `camera_fov` | float | Field of view in degrees |
| `render_latency_ms` | int | Simulated latency |
| `texture_quality` | string | `"512"`, `"1024"`, `"2048"` |
| `audio_mode` | string | `"none"`, `"stereo"`, `"hrtf"` |
| `haptic_level` | string | `"none"`, `"vibration"` |

---

## 4.4 Questionnaire Data Schema

### IPQ (stored as JSON after each block)
```json
{
  "block_num": 1,
  "fidelity_level": "high",
  "timestamp": "2025-01-15T14:32:00Z",
  "gp": 5,
  "sp1": 5, "sp2": 4, "sp3": 6, "sp4": 5, "sp5": 4, "sp6": 5,
  "inv1": 5, "inv2": 4, "inv3": 5, "inv4": 4,
  "real1": 4, "real2": 5, "real3": 4, "real4": 3,
  "sp_total": 29,
  "inv_total": 18,
  "real_total": 16
}
```

### SSQ (stored as JSON)
```json
{
  "measurement_point": "post_block_1",
  "timestamp": "2025-01-15T14:35:00Z",
  "nausea_items": [0, 1, 0, 0, 0, 0, 0, 0, 0],
  "oculomotor_items": [1, 0, 1, 0, 0, 0, 0, 0, 0],
  "disorientation_items": [0, 0, 0, 1, 0, 0, 0],
  "N": 3.74,
  "O": 7.58,
  "D": 7.55,
  "TS": 8.54
}
```

---

## 4.5 Head Tracker CSV Schema

Each `head_movement_T###.csv` file contains continuous head position/rotation data:

| Column | Description |
|---|---|
| `time` | Time within trial (seconds) |
| `pos_x`, `pos_y`, `pos_z` | Head position (metres, Unity world space) |
| `rot_x`, `rot_y`, `rot_z`, `rot_w` | Head rotation (quaternion) |
| `euler_x`, `euler_y`, `euler_z` | Euler angles (degrees) |

Sampled at 90 Hz (matching display refresh rate).

---

## 4.6 Experimenter Checklist

### Pre-Session
- [ ] Room temperature comfortable (18–22°C)
- [ ] HMD lenses cleaned
- [ ] Guardian boundary configured (min 2×2 m)
- [ ] Baseline SSQ administered on paper
- [ ] Participant ID entered in UXF UI
- [ ] Settings profile selected (correct counterbalance group)
- [ ] Practice block completed and participant comfortable

### During Session
- [ ] Monitor SSQ scores after each block
- [ ] Note any verbalised discomfort
- [ ] Ensure mandatory 2-min rests observed
- [ ] Check head tracker data quality in real time

### Post-Session
- [ ] Final SSQ administered
- [ ] Data output folder verified (all CSVs present)
- [ ] Settings JSON snapshot confirmed
- [ ] Debrief completed
- [ ] Participant payment/credits processed
