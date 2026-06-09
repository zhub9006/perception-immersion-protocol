# 01 — Theoretical Background: Presence, Immersion & Perception in VR

> **Document status:** Canonical reference | **Version:** 1.0.0 | **Last updated:** 2026

---

## 1. Conceptual Foundations

### 1.1 The Presence–Immersion Distinction

The terms *presence* and *immersion* are frequently conflated in the VR literature but refer to distinct phenomena:

| Concept | Definition | Measurement Domain |
|---|---|---|
| **Immersion** | The *objective, measurable* technical properties of a VR system that deliver sensory stimuli (display resolution, frame rate, field-of-view, latency, tracking fidelity) | Hardware / engineering |
| **Presence** | The *subjective psychological* sensation of "being there" — the mind's response to immersive stimuli | Self-report / psychophysiology |
| **Engagement** | Sustained attentional investment in the virtual task or environment | Behavioural / cognitive |
| **Flow** | Optimal experience state where challenge matches skill, often co-occurring with deep presence | Self-report / performance |

> **Key principle (Slater, 2003):** Immersion is a necessary but not sufficient condition for presence. High immersion increases the *probability* of presence but does not guarantee it.

### 1.2 Slater's Place Illusion / Plausibility Illusion Model

Mel Slater (2009) decomposed presence into two orthogonal illusions:

- **Place Illusion (PI):** The quasi-perceptual illusion of being in the virtual place (driven by *immersion* — sensorimotor contingency).
- **Plausibility Illusion (Psi):** The illusion that events occurring in the virtual environment are actually happening (driven by *narrative/ecological validity*).

Both PI and Psi are necessary for full presence. This protocol primarily manipulates **PI** by varying immersive hardware parameters, while holding Psi constant via a standardised virtual environment.

### 1.3 Witmer & Singer's Presence Model (1998)

Four factor classes drive presence:
1. **Control factors** — ability to interact with and manipulate the environment
2. **Sensory factors** — modality match, consistency, quality
3. **Distraction factors** — isolation from physical world, attention capture
4. **Realism factors** — scene realism, information consistency

The IPQ questionnaire used in this protocol maps directly onto these factors.

---

## 2. The Immersion Threshold Concept

### 2.1 Definition

The **Immersion Threshold (IT)** is defined operationally as:

> The minimum combination of technical VR parameters at which the mean IPQ General Presence subscale score of a representative participant sample crosses a pre-specified criterion value (default: **4.0 / 7.0**).

This is a *group-level* threshold. Individual thresholds vary substantially and are modelled using mixed-effects regression (see `docs/05_analysis_guide.md`).

### 2.2 Literature Benchmarks for Individual Parameters

The following thresholds are derived from peer-reviewed literature and serve as the basis for the three fidelity levels in this protocol:

#### Frame Rate
- Below **~45 fps**: Motion sickness risk increases sharply; presence drops (Jerald, 2015)
- **60 fps**: Widely cited minimum for comfortable VR (Oculus Best Practices, 2014)
- **90 fps**: Recommended target for Valve Index, Meta Quest Pro (manufacturer specs)
- **120 fps**: Threshold for expert users reporting maximal presence (Moss et al., 2011)

#### Head-Tracking Latency (Motion-to-Photon)
- **>80 ms**: Highly disruptive; breaks presence and induces nausea
- **20–40 ms**: Acceptable; marginal disruption
- **<20 ms**: Below perceptual threshold for most users (Moss et al., 2011)
- **<7 ms**: Imperceptible under any conditions (display-dependent)

#### Field of View (FoV)
- Human binocular FoV: ~200° horizontal, ~130° vertical
- **<60°**: Strong sense of viewing through a "porthole"; low presence
- **90°**: Approximate threshold for natural-feeling peripheral vision
- **110°+**: Current HMD maximum; near-natural horizontal coverage

#### Spatial Audio
- Monaural/no audio: Weakest presence contribution
- Stereo: Moderate improvement for distance cues
- **HRTF binaural audio**: Significant presence boost, especially for distance and elevation (Loomis et al., 1992; Hendrix & Barfield, 1996)

#### Haptic Feedback
- No haptics: Presence relies entirely on visual/auditory modalities
- Simple vibrotactile: Marginal improvement
- **6-DOF force feedback / controller rumble**: Significant improvement for manipulation tasks (Burkhardt et al., 2004)

### 2.3 Multi-Parameter Interactions

Parameters interact non-linearly. Key interactions identified in the literature:

- **Frame rate × latency:** Low latency partially compensates for lower frame rate (Jerald, 2015)
- **FoV × spatial audio:** Wide FoV with HRTF audio produces superadditive presence gains (Hendrix & Barfield, 1996)
- **Haptics × visual fidelity:** Haptic feedback disproportionately boosts presence in object-manipulation tasks but has minimal effect in passive observation (Burkhardt et al., 2004)

---

## 3. Cognitive Psychology Context

### 3.1 Embodied Cognition in VR

VR presence engages embodied cognitive processes:
- **Body ownership illusion (Rubber Hand Illusion analogue):** Congruent visuotactile stimulation in VR produces ownership of virtual body parts (Botvinick & Cohen, 1998; Ehrsson et al., 2004)
- **Peripersonal space representation:** VR can extend or contract perceived peripersonal space, affecting threat responses and spatial cognition
- **Sensorimotor contingency:** Presence depends on the brain's predictive model of how movements change sensory input — disrupted by high latency

### 3.2 Attentional Resources & Cognitive Load
- Immersive VR demands attentional resources; high cognitive load reduces presence (Witmer & Singer, 1998)
- The protocol controls task complexity (Low / High) as a secondary factor to assess the presence × cognitive load interaction

### 3.3 Individual Differences Relevant to Immersion Thresholds
- **VR experience:** Novice users report lower presence at equivalent immersion levels; adapts over sessions
- **Susceptibility to simulator sickness:** Correlated with lower presence tolerance at high immersion
- **Dissociation tendency:** Higher dissociation trait predicts higher presence scores (Spiegel et al., 2000)
- **Visuospatial ability:** Higher spatial ability → faster presence onset

---

## 4. Theoretical Models Synthesised in This Protocol

| Model | Key Contribution | Protocol Application |
|---|---|---|
| Slater (2003, 2009) | PI/Psi distinction; immersion–presence causal chain | Fidelity level manipulation targets PI specifically |
| Witmer & Singer (1998) | Four-factor presence model; PQ questionnaire | IPQ scales map to W&S factors |
| Jerald (2015) | Engineering thresholds for VR comfort | Frame rate and latency parameter values |
| Kennedy et al. (1993) | SSQ as safety/comfort screen | SSQ administered pre/post each block |
| Brookes et al. (2020) | UXF as reproducible experiment framework | Core software infrastructure |
| Csikszentmihalyi (1990) | Flow state theory | Secondary measure; high presence + high task match |

---

## 5. Hypotheses

**H1 (Primary):** Mean IPQ General Presence score will be significantly higher in the High-Fidelity condition than in the Medium-Fidelity condition (paired t-test, one-tailed, α = .05).

**H2 (Threshold):** There exists a critical frame rate between 45 and 90 fps at which presence scores show a discontinuous increase (change-point analysis).

**H3 (Interaction):** The effect of fidelity level on presence will be moderated by VR experience, such that novice users show a larger fidelity effect than experienced users.

**H4 (Sickness):** SSQ total score will increase monotonically with fidelity level (more motion, higher sickness risk), but will remain below the clinical threshold of 15 in the High-Fidelity condition.

**H5 (Audio):** Addition of HRTF spatial audio will produce a significant increase in IPQ Spatial Presence subscale score, independent of visual fidelity level.

---

## 6. Key References

- Botvinick, M., & Cohen, J. (1998). Rubber hands 'feel' touch that eyes see. *Nature*, 391, 756.
- Brookes, J., et al. (2020). Studying human behavior with virtual reality: The Unity Experiment Framework. *Behavior Research Methods*, 52, 455–463.
- Burkhardt, J.-M., et al. (2004). Haptic feedback in VR. *Presence*, 13(3), 303–322.
- Csikszentmihalyi, M. (1990). *Flow: The Psychology of Optimal Experience*. Harper & Row.
- Ehrsson, H. H., et al. (2004). That's my hand! Activity in premotor cortex reflects feeling of ownership of a limb. *Science*, 305, 875–877.
- Hendrix, C., & Barfield, W. (1996). Presence within virtual environments as a function of visual display parameters. *Presence*, 5(3), 274–289.
- Jerald, J. (2015). *The VR Book: Human-Centered Design for Virtual Reality*. ACM Books.
- Kennedy, R. S., et al. (1993). Simulator sickness questionnaire. *International Journal of Aviation Psychology*, 3(3), 203–220.
- Loomis, J. M., et al. (1992). Noncontact haptic perception. *Ecological Psychology*, 4(2), 77–91.
- Moss, J. D., et al. (2011). Motion sickness and visual surroundings. *Human Factors*, 53(3), 308–323.
- Schubert, T., et al. (2001). The experience of presence: Factor analytic insights. *Presence*, 10(3), 266–281.
- Slater, M. (2003). A note on presence terminology. *Presence Connect*, 3(3).
- Slater, M. (2009). Place illusion and plausibility can lead to realistic behaviour in immersive virtual environments. *Philosophical Transactions of the Royal Society B*, 364, 3549–3557.
- Witmer, B. G., & Singer, M. J. (1998). Measuring presence in virtual environments. *Presence*, 7(3), 225–240.
