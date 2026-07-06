# BOARD_DECISIONS_v0.1.md

# The Board Protocol: Identity and Lifecycle Locks

Status: normative for v0.1.

This document resolves the identity, confirmation, Finding, gap, Projection, and implementation-order rules required before coding the v0.1 vertical slice.

The central decision is simple:

```text
Shift first.
Produced object cites Shift.
Produced object hash is computed.
ShiftResult binds the Shift to the produced object.
```

No provisional hashes are allowed. No identity hash is updated after creation.

---

# 1. Core identity decision

A `Shift` is the unit of agency.

A produced object may cite the `shift_hash` of the Shift that produced it. Therefore, the `Shift` identity MUST NOT include the final hash of the object it produces.

If `Shift.output_hash` is part of `ShiftIdentityBody`, the following circular dependencies appear:

```text
Condensation Shift -> Proposal.proposed_by -> proposal_version_hash -> Shift.output_hash
Confirmation Shift -> BoardAct.provenance.confirmation_shift -> board_act_hash -> Shift.output_hash
Projection Shift -> Projection.built_by -> projection_hash -> Shift.output_hash
```

Normative rule:

```text
shift_hash proves the agency event.
shift_result proves what the Shift produced.
```

A `Shift` is immutable once closed.

A `ShiftResult` is an immutable receipt recorded after the output hash exists.

---

# 2. Revised Shift schema

`output_hash` is removed from `Shift` identity.

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

Canonical identity body:

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

Reference computation:

```python
evidence_hashes = [hash_value(e) for e in shift["evidence"]]
shift_hash = hash_value(shift_identity_body(shift, evidence_hashes))
```

`duration_ms` MUST equal `closed_at - opened_at`.

---

# 3. ShiftResult receipt

A `ShiftResult` binds a closed Shift to its produced output.

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

Canonical identity body:

```ts
type ShiftResultIdentityBody = {
  shift_hash: Hash;
  output_kind: string;
  output_hash: Hash;
  output_refs: string[];
  recorded_at: Millis;
};
```

Reference computation:

```python
result_hash = hash_value(shift_result_identity_body(result))
```

For one-output Shifts, `output_hash` is the produced object hash.

For multi-output Shifts, `output_hash` is the hash of an output manifest:

```json
{
  "kind": "output_set",
  "refs": [
    {"kind": "finding", "id": "finding_hash_1"},
    {"kind": "finding", "id": "finding_hash_2"}
  ]
}
```

For v0.1, the vertical slice SHOULD enforce one `ShiftResult` per Shift. Production MAY allow multiple receipts only under an explicit policy.

---

# 4. SQL patch

Replace the old `output_hash` column on `shifts` with a separate `shift_results` table.

```sql
CREATE TABLE shifts (
    shift_hash TEXT PRIMARY KEY,
    stream_id TEXT NOT NULL,
    role TEXT NOT NULL,
    kind TEXT NOT NULL,
    input_hash TEXT NOT NULL,
    actor TEXT NOT NULL,
    risk_observed TEXT NOT NULL,
    evidence_json TEXT NOT NULL,
    opened_at INTEGER NOT NULL,
    closed_at INTEGER NOT NULL,
    duration_ms INTEGER NOT NULL,
    model_call_json TEXT
);

CREATE TABLE shift_results (
    result_hash TEXT PRIMARY KEY,
    shift_hash TEXT NOT NULL,
    output_kind TEXT NOT NULL,
    output_hash TEXT NOT NULL,
    output_refs_json TEXT NOT NULL,
    recorded_at INTEGER NOT NULL,
    identity_body_json TEXT NOT NULL,
    FOREIGN KEY (shift_hash) REFERENCES shifts(shift_hash)
);

CREATE INDEX idx_shift_results_shift ON shift_results(shift_hash);
CREATE INDEX idx_shift_results_output ON shift_results(output_kind, output_hash);
```

Vertical-slice rule:

```sql
CREATE UNIQUE INDEX idx_shift_results_one_per_shift
ON shift_results(shift_hash);
```

---

# 5. Deterministic creation order

## 5.1 Condensation Shift -> Proposal

1. Read Event window.
2. Compute `source_digest`.
3. Run the model or deterministic condenser.
4. Build a Condensation Shift without `output_hash`.
5. Compute `condensation_shift_hash`.
6. Build Proposal with `proposed_by = condensation_shift_hash`.
7. Compute `proposal.version_hash`.
8. Store Shift.
9. Store Proposal.
10. Store `ShiftResult(output_kind="proposal", output_hash=proposal.version_hash)`.

## 5.2 Confirmation Shift -> BoardAct

1. Lock Proposal row.
2. Lock Stream sequence row.
3. Validate `proposal.status == "ready"`.
4. Validate the current `proposal.version_hash` matches the Confirmation input.
5. Validate arming delay.
6. Validate actor authority.
7. Build confirmation evidence.
8. Build Confirmation Shift without `output_hash`.
9. Compute `confirmation_shift_hash`.
10. Build BoardAct identity body with `provenance.confirmation_shift = confirmation_shift_hash`.
11. Compute `board_act_hash`.
12. Store Shift.
13. Store BoardAct.
14. Store `ShiftResult(output_kind="board_act", output_hash=board_act_hash)`.
15. Mark Proposal board_committed as query/index state.
16. Run idempotent post-board_commit jobs.

## 5.3 Projection Shift -> Projection

1. Select BoardActs and Findings for the requested audience and `as_of_seq`.
2. Compute projection input hash.
3. Build Projection Shift without `output_hash`.
4. Compute `projection_shift_hash`.
5. Build Projection with `built_by = projection_shift_hash`.
6. Compute `projection_hash`.
7. Store Shift.
8. Store Projection.
9. Store `ShiftResult(output_kind="projection", output_hash=projection_hash)`.

## 5.4 Investigation or matcher Shift -> Finding(s)

1. Read the relevant BoardActs, Expectations, Proposals, workflow state, or prior Findings.
2. Compute the investigation input hash.
3. Build Investigation/System Shift without `output_hash`.
4. Compute `shift_hash`.
5. Build Finding objects with `detected_by = shift_hash`.
6. Compute each Finding identity hash.
7. Store Shift.
8. Store Finding object(s).
9. Store a `ShiftResult` for one Finding, or an `output_set` manifest for multiple Findings.

---

# 6. Proposal arming fields

The Proposal schema MUST include explicit arming and version fields.

```ts
type Proposal = {
  proposal_id: string;
  stream_id: StreamID;
  proposed_by: Hash;
  source_window: string[];
  source_digest: Hash;
  slots: BoardActSlots;
  confidence: number;
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

The arming delay starts only when the exact confirmable Proposal version is marked ready.

Mark ready:

```python
proposal["status"] = "ready"
proposal["ready_at"] = now_ms
proposal["arming_started_at"] = now_ms
proposal["version"] += 1
proposal["version_hash"] = hash_value(proposal_version_body(proposal))
```

A material edit after ready MUST return the Proposal to editing and clear arming state.

```python
proposal["status"] = "editing"
proposal["ready_at"] = None
proposal["arming_started_at"] = None
proposal["last_material_edit_at"] = now_ms
proposal["version"] += 1
proposal["version_hash"] = hash_value(proposal_version_body(proposal))
```

Canonical Proposal version body:

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

The Confirmation Shift `input_hash` MUST be the exact `proposal.version_hash` board_committed.

---

# 7. Confirmation rule

Confirmation does not prove comprehension.

It proves:

1. The exact Proposal version was available for review.
2. The actor had enough authority.
3. The arming delay elapsed.
4. The actor accepted responsibility.
5. The exact board_committed BoardAct can be traced to that Confirmation Shift.

Minimum confirmation evidence:

```json
[
  {
    "kind": "attention_latency",
    "value": 12500,
    "ts": 1700000018000
  },
  {
    "kind": "arming_delay",
    "value": {
      "required_ms": 10000,
      "actual_ms": 12500
    },
    "ts": 1700000018000
  },
  {
    "kind": "authority_check",
    "value": {
      "actor": "human:pilot",
      "effective_authority": 2,
      "required_authority": 2
    },
    "ts": 1700000018000
  }
]
```

```python
attention_latency_ms = confirmed_at - proposal["arming_started_at"]
```

---

# 8. Finding lifecycle lock

Findings are derived Board objects with stable identity.

Resolution remains excluded from Finding identity.

A Finding lifecycle transition MAY be lightweight when it is purely informational.

A Finding lifecycle transition MUST be represented by a BoardAct or auditable Shift when it does any of the following:

1. Blocks confirmation.
2. Restores authority.
3. Dismisses a violation.
4. Resolves a gap.
5. Authorizes a cancelled item to reappear.
6. Changes risk interpretation.
7. Affects external effects.

Normative rule:

```text
No consequential Finding transition may happen by silent mutation.
```

---

# 9. Gap vs pending expectation

For v0.1, use two states:

```text
pending_expectation = expected but not late
gap                 = expected and violated or intentionally rendered as open Board work
```

Vertical-slice policy:

```text
The vertical slice MAY render any unfulfilled expectation immediately as Finding(kind="gap").
```

This is allowed because the demo must prove the Board surface.

Production policy:

```text
Create Expectation(status="open") first.
Create Finding(kind="gap") only when deadline_ms != null and now >= deadline_ms,
or when workflow policy says the expectation is immediately blocking.
```

---

# 10. Projection validity

A Projection is valid only if its source BoardActs match the ledger prefix it claims to render.

For a Projection with `as_of_seq = N`:

```text
Projection.source.board_act_hashes MUST equal the ordered hash list of BoardActs seq 1..N for the stream,
unless the Projection explicitly declares itself partial.
```

Default projections are not partial.

If a Projection is partial, it must include:

```json
{
  "partial": true,
  "selection_reason": "string"
}
```

A normal newcomer, operator, or auditor Projection MUST NOT silently cherry-pick BoardActs.

---

# 11. Canonicalization lock

For the vertical slice, use:

```python
json.dumps(value, sort_keys=True, separators=(",", ":"), ensure_ascii=False)
```

For protocol-level interoperability, the target canonicalization is JCS/RFC 8785.

A stream MUST declare its canonicalization version at creation.

```json
{
  "canonicalization": "board-json-v0",
  "hash_algorithm": "sha256"
}
```

A stream MUST NOT change canonicalization version mid-chain.

A future JCS stream should be a new stream, migration, or checkpointed successor chain.

---

# 12. BoardCommit algorithm, final form

```python
def commit_proposal(proposal_id, actor, now_ms):
    with transaction():
        proposal = load_proposal_for_update(proposal_id)
        stream = lock_stream(proposal["stream_id"])

        if proposal["status"] != "ready":
            return reject("proposal_not_ready")

        if proposal["version_hash"] != load_current_proposal_version_hash(proposal_id):
            return reject("proposal_changed_during_confirmation")

        risk = proposal["slots"]["risk"]
        required_delay = ARMING_DELAY_MS[risk]
        actual_delay = now_ms - proposal["arming_started_at"]

        if actual_delay < required_delay:
            return reject("arming_delay_not_satisfied")

        if effective_authority(actor) < RISK_RANK[risk]:
            return reject("insufficient_authority")

        if attention_anomaly_blocked(actor):
            return reject("attention_anomaly_block")

        previous = load_latest_board_act_for_update(proposal["stream_id"])
        seq = 1 if previous is None else previous["seq"] + 1
        prev_hash = GENESIS_HASH if previous is None else previous["board_act_hash"]

        evidence = [
            evidence_attention_latency(actual_delay, now_ms),
            evidence_arming_delay(required_delay, actual_delay, now_ms),
            evidence_authority_check(actor, RISK_RANK[risk], now_ms),
        ]

        confirmation_shift_body = build_shift_identity_body({
            "stream_id": proposal["stream_id"],
            "role": "confirmer",
            "kind": "confirmation",
            "input_hash": proposal["version_hash"],
            "actor": actor["actor_id"],
            "risk_observed": risk,
            "evidence_hashes": [hash_value(e) for e in evidence],
            "opened_at": proposal["arming_started_at"],
            "closed_at": now_ms,
            "duration_ms": now_ms - proposal["arming_started_at"],
            "model_call": None,
        })
        confirmation_shift_hash = hash_value(confirmation_shift_body)

        provenance = {
            "source_digest": proposal["source_digest"],
            "condensation_shift": proposal["proposed_by"],
            "confirmation_shift": confirmation_shift_hash,
            "board_committed_at": now_ms,
        }
        board_commit = {
            "board_committed_by": actor["actor_id"],
            "board_committed_risk": risk,
            "proposal_id": proposal["proposal_id"],
            "proposal_version_hash": proposal["version_hash"],
        }
        audit = {
            "original_slots_hash": hash_value(proposal["slots"]),
            "edit_history": proposal.get("edit_history", []),
        }
        board_act_identity = {
            "prev_board_act_hash": prev_hash,
            "seq": seq,
            "stream_id": proposal["stream_id"],
            "slots": proposal["slots"],
            "provenance": provenance,
            "board_commit": board_commit,
            "audit": audit,
        }
        board_act_hash = hash_value(board_act_identity)

        shift = {
            "shift_hash": confirmation_shift_hash,
            **confirmation_shift_body,
            "evidence": evidence,
        }
        board_act = {
            "board_act_hash": board_act_hash,
            **board_act_identity,
        }
        result_body = {
            "shift_hash": confirmation_shift_hash,
            "output_kind": "board_act",
            "output_hash": board_act_hash,
            "output_refs": [board_act_hash],
            "recorded_at": now_ms,
        }
        shift_result = {
            "result_hash": hash_value(result_body),
            **result_body,
        }

        store_shift(shift)
        store_board_act(board_act)
        store_shift_result(shift_result)
        mark_proposal_board_committed(proposal_id, board_act_hash)

    run_post_board_commit_jobs_idempotently(board_act_hash)

    return {
        "ok": True,
        "board_act_hash": board_act_hash,
        "seq": seq,
        "confirmation_shift": confirmation_shift_hash,
        "shift_result": shift_result["result_hash"],
    }
```

---

# 13. Hard invariants for v0.1 tests

1. Shift hash does not change when `ShiftResult` is recorded.
2. Proposal can cite Condensation Shift without circular hashing.
3. BoardAct can cite Confirmation Shift without circular hashing.
4. Projection can cite Projection Shift without circular hashing.
5. Every produced Proposal, BoardAct, Finding, Projection, or output set has a `ShiftResult`.
6. Proposal edit after ready clears `ready_at` and `arming_started_at`.
7. Confirmation `input_hash` equals the current Proposal version hash.
8. Projection source `board_act_hashes` equal ledger seq `1..as_of_seq` unless `partial = true`.
9. Consequential Finding dismissal or resolution has a BoardAct or Shift reference.
10. Stream canonicalization version is fixed at stream creation.

---

# 14. Final implementation rule

Do not use provisional hashes.

Do not update a Shift hash after output exists.

Do not put `output_hash` in `ShiftIdentityBody`.

The canonical pattern is:

```text
Shift first.
Produced object cites Shift.
Produced object hash computed.
ShiftResult binds Shift to produced object.
```

That is the identity solution for v0.1.
