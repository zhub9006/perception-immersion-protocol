# 04 — Data Collection Guide

> **Document status:** Operational reference | **Version:** 1.0.0 | **Last updated:** 2026

This guide covers the end-to-end data collection workflow: environment setup, UXF configuration, session management, data pipeline, and quality control.

---

## 1. Pre-Session Setup Checklist

### 1.1 Hardware
- [ ] HMD charged to >80% (or plugged in via Link cable)
- [ ] Base stations powered on and tracking confirmed (Valve Index only)
- [ ] Controllers paired and tracking confirmed
- [ ] Test PC connected to HMD via USB 3.0 / Air Link
- [ ] Room boundary defined and clear of obstacles
- [ ] Backup HMD charged and available
- [ ] FPSVR or equivalent performance monitor installed

### 1.2 Software
- [ ] Unity project opened and `PIP_Forest_v1` scene loaded
- [ ] UXF Session component configured:
  - `Experiment Name`: `pip_experiment`
  - `Session Number`: set per participant schedule
  - `Settings Search Pattern`: `*.json`
  - `Data Handlers`: FileSaver (local) + optional cloud sync
- [ ] `config/session_settings.json` copied to `Assets/StreamingAssets/`
- [ ] Correct fidelity block configs present in `StreamingAssets/`
- [ ] Data output directory confirmed: `data_output/<experiment>/<ppid>/<session>/`
- [ ] Time synchronisation: PC clock synced to NTP

### 1.3 Environment
- [ ] Quiet room, minimal external noise
- [ ] Dim lighting (reduces HMD reflections)
- [ ] Chair available for seated conditions
- [ ] Water and rest area available
- [ ] Informed consent forms printed
- [ ] Debrief documents printed

---

## 2. UXF Configuration

### 2.1 Session Settings JSON Schema

The `config/session_settings.json` file controls top-level session parameters:

```json
{
  "experiment_name": "pip_experiment",
  "protocol_version": "1.0.0",
  "researcher_id": "RESEARCHER_ID_HERE",
  "ethics_ref": "ETHICS_REF_HERE",
  "target_n": 30,
  "blocks_per_session": 3,
  "trials_per_block": 20,
  "iti_min_ms": 1000,
  "iti_max_ms": 1500,
  "response_window_ms": 3000,
  "ssq_stop_threshold": 15,
  "data_output_path": "data_output",
  "log_frame_rate": true,
  "log_head_rotation": true,
  "head_rotation_sample_rate_hz": 90
}
```

### 2.2 Fidelity Block Config JSON Schema

Each fidelity block has its own config file. Example for High Fidelity:

```json
{
  "block_name": "block_high_immersion",
  "fidelity_level": "HF",
  "frame_rate_target": 90,
  "latency_injection_ms": 0,
  "fov_degrees": 110,
  "render_scale": 1.4,
  "texture_resolution": 2048,
  "anti_aliasing": "MSAA_4X_TAA",
  "spatial_audio_mode": "HRTF",
  "ambient_soundscape": "multi_source_spatialised",
  "haptics_mode": "parametric",
  "tracking_dof": 6,
  "tracking_noise_mm": 0,
  "shadow_quality": "soft",
  "post_processing": "full_urp",
  "grass_foliage": "animated_gpu"
}
```

---

## 3. Participant Onboarding Procedure

### Step 1: Consent (T+00:00)
1. Greet participant; provide information sheet
2. Allow time to read (minimum 5 minutes)
3. Collect signed consent form
4. Assign participant ID (format: `PIP_XXX`, e.g., `PIP_001`)

### Step 2: Screening (T+05:00)
1. Administer baseline SSQ
2. If SSQ Total Score ≥10: exclude participant; log reason
3. Collect demographic questionnaire:
   - Age, gender
   - VR experience (Novice / Intermediate / Experienced)
   - Gaming experience (hours/week)
   - Any vestibular conditions

### Step 3: HMD Fitting (T+10:00)
1. Fit HMD to participant; adjust head strap
2. Set IPD (interpupillary distance) to participant's measured IPD
   - Measure with pupillometer or ruler
   - Enter into HMD IPD adjustment
3. Confirm visual clarity (ask participant to describe a static text target)
4. Confirm controller grip comfort
5. Explain room boundary system

### Step 4: Orientation Trial (T+12:00)
1. Load neutral condition (MF settings, no task)
2. Allow 5 minutes free exploration of the virtual forest
3. Answer any questions about the environment
4. Explain task instructions (Simple and Complex tasks)
5. Conduct 5 practice trials (not recorded)

---

## 4. Block Execution Procedure

Repeat for each of the 3 (or 6) blocks:

1. **Load block config:** Select participant's counterbalanced condition order from the Latin square schedule (see `docs/03_experiment_design.md`)
2. **Pre-block SSQ:** Administer SSQ; check Total Score < 15
3. **Block start:** Press Play; UXF auto-starts trial sequence
4. **Monitor:** Watch FPSVR overlay for frame rate; if fps < 25 for >5s, abort block
5. **Trial completion:** UXF auto-records all trial data
6. **Post-block questionnaires:** IPQ + CIS-10 administered in-headset via UXF questionnaire component
7. **Post-block SSQ:** Administer SSQ; if Total Score ≥15, terminate session
8. **Rest:** Minimum 2-minute seated rest; HMD can be removed
9. **Data verification:** Check `data_output/` for correct file creation before proceeding

---

## 5. Data Output Structure

UXF generates the following directory and file structure:

```
data_output/
  pip_experiment/
    PIP_001/
      1/                          ← Session 1
        trial_results.csv         ← One row per trial
        block_1/
          questionnaire_ipq.csv
          questionnaire_cis.csv
          questionnaire_ssq_pre.csv
          questionnaire_ssq_post.csv
          head_rotation_log.csv   ← 90Hz rotation data
        block_2/
          ...
        block_3/
          ...
        session_log.txt           ← UXF event log
    PIP_002/
      ...
```

### 5.1 trial_results.csv Schema

| Column | Type | Description |
|---|---|---|
| `ppid` | string | Participant ID |
| `session` | int | Session number |
| `block_num` | int | Block number |
| `trial_num` | int | Trial number within block |
| `condition` | string | e.g., `HF-C` |
| `fidelity_level` | string | `LF`, `MF`, or `HF` |
| `task_complexity` | string | `S` or `C` |
| `stimulus_id` | string | Unique stimulus identifier |
| `correct_response` | int | 0 or 1 (or 1-3 for 3-AFC) |
| `participant_response` | int | Recorded response |
| `correct` | bool | Response correctness |
| `reaction_time_ms` | float | Response latency (ms) |
| `frame_rate_mean` | float | Mean fps during trial |
| `frame_rate_min` | float | Min fps during trial |
| `latency_injected_ms` | int | Artificial latency added |
| `timestamp_utc` | datetime | Trial start timestamp |

---

## 6. Quality Control Checks

### 6.1 Real-Time Checks (During Session)
- Frame rate monitor: Alert if mean fps drops below target by >10%
- SSQ monitoring: Mandatory post-block check
- Response rate: If >5 consecutive non-responses, pause and check participant

### 6.2 Post-Session Checks (Within 24h)

Run `scripts/pip_qc_check.py --ppid PIP_XXX` to automatically verify:

| Check | Pass Criterion |
|---|---|
| Trial count | Exactly 20 trials per block |
| Response rate | ≥80% responses within window |
| Frame rate | Mean fps within 5% of target |
| Questionnaire completeness | All items answered |
| Timestamp monotonicity | No time reversals |
| SSQ trajectory | No post-block SSQ exceeding stop threshold |

### 6.3 Exclusion Criteria (Post-Session)

| Criterion | Action |
|---|---|
| >20% missing responses in any block | Exclude that block |
| SSQ stop criterion triggered | Exclude session |
| Mean fps <80% of target for >30% of trials | Exclude block, flag for re-run |
| IPQ-GP score identical across all blocks (invariant response) | Flag for review; exclude if confirmed |

---

## 7. Data Backup and Storage

- **Primary:** Local SSD (encrypted, password-protected)
- **Secondary:** Institutional cloud storage (auto-sync after each session)
- **Backup frequency:** After every session
- **Retention period:** Minimum 10 years post-publication (institutional policy)
- **Anonymisation:** Participant name-to-ID mapping stored separately; only pseudonymous IDs in data files

---

## 8. Troubleshooting

| Problem | Likely Cause | Solution |
|---|---|---|
| Frame rate below target | GPU overloaded | Reduce render scale; close background apps |
| HMD tracking lost | Lighting too bright/dark | Adjust room lighting; check base stations |
| UXF crash mid-block | Unity exception | Check Unity console; re-run block from start |
| Participant reports nausea | SSQ-N rising | Administer SSQ; if ≥15, terminate |
| Questionnaire not saving | `StreamingAssets` misconfigured | Verify file paths; restart Unity |
| Head rotation log empty | Script not attached | Check `HeadRotationLogger` component in scene |

---

## 9. Post-Session Participant Debrief

1. Remove HMD; allow 2 minutes seated recovery
2. Administer final SSQ (post-VR recovery check)
3. Provide written debrief document explaining:
   - Study purpose (VR fidelity and presence)
   - What was varied (participants are naïve to conditions during experiment)
   - How to contact research team with concerns
4. Provide researcher contact card
5. Log participant dismissal time
