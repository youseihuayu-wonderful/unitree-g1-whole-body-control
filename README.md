# Unitree G1 Whole-Body Control

Research project in **Professor Yanzhi Wang’s Research Group**, Northeastern University.

Whole-body control pipeline for the **Unitree G1** humanoid (real robot available), using **Unitree official simulation + SDK**, with a paper-oriented focus on **control algorithms**, stability, and measurable real-time behavior.

## Research goal (2-month scope)

**Primary question**

> Can we improve *coordinated whole-body motion* on G1 (tracking + balance) under realistic control latency, with results that transfer from official sim to the physical robot?

**Intended paper angle (control, not VLA-first)**

A compact contribution around one of:

1. **Whole-body trajectory tracking + balance** with a clear baseline vs proposed controller, or
2. **Latency-aware / delayed whole-body control** on G1 (characterize delay → compensate / schedule → measure stability), or
3. **Hierarchical whole-body interface**: high-level action commands → low-level joint/torque tracking, with failure-mode analysis.

Embodied-AI / VLA action sources are **future integration**, not month-1 deliverables. First ship a trustworthy low-level whole-body stack and evaluation protocol.

## Constraints

| Item | Choice |
| --- | --- |
| Robot | Unitree **G1** (physical robot available) |
| Stack | Official **unitree_sdk2 / unitree_sdk2_python** + **unitree_mujoco** (primary sim); optional Isaac Lab later |
| Group fit | Control-algorithm contribution suitable for a paper |
| Timeline | **~8 weeks** |

### Hardware caveat

**Left camera** was broken on demo day and is **glued back** — treat it as fragile; avoid stress on the head/left vision path and note any camera asymmetry in experiments.

## Resources (all current materials)

Canonical list with links and reading order:

→ [`docs/resources.md`](docs/resources.md)

Highlights:

- Lab Google Doc (start at **Starting Point** tab)
- Unitree G1 official developer docs
- NVIDIA GR00T-WholeBodyControl + GEAR-SONIC
- Pi0.5 and LingBot-VA (later VLA / video-action integration)

## 8-week plan

| Week | Milestone | Done when |
| --- | --- | --- |
| 1 | Bring-up | G1 examples run in `unitree_mujoco`; same DDS path understood for real robot |
| 2 | Logging & metrics | Joint/IMU logs; define metrics: tracking error, CoM/orientation sway, fall rate, end-to-end latency |
| 3–4 | Baseline WBC | Reproduce a simple baseline (e.g. PD tracking / official loco example / QP-WBC lite) in sim |
| 5 | Method | Implement **one** proposed control improvement (see candidates below) |
| 6 | Sim ablations | Baseline vs proposed under delay / disturbance / trajectory suite |
| 7 | Real G1 | Safety-limited experiments; same metrics as sim |
| 8 | Paper draft | Figures, tables, failure cases, limitations |

### Control method candidates (pick one by end of week 2)

1. QP-based whole-body tracking with task priorities (stance / swing / posture)
2. Latency compensation for delayed state/command on G1 DDS loop
3. Adaptive / robust tracking under model mismatch (sim→real)
4. Contact-aware whole-body adjustment for push / uneven start (keep scope small)

**Do not** attempt all four in 8 weeks.

## Safety (real G1)

- Never run untested controllers on the real robot.
- Start with soft limits, emergency stop ready, clear area, spotter present.
- Use the same DDS interface as sim only after sim metrics look stable.
- Log every real-robot trial (config hash, latency, outcome).

## Stack references

- [unitree_sdk2](https://github.com/unitreerobotics/unitree_sdk2)
- [unitree_sdk2_python](https://github.com/unitreerobotics/unitree_sdk2_python) — G1 examples
- [unitree_mujoco](https://github.com/unitreerobotics/unitree_mujoco) — official MuJoCo sim (DDS-compatible)
- Optional later: [unitree_sim_isaaclab](https://github.com/unitreerobotics/unitree_sim_isaaclab)

## Repo layout

```text
docs/                 design notes, experiment protocol, paper outline
configs/              controller / task YAML
src/g1_wbc/           control code (interfaces, controllers, metrics)
scripts/              launch sim, run trials, plot
third_party/          notes or git submodules for Unitree official repos (not vendored blindly)
results/              logs, plots (git-ignore large binaries)
```

## Status

Project scaffolding. Development starts with Week 1 bring-up.

## License

TBD (align with Unitree SDK licenses and lab policy before releasing code that talks to hardware).
