---
Status: ACTIVE DEVELOPMENT SPEC
Authority Level: PUBLIC ENGINEERING SPEC
Document Version: Round 4
Owner: Atlas Evidence Architecture
Derived From: atlas-core-private/constitution/replay/evidence_chain_spec.md
---

# Atlas Evidence Chain Specification

File: `docs/evidence_chain_spec.md`  
Target Repo: `atlas-dsil-sdk`  
Audience: Firmware, Runtime, Replay, Acceptance, Reporting  
Round: 4  

---

# 1. Purpose

This document defines the evidence chain semantics used by Atlas Round 4.

It clarifies:

- what constitutes evidence
- how evidence is preserved
- what invalidates evidence
- what Replay contributes
- what Runtime contributes
- what Firmware contributes
- what auditability requires

This document is implementation-facing.

It is derived from the constitutional evidence chain specification stored in the private core repository.

---

# 中文关键点

Atlas：

不是普通软件系统。

Atlas：

本质是：

```text
Deterministic Evidence System
```

Replay、
Authority、
Certification、

最终都建立在：

```text
Evidence Chain
```

之上。

---

# 2. Evidence Definition

Atlas defines evidence as:

```text
replayable, reproducible, auditable observable temporal truth
```

Evidence REQUIRES:

- deterministic replay
- protocol consistency
- auditability
- authority visibility
- degradation visibility
- traceability

Evidence is NOT:

- screenshots
- UI appearance
- “looks stable”
- cosmetic visualization

---

# 中文关键点

Evidence：

不是截图。

不是 demo 很顺。

Evidence：

必须：

```text
可 replay
可 reproduce
可 audit
```

---

# 3. Evidence Chain Definition

An evidence chain means:

```text
observable temporal truth remains traceable
across acquisition, replay, analysis, and reporting
```

Evidence Chain includes:

- raw acquisition
- protocol frames
- replay logs
- parser interpretation
- replay interpretation
- authority transitions
- report generation
- audit reconstruction

---

# 中文关键点

Evidence Chain：

不是单个 log。

而是：

```text
采集 → replay → report → audit
```

全链条：

都可追溯。

---

# 4. Raw Evidence Rule

Raw evidence is constitutional truth.

Raw evidence includes:

- protocol frames
- seq_num history
- board_time_us
- replay logs
- power health payloads
- authority transitions
- degraded states

Raw evidence MUST NEVER be overwritten.

---

# 中文关键点

Raw Evidence：

是最终真相。

不能：

偷偷覆盖。

不能：

只保留“修复后结果”。

---

# 5. Replay Evidence Rule

Replay contributes to evidence by preserving:

- packet ordering
- authority transitions
- drift visibility
- degraded states
- packet gaps
- replay reorder
- timing instability
- power instability

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

Replay：

必须 preserve 真相。

不能 beautify evidence。

---

# 6. Runtime Evidence Boundary

Runtime MAY:

- interpret authority
- reconstruct replay
- calculate corrected timelines
- expose degradation visibility
- generate replay reports

Runtime MUST NOT:

- overwrite raw truth
- fabricate authority
- normalize instability invisibly
- hide degraded states
- rewrite packet history

---

# 中文关键点

Runtime：

负责 reconstruction。

但：

不能偷偷：

修改 evidence。

---

# 7. Firmware Evidence Boundary

Firmware contributes to evidence by preserving:

- deterministic payload generation
- stable seq_num ordering
- monotonic local timing
- raw event integrity

Firmware MUST NOT:

- rewrite historical timing
- fabricate synchronization
- repair replay gaps silently
- hide instability

---

# 中文关键点

Firmware：

是 evidence producer。

不是 evidence editor。

---

# 8. Auditability Requirement

Evidence MUST remain auditable.

Auditability requires:

- replayable logs
- protocol consistency
- deterministic interpretation
- reproducible replay
- authority visibility
- replay traceability

Without auditability:

```text
evidence is INVALID
```

---

# 中文关键点

Evidence：

必须：

未来还能 audit。

否则：

不能叫 evidence。

---

# 9. Traceability Rule

Evidence MUST remain traceable across:

- acquisition
- replay
- replay interpretation
- authority transitions
- report generation
- certification
- dispute resolution

---

# 中文关键点

Evidence：

必须：

未来还能追责。

---

# 10. Visibility Rule

Evidence MUST preserve visibility of:

- drift
- packet gaps
- PPS instability
- stale state
- degraded authority
- holdover
- replay reorder
- power instability

Atlas philosophy:

```text
Visibility > Cosmetic Stability
```

---

# 中文关键点

Atlas：

宁可看到问题。

也不能：

假装稳定。

---

# 11. Power ↔ Timing Evidence Rule

Power instability and timing instability MUST remain replay-correlated.

Evidence MUST preserve:

- timing events
- power events
- causal relationships
- temporal relationships

---

# 中文关键点

Power：

也是 Evidence Chain 一部分。

Power ↔ Timing：

必须 replay 可关联。

---

# 12. Invalid Evidence Behaviors

The following behaviors invalidate evidence:

- hidden replay normalization
- drift smoothing
- fake synchronization
- authority synthesis
- hidden degraded states
- packet history rewrite
- replay beautification
- stale hiding

---

# 中文关键点

Evidence：

不能偷偷 beautify。

否则：

Evidence Chain 崩塌。

---

# 13. Report Evidence Rule

Reports are NOT evidence.

Reports are:

```text
organizational interpretations of replayable evidence
```

Replayable raw evidence remains the source of truth.

---

# 中文关键点

Report：

不是 evidence。

真正 evidence：

是 replayable raw truth。

---

# 14. Certification Evidence Rule

Certification REQUIRES:

- replayable evidence
- reproducible replay
- deterministic interpretation
- protocol consistency
- authority visibility

Without replayable evidence:

```text
certification is INVALID
```

---

# 中文关键点

Certification：

必须：

evidence-backed。

---

# 15. Observable Mode Evidence Boundary

Observable Mode MAY establish:

- replay evidence
- timing instability visibility
- deterministic observation

Observable Mode MUST NOT claim:

- physical authority ownership
- deterministic hardware integration
- authoritative synchronization ownership

---

# 中文关键点

Observable Mode：

只能建立 observational evidence。

---

# 16. Runtime Expectations

Round 4 Runtime MUST:

- preserve replay determinism
- preserve replay reproducibility
- preserve authority transition visibility
- preserve degradation visibility
- preserve packet ordering truth
- preserve evidence traceability

Round 4 Runtime MUST NOT:

- hide degraded states
- normalize replay invisibly
- fabricate synchronization
- reinterpret firmware truth

---

# 中文关键点

Runtime：

不是：

“自动修系统”。

而是：

deterministic evidence reconstruction engine。

---

# 17. Final Engineering Statement

Inside Atlas:

```text
Evidence
=
Replayable + Reproducible + Auditable Temporal Truth
```

Not:

- screenshots
- visual smoothness
- cosmetic synchronization
- fake stability

Evidence Chains MUST remain intact.

If evidence chain continuity breaks,
replay trust collapses.

If replay trust collapses,
authority trust collapses.

🔒 END OF FILE
