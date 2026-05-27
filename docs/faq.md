# FAQ

## How is Atlas different from rosbag replay tools?

Atlas Runtime is not a rosbag replay utility.

Atlas focuses on runtime timing governance, replay admissibility, runtime evidence generation, and deterministic investigation workflows.

---

## Does Atlas replace PTP or GNSS synchronization?

No.

Atlas operates above existing timing infrastructure.

---

## Does Atlas require hardware changes?

No.

Atlas Runtime is designed for non-invasive deployment.

---

## Can Atlas determine root cause automatically?

Not always.

Atlas focuses on replay-backed runtime investigation and admissibility analysis.

---

## What ROS2 distributions are supported?

Current baseline support:

- Humble
- Iron

Experimental:

- Rolling

---

## What hardware platforms are supported?

Current public baseline targets:

- x86_64
- Jetson
- ARM64

---

## Are evidence package schemas versioned?

Yes.

Runtime evidence structures are expected to evolve under explicit schema versioning.