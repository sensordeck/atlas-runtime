---
Status: ACTIVE DEVELOPMENT SPEC
Authority Level: PUBLIC ENGINEERING SPEC
Document Version: Round 4
Owner: Atlas Runtime / Replay Architecture
Derived From: atlas-core-private/constitution/authority/authority_boundary_spec.md
---

# Atlas Authority Boundary Specification

File: `docs/authority_boundary_spec.md`  
Target Repo: `atlas-dsil-sdk`  
Audience: Firmware, Runtime, Replay, Acceptance  
Round: 4  

---

# 1. Purpose

This document defines the operational authority boundaries used by Atlas Round 4.

It clarifies:

- what constitutes authority
- where authority originates
- where authority terminates
- what Firmware may emit
- what Runtime may interpret
- what Observable Mode may claim
- what Holdover may claim
- what is forbidden

This document is implementation-facing.

It is derived from the constitutional authority boundary specification stored in the private core repository.

---

# 中文关键点

本文件：

给：

- 张工（Firmware）
- 梁工（Runtime）
- replay tooling
- acceptance tooling

统一 authority semantics。

---

# 2. Authority Definition

Atlas defines authority as:

```text
a replayable, observable, auditable source of temporal truth
```

Authority requires:

- deterministic observability
- replay reproducibility
- auditability
- explicit ownership boundary

Authority is NOT defined by:

- UI appearance
- “looks synchronized”
- software confidence alone
- low drift alone

---

# 中文关键点

Authority：

不是：

“感觉很准”。

而是：

```text
Replayable + Observable + Auditable
```

---

# 3. Authority Layers

Atlas defines three operational layers.

---

## L1 — Physical Authority

L1 represents authoritative physical timing.

Examples:

- GNSS PPS edge
- hardware trigger edge
- deterministic physical timing source

L1 establishes:

- physical synchronization boundary
- authoritative timing reference
- replayable physical timing truth

---

## 中文关键点

L1：

是真实物理 authority。

例如：

- PPS interrupt
- hardware edge
- deterministic trigger

不是软件猜测。

---

## L2 — Deterministic Runtime Continuity

L2 represents deterministic continuity authority.

L2 provides:

- replay continuity
- monotonic continuity
- deterministic ordering
- holdover continuity
- bounded drift interpretation

L2 does NOT guarantee:

- UTC truth
- physical synchronization truth

---

## 中文关键点

L2：

不是 physical authority。

L2：

目标是：

```text
Deterministic Continuity
```

即：

即使 PPS 消失：

系统仍然：
- replayable
- deterministic
- monotonic
- drift visible

---

## L3 — Observed System Timing

L3 represents observed but non-authoritative timing.

Examples:

- Linux clock
- ROS timestamps
- sensor timestamps
- application timestamps

L3 MAY:
- drift
- jitter
- reorder
- become inconsistent

L3 is observable but not authoritative.

---

## 中文关键点

L3：

只是：

被观察到的时间。

不是 authority。

---

# 4. Firmware Authority Boundary

Firmware authority ends at:

```text
raw deterministic observation emission
```

Firmware MAY:

- emit local timing
- emit PPS events
- emit power state
- emit deterministic payloads

Firmware MUST NOT:

- reinterpret authority
- fabricate synchronization
- synthesize replay truth
- normalize causality
- emit corrected authority time

---

# 中文关键点

Firmware：

只能：

```text
observe → encode → emit
```

不能：

```text
reinterpret → beautify → normalize
```

---

# 5. Runtime Authority Boundary

Runtime authority begins at:

```text
payload interpretation
```

Runtime MAY:

- calculate corrected timelines
- reconstruct replay continuity
- interpret authority transitions
- estimate drift behavior
- expose degradation visibility

Runtime MUST NOT:

- rewrite protocol truth
- fabricate physical authority
- hide instability
- normalize replay invisibly
- overwrite firmware evidence

---

# 中文关键点

Runtime：

负责解释 authority。

但：

不能偷偷：

创造 authority。

---

# 6. Observable Mode Boundary

Observable Mode MAY:

- observe timing behavior
- compare timing sources
- reconstruct replay
- expose instability
- establish deterministic observation

Observable Mode MUST NOT claim:

- physical synchronization authority
- deterministic hardware integration
- authoritative timing lock

Observable Mode is:

```text
non-invasive observational authority
```

not:

```text
physical authority
```

---

# 中文关键点

Observable Mode：

只能 observe。

不能 claim：

physical authority。

否则：

Non-invasive claim 崩塌。

---

# 7. Holdover Authority Boundary

Holdover represents:

```text
loss of physical authority
while preserving deterministic continuity
```

During holdover:

- replay continuity MAY remain valid
- monotonic continuity MUST remain valid
- drift visibility MUST remain visible

Holdover MUST NEVER:

- fake PPS authority
- fake UTC authority
- pretend synchronization remains physically locked

---

# 中文关键点

Holdover：

不是：

“继续假装 sync”。

而是：

physical authority 消失后：

deterministic continuity 仍然存在。

---

# 8. seq_num Rule

`seq_num` defines:

```text
packet emission order
```

`seq_num` does NOT define:

- authority level
- corrected time
- replay causality
- physical synchronization

---

# 中文关键点

seq_num：

只是：

发包顺序。

不是 authority。

---

# 9. Replay Authority Rule

Replay authority requires:

- deterministic replay
- replayable authority transitions
- visible degradation
- replayable packet ordering
- replayable instability

Replay MUST NOT:

- beautify replay behavior
- hide degraded states
- smooth instability invisibly

---

# 中文关键点

Replay：

必须真实重建 authority 行为。

不能偷偷修好看。

---

# 10. Drift Visibility Rule

Authority systems MUST preserve visibility of:

- drift
- holdover
- PPS instability
- stale state
- packet gaps
- replay reorder
- degraded synchronization

Atlas philosophy:

```text
Visibility > Cosmetic Stability
```

---

# 中文关键点

Atlas：

宁可看到问题。

也不能：

假装 authority 稳定。

---

# 11. Power ↔ Timing Correlation Rule

Authority interpretation MUST preserve:

- power instability visibility
- timing instability visibility
- causal replay visibility

Power and timing MUST remain replay-correlated.

---

# 中文关键点

Power：

也是 authority truth layer。

Power ↔ Timing：

必须 replay 可关联。

---

# 12. Forbidden Authority Claims

The following claims are invalid:

- “Looks synchronized.”
- “Runtime inferred authority.”
- “Replay was normalized.”
- “Drift was hidden.”
- “Synchronization confidence was synthesized.”
- “Authority transitions were hidden.”
- “L3 timestamps are authoritative.”

---

# 中文关键点

禁止：

fake authority。

---

# 13. Runtime Expectations

Round 4 Runtime MUST:

- preserve replay determinism
- preserve packet ordering truth
- preserve authority transition visibility
- preserve degradation visibility
- preserve replay reproducibility

Round 4 Runtime MUST NOT:

- hide degraded states
- normalize replay invisibly
- fabricate synchronization
- reinterpret firmware truth

---

# 中文关键点

Runtime：

不是：

“自动修复系统”。

而是：

deterministic replay reconstruction engine。

---

# 14. Final Engineering Statement

Inside Atlas:

```text
Authority
=
Replayable + Observable + Auditable Temporal Truth
```

Not:

- visual smoothness
- cosmetic synchronization
- inferred confidence
- hidden instability

Authority boundaries MUST remain explicit.

If authority boundaries collapse,
replay truth collapses.

🔒 END OF FILE
