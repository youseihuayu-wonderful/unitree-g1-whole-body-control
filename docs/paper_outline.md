# Paper outline (draft)

## Working title options

1. Latency-Aware Whole-Body Tracking on a Unitree G1 Humanoid
2. From Official Sim to Real G1: A Whole-Body Control Study with Stability Metrics
3. Hierarchical Whole-Body Command Tracking for Unitree G1 under Real-Time Constraints

## Claim (one sentence)

We present a whole-body control pipeline on Unitree G1 that improves [tracking / balance / delayed-loop stability] relative to a clear baseline, validated in official simulation and on the physical robot with explicit latency and failure metrics.

## Must-have experiments

- Baseline vs proposed on the same trajectory suite (sim)
- Latency sweep or measured DDS loop delay impact
- At least one disturbance or model-mismatch condition
- Real-robot subset with identical metrics (even if smaller N)
- Failure taxonomy: fall, tracking blow-up, oscillation, emergency stop

## Out of scope for this paper window

- Training a new VLA foundation model
- Full open-world loco-manipulation
- Multi-robot / multi-view perception as the main contribution
