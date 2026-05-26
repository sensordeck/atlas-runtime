# Atlas DSIL Protocol Specification v1.0

Status: FROZEN POST-AUDIT  
Document Authority: Highest wire contract authority  
Wire Format: FROZEN  
中文说明：本文件是 Atlas DSIL v1.0 的最高 wire contract。张工、梁工、霍工、host parser、replay 工具都必须按此执行。任何代码、个人理解、历史实现都不能覆盖本文件。

---

## 1. Purpose / 目的

This document defines the Atlas DSIL v1.0 wire protocol between firmware and host/runtime.

中文关键点：

- 本文件只定义 firmware 发出来的 raw binary frame。
- 本文件决定字段顺序、字段长度、payload_len、CRC、endian、seq_num、board_time_us 原始语义。
- 本文件不定义 runtime corrected_time，也不定义销售报告解释。
- 如果代码和本文件冲突，以本文件为准。

---

## 2. Scope / 范围

This protocol defines raw transport evidence.

This protocol does NOT define:

- corrected_time_us generation
- runtime authority normalization
- deterministic replay algorithm
- report scoring
- OEM business interpretation
- sensor certification decision

中文关键点：

- Firmware 只负责发原始事实。
- Runtime 才负责 corrected_time / authority normalization。
- Replay 工具负责确定性复现。
- Report 负责展示证据，不允许篡改 raw 字段。

---

## 3. Endianness / 字节序

All multi-byte integer fields MUST be little-endian.

Applies to:

- uint16
- uint32
- uint64
- int32

中文关键点：

- 所有多字节字段都是 little-endian。
- 不允许某些字段 big-endian、某些字段 little-endian。
- Host parser 必须按 little-endian 解码。

---

## 4. Frame Layout / 帧格式

| Field | Size | Type | Description |
|---|---:|---|---|
| header | 2 | uint16 | Fixed magic `0x4453` |
| version | 2 | uint16 | Protocol version `0x0100` |
| type | 1 | uint8 | Packet type |
| seq_num | 4 | uint32 | Global packet emission sequence |
| board_time_us | 8 | uint64 | Firmware-local board timestamp |
| payload_len | 2 | uint16 | Payload length in bytes |
| payload | N | bytes | Packet payload |
| crc32 | 4 | uint32 | DSIL_CRC32_V1 |

CRC input length:

    19 + payload_len

Example:

    payload_len = 20
    CRC input length = 39 bytes

中文关键点：

- CRC 不包含 crc32 字段本身。
- payload_len 必须等于实际 payload 字节数。
- 不允许 parser “猜长度”。

---

## 5. Header / 帧头

`header` MUST equal `0x4453`.

Wire encoding:

    0x53 0x44

中文关键点：

- header 错误，frame 必须 reject。
- 不允许 host 自动修复 header。

---

## 6. Version / 协议版本

`version` MUST equal `0x0100`.

中文关键点：

- protocol version 和 firmware version 是两回事。
- firmware 可以升级，但只要 wire format 不变，version 仍然是 `0x0100`。
- firmware app version 不允许改变 protocol semantics。

---

## 7. seq_num / 全局发送顺序

`seq_num` is a uint32 global packet emission sequence.

Rules:

- MUST start from 0 after firmware boot/reset
- MUST increment by 1 for every emitted frame
- MUST be global across all packet types
- MAY wrap from `0xFFFFFFFF` to `0x00000000`
- Host MUST detect gaps
- Host MUST detect duplicates

### 7.1 Transmission Ordering Authority / 发送顺序权威

`seq_num` is the ONLY global transmission-order authority.

中文关键点：

- `seq_num` 定义 packet emission order。
- replay 必须按 `seq_num` 顺序。
- 不允许用 `board_time_us` 重新排序 replay。
- 不允许用 corrected_time 重新排序 raw log。

---

## 8. board_time_us / 板端时间

`board_time_us` is firmware-local board time in microseconds.

Rules:

- Epoch: firmware boot / power-on reset
- Unit: microseconds
- Source: firmware hardware timer
- Firmware MUST NOT apply runtime correction
- Firmware MUST NOT emit corrected_time_us
- Firmware MUST NOT rewrite historical board_time_us

中文关键点：

- `board_time_us` 是 firmware 看到 event/sample 发生时的板端时间。
- 它不是 UTC。
- 它不是全局发送顺序。
- 它不是 replay 排序依据。
- 它不是 corrected_time。

### 8.1 Causal Ordering / 因果顺序

For same `(type, sensor_id)`:

- `board_time_us` MUST be strictly increasing

For different `sensor_id`:

- `board_time_us` MAY be equal
- `board_time_us` MAY appear non-monotonic relative to seq_num
- equal board_time_us does NOT imply physical causality

Tie-break rule:

- If different sensors have equal `board_time_us`, `seq_num` is deterministic tie-breaker.
- Host MUST NOT infer cross-sensor causality from board_time_us alone.

中文关键点：

- 同一个 sensor_id 不能时间倒退。
- 不同 sensor_id 之间允许 board_time_us 相同。
- 相同时间不代表物理同时发生。
- 跨 sensor 因果关系由 runtime/time_model 处理，不由 wire protocol 直接声明。

---

## 9. corrected_time_us Ownership / corrected_time 所属权

`corrected_time_us` is NOT part of this wire protocol.

Firmware MUST NOT emit corrected_time_us.

Runtime MAY generate corrected_time_us as derived output.

Host/runtime MUST preserve original:

- seq_num
- board_time_us
- sensor_time_us

中文关键点：

- firmware 不能修时间。
- parser 不能覆盖原始 board_time_us。
- runtime 可以生成 corrected_time_us，但必须保留 original raw fields。
- dispute 时，original raw log 是第一证据。

Recommended decoded schema:

    original:
      seq_num: 100
      board_time_us: 12345678
      sensor_time_us: 12345600
    corrected:
      corrected_time_us: 12345700

---

## 10. Packet Types / 包类型

| Type | Name | Status |
|---:|---|---|
| 0x01 | HEARTBEAT | REQUIRED |
| 0x02 | TIMING_EVENT | REQUIRED |
| 0x03 | POWER_HEALTH | REQUIRED |
| 0x04 | GNSS_STATUS | RESERVED / NOT EMITTED |
| 0x05 | IMU_SAMPLE | RESERVED / NOT EMITTED |
| 0x06 | DEBUG_EVENT | OPTIONAL |

中文关键点：

- v1.0 firmware 不发 `0x04` / `0x05`。
- IMU 事件应通过 `TIMING_EVENT` 表达，不通过 `IMU_SAMPLE`。
- GNSS/PPS authority 事件也通过 `TIMING_EVENT` 表达。
- 未定义 packet type 不允许靠猜解析。

---

## 11. HEARTBEAT

Packet type: `0x01`  
Payload length: `8 bytes`

| Offset | Field | Type | Size |
|---:|---|---|---:|
| 0 | device_state | uint8 | 1 |
| 1 | sync_state | uint8 | 1 |
| 2 | pps_locked | uint8 | 1 |
| 3 | holdover_active | uint8 | 1 |
| 4 | app_version_major | uint8 | 1 |
| 5 | app_version_minor | uint8 | 1 |
| 6 | reserved0 | uint8 | 1 |
| 7 | reserved1 | uint8 | 1 |

Rules:

- reserved0 MUST be 0
- reserved1 MUST be 0
- Recommended cadence: 1 Hz
- Minimum protocol compliance: 0.2 Hz
- Missing HEARTBEAT > 5 seconds: host MAY declare transport degraded

中文关键点：

- HEARTBEAT 固定 8 bytes。
- app_version 是 firmware app version，不是 protocol version。
- reserved 字段不能偷偷塞语义。

---

## 12. TIMING_EVENT

Packet type: `0x02`  
Payload length: `20 bytes`

| Offset | Field | Type | Size |
|---:|---|---|---:|
| 0 | event_type | uint8 | 1 |
| 1 | sensor_id | uint8 | 1 |
| 2 | tier | uint8 | 1 |
| 3 | confidence | uint8 | 1 |
| 4 | sensor_time_us | uint64 | 8 |
| 12 | delta_us | int32 | 4 |
| 16 | flags | uint32 | 4 |

中文关键点：

- TIMING_EVENT 固定 20 bytes。
- 这是 Atlas timing evidence 的核心 payload。
- 不允许新增字段、不允许压缩字段、不允许重排字段。

### 12.1 event_type

`event_type` identifies the timing event.

中文关键点：

- event_type 表示事件类型，不改变 frame decoding。
- 未知 event_type 不允许破坏 parser。

### 12.2 sensor_id

`sensor_id` identifies the source sensor or timing source.

Rules:

- MUST be stable during one firmware run
- MUST NOT be reused for different physical sources during one run

中文关键点：

- 同一次运行中，sensor_id 不能复用给不同物理源。
- sensor_id registry 后续由 certification 文件管理。

### 12.3 tier

| Value | Name | Meaning |
|---:|---|---|
| 0 | NONE | No authority |
| 1 | TRANSPORT | Transport/local timing only |
| 2 | DERIVED | Derived timing source |
| 3 | AUTH | PPS-qualified authority observation |

中文关键点：

- `AUTH` 只能用于 PPS-qualified observation。
- 具体 PPS-qualified 条件放在 `firmware_interface_spec.md`。
- protocol 只保留 tier 字段，不在这里实现 PPS 状态机。

### 12.4 confidence

| Value | Meaning |
|---:|---|
| 0 | NO_CONFIDENCE / INVALID |
| 1-100 | Reserved for future use |
| 101-200 | Normal operation range |
| 201-254 | High confidence |
| 255 | MAX_CONFIDENCE / AUTH tier |

Rules:

- confidence MUST NOT be used as boolean
- confidence = 0 means event MUST NOT enter runtime time model
- confidence = 255 only allowed when tier = AUTH
- firmware provides instantaneous confidence
- runtime confidence decay is defined in time_model_v0_1.md, not here

中文关键点：

- confidence 不是 true/false。
- firmware 不做 time-based confidence decay。
- PPS 丢失后的 decay 由 runtime/time_model 定义。

### 12.5 confidence + tier constraints

| tier | Allowed confidence |
|---|---|
| AUTH | 255 only |
| DERIVED | 101-200 |
| TRANSPORT | 1-200 |
| NONE | 0 |

Violation MUST be reported as protocol contract violation.

中文关键点：

- `tier=AUTH` 但 `confidence!=255` 是违规。
- `tier=NONE` 但 `confidence>0` 是违规。

### 12.6 sensor_time_us

Rules:

- If sensor timestamp unavailable, set to 0
- Firmware MUST NOT synthesize UTC into sensor_time_us
- Runtime MUST preserve original sensor_time_us

中文关键点：

- 没有 sensor timestamp 就填 0。
- 不允许 firmware 编造 UTC。
- runtime 后续解释，但不能覆盖原始值。

### 12.7 delta_us

Definition:

    delta_us = sensor_time_us - board_time_us

Rules:

- If sensor_time_us = 0, firmware SHOULD set delta_us = 0
- delta_us sign does NOT imply physical causality
- delta_us interpretation belongs to time_model_v0_1.md

中文关键点：

- delta_us 正负只表示 sensor time 与 board time 的差值方向。
- delta_us 不能单独证明因果关系。

### 12.8 flags

`flags` is uint32.

中文关键点：

- flags 是 uint32。
- 未知 flags 不允许改变基本解析。
- flags 具体语义由 firmware interface 定义。

---

## 13. POWER_HEALTH

Packet type: `0x03`  
Payload length: `16 bytes`

| Offset | Field | Type | Size |
|---:|---|---|---:|
| 0 | vin_protected_mv | uint16 | 2 |
| 2 | v5_sys_mv | uint16 | 2 |
| 4 | v5_usb_vbus_mv | uint16 | 2 |
| 6 | v3v3_main_mv | uint16 | 2 |
| 8 | port_fault | uint8 | 1 |
| 9 | port_enabled | uint8 | 1 |
| 10 | power_health_state | uint8 | 1 |
| 11 | reserved | uint8 | 1 |
| 12 | stale_ms | uint16 | 2 |
| 14 | v1v1_main_mv | uint16 | 2 |

中文关键点：

- POWER_HEALTH 固定 16 bytes。
- Power 是 first-class evidence，不是 debug extra。
- `v1v1_main_mv` 即使 Fusion V2 暂无 ADC，也保留字段，值为 0。

### 13.1 Rails

Tracked rails:

- vin_protected_mv
- v5_sys_mv
- v5_usb_vbus_mv
- v3v3_main_mv
- v1v1_main_mv

If a rail is not physically measured:

- set that rail value to 0
- do NOT force global power_health_state to STALE
- determine power_health_state from available rails only

Fusion V2 rule:

    v1v1_main_mv = 0
    power_health_state is NOT forced to STALE

中文关键点：

- rail unavailable 不等于 rail stale。
- Fusion V2 没有 1V1 ADC，不代表整个 POWER_HEALTH stale。

### 13.2 stale_ms

Meaning:

    Time since last valid POWER_HEALTH measurement in milliseconds

Special value:

    0xFFFF = never measured

中文关键点：

- stale_ms 是整个 POWER_HEALTH payload 的 measurement freshness。
- 不是单独某一条 rail 的 freshness。

### 13.3 power_health_state

| Value | Name |
|---:|---|
| 0 | NORMAL |
| 1 | DEGRADED |
| 2 | FAULT |
| 3 | STALE |

### 13.4 state + stale_ms consistency

| State | stale_ms allowed range |
|---|---|
| NORMAL | 0 ≤ stale_ms < 1000 |
| DEGRADED | 0 ≤ stale_ms < 5000 |
| FAULT | 0 ≤ stale_ms < 10000 |
| STALE | stale_ms ≥ 1000 or 0xFFFF |

中文关键点：

- state 和 stale_ms 要一致。
- host 可以把不一致标为 warning/violation。

### 13.5 Cadence

Recommended cadence:

    10 Hz during NORMAL operation

Minimum protocol compliance:

    1 Hz

中文关键点：

- 10 Hz 是推荐基线。
- 低于 1 Hz 不满足 protocol compliance。
- 完全测量丢失才进入 STALE。

---

## 14. DEBUG_EVENT

Packet type: `0x06`  
Status: OPTIONAL

Rules:

- DEBUG_EVENT MUST NOT be required for acceptance
- DEBUG_EVENT MUST NOT carry authoritative timing data
- DEBUG_EVENT MUST NOT repair malformed frames

中文关键点：

- DEBUG_EVENT 不能作为证据链核心。
- debug 不能修复 protocol violation。

---

## 15. Reserved Fields

Rules:

- reserved fields MUST be zero
- reserved fields MUST NOT carry semantic meaning
- reserved fields MUST be ignored by defensive parsers
- reserved fields MUST NOT be reused in protocol v1.0
- future use requires protocol version upgrade

中文关键点：

- reserved 不准偷偷用。
- 同版本内不允许激活 reserved 字段。
- 未来要用 reserved，必须升 protocol version。

---

## 16. CRC Specification

CRC name:

    DSIL_CRC32_V1

| Parameter | Value |
|---|---|
| Polynomial | 0x04C11DB7 |
| Initial value | 0xFFFFFFFF |
| Final XOR | 0xFFFFFFFF |
| Bit order | LSB processed first |
| Shift direction | Right shift |
| Byte reflection | No explicit byte reflection |
| CRC field encoding | little-endian |

中文关键点：

- 不要 bit-reverse each byte。
- 按自然 byte order 输入 CRC。
- CRC 输出字段 little-endian 放入 frame。

### 16.1 CRC Coverage

CRC input exact sequence:

    [header (2)]
    [version (2)]
    [type (1)]
    [seq_num (4)]
    [board_time_us (8)]
    [payload_len (2)]
    [payload (N)]

CRC input length:

    19 + payload_len

CRC field itself is excluded.

中文关键点：

- CRC 必须覆盖 header 到 payload。
- 不包含 crc32 字段本身。
- payload_len=20 时，CRC 输入长度是 39 bytes。

### 16.2 C Reference

    uint32_t crc32_dsil(const uint8_t *data, size_t len) {
        uint32_t crc = 0xFFFFFFFF;

        for (size_t i = 0; i < len; i++) {
            crc ^= data[i];

            for (int bit = 0; bit < 8; bit++) {
                if (crc & 1) {
                    crc = (crc >> 1) ^ 0x04C11DB7;
                } else {
                    crc = crc >> 1;
                }
            }
        }

        return crc ^ 0xFFFFFFFF;
    }

### 16.3 Test Vector

Input bytes:

    44 53 00 01 01 00 00 00

Expected CRC integer:

    0xFFFEDCFB

Expected CRC wire encoding:

    FB DC FE FF

中文关键点：

- 张工 firmware 和梁工 parser 必须用同一个 test vector 对齐。
- 如果 test vector 不一致，不允许进入 Round 4。

---

## 17. Replay Ordering Rule

Replay tools MUST preserve original transmission order.

Replay order is seq_num order.

Replay tools MUST NOT reorder frames by board_time_us.

中文关键点：

- replay 永远按 seq_num。
- board_time_us 只用于 timing reconstruction，不用于 raw replay 排序。
- OEM dispute 时，raw log + seq_num 是第一顺序证据。

---

## 18. Host Evidence Preservation

Host decode output MUST preserve original:

- seq_num
- board_time_us
- sensor_time_us
- payload bytes
- packet type
- payload_len
- CRC validation result

中文关键点：

- parser 不允许悄悄修字段。
- runtime 可以派生字段，但 original 必须保留。
- dispute 时必须能回到 raw evidence。

---

## 19. Undefined / Invalid Behavior

Invalid behavior includes:

- invalid header
- unsupported version
- invalid payload_len
- CRC mismatch
- seq_num duplicate
- seq_num unexpected gap
- malformed payload
- non-zero reserved fields in strict mode
- illegal tier/confidence combination
- ASCII/debug printf bytes mixed into binary stream

中文关键点：

- strict parser 必须 reject invalid frame。
- diagnostic parser 可以继续跑，但必须报告错误。
- 不允许 silent repair。

---

## 20. Firmware Responsibilities

Firmware MUST:

- emit valid DSIL frames
- maintain global seq_num increment
- populate board_time_us from local hardware timer
- emit correct payload_len
- compute DSIL_CRC32_V1
- encode all multi-byte fields little-endian
- preserve raw timing evidence
- avoid debug printf bytes in protocol stream

Firmware MUST NOT:

- emit corrected_time_us
- perform runtime time normalization
- reorder past events
- reinterpret protocol fields
- use reserved fields for hidden semantics
- emit GNSS_STATUS or IMU_SAMPLE in v1.0

中文关键点：

- firmware 只发 raw truth。
- firmware 不做 runtime time model。
- firmware 不负责 replay。
- firmware 不负责 corrected_time。

---

## 21. Host Parser Responsibilities

Host parser MUST:

- decode according to this protocol
- validate header/version/payload_len/CRC
- preserve raw fields
- detect seq gap/duplicate
- detect contract violations
- avoid silently repairing malformed frames

Host parser MUST NOT:

- overwrite raw board_time_us
- infer physical causality from cross-sensor board_time_us
- reorder replay output by board_time_us
- reinterpret reserved fields

中文关键点：

- parser 是证据读取器，不是证据修复器。
- parser 不得为了 pass test 自动修 frame。

---

## 22. Compatibility Rule

Any change to the following requires protocol version upgrade:

- frame layout
- field order
- field size
- payload length
- CRC algorithm
- endian rule
- reserved field semantics
- packet type semantics

中文关键点：

- payload 不能同版本悄悄改。
- reserved 字段不能同版本激活。
- 语义澄清可以，不改 wire format。

---

## 23. Ratification Status

Status:

    FROZEN POST-AUDIT

Allowed before ratification:

- typo correction
- ambiguity clarification
- cross-reference addition

Forbidden without CTO approval:

- field reorder
- field resize
- payload resize
- CRC change
- endian change
- packet type behavior change

中文关键点：

- 现在不是重写 protocol。
- 现在是 constitutional audit 后的 post-audit freeze。
- Round 4 前只允许 clarification，不允许 wire format 改动。

---

## 24. Authority Statement

If conflict exists between this document and implementation code, this document wins.

If conflict exists between this document and firmware_interface_spec.md, this document wins for wire format.

If conflict exists between this document and replay_spec.md, this document wins for raw frame order and field semantics.

If conflict exists between this document and time_model_v0_1.md, this document wins for raw firmware emission; time_model_v0_1.md wins only for derived runtime interpretation.

中文关键点：

- protocol_spec.md 是 wire contract 最高权威。
- firmware_interface_spec.md 不能改 wire format。
- replay_spec.md 不能重排 raw frame。
- time_model_v0_1.md 只能解释 derived runtime time，不能改 raw evidence。