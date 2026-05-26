# Atlas Post-V1 Development Plan
# Status: ROADMAP
# Scope: After Atlas Runtime Release V1 Freeze

---

## 1. Purpose

This document defines Atlas development after Runtime Release V1.

V1 proves:

- runtime governance extraction
- replay admissibility
- limitation preservation
- neutral comparison
- customer trust surface
- portal payload foundation

Post-V1 development MUST NOT reopen V1 governance semantics unless a formal change proposal is approved.

---

## 2. Track A — Runtime Governance Foundation

Status:

LOCK AFTER CANONICAL SNAPSHOT

Track A includes:

- PR53 Runtime Governance Extraction
- PR54 Governance Semantics
- PR55 OEM Evidence Pack
- PR56 Neutral Governance Comparison
- PR57 Customer Trust Surface
- PR58 Portal Payload Foundation

Allowed after freeze:

- bug fix
- compatibility fix
- packaging fix
- documentation clarification
- release automation

Not allowed after freeze:

- redefining replay admissibility
- redefining limitation semantics
- changing comparison boundary
- adding scoring
- adding deployment authorization semantics
- adding certification pass/fail semantics

---

## 3. Track B — Reference Domain / Fusion Runtime Baseline

Track B develops Atlas reference-domain capability.

Track B is NOT required for V1 runtime release.

Track B includes:

- Fusion V2 bring-up
- GNSS + IMU canonical sensor baseline
- power reference baseline
- external reference domain prototype
- reference-assisted runtime governance
- certified runtime reference package
- Fusion Core / external reference board direction

Track B consumes Track A.

Track B MUST NOT rewrite Track A governance semantics.

---

## 4. Track C — Portal + Certification Productization

Track C develops customer-facing product infrastructure.

Track C includes:

- Portal frontend
- evidence window viewer
- limitation viewer
- governance comparison viewer
- supplier / OEM comparison workflow
- Atlas Verified Sensor workflow
- certified sensor registry
- pilot onboarding flow
- customer-facing 1-page PDF surfaces

Track C consumes Track A and Track B outputs.

Track C MUST NOT become a hidden decision engine.

---

## 5. Track D — Authority Infrastructure

Track D is future authority expansion.

Track D includes:

- stronger GNSS authority boundary
- IMU alignment authority
- cross-device timing governance
- certified reference domain
- power-truth correlation
- distributed authority
- signing / verification
- fleet-level governance

Track D is future moat expansion.

Track D is NOT V1 launch prerequisite.

---

## 6. Release Discipline

V1 freeze means:

- Track A is the canonical runtime governance baseline
- future development must be additive
- future development must preserve limitation visibility
- future development must avoid fake certainty

No future track may convert:

- FAIL → PASS
- UNKNOWN → RESOLVED
- LIMITED → CERTIFIED

without explicit evidence and approved governance change.

---

## 7. Current Priority After Snapshots

After all freelancer snapshots are received:

1. merge PR58 if accepted
2. create ATLAS_V1_CANONICAL_FREEZE_001
3. archive to vault
4. cold backup
5. package runtime release v1
6. prepare static 1-page launch PDFs
7. prepare portal mock / canary
8. start Track B / Track C planning

---