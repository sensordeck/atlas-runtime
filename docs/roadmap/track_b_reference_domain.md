# Track B — Reference Domain Roadmap
# Status: ROADMAP
# Scope: Post-V1 Reference-Domain Development

---

## 1. Purpose

Track B defines Atlas reference-domain development after V1 runtime governance freeze.

Track B turns Atlas from runtime-only governance into reference-assisted governance.

Track B is NOT required for V1 runtime release.

---

## 2. Core Direction

Track B focuses on:

- Fusion V2 bring-up
- GNSS + IMU canonical sensor baseline
- power reference baseline
- timing-power weak co-observation
- external reference board prototype
- Fusion Core / Runtime Reference Board
- certified reference-domain package

---

## 3. Canonical Sensor Baseline

Atlas reference baseline uses:

- GNSS / PPS
- IMU
- POWER_HEALTH

GNSS + IMU are fixed baseline sensors for future reference-domain development.

IMU stop / recovery / short silence MUST become baseline controlled events.

---

## 4. Controlled Event Baseline

Track B CE baseline includes:

- PPS / GNSS authority transition
- IMU continuity transition
- runtime disturbance
- weak timing-power co-observation
- Fusion power reference baseline
- Linux scheduling disturbance
- DDS / topic burst
- stable governance window
- replay visibility boundary
- metadata integrity boundary

Every CE must be either:

- replay-visible
- or honestly limitation-preserved

No fabricated evidence windows are allowed.

---

## 5. Fusion Role

Fusion is:

- optional trust accelerator
- reference-domain prototype
- controlled runtime reference node
- future certified reference baseline candidate

Fusion is NOT:

- mandatory for V1 runtime release
- mandatory for runtime-only governance
- automatic authority promotion
- certification proof by itself

---

## 6. Firmware Scope

Firmware-side Track B work may include:

- GNSS / PPS capture
- IMU stream continuity
- POWER_HEALTH visibility
- CE trigger visibility
- stable raw packet stream
- build identity
- board identity
- snapshot packaging

Firmware is NOT responsible for:

- governance classification
- legal interpretation
- supplier / OEM blame
- portal rendering
- DCA acceptance

---

## 7. Acceptance Target

Track B acceptance requires:

- replay-visible GNSS + IMU baseline
- stable raw runtime stream
- CE operator notes
- raw SHA256 sidecar
- firmware build ID
- board/setup ID
- runtime command
- limitation notes
- canonical reference snapshot

---

## 8. Relationship to Track A

Track B consumes Track A.

Track B MUST preserve:

- replay admissibility boundary
- limitation preservation
- comparison neutrality
- no scoring
- no authority promotion without explicit evidence

---