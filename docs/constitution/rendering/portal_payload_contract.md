# Portal Payload Contract

Status: PR58 foundation contract
Scope: portal-renderable governance payload data

This contract defines the deterministic JSON structure produced by Atlas runtime for portal rendering. It is implementation-facing render data only.

## Required Top-Level Fields

- `report_id`: stable payload id.
- `generated_at`: deterministic generation marker. For canonical payloads this MUST be `null`.
- `generated_at_policy`: explain why wall-clock time is omitted.
- `source_pack_path`: source PR57 customer trust artifact path.
- `sections[]`: ordered render sections.

## Required Section Fields

Each item in `sections[]` MUST include:

- `section_id`
- `section_title`
- `section_type`
- `severity`
- `audience_role`
- `summary`
- `evidence_refs[]`
- `limitation_refs[]`
- `governance_window_refs[]`
- `comparison_refs[]`
- `render_order`
- `display_policy[]`

Each section SHOULD make clear:

- who the section is for
- why it matters
- which source artifact it came from
- what limitations apply
- whether it is replay-backed
- whether it can support controlled pilot evaluation

## Severity Policy

Allowed severity values:

- `INFO`
- `NOTICE`
- `LIMITATION`
- `ATTENTION`

Severity is not scoring. It MUST NOT be used as pass/fail, trust, approval, rejection, or production status.

## Display Policy

Display policy is declarative only. Allowed examples:

- `show_limitations`
- `show_replay_boundary`
- `show_governance_context`
- `show_comparison_boundary`
- `hide_decision_controls`
- `no_score_display`
- `no_certification_badge`
- `no_deployment_badge`

Display policy MUST NOT define colors, pixels, components, CSS classes, HTML, or frontend layout.

## Deterministic Timestamp Policy

`generated_at` MUST be `null` for canonical deterministic payloads.

`generated_at_policy` MUST be:

`omitted_for_deterministic_canonical_payload`

Current wall-clock time is forbidden.

## Boundary

Portal payload is render data only.

It is not:

- HTML
- React
- UI style
- decision engine
- legal interpretation
- deployment authorization
- certification override
- authority promotion
- hidden scoring
