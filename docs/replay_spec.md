---
Status: ACTIVE DEVELOPMENT SPEC
Authority Level: PUBLIC ENGINEERING SPEC
Document Version: Round 4
Owner: Atlas Replay Architecture
Derived From: atlas-core-private/constitution/replay/replay_spec.md
---

# Atlas Replay Specification

File: `docs/replay_spec.md`  
Target Repo: `atlas-dsil-sdk`  
Audience: Firmware, Runtime, Replay, Acceptance  
Round: 4  

---

# 1. Purpose

This document defines the replay semantics used by Atlas Round 4.

It clarifies:

- what replay fundamentally is
- what replay MUST preserve
- what replay MUST NEVER modify
- what replay truth means
- what invalidates replay evidence
- what Runtime owns
- what Firmware contributes

This document is implementation-facing.

It is derived from the constitutional replay specification stored in the private core repository.

---

# 中文关键点

Replay：

不是播放器。

Replay：

是：

```text
Deterministic Evidence Reconstruction
```

---

# Atlas 最大核心之一

如果 replay 不可信：

则：

- deterministic claim 崩塌
- authority claim 崩塌
- evidence 崩塌
- certification 崩塌

所以：

Replay 是 Round 4 核心基础设施。

---

# 2. Replay Definition

Atlas defines replay as:

```text
deterministic reconstruction of observable temporal evidence
```

Replay requires:

- reproducibility
- auditability
- ordering preservation
- authority preservation
- degradation preservation
- causal visibility

Replay is NOT:

- cosmetic playback
- smoothing engine
- visualization only
- inferred synchronization

---

# 中文关键点

Replay：

不是：

“视频回放”。

Replay：

是：

Temporal Evidence Reconstruction。

---

# 3. Replay Truth Definition

Replay truth means:

```text
replay output preserves original observable temporal behavior
```

Replay truth REQUIRES preservation of:

- packet ordering
- seq_num continuity
- authority transitions
- drift visibility
- degraded states
- replay reorder
- packet gaps
- timing instability
- power instability

---

# 中文关键点

Replay Truth：

要求：

真实重建：

- drift
- PPS lost
- holdover
- packet gap
- instability

不能偷偷修平。

---

# 4. Replay MUST Preserve

Replay MUST preserve:

- raw packet ordering
- seq_num ordering
- authority transitions
- degradation visibility
- timing instability
- power instability
- packet gap visibility
- stale visibility
- replay reorder visibility

Replay MUST preserve:

```text
observable truth
```

not:

```text
cosmetic smoothness
```

---

# 中文关键点

Replay：

宁可 ugly。

也必须真实。

---

# 5. Replay MUST NEVER Modify

Replay MUST NEVER:

- rewrite payload truth
- hide drift
- normalize reorder invisibly
- smooth instability invisibly
- fabricate synchronization
- hide degraded states
- synthesize authority
- overwrite seq_num history

Replay MUST NEVER prioritize:

```text
demo smoothness
```

over:

```text
evidence integrity
```

---

# 中文关键点

Replay 最大禁令：

禁止 beautify replay。

---

# 6. Replay Determinism Rule

Replay determinism means:

```text
Given the same replay input,
Atlas MUST produce the same replay interpretation.
```

Replay determinism includes:

- ordering consistency
- authority transition consistency
- degradation consistency
- replay output consistency

---

# 中文关键点

Replay deterministic：

不是：

“很准”。

而是：

同样 replay 输入：

得到同样 replay 输出。

---

# 7. Replay Ordering Rule

Replay MUST preserve:

```text
packet emission ordering truth
```

Primary replay ordering source:

```text
seq_num
```

Replay MUST NOT silently reorder packets.

If reorder occurs:

```text
reorder visibility MUST remain observable
```

---

# 中文关键点

Replay：

不能偷偷 reorder。

如果 reorder：

必须 visible。

---

# 8. Replay Authority Rule

Replay MUST preserve:

- authority transitions
- holdover visibility
- PPS loss visibility
- degraded authority visibility
- stale visibility

Replay MUST NEVER:

- fabricate stable authority
- pretend PPS remained locked
- synthesize authority continuity

---

# 中文关键点

Replay：

不能：

“继续假装 sync”。

---

# 9. Replay Drift Rule

Replay MUST preserve visibility of:

- drift
- jitter
- degraded synchronization
- stale timing
- unstable timing behavior

Atlas philosophy:

```text
Visibility > Cosmetic Stability
```

---

# 中文关键点

Atlas：

不能：

“漂移太丑，所以隐藏”。

真正 replay：

必须保留 drift visibility。

---

# 10. Replay and Holdover

During holdover:

Replay MUST preserve:

- monotonic continuity
- replay continuity
- drift visibility
- degraded authority visibility

Replay MUST NEVER:

- fake PPS authority
- hide holdover
- synthesize UTC continuity

---

# 中文关键点

Holdover replay：

必须 visible。

不能：

假装 PPS 还存在。

---

# 11. Replay and Power Correlation

Replay MUST preserve:

- power instability visibility
- timing instability visibility
- power ↔ timing causal relationships

Replay MUST support:

```text
Power ↔ Timing Evidence Correlation
```

---

# 中文关键点

Power：

也是 replay truth layer。

---

# 12. Runtime Replay Boundary

Runtime owns:

- replay reconstruction
- replay interpretation
- replay continuity
- authority transition interpretation
- corrected timeline generation

Runtime MUST NOT:

- overwrite raw truth
- rewrite packet history
- normalize instability invisibly
- fabricate stable replay

---

# 中文关键点

Runtime：

负责 replay reconstruction。

但：

不能偷偷修 replay。

---

# 13. Firmware Replay Boundary

Firmware contributes to replay truth by preserving:

- deterministic payload generation
- stable seq_num ordering
- monotonic local timing
- raw event integrity

Firmware MUST NOT:

- rewrite historical timing
- fabricate synchronization
- repair replay gaps silently

---

# 中文关键点

Firmware：

是 replay evidence producer。

不是 replay editor。

---

# 14. Observable Mode Replay Boundary

Observable Mode MAY establish:

- replay evidence
- deterministic observation
- timing instability visibility

Observable Mode MUST NOT claim:

- physical synchronization replay authority
- deterministic hardware integration
- authoritative timing ownership

---

# 中文关键点

Observable Mode replay：

只能观察 replay truth。

---

# 15. Invalid Replay Behaviors

The following replay behaviors are invalid:

- hidden reorder normalization
- drift smoothing
- replay beautification
- authority synthesis
- replay gap hiding
- stale hiding
- fake synchronization continuity

---

# 中文关键点

Replay：

不能偷偷变好看。

否则：

Replay Truth 崩塌。

---

# 16. Replay Evidence Requirement

Replay evidence REQUIRES:

- replayable raw logs
- reproducible replay output
- deterministic replay interpretation
- visible degradation
- visible instability
- protocol consistency

Without replayable evidence:

```text
Replay claim is INVALID
```

---

# 中文关键点

没有 replay evidence：

等于：

没有 deterministic evidence。

---

# 17. Runtime Expectations

Round 4 Runtime MUST:

- preserve replay determinism
- preserve authority transition visibility
- preserve degradation visibility
- preserve replay reproducibility
- preserve packet ordering truth

Round 4 Runtime MUST NOT:

- hide degraded states
- normalize replay invisibly
- fabricate synchronization
- reinterpret firmware truth

---

# 中文关键点

Runtime：

不是：

“自动修 replay”。

而是：

deterministic replay reconstruction engine。

---

# 18. Final Engineering Statement

Inside Atlas:

```text
Replay
=
Deterministic reconstruction of observable temporal truth
```

Not:

- cosmetic playback
- visual smoothing
- fake synchronization
- hidden degradation

Replay MUST preserve truth,
even when truth is ugly.

If replay truth collapses,
deterministic evidence collapses.

🔒 END OF FILE
