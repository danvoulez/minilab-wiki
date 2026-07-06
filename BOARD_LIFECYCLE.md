# BOARD_LIFECYCLE.md

# The Board Protocol: Lifecycle and State Transitions

Status: v0.1, locked against `BOARD_DECISIONS_v0.1.md`.

This document defines how state moves through The Board: Events become Proposals, Proposals become BoardActs, BoardActs produce Findings, Findings shape the Board, and Projections explain the current state to humans.

Core rule:

```text
The model proposes. The human confirms. The ledger proves. The Board explains.
```

Identity rule:

```text
Shift first.
Produced object cites Shift.
Produced object hash computed.
ShiftResult binds Shift to produced object.
```

No lifecycle step may rely on provisional hashes or on updating a Shift identity after output exists.

---

# 1. Lifecycle overview

```text
Fast loop Event(s)
      |
      v
Condensation Shift
      |
      v
Proposal
      |
      v
ShiftResult(output_kind="proposal")
      |
      v
Human review -> ready_at -> arming delay
      |
      v
Confirmation Shift
      |
      v
BoardAct appended to hash chain
      |
      v
ShiftResult(output_kind="board_act")
      |
      v
WorkflowMatcher and expectation checks
      |
      v
Finding(s) on the Board
      |
      v
Projection Shift
      |
      v
Projection for operator, newcomer, or auditor
      |
      v
ShiftResult(output_kind="projection")
```

The chat stream is not the source of truth. It is only the raw material from which Proposals are proposed. The BoardAct ledger is the source of board-committed Envelope truth.

---

# 2. Event lifecycle

Events live in the fast loop.

## 2.1 Event states

```text
live -> buffered -> evaporated
```

| State | Meaning | Default timing |
|---|---|---|
| `live` | Newly received and visible in the active stream. | 0 to 90 seconds |
| `buffered` | Stable enough for condensation, still available as raw content. | 90 to 600 seconds |
| `evaporated` | Raw payload may be deleted from hot storage. | after 600 seconds |

Default TTLs:

```text
TTL_LIVE_MS = 90_000
TTL_TOTAL_MS = 600_000
TTL_BUFFER_MS = TTL_TOTAL_MS - TTL_LIVE_MS
```

## 2.2 Event transition rules

1. A new Event starts as `live`.
2. A system job moves it to `buffered` after `TTL_LIVE_MS`.
3. A system job moves it to `evaporated` after `TTL_TOTAL_MS`.
4. The raw payload of an evaporated Event may be deleted.
5. A Proposal can only be created from Events that are still `live` or `buffered`.
6. `source_digest` must be computed before any Event in the source window evaporates.

## 2.3 Source window rule

A Condensation Shift must define the exact Event window it read.

```text
source_window = ordered Events used for condensation
source_digest = SHA-256(canonical_json(source_window_without_zone))
```

The digest does not preserve the raw conversation. It preserves a way to verify that a later presented source window is the same one used during condensation.

---

# 3. Condensation lifecycle

Condensation is the membrane between raw activity and structured proposal.

## 3.1 Condensation inputs

A Condensation Shift reads:

1. A bounded Event window.
2. A condensation prompt or deterministic condenser definition.
3. Any relevant workflow vocabulary.
4. Optional active Board context.

Its `input_hash` SHOULD be the hash of the canonical condensation input bundle:

```ts
type CondensationInput = {
  stream_id: StreamID;
  event_digest_bodies: EventDigestBody[];
  source_digest: Hash;
  condenser_version: string;
  workflow_hash: Hash | null;
};
```

## 3.2 Deterministic creation order

```text
1. Read Event window.
2. Compute source_digest.
3. Run model or deterministic condenser.
4. Build Condensation Shift without output_hash.
5. Compute condensation shift_hash.
6. Build Proposal with proposed_by = condensation shift_hash.
7. Compute proposal.version_hash.
8. Store Shift.
9. Store Proposal.
10. Store ShiftResult(output_kind="proposal", output_hash=proposal.version_hash).
```

## 3.3 Condensation output rules

1. The model output is not truth.
2. The Proposal is only a proposal.
3. `Proposal.proposed_by` MUST cite the Condensation Shift.
4. The Condensation Shift MUST NOT include the Proposal hash in its identity.
5. A ShiftResult MUST bind the Condensation Shift to the Proposal version hash.

---

# 4. Proposal lifecycle

Proposal states:

```text
pending -> editing -> ready -> board_committed
        \          \       \-> rejected
         \          \-> withdrawn
          \-> expired
```

Statuses:

| Status | Meaning |
|---|---|
| `pending` | Created by condensation, not yet reviewed. |
| `editing` | Human or authorized process is editing material slots. |
| `ready` | Exact Proposal version is confirmable; arming clock is running. |
| `rejected` | Human refused the Proposal. |
| `withdrawn` | Proposer or system withdrew it before BoardCommit. |
| `board_committed` | Query/index status after a BoardAct has been board_committed. |
| `expired` | Proposal exceeded `expires_at`. |

## 4.1 pending -> editing

Occurs when any material field is edited.

Material fields include:

```text
slots
confidence if used for BoardCommit policy
expires_at if it affects confirmability
```

## 4.2 editing/pending -> ready

Marking ready is a versioned operation.

```python
def mark_ready(proposal, now_ms):
    if proposal["status"] not in ("pending", "editing"):
        raise ValueError("proposal_not_readyable")

    proposal["status"] = "ready"
    proposal["ready_at"] = now_ms
    proposal["arming_started_at"] = now_ms
    proposal["version"] += 1
    proposal["version_hash"] = hash_value(proposal_version_body(proposal))
    return proposal
```

The arming delay starts at `ready_at` for this exact Proposal version.

## 4.3 ready -> editing

Any material edit after ready MUST clear arming state.

```python
def material_edit(proposal, new_slots, now_ms):
    proposal["slots"] = new_slots
    proposal["status"] = "editing"
    proposal["ready_at"] = None
    proposal["arming_started_at"] = None
    proposal["last_material_edit_at"] = now_ms
    proposal["version"] += 1
    proposal["version_hash"] = hash_value(proposal_version_body(proposal))
    return proposal
```

This prevents a user from marking ready, waiting out the clock, editing the substance, and confirming instantly.

## 4.4 ready -> board_committed

Allowed only through a Confirmation Shift and BoardCommit transaction.

## 4.5 ready -> rejected

Allowed when a human refuses the Proposal. Rejection does not create a BoardAct unless the product policy wants explicit rejection BoardActs.

## 4.6 pending/editing/ready -> withdrawn

Allowed when the system or proposer withdraws the Proposal.

## 4.7 pending/editing/ready -> expired

Allowed when `expires_at` is present and `now >= expires_at`.

---

# 5. Confirmation as the truth gate

Confirmation is not a UI click. It is an auditable Shift.

A Confirmation Shift takes the exact Proposal version hash as input and produces one BoardAct.

## 5.1 What confirmation proves

Confirmation does not prove comprehension.

It proves:

1. The exact Proposal version was available for review.
2. The actor had enough authority.
3. The arming delay elapsed.
4. The actor accepted responsibility.
5. The exact board_committed BoardAct can be traced to that Confirmation Shift.

## 5.2 BoardCommit preconditions

A BoardAct can be board_committed only if all checks pass:

```text
proposal.status == "ready"
proposal.version_hash == load_current_proposal_version_hash(proposal_id)
now_ms - proposal.arming_started_at >= ARMING_DELAY_MS[risk]
authority(actor) >= risk_rank(proposal.slots.risk)
no_blocking_attention_anomaly(actor)
stream_sequence_lock_acquired(stream_id)
```

## 5.3 Arming delays

Default arming delays by risk:

| Risk | Delay | Milliseconds |
|---|---:|---:|
| L0 | 5 seconds | 5,000 |
| L1 | 5 seconds | 5,000 |
| L2 | 10 seconds | 10,000 |
| L3 | 20 seconds | 20,000 |
| L4 | 60 seconds | 60,000 |

The delay is deliberate friction. Its purpose is not to make work slow; its purpose is to make blind confirmation visible and harder to perform accidentally.

## 5.4 `can_confirm` reference algorithm

```python
RISK_RANK = {"L0": 0, "L1": 1, "L2": 2, "L3": 3, "L4": 4}
ARMING_DELAY_MS = {"L0": 5000, "L1": 5000, "L2": 10000, "L3": 20000, "L4": 60000}


def can_confirm(proposal, actor, now_ms):
    if proposal["status"] != "ready":
        return False, "proposal_not_ready"

    current_hash = load_current_proposal_version_hash(proposal["proposal_id"])
    if proposal["version_hash"] != current_hash:
        return False, "proposal_changed_during_confirmation"

    risk = proposal["slots"]["risk"]
    actual_delay = now_ms - proposal["arming_started_at"]
    if actual_delay < ARMING_DELAY_MS[risk]:
        return False, "arming_delay_not_satisfied"

    if effective_authority(actor) < RISK_RANK[risk]:
        return False, "insufficient_authority"

    if attention_anomaly_blocked(actor):
        return False, "attention_anomaly_block"

    return True, "ok"
```

## 5.5 Confirmation evidence

A Confirmation Shift must include at least:

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

## 5.6 BoardCommit algorithm, final form

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

# 6. Supersession lifecycle

Corrections never edit the past.

A correction, cancellation, or replacement is represented by a new Proposal and a new BoardAct. The new BoardAct identifies what it supersedes through `slots.cancellations` or a domain-specific supersession field.

Derived state may then show:

```text
old BoardAct -> superseded by later BoardAct
```

Rules:

1. The original BoardAct remains in the ledger.
2. The new BoardAct is appended later with its own `seq` and `board_act_hash`.
3. A projection may render the old BoardAct as superseded.
4. Verification still includes the old BoardAct in the chain.
5. No database update to the old BoardAct is required for canonical correctness.

---

# 7. Findings lifecycle

Findings are derived Board objects with stable identity.

## 7.1 Finding creation

Findings may be produced by:

1. WorkflowMatcher.
2. Expectation checker.
3. Verification service.
4. Attention anomaly detector.
5. Investigation Shift.
6. System Shift.

Creation order:

```text
1. Read relevant input state.
2. Compute input_hash.
3. Build Shift without output_hash.
4. Compute shift_hash.
5. Build Finding(s) with detected_by = shift_hash.
6. Compute Finding identity hash(es).
7. Store Shift.
8. Store Finding(s).
9. Store ShiftResult for one Finding or an output_set manifest.
```

## 7.2 Consequential transitions

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

## 7.3 Resolving a gap

A gap can be resolved when a later BoardAct fulfills the expectation or workflow requirement that generated it.

Recommended resolution record:

```json
{
  "resolved": true,
  "resolved_at": 1700000100000,
  "resolved_by": "board_act_hash_that_fulfilled_expectation",
  "resolution_note": "verify_engine board_committed for engine:A"
}
```

If the resolution changes what actors can do, restores authority, or clears a blocking violation, the transition MUST cite a BoardAct or auditable Shift.

---

# 8. Pending expectations vs gaps

For v0.1, use two states:

```text
pending_expectation = expected but not late
gap                 = expected and violated or intentionally rendered as open Board work
```

## 8.1 Vertical-slice policy

The vertical slice MAY render any unfulfilled expectation immediately as:

```text
Finding(kind="gap")
```

This is allowed because the demo must prove the Board surface.

## 8.2 Production policy

Production SHOULD create:

```text
Expectation(status="open")
```

first.

It SHOULD create:

```text
Finding(kind="gap")
```

only when:

```text
deadline_ms != null and now >= deadline_ms
```

or when workflow policy says the expectation is immediately blocking.

---

# 9. Projection lifecycle

A Projection translates board_committed BoardActs and Board Findings into a readable view for a specific audience.

## 9.1 Creation order

```text
1. Select BoardActs and Findings for audience and as_of_seq.
2. Compute projection input hash.
3. Build Projection Shift without output_hash.
4. Compute projection shift_hash.
5. Build Projection with built_by = projection shift_hash.
6. Compute projection_hash.
7. Store Shift.
8. Store Projection.
9. Store ShiftResult(output_kind="projection", output_hash=projection_hash).
```

## 9.2 Source-prefix validity

A Projection is valid only if its source BoardActs match the ledger prefix it claims to render.

For a Projection with `as_of_seq = N`:

```text
Projection.source.board_act_hashes MUST equal the ordered hash list of BoardActs seq 1..N for the stream,
unless Projection.source.partial = true.
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

## 9.3 Projection audiences

| Audience | Required emphasis |
|---|---|
| `operator` | Active Board, blocking Findings, next actions. |
| `newcomer` | Narrative of what has been board_committed and what remains open. |
| `auditor` | Hash chain, provenance, ShiftResult receipts, and verification status. |

---

# 10. Verification lifecycle

Verification recomputes the chain and the key object identities.

Minimum verification checks:

1. Stream canonicalization version is fixed.
2. BoardAct `seq` values are contiguous per stream.
3. First BoardAct points to `GENESIS_HASH`.
4. Each BoardAct `prev_board_act_hash` points to the previous BoardAct hash.
5. Each `board_act_hash` matches `hash_value(BoardActIdentityBody)`.
6. Every BoardAct has a Confirmation Shift.
7. Every BoardAct has a ShiftResult binding that Confirmation Shift to `board_act_hash`.
8. Every Projection source prefix matches BoardActs `1..as_of_seq` unless `partial = true`.
9. Every produced Proposal, BoardAct, Finding, Projection, or output set has a ShiftResult.
10. No Shift identity body contains `output_hash`.

Verification may produce a `Verification Shift` and a `ShiftResult(output_kind="verification")`. A failure may also produce `Finding(kind="tamper_detected")`.

---

# 11. Attention and dynamic authority

Authority is not just a role. It is effective permission adjusted by observed integrity.

```text
effective_authority = role cap adjusted by integrity_score
```

A confirmation of risk `L3` requires effective authority of at least 3.

Attention anomaly checks SHOULD consider recent confirmation evidence. Minimum v0.1 signal:

```python
latency_ms = confirmed_at - proposal["arming_started_at"]
```

Suggested anomaly rule:

```python
def check_attention_anomaly(actor, K, threshold_ms, window_ms):
    recent = last_k_confirmations(actor, K, window_ms)
    if len(recent) == K and all(r["latency_ms"] < threshold_ms for r in recent):
        return Finding(kind="attention_anomaly", severity="warn")
```

A blocking anomaly must be represented as a Finding. If it blocks confirmation, downgrades authority, or is dismissed, that transition must be auditable.

---

# 12. Lifecycle invariants

1. A Shift hash does not change when ShiftResult is recorded.
2. Proposal can cite Condensation Shift without circular hashing.
3. BoardAct can cite Confirmation Shift without circular hashing.
4. Projection can cite Projection Shift without circular hashing.
5. Every produced Proposal, BoardAct, Finding, Projection, or output set has a ShiftResult.
6. Proposal edit after ready clears `ready_at` and `arming_started_at`.
7. Confirmation `input_hash` equals the current Proposal version hash.
8. Projection source `board_act_hashes` equal ledger seq `1..as_of_seq` unless `partial = true`.
9. Consequential Finding dismissal or resolution has a BoardAct or Shift reference.
10. Stream canonicalization version is fixed at stream creation.
11. No provisional hashes are used.
12. `output_hash` is never part of `ShiftIdentityBody`.
