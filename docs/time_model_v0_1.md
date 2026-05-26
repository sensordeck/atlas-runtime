---
Status: ACTIVE DEVELOPMENT SPEC
Authority Level: PUBLIC ENGINEERING SPEC
Document Version: Round 4
Owner: Atlas Runtime / Firmware Integration
Derived From: atlas-core-private/constitution/authority/time_model_v0_1.md
---

# Atlas Time Model V0.1

File: `docs/time_model_v0_1.md`  
Target Repo: `atlas-dsil-sdk`  
Audience: Firmware, Runtime, ROS2, Replay, Acceptance  
Round: 4  

---

# 1. Purpose

This document defines the operational time semantics used by Atlas Round 4 runtime and firmware development.

It clarifies:

- what time means inside Atlas
- what Runtime is allowed to interpret
- what Firmware must preserve
- how deterministic replay is defined
- how authority transitions behave
- how monotonic continuity is preserved

This document is implementation-facing.

It is derived from the constitutional time model stored in the private core repository.

---

# 中文关键点

本文件给：

- 张工（Firmware）
- 梁工（Runtime / ROS2）
- replay / verification tooling

进行 Round 4 开发使用。

它不是：

核心仓 Constitution 文件。

它是：

工程执行语义文件。

---

# 2. Atlas Time Philosophy

Atlas time is NOT defined by:

- UI appearance
- “looks synchronized”
- Linux clock
- ROS clock
- visual smoothness

Atlas time IS defined by:

- deterministic ordering
- replay reproducibility
- authority visibility
- monotonic continuity
- observable degradation

---

# 中文关键点

Atlas 不相信：

“看起来同步”。

Atlas 只相信：

- replay 可 reproduce
- authority 可观察
- degradation 可见
- timing 行为 deterministic

---

# 3. Time Layers

Atlas defines three operational layers of time.

---

## L1 — Authority Time

L1 represents authoritative physical timing.

Examples:

- GNSS PPS edge
- hardware trigger edge
- deterministic physical timing source

L1 establishes:

- physical synchronization boundary
- authoritative timing reference
- replayable synchronization evidence

---

## 中文关键点

L1：

是真实物理 authority。

不是软件猜测。

例如：

- PPS interrupt
- hardware edge
- deterministic trigger

---

## L2 — Deterministic Runtime Time

L2 represents deterministic runtime continuity.

L2 provides:

- replay continuity
- monotonic continuity
- deterministic ordering
- bounded drift behavior
- holdover continuity

L2 does NOT guarantee:

- UTC truth
- physical synchronization truth

---

## 中文关键点

L2：

目标不是“世界标准时间”。

目标是：

```text
Deterministic Continuity
```

即：

即使 PPS 丢失：

系统仍然：
- monotonic
- replayable
- deterministic
- drift visible

---

## L3 — Observed System Time

L3 represents observed but non-authoritative timing.

Examples:

- Linux system time
- ROS header stamp
- sensor internal timestamp
- application timestamps

L3 may drift or reorder.

L3 is observable but not authoritative.

---

## 中文关键点

L3：

只是：

“被观察到的时间”。

不是 authority。

很多 OEM 的问题：

就是：

把 L3 当 authority。

---

# 4. `board_time_us` Definition

`board_time_us` represents:

```text
device-local monotonic observation time
```

It is emitted by Firmware.

`board_time_us` MUST:

- remain monotonic within the same packet family
- preserve local event ordering
- preserve deterministic replay input

`board_time_us` does NOT represent:

- UTC
- corrected runtime time
- replay order
- cross-family causal order

---

# 中文关键点

新人最容易误解：

```text
board_time_us
```

不是：

最终 authority time。

它只是：

设备本地观察时间。

---

# 5. Cross-Family Ordering Rule

Round 4 clarification:

Within the same packet family:

```text
board_time_us MUST remain monotonic
```

Across different packet families:

```text
board_time_us is NOT required to be globally strictly monotonic
```

Global packet emission order is defined by:

```text
seq_num
```

---

# 中文关键点

Round 4 已正式锁定：

同一 packet family 内：

```text
board_time_us 必须 monotonic
```

不同 packet family 之间：

```text
不要求全局严格单调
```

全局发包顺序：

由：

```text
seq_num
```

定义。

---

# 6. `corrected_time_us` Definition

`corrected_time_us` represents:

```text
runtime-interpreted deterministic continuity time
```

It is calculated by Runtime.

Firmware MUST NEVER emit corrected time.

---

# 中文关键点

corrected_time_us：

属于 Runtime。

Firmware：

只能发 raw observation。

不能直接生成 corrected time。

---

# 7. Monotonicity Rule

Monotonicity means:

```text
time MUST NOT move backward
```

within the same deterministic timeline.

Atlas prioritizes:

```text
monotonic continuity
```

over:

```text
cosmetic synchronization appearance
```

---

# 中文关键点

Atlas 宁可：

慢慢 drift。

也不能：

时间倒退。

因为：

rollback 会毁掉 replay truth。

---

# 8. Replay Rule

Replay MUST preserve:

- packet ordering
- timing visibility
- degradation visibility
- authority transitions
- packet gap visibility
- deterministic interpretation

Replay MUST NOT:

- smooth instability invisibly
- hide degraded authority
- fabricate stable timing
- normalize packet reorder silently

---

# 中文关键点

Replay：

不是 demo 播放器。

Replay：

必须真实重建：

- drift
- PPS lost
- degradation
- packet gap
- authority transition

---

# 9. Holdover Rule

Holdover means:

```text
loss of authoritative physical timing
while preserving deterministic continuity
```

During holdover:

- monotonic continuity MUST remain
- replay continuity MUST remain
- drift visibility MUST remain

Runtime MAY estimate drift behavior.

Firmware MUST NOT fabricate PPS lock.

---

# 中文关键点

Holdover：

不是：

“继续假装 PPS 正常”。

而是：

PPS 消失后：

系统仍然 deterministic。

同时：

drift 必须 visible。

---

# 10. Authority Transition Rule

Authority transitions MUST remain:

- observable
- replayable
- deterministic
- auditable

Transitions include:

- UNSYNCED → SYNC
- SYNC → HOLDOVER
- HOLDOVER → RECOVERY
- RECOVERY → SYNC

Transitions MUST NOT be hidden.

---

# 中文关键点

Authority transition：

必须 replay 可见。

不能偷偷切换。

否则：

未来：
- replay audit
- certification
- dispute analysis

都会崩。

---

# 11. `seq_num` Rule

`seq_num` defines:

```text
packet emission order
```

`seq_num` exists to preserve:

- emission determinism
- packet gap visibility
- replay consistency

`seq_num` does NOT define:

- authority level
- corrected time
- causal truth

---

# 中文关键点

seq_num：

只是：

发包顺序。

不是 authority。

---

# 12. Drift Visibility Rule

Atlas philosophy:

```text
Visibility > Cosmetic Stability
```

Runtime MUST NOT:

- hide drift
- hide instability
- smooth degradation invisibly

Firmware MUST NOT:

- fabricate stable timing
- fake synchronization confidence

---

# 中文关键点

Atlas 最核心原则之一：

```text
Visibility > Pretty Demo
```

看到问题，
比假装稳定重要。

---

# 13. Deterministic Continuity Rule

Deterministic continuity means:

Given the same replay input,
Atlas MUST produce:

- the same ordering
- the same authority transitions
- the same degradation visibility
- the same replay interpretation

---

# 中文关键点

Deterministic：

不是：

“时间很准”。

而是：

同样 replay 输入：

必须得到同样结果。

---

# 14. Runtime Ownership Boundary

Runtime owns:

- authority interpretation
- corrected time calculation
- replay reconstruction
- degradation visibility
- authority transitions
- replay audit behavior

Firmware owns:

- raw event emission
- local timing observation
- deterministic payload generation

---

# 中文关键点

Firmware：

发出真相。

Runtime：

解释真相。

边界不能混。

---

# 15. Forbidden Assumptions

The following assumptions are rejected for Round 4:

- “Runtime can silently fix drift.”
- “Firmware can smooth replay.”
- “L3 timestamps are authoritative.”
- “Replay only needs to look stable.”
- “Authority transitions can be hidden.”
- “Packet reorder can be normalized invisibly.”

---

# 中文关键点

Atlas 最大危险：

不是 bug。

而是：

偷偷“美化时间”。

---

# 16. Round 4 Runtime Expectations

Round 4 Runtime MUST:

- preserve replay determinism
- expose degraded authority
- expose drift visibility
- preserve packet ordering truth
- preserve packet gap visibility
- reconstruct deterministic replay behavior
- preserve authority transition visibility

Round 4 Runtime MUST NOT:

- silently normalize reorder
- fabricate synchronization confidence
- hide degraded states
- reinterpret protocol semantics
- rewrite firmware truth

---

# 中文关键点

梁工 Runtime：

不是：

“自动修复系统”。

而是：

deterministic evidence reconstruction engine。

---

# 17. Final Engineering Statement

Atlas time exists to establish:

- authoritative temporal truth
- deterministic continuity
- replayable evidence
- auditable authority transitions
- observable degradation

Firmware emits truth.

Runtime reconstructs deterministic evidence from that truth.

Both sides must preserve replay integrity.

🔒 END OF FILE
