# Project resources (canonical list)

Last updated: 2026-07-28

This is the working bibliography / doc set for the Unitree G1 whole-body control project in **INSI Lab** (Professor Yanzhi Wang).

## Start here (lab internal)

**Main project documentation (Google Doc)** — if you are new, open the **“Starting Point”** tab on the left:

https://docs.google.com/document/d/1QoZlcU7rkTfpKey8VM_59dIZGsulCQKsC3P8WNycYpw/edit?tab=t.i74e3rpuj2kc

Treat this as the primary onboarding source for lab process, setup, and day-to-day notes.

## Robot hardware & official docs (Unitree G1)

- Unitree G1 developer docs (official; site has AI assistant):  
  https://support.unitree.com/home/zh/G1_developer/about_G1
- Prefer official SDK / sim paths linked from Unitree docs and open-source portals:
  - `unitree_sdk2` / `unitree_sdk2_python`
  - `unitree_mujoco`

### Hardware caveat (critical)

> **Left camera** was broken during demo day and has been **glued back**.  
> Treat head/left vision as **fragile**. Avoid bumps, aggressive head motion demos, and unnecessary camera disassembly. Prefer right / other working sensors when possible; document any vision asymmetry in experiments.

## Whole-body control references (NVIDIA)

- **GR00T Whole-Body Control** docs:  
  https://nvlabs.github.io/GR00T-WholeBodyControl/
- **GEAR-SONIC** (humanoid whole-body control / related GEAR stack):  
  https://nvlabs.github.io/GEAR-SONIC/

Use these as high-level WBC / humanoid control references. They are **not** a drop-in replacement for Unitree G1 SDK bring-up; map ideas carefully onto G1 + official DDS.

## High-level VLA / video-action models (integration candidates, not week-1)

| Name | Role | Links |
| --- | --- | --- |
| **π₀.₅ (Pi0.5)** | Physical Intelligence VLA with open-world generalization | [blog](https://www.pi.website/blog/pi05), [openpi](https://github.com/Physical-Intelligence/openpi), [arxiv](https://arxiv.org/abs/2504.16054) |
| **LingBot-VA** | Robbyant video-action / world-model style control | [site](https://technology.robbyant.com/lingbot-va), [github](https://github.com/Robbyant/lingbot-va) |
| **LingBot-VLA** (related) | Broader VLA line from Robbyant | [vla-v2 repo](https://github.com/robbyant/lingbot-vla-v2) |

**Project stance:** control stack + metrics first; Pi0.5 / LingBot-VA are **later high-level action sources** once G1 low-level whole-body interface is stable.

## Suggested reading order for a new contributor

1. Google Doc → **Starting Point** tab  
2. Unitree G1 developer docs (about G1 + control / SDK sections)  
3. Run official G1 example via `unitree_sdk2_python` + `unitree_mujoco`  
4. Skim GR00T-WholeBodyControl / GEAR-SONIC for method ideas  
5. Only then evaluate Pi0.5 or LingBot-VA as a high-level policy plug-in  

## Team note

Small **INSI Lab** team (student + physicist collaborator + advisor). Keep the first paper contribution **control-centric**; use VLA repos as optional upstream brains, not the critical path for week 1–4.
