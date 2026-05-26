---
Status: RATIFIED
Authority Level: ACCEPTANCE CONSTITUTION
Document Version: V0.1
Ratification ID: ATR-ACCEPTANCE-RAT-2026-001
Owner: Atlas Acceptance Authority
Immutable: YES
---

# Atlas Constitution
# acceptance_v0_1.md

---

# 1. Purpose

This document defines the constitutional acceptance semantics for Atlas V0.1.

It establishes:

- what constitutes completion
- what constitutes acceptance
- what evidence is required
- what replay guarantees are required
- what invalidates acceptance
- what Runtime must demonstrate
- what Firmware must demonstrate

This document exists to prevent:

- fake completion
- feature-only validation
- unverifiable demos
- replay inconsistency
- authority ambiguity
- certification instability

---

# 中文关键说明（新人必读）

Atlas：

不是 feature-driven。

Atlas：

是：

```text
Evidence-Driven System
```

---

# Atlas 最大原则之一

在 Atlas：

```text
“代码能跑”
≠
完成
```

```text
“demo 很顺”
≠
acceptance pass
```

真正 acceptance：

必须：

```text
Replayable + Reproducible + Auditable
```

---

# 2. Constitutional Definition of Acceptance

Atlas defines acceptance as:

```text
successful demonstration of replayable, reproducible, auditable temporal evidence
```

Acceptance REQUIRES:

- protocol consistency
- replay reproducibility
- deterministic interpretation
- authority visibility
- degradation visibility
- auditability

Acceptance is NOT:

- screenshots
- UI appearance
- visual smoothness
- feature existence
- temporary demo success

---

# 中文关键点

Acceptance：

不是：

“功能有了”。

而是：

Evidence Pass。

---

# 3. Acceptance Evidence Rule

Acceptance REQUIRES replayable evidence.

Required evidence includes:

- raw protocol logs
- replay logs
- replay reproducibility
- seq_num continuity visibility
- authority transition visibility
- degradation visibility
- power ↔ timing visibility

Without replayable evidence:

```text
acceptance is INVALID
```

---

# 中文关键点

没有 replay evidence：

等于：

没有 acceptance。

---

# 4. Protocol Acceptance Requirements

Accepted systems MUST demonstrate:

- protocol-compliant payloads
- correct CRC behavior
- correct endian behavior
- deterministic seq_num behavior
- reserved field zeroing
- payload length consistency
- protocol replay compatibility

---

# 中文关键点

Acceptance：

必须：

protocol 正确。

不是：

“差不多能 decode”。

---

# 5. Firmware Acceptance Requirements

Firmware MUST demonstrate:

- deterministic payload generation
- monotonic board_time_us behavior
- seq_num continuity
- replay-compatible payload emission
- truthful degraded state visibility
- truthful power state visibility

Firmware MUST NOT:

- smooth instability invisibly
- fabricate synchronization
- rewrite historical timing
- silently repair replay gaps

---

# 中文关键点

Firmware acceptance：

重点：

不是“稳定”。

而是：

```text
真实 + deterministic
```

---

# 6. Runtime Acceptance Requirements

Runtime MUST demonstrate:

- deterministic replay reconstruction
- replay reproducibility
- authority transition visibility
- degradation visibility
- packet gap visibility
- replay ordering consistency
- corrected timeline consistency

Runtime MUST NOT:

- normalize replay invisibly
- fabricate authority
- hide degraded states
- rewrite protocol truth

---

# 中文关键点

Runtime：

不是：

“自动修 replay”。

而是：

Evidence Reconstruction Engine。

---

# 7. Replay Acceptance Requirements

Replay MUST demonstrate:

- reproducible replay behavior
- deterministic interpretation
- replay ordering preservation
- authority transition preservation
- replay instability visibility

Replay MUST preserve:

```text
truth
```

not:

```text
cosmetic smoothness
```

---

# 中文关键点

Replay acceptance：

不是：

“回放很顺”。

而是：

Replay Truth preserved。

---

# 8. Authority Acceptance Requirements

Accepted systems MUST preserve visibility of:

- authority transitions
- holdover
- PPS instability
- degraded synchronization
- stale state
- drift visibility

Authority MUST NEVER be fabricated.

---

# 中文关键点

Authority acceptance：

不能：

fake sync。

---

# 9. Power ↔ Timing Acceptance Requirements

Accepted systems MUST preserve replay visibility of:

- timing instability
- power instability
- power ↔ timing causal relationships

Power MUST remain:

```text
first-class replay evidence
```

---

# 中文关键点

Power：

不是附属功能。

Power：

是 acceptance truth layer。

---

# 10. Observable Mode Acceptance Boundary

Observable Mode MAY establish:

- replay evidence
- deterministic observation
- timing instability visibility

Observable Mode MUST NOT claim:

- physical synchronization authority
- deterministic hardware integration
- authoritative timing ownership

---

# 中文关键点

Observable Mode：

只能证明：

observational evidence。

---

# 11. Holdover Acceptance Requirements

Holdover acceptance REQUIRES:

- monotonic continuity
- replay continuity
- drift visibility
- degraded authority visibility

Holdover MUST NEVER:

- fake PPS authority
- hide degraded authority
- synthesize synchronization continuity

---

# 中文关键点

Holdover acceptance：

必须 visible degraded。

不能：

继续假装 sync。

---

# 12. seq_num Acceptance Rule

Accepted systems MUST preserve:

```text
packet emission ordering truth
```

Primary replay ordering source:

```text
seq_num
```

Replay MUST preserve seq_num visibility.

---

# 中文关键点

seq_num：

必须 replay 可见。

否则：

packet truth 崩塌。

---

# 13. Invalid Acceptance Behaviors

The following invalidate acceptance:

- hidden replay normalization
- drift smoothing
- fake synchronization
- authority synthesis
- replay beautification
- packet history rewrite
- hidden degraded states
- stale hiding

---

# 中文关键点

Acceptance：

不能：

偷偷 beautify。

---

# 14. Report Acceptance Rule

Reports are NOT acceptance truth.

Reports are:

```text
organizational interpretations of replayable evidence
```

Replayable raw evidence remains constitutional truth.

---

# 中文关键点

Report：

不是 acceptance。

真正 acceptance：

来自 replay evidence。

---

# 15. Certification Dependency Rule

Certification depends on:

- replayable evidence
- deterministic replay
- protocol consistency
- authority visibility
- auditability

If acceptance evidence collapses:

```text
certification collapses
```

---

# 中文关键点

Certification：

建立在 acceptance evidence 之上。

---

# 16. New Engineer Warning

New engineers often incorrectly assume:

- feature complete = accepted
- demo stable = accepted
- replay smooth = accepted
- UI stable = accepted
- screenshots prove success

These assumptions are constitutionally invalid.

---

# 中文关键点

Atlas：

不是：

Demo Civilization。

Atlas：

是：

Evidence Civilization。

---

# 17. Final Constitutional Statement

Inside Atlas:

```text
Acceptance
=
Replayable + Reproducible + Auditable Temporal Evidence Pass
```

Not:

- feature existence
- visual smoothness
- demo stability
- cosmetic synchronization

Acceptance MUST preserve truth.

If replay truth collapses,
acceptance collapses.

If acceptance collapses,
certification collapses.

Implementation evolves.

Acceptance constitutional truth does not.

🔒 END OF FILE
