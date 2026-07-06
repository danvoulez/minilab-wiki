# BOARD_OBJECTS.md

# The Board Protocol: Canonical Objects

Status: v0.1, locked against `BOARD_DECISIONS_v0.1.md`.

This document defines the canonical objects for The Board: `Event`, `Proposal`, `BoardAct`, `Shift`, `ShiftResult`, `Finding`, and `Projection`.

The main identity rule for v0.1 is:

```text
A Shift proves the agency event.
A ShiftResult proves what the Shift produced.
A produced object may cite the Shift that produced it.
A Shift identity never includes the produced object's hash.
```

This prevents circular hashing for Condensation -> Proposal, Confirmation -> BoardAct, and Projection Shift -> Projection.

---

# 1. Canonicalization rules

Every canonical object MUST be serializable as deterministic JSON.

Vertical-slice reference algorithm:

```python
import hashlib
import json


def canonical_json(value):
    return json.dumps(
        value,
        sort_keys=True,
        separators=(",", ":"),
        ensure_ascii=False,
    )


def hash_value(value):
    return hashlib.sha256(canonical_json(value).encode("utf-8")).hexdigest()
```

Rules:

1. Object keys are sorted recursively.
2. No insignificant whitespace is emitted.
3. Text is UTF-8.
4. `null`, `false`, `true`, strings, numbers, arrays, and objects retain JSON semantics.
5. Hash fields never include themselves in the hashed body.
6. Future pointers, cache fields, database indexes, and rendered UI fields are not part of identity hashes unless explicitly listed.
7. `output_hash` MUST NOT appear in `ShiftIdentityBody`.

Protocol-level interoperability target: JCS/RFC 8785.

A stream MUST declare its canonicalization and hash algorithm at creation:

```ts
type StreamConfig = {
  stream_id: StreamID;
  canonicalization: "board-json-v0" | "jcs-rfc8785";
  hash_algorithm: "sha256";
  created_at: Millis;
};
```

A stream MUST NOT change canonicalization version mid-chain. A future canonicalization change requires a new stream, migration, or checkpointed successor chain.

---

# 2. Shared scalar types

```ts
type Hash = string;          // lowercase hex SHA-256, 64 characters
type UUID = string;          // implementation may use UUIDv4 or ULID
type Millis = number;        // UTC epoch milliseconds
type StreamID = string;      // namespace for one board stream
type ActorID = string;       // authenticated actor reference
type RiskTier = "L0" | "L1" | "L2" | "L3" | "L4";
type Severity = "info" | "warn" | "violation";
```

Risk tiers are ordered:

```text
L0 < L1 < L2 < L3 < L4
```

Suggested meaning:

| Tier | Meaning |
|---|---|
| L0 | Informational or read-only. |
| L1 | Low-risk state annotation. |
| L2 | Ordinary confirmed work. |
| L3 | High-impact decision or irreversible local effect. |
| L4 | External, safety-critical, financial, legal, or destructive effect. |

---

# 3. Common sub-schemas

## 3.1 BoardActSlots

`BoardActSlots` is the semantic payload that a Proposal proposes and a BoardAct seals.

```ts
type BoardActSlots = {
  gloss: string;              // required one-line human summary
  who: ActorID | string;      // responsible actor or role
  what: string;               // controlled verb or board_act type
  scope: string;              // domain, region, process, or boundary affected
  object: string | null;      // canonical target identifier, if any
  cancellations: string[];    // board_act hashes, identifiers, or normalized terms being superseded
  expects: ExpectationDraft[];
  risk: RiskTier;
  tags: string[];
};
```

Example:

```json
{
  "gloss": "Start engine A before taxi",
  "who": "human:pilot",
  "what": "start_engine",
  "scope": "flight.pre_taxi",
  "object": "engine:A",
  "cancellations": [],
  "expects": [],
  "risk": "L2",
  "tags": ["preflight", "engine"]
}
```

Validation rules:

1. `gloss`, `who`, `what`, `scope`, and `risk` are required.
2. `what` SHOULD come from a stream-specific controlled vocabulary.
3. `cancellations` SHOULD prefer board_act hashes when cancelling board_committed BoardActs.
4. `object` SHOULD be a canonical identifier, not a display label.
5. `tags` are advisory and MUST NOT be used as the only basis for authority or workflow checks.

## 3.2 ExpectationDraft

An expectation is a future obligation created by a board-committed BoardAct.

```ts
type ExpectationDraft = {
  expectation_id: string;
  expected_what: string;
  expected_object: string | null;
  deadline_ms: Millis | null;
  severity_on_miss: Severity;
};
```

`ExpectationDraft` appears inside a Proposal. When the Proposal is board_committed, each expectation is bound to the resulting `board_act_hash`.

## 3.3 BoundExpectation

```ts
type BoundExpectation = {
  expectation_id: string;
  stream_id: StreamID;
  expected_what: string;
  expected_object: string | null;
  deadline_ms: Millis | null;
  created_by: Hash;           // board_act_hash
  bound_to: Hash;             // normally same as created_by, unless delegated
  status: "open" | "fulfilled" | "violated" | "cancelled";
  fulfilled_by: Hash | null;  // board_act_hash
};
```

Bound expectations are not part of the required top-level object set, but they are referenced by BoardActs, Findings, and lifecycle rules.

v0.1 distinguishes two Board states:

```text
pending_expectation = expected but not late
gap                 = expected and violated or intentionally rendered as open Board work
```

## 3.4 Evidence

```ts
type Evidence = {
  evidence_id?: string;
  kind:
    | "attention_latency"
    | "source_digest"
    | "model_output"
    | "human_input"
    | "authority_check"
    | "arming_delay"
    | "chain_verification"
    | "other";
  value: string | number | boolean | object | null;
  ts: Millis;
};
```

Evidence is attached to a Shift, not directly to a BoardAct. The BoardAct points to the Shift that produced it.

## 3.5 ModelCallRef

```ts
type ModelCallRef = {
  provider: string;
  model: string;
  request_hash: Hash;
  response_hash: Hash;
  tokens_in: number | null;
  tokens_out: number | null;
  started_at: Millis;
  completed_at: Millis;
};
```

The model response itself is not truth. It is an output whose hash can be audited.

---

# 4. Event

An `Event` is raw activity in the fast loop. It may be a human utterance, a model response, a tool result, an agent notification, or a system event.

Events are useful for perception and condensation. They are not permanent truth.

## 4.1 Schema

```ts
type Event = {
  event_id: string;
  stream_id: StreamID;
  source: string;             // e.g. "human:pilot", "model:gpt", "tool:shell"
  payload: string | object;
  ts: Millis;
  zone: "live" | "buffered" | "evaporated";
  parent_event: string | null;
};
```

## 4.2 Canonical event body for source digests

The `zone` field MUST NOT be included in `source_digest`, because zone changes over time.

```ts
type EventDigestBody = {
  event_id: string;
  source: string;
  payload: string | object;
  ts: Millis;
  parent_event: string | null;
};
```

A source window digest is:

```python
window = [event_digest_body(e) for e in sorted(events, key=lambda e: (e["ts"], e["event_id"]))]
source_digest = hash_value(window)
```

## 4.3 Invariants

1. `event_id` is unique within a stream.
2. `ts` is assigned by the system on receipt.
3. `parent_event` captures causal order within one source when available.
4. Events may evaporate; their digest can survive through Proposals and BoardActs.
5. An evaporated Event is not evidence by itself unless the original event body can be re-presented and rehashed.

---

# 5. Proposal

A `Proposal` is a structured proposal extracted from one or more Events. It is produced by the membrane, usually through a Condensation Shift.

A Proposal is mutable until BoardCommit. It may be edited, rejected, withdrawn, expired, or marked ready.

## 5.1 Schema

```ts
type Proposal = {
  proposal_id: string;
  stream_id: StreamID;
  proposed_by: Hash;          // shift_hash of the Condensation Shift
  source_window: string[];    // event_ids used at condensation time
  source_digest: Hash;
  slots: BoardActSlots;
  confidence: number;         // 0.0 to 1.0
  status: "pending" | "editing" | "ready" | "rejected" | "withdrawn" | "board_committed" | "expired";
  opened_at: Millis;
  ready_at: Millis | null;
  last_material_edit_at: Millis;
  arming_started_at: Millis | null;
  expires_at: Millis | null;
  version: number;
  version_hash: Hash;
  edit_history: Hash[];
};
```

## 5.2 Proposal version body

Proposals are mutable, so their identity hash is a version hash, not permanent truth.

```ts
type ProposalVersionBody = {
  proposal_id: string;
  stream_id: StreamID;
  proposed_by: Hash;
  source_digest: Hash;
  slots: BoardActSlots;
  confidence: number;
  status: string;
  opened_at: Millis;
  ready_at: Millis | null;
  last_material_edit_at: Millis;
  arming_started_at: Millis | null;
  expires_at: Millis | null;
  version: number;
};
```

```python
proposal["version_hash"] = hash_value(proposal_version_body(proposal))
```

A Confirmation Shift MUST use the hash of the exact Proposal version being board_committed as its `input_hash`.

## 5.3 Arming fields

The arming delay starts only when the exact confirmable Proposal version is marked ready.

Marking ready MUST set:

```python
proposal["status"] = "ready"
proposal["ready_at"] = now_ms
proposal["arming_started_at"] = now_ms
proposal["version"] += 1
proposal["version_hash"] = hash_value(proposal_version_body(proposal))
```

A material edit after ready MUST clear arming state:

```python
proposal["status"] = "editing"
proposal["ready_at"] = None
proposal["arming_started_at"] = None
proposal["last_material_edit_at"] = now_ms
proposal["version"] += 1
proposal["version_hash"] = hash_value(proposal_version_body(proposal))
```

## 5.4 Validation rules

1. A Proposal can only be created from Events still in `live` or `buffered` state.
2. `source_digest` MUST be computed before any Event in `source_window` evaporates.
3. `confidence` is model self-assessment, not authority.
4. Only `ready` Proposals can be board_committed.
5. The arming clock starts at `arming_started_at`, which is set only by marking a specific version ready.
6. A Proposal version cannot be board_committed if another actor edits it before confirmation commits.
7. Once board_committed, the Proposal status may be set to `board_committed` as an index, but the BoardAct is the source of truth.

---

# 6. BoardAct

A `BoardAct` is the atomic unit of board-committed truth in the slow loop.

BoardActs are append-only. They are never edited. Corrections happen through later BoardActs that supersede, cancel, or resolve earlier BoardActs.

## 6.1 Schema

```ts
type BoardAct = {
  board_act_hash: Hash;
  prev_board_act_hash: Hash;
  seq: number;
  stream_id: StreamID;
  slots: BoardActSlots;
  provenance: BoardActProvenance;
  board_commit: BoardActCommit;
  audit: BoardActAudit;
};

type BoardActProvenance = {
  source_digest: Hash;
  condensation_shift: Hash;
  confirmation_shift: Hash;
  board_committed_at: Millis;
};

type BoardActCommit = {
  board_committed_by: ActorID;
  board_committed_risk: RiskTier;
  proposal_id: string;
  proposal_version_hash: Hash;
};

type BoardActAudit = {
  original_slots_hash: Hash;
  edit_history: Hash[];
};
```

`BoardAct.provenance.confirmation_shift` cites the Confirmation Shift that produced the BoardAct. This is non-circular because the Shift identity does not include the BoardAct hash.

## 6.2 BoardAct identity body

`board_act_hash` is computed over this immutable body:

```ts
type BoardActIdentityBody = {
  prev_board_act_hash: Hash;
  seq: number;
  stream_id: StreamID;
  slots: BoardActSlots;
  provenance: BoardActProvenance;
  board_commit: BoardActCommit;
  audit: BoardActAudit;
};
```

```python
board_act_hash = hash_value(board_act_identity_body(board_act))
```

## 6.3 Derived BoardAct state

The BoardAct record itself should remain immutable. Operational state can be derived from later BoardActs and Findings.

```ts
type BoardActDerivedState = {
  board_act_hash: Hash;
  state: "active" | "resolved" | "cancelled" | "superseded";
  superseded_by: Hash | null;
  resolved_by: Hash | null;
  cancelled_by: Hash | null;
};
```

Implementations MAY store derived fields in a database table for query performance, but they MUST NOT be part of the canonical BoardAct identity unless the system is willing to treat any change as a new canonical object.

## 6.4 Invariants

1. `seq` is strictly increasing within `stream_id`.
2. `prev_board_act_hash` equals the previous BoardAct hash in the same stream, or `GENESIS_HASH` for the first BoardAct.
3. `board_act_hash` is deterministic from `BoardActIdentityBody`.
4. A BoardAct can be superseded only by a later BoardAct.
5. A BoardAct cannot be deleted from the ledger.
6. The slow loop references source Events through `source_digest`, not by requiring raw Event retention.
7. Every BoardAct must have a `ShiftResult` whose `output_kind = "board_act"` and `output_hash = board_act_hash`.

---

# 7. Shift

A `Shift` is the unit of agency. Any meaningful operation with an input, actor, duration, evidence, and closed result path is a Shift.

Examples: condensation, confirmation, investigation, projection, verification, and external effects.

The Shift identity proves that an agency event occurred. It does not prove what object was produced. That proof lives in `ShiftResult`.

## 7.1 Schema

```ts
type Shift = {
  shift_hash: Hash;
  stream_id: StreamID;
  role: string;
  kind:
    | "condensation"
    | "confirmation"
    | "investigation"
    | "projection"
    | "verification"
    | "effect"
    | "system";
  input_hash: Hash;
  actor: ActorID;
  risk_observed: RiskTier;
  evidence: Evidence[];
  opened_at: Millis;
  closed_at: Millis;
  duration_ms: number;
  model_call: ModelCallRef | null;
};
```

## 7.2 Shift identity body

```ts
type ShiftIdentityBody = {
  stream_id: StreamID;
  role: string;
  kind: string;
  input_hash: Hash;
  actor: ActorID;
  risk_observed: RiskTier;
  evidence_hashes: Hash[];
  opened_at: Millis;
  closed_at: Millis;
  duration_ms: number;
  model_call: ModelCallRef | null;
};
```

```python
evidence_hashes = [hash_value(e) for e in shift["evidence"]]
shift_hash = hash_value(shift_identity_body(shift, evidence_hashes))
```

`output_hash` MUST NOT be included in `Shift` or `ShiftIdentityBody`.

## 7.3 Invariants

1. Every model call that proposes state is represented by a Shift.
2. Every human confirmation is represented by a Shift.
3. Every generated Projection is represented by a Shift.
4. `duration_ms = closed_at - opened_at`.
5. If a Shift involves an LLM, `model_call` is required.
6. If a Shift confirms a BoardAct, its evidence MUST include `attention_latency`, `arming_delay`, and `authority_check`.
7. A Shift is immutable once closed.
8. A Shift that produces a Proposal, BoardAct, Finding, Projection, verification receipt, external effect result, or output set MUST have a ShiftResult.

---

# 8. ShiftResult

A `ShiftResult` is an immutable receipt that binds a closed Shift to the output it produced.

It is recorded after the output hash exists.

## 8.1 Schema

```ts
type ShiftResult = {
  result_hash: Hash;
  shift_hash: Hash;
  output_kind:
    | "proposal"
    | "board_act"
    | "finding"
    | "projection"
    | "verification"
    | "effect_result"
    | "output_set";
  output_hash: Hash;
  output_refs: string[];
  recorded_at: Millis;
};
```

## 8.2 ShiftResult identity body

```ts
type ShiftResultIdentityBody = {
  shift_hash: Hash;
  output_kind: string;
  output_hash: Hash;
  output_refs: string[];
  recorded_at: Millis;
};
```

```python
result_hash = hash_value(shift_result_identity_body(result))
```

## 8.3 Output set manifest

For one-output Shifts, `output_hash` is the produced object hash.

For multi-output Shifts, `output_hash` is the hash of an output manifest:

```ts
type OutputSetManifest = {
  kind: "output_set";
  refs: {
    kind: "proposal" | "board_act" | "finding" | "projection" | "verification" | "effect_result";
    id: string;
  }[];
};
```

Example:

```json
{
  "kind": "output_set",
  "refs": [
    {"kind": "finding", "id": "finding_hash_1"},
    {"kind": "finding", "id": "finding_hash_2"}
  ]
}
```

## 8.4 Invariants

1. A ShiftResult cannot exist before its output hash exists.
2. A ShiftResult cannot change the Shift hash.
3. A ShiftResult MUST point to an existing Shift.
4. For v0.1, enforce one ShiftResult per Shift unless an explicit output-set policy is implemented.
5. `output_refs` MUST contain the canonical IDs or hashes of the produced objects.

---

# 9. Finding

A `Finding` is a detected discrepancy between expected state and board_committed state, or between required process and observed process.

Findings are the content of the Board. They make drift visible.

## 9.1 Schema

```ts
type Finding = {
  finding_id: string;          // SHOULD be hash of FindingIdentityBody
  stream_id: StreamID;
  kind:
    | "missing"
    | "extra"
    | "reordered"
    | "looped"
    | "premature_closure"
    | "gap"
    | "cancellation_violation"
    | "attention_anomaly"
    | "risk_delta"
    | "conflict"
    | "tamper_detected";
  refs: FindingRef[];
  severity: Severity;
  detail: string;
  detected_at: Millis;
  detected_by: Hash;          // shift_hash
  resolution: FindingResolution | null;
};

type FindingRef = {
  ref_type: "board_act" | "event" | "proposal" | "shift" | "expectation" | "projection";
  ref_id: string;
};

type FindingResolution = {
  resolved: boolean;
  resolved_at: Millis | null;
  resolved_by: Hash | null;   // board_act_hash or shift_hash
  resolution_note: string | null;
};
```

## 9.2 Finding identity body

Resolution is excluded from Finding identity.

```ts
type FindingIdentityBody = {
  stream_id: StreamID;
  kind: string;
  refs: FindingRef[];
  severity: Severity;
  detail: string;
  detected_at: Millis;
  detected_by: Hash;
};
```

```python
finding_id = hash_value(finding_identity_body(finding))
```

## 9.3 Finding lifecycle rules

A Finding lifecycle transition MAY be lightweight when it is purely informational.

A Finding lifecycle transition MUST be represented by a BoardAct or auditable Shift when it does any of the following:

1. Blocks confirmation.
2. Restores authority.
3. Dismisses a violation.
4. Resolves a gap.
5. Authorizes a cancelled item to reappear.
6. Changes risk interpretation.
7. Affects external effects.

No consequential Finding transition may happen by silent mutation.

## 9.4 Invariants

1. A Finding is derived from board_committed state, Proposals, Events, expectations, workflow state, or verification state.
2. A Finding MUST cite what it is about through `refs`.
3. `detected_by` MUST point to an investigation, verification, system, or matcher Shift.
4. Resolution does not change `finding_id`.
5. Every produced Finding must have a ShiftResult or be included in a ShiftResult output set.

---

# 10. Projection

A `Projection` is a readable rendering of the ledger and Board for a specific audience.

It is not raw chat. It is a structured view of board_committed BoardActs, open Findings, supersessions, risks, and verification metadata.

## 10.1 Schema

```ts
type Projection = {
  projection_hash: Hash;
  stream_id: StreamID;
  built_by: Hash;             // shift_hash of Projection Shift
  audience: "operator" | "newcomer" | "auditor";
  narrative: ProjectionBlock[];
  open_findings: string[];    // finding_ids
  as_of_seq: number;
  generated_at: Millis;
  source: ProjectionSource;
};

type ProjectionSource = {
  stream_id: StreamID;
  as_of_seq: number;
  board_act_hashes: Hash[];
  finding_ids: string[];
  partial: boolean;
  selection_reason: string | null;
};

type ProjectionBlock = {
  block_id: string;
  kind: "board_act" | "summary" | "gap" | "supersession" | "risk_note" | "verification" | "divider";
  ref: string | null;         // board_act_hash, finding_id, or null
  title: string;
  body: string;
  risk: RiskTier | null;
};
```

## 10.2 Projection identity body

```ts
type ProjectionIdentityBody = {
  stream_id: StreamID;
  built_by: Hash;
  audience: string;
  narrative: ProjectionBlock[];
  open_findings: string[];
  as_of_seq: number;
  generated_at: Millis;
  source: ProjectionSource;
};
```

```python
projection_hash = hash_value(projection_identity_body(projection))
```

## 10.3 Source-prefix validity

A Projection is valid only if its source BoardActs match the ledger prefix it claims to render.

For a Projection with `as_of_seq = N`:

```text
Projection.source.board_act_hashes MUST equal the ordered hash list of BoardActs seq 1..N for the stream,
unless Projection.source.partial = true.
```

Default projections are not partial.

If a Projection is partial, it MUST include:

```json
{
  "partial": true,
  "selection_reason": "why this view intentionally selects less than the ledger prefix"
}
```

A normal newcomer, operator, or auditor Projection MUST NOT silently cherry-pick BoardActs.

## 10.4 Invariants

1. A Projection MUST be produced by a Projection Shift.
2. A Projection Shift hash is known before `projection_hash` is computed.
3. The Projection Shift does not include `projection_hash` in its identity.
4. A ShiftResult binds the Projection Shift to `projection_hash`.
5. `open_findings` SHOULD contain unresolved Finding IDs relevant as of `as_of_seq`.
6. An auditor Projection SHOULD include verification blocks or references.
7. A default Projection MUST render the full BoardAct prefix `1..as_of_seq`.

---

# 11. Canonical object relationships

```text
Event window
  -> source_digest
  -> Condensation Shift
  -> Proposal(proposed_by = condensation_shift_hash)
  -> ShiftResult(proposal.version_hash)

Proposal(version_hash)
  -> Confirmation Shift(input_hash = proposal.version_hash)
  -> BoardAct(provenance.confirmation_shift = confirmation_shift_hash)
  -> ShiftResult(board_act_hash)

BoardActs and Findings
  -> Projection Shift
  -> Projection(built_by = projection_shift_hash)
  -> ShiftResult(projection_hash)
```

The relationship graph may contain references in both directions at the semantic level, but identity hashing remains acyclic because outputs are bound by ShiftResult, not by mutating Shift identity.
