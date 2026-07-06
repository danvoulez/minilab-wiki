# BOARD_VERTICAL_SLICE.md

# The Board Protocol: Minimal Vertical Slice

Status: v0.1, locked against `BOARD_DECISIONS_v0.1.md`.

This document defines the smallest useful build that proves the complete Board loop:

```text
Event -> Proposal -> BoardAct -> Finding -> Projection -> Verify
```

The vertical slice should be small enough to build quickly, but complete enough to prove the central thesis: a third party can read the Projection, understand what happened, see what remains unresolved, and verify that the state corresponds to a board-committed hash-chained ledger.

v0.1 implementation law:

```text
Do not use provisional hashes.
Do not update a Shift hash after output exists.
Do not put output_hash in ShiftIdentityBody.
Use ShiftResult to bind a Shift to what it produced.
```

---

# 1. Success criteria

The build succeeds when all of the following are true:

1. A human or tool can create Events in a stream.
2. A real or strict-stub Condensation step converts Event windows into structured Proposals.
3. A human can review a Proposal and commit it only after arming delay.
4. BoardCommit creates an immutable BoardAct in a hash-chained ledger.
5. Every produced Proposal, BoardAct, Finding, Projection, or output set has a ShiftResult.
6. A workflow check creates at least one Finding from board_committed state.
7. A Projection explains BoardActs and open Findings to a newcomer.
8. A verification command recomputes hashes and detects tampering.
9. No identity routine uses provisional hashes.

The demo should work even for a person who never saw the raw fast loop.

---

# 2. Non-goals

Do not build these in the first slice:

1. Multi-organization permissions.
2. Complex CRDT or multi-region collaboration.
3. Full external effect execution.
4. Rich visual board UI.
5. Cryptographic signatures.
6. Long-term archival policy.
7. Adaptive authority calibration beyond simple counters.
8. Full workflow state-machine language.
9. Multiple ShiftResult receipts per Shift, unless implemented through an explicit output-set policy.

The slice should prove the spine, not the whole product.

---

# 3. Demo scenario

Use a tiny operational workflow:

```text
start_engine -> verify_engine -> taxi
```

Demonstration path:

1. Human Event says: "Start engine A."
2. Condensation creates Proposal with `what = "start_engine"`, `object = "engine:A"`, `risk = "L2"`, and an expectation for `verify_engine`.
3. Proposal cites the Condensation Shift through `proposed_by`.
4. ShiftResult binds the Condensation Shift to `proposal.version_hash`.
5. Human marks Proposal ready.
6. System enforces a 10 second L2 arming delay starting at `ready_at`.
7. Human confirms.
8. Confirmation Shift is built without `output_hash`.
9. BoardAct cites the Confirmation Shift in `provenance.confirmation_shift`.
10. ShiftResult binds the Confirmation Shift to `board_act_hash`.
11. Workflow/expectation check creates a `gap` Finding because `verify_engine` has not happened yet.
12. Projection says engine A was started and verification is still pending.
13. Verify command confirms the BoardAct chain and ShiftResult links.
14. Tamper test changes one byte in the BoardAct and verify fails.

This scenario is intentionally small but contains every important mechanism.

---

# 4. Minimal architecture

```text
Browser or CLI
    |
    v
API server
    |
    +-- Stream config store          canonicalization and hash algorithm
    +-- Event store                  hot, TTL-managed
    +-- Proposal store               mutable proposals
    +-- Shift store                  agency audit log, no output_hash
    +-- ShiftResult store            output receipts
    +-- BoardAct ledger              append-only Envelope canon
    +-- Finding store                Board contents
    +-- Projection builder           readable views
    +-- Verification service         hash checks
    |
    v
LLM gateway                          condensation and optional projection prose
```

Recommended first stack:

| Layer | Recommendation |
|---|---|
| Backend | Python with FastAPI, or Node with Express/Fastify |
| Database | SQLite for local slice |
| Hashing | Standard library SHA-256 |
| Canonical JSON | `json.dumps(sort_keys=True, separators=(",", ":"), ensure_ascii=False)` |
| LLM gateway | Thin adapter with one JSON-returning condensation call |
| Frontend | Minimal HTML forms, or CLI-only for first pass |
| CLI | `event`, `condense`, `ready`, `commit`, `board`, `project`, `verify` |

A CLI-only implementation is acceptable if it produces readable Markdown or JSON Projections.

---

# 5. Minimal data tables

Use SQLite tables that store canonical JSON bodies. Avoid premature normalization.

## 5.1 streams

```sql
CREATE TABLE streams (
    stream_id TEXT PRIMARY KEY,
    canonicalization TEXT NOT NULL,
    hash_algorithm TEXT NOT NULL,
    created_at INTEGER NOT NULL
);
```

A stream MUST NOT change `canonicalization` or `hash_algorithm` mid-chain.

## 5.2 events

```sql
CREATE TABLE events (
    event_id TEXT PRIMARY KEY,
    stream_id TEXT NOT NULL,
    source TEXT NOT NULL,
    payload_json TEXT NOT NULL,
    ts INTEGER NOT NULL,
    zone TEXT NOT NULL,
    parent_event TEXT
);

CREATE INDEX idx_events_stream_ts ON events(stream_id, ts);
```

## 5.3 proposals

```sql
CREATE TABLE proposals (
    proposal_id TEXT PRIMARY KEY,
    stream_id TEXT NOT NULL,
    proposed_by TEXT NOT NULL,
    source_window_json TEXT NOT NULL,
    source_digest TEXT NOT NULL,
    slots_json TEXT NOT NULL,
    confidence REAL NOT NULL,
    status TEXT NOT NULL,
    opened_at INTEGER NOT NULL,
    ready_at INTEGER,
    last_material_edit_at INTEGER NOT NULL,
    arming_started_at INTEGER,
    expires_at INTEGER,
    version INTEGER NOT NULL,
    version_hash TEXT NOT NULL,
    edit_history_json TEXT NOT NULL
);

CREATE INDEX idx_proposals_stream_status ON proposals(stream_id, status);
```

## 5.4 shifts

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
```

`output_hash` is intentionally absent.

## 5.5 shift_results

```sql
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

CREATE UNIQUE INDEX idx_shift_results_one_per_shift
ON shift_results(shift_hash);
```

The unique index enforces one result per Shift in the vertical slice.

## 5.6 board_acts

```sql
CREATE TABLE board_acts (
    board_act_hash TEXT PRIMARY KEY,
    prev_board_act_hash TEXT NOT NULL,
    seq INTEGER NOT NULL,
    stream_id TEXT NOT NULL,
    slots_json TEXT NOT NULL,
    provenance_json TEXT NOT NULL,
    board_commit_json TEXT NOT NULL,
    audit_json TEXT NOT NULL,
    identity_body_json TEXT NOT NULL,
    UNIQUE(stream_id, seq)
);

CREATE INDEX idx_board_acts_stream_seq ON board_acts(stream_id, seq);
```

## 5.7 findings

```sql
CREATE TABLE findings (
    finding_id TEXT PRIMARY KEY,
    stream_id TEXT NOT NULL,
    kind TEXT NOT NULL,
    refs_json TEXT NOT NULL,
    severity TEXT NOT NULL,
    detail TEXT NOT NULL,
    detected_at INTEGER NOT NULL,
    detected_by TEXT NOT NULL,
    resolution_json TEXT,
    identity_body_json TEXT NOT NULL
);

CREATE INDEX idx_findings_stream_kind ON findings(stream_id, kind);
```

## 5.8 projections

```sql
CREATE TABLE projections (
    projection_hash TEXT PRIMARY KEY,
    stream_id TEXT NOT NULL,
    built_by TEXT NOT NULL,
    audience TEXT NOT NULL,
    narrative_json TEXT NOT NULL,
    open_findings_json TEXT NOT NULL,
    as_of_seq INTEGER NOT NULL,
    generated_at INTEGER NOT NULL,
    source_json TEXT NOT NULL,
    identity_body_json TEXT NOT NULL
);
```

---

# 6. Required commands or endpoints

You can expose these as REST endpoints, CLI commands, or both.

## 6.1 Stream create

```text
POST /streams
```

CLI:

```text
board stream create demo --canonicalization board-json-v0 --hash-algorithm sha256
```

Output:

```json
{
  "stream_id": "demo",
  "canonicalization": "board-json-v0",
  "hash_algorithm": "sha256"
}
```

## 6.2 Event

```text
POST /streams/{stream_id}/events
```

CLI:

```text
board event push --stream demo --source human:pilot --payload "Start engine A"
```

Output:

```json
{
  "event_id": "evt_001",
  "stream_id": "demo",
  "zone": "live"
}
```

## 6.3 Condense

```text
POST /streams/{stream_id}/condense
```

CLI:

```text
board condense --stream demo --last 1
```

Output:

```json
{
  "proposal_id": "prop_001",
  "source_digest": "...",
  "status": "pending",
  "proposed_by": "condensation_shift_hash",
  "version_hash": "proposal_version_hash",
  "shift_result": "shift_result_hash",
  "slots": {
    "gloss": "Start engine A",
    "who": "human:pilot",
    "what": "start_engine",
    "scope": "flight.pre_taxi",
    "object": "engine:A",
    "cancellations": [],
    "expects": [
      {
        "expectation_id": "exp_001",
        "expected_what": "verify_engine",
        "expected_object": "engine:A",
        "deadline_ms": null,
        "severity_on_miss": "warn"
      }
    ],
    "risk": "L2",
    "tags": ["engine", "preflight"]
  }
}
```

## 6.4 Mark ready

```text
POST /proposals/{proposal_id}/ready
```

CLI:

```text
board proposal ready prop_001 --actor human:pilot
```

Output:

```json
{
  "proposal_id": "prop_001",
  "status": "ready",
  "ready_at": 1700000005000,
  "arming_started_at": 1700000005000,
  "arming_required_ms": 10000,
  "armed_at": 1700000015000,
  "version_hash": "new_proposal_version_hash"
}
```

## 6.5 Commit

```text
POST /proposals/{proposal_id}/commit
```

CLI:

```text
board commit prop_001 --actor human:pilot
```

Successful output:

```json
{
  "ok": true,
  "board_act_hash": "board_act_hash_001",
  "seq": 1,
  "confirmation_shift": "shift_conf_001",
  "shift_result": "result_conf_001"
}
```

Early output:

```json
{
  "ok": false,
  "reason": "arming_delay_not_satisfied"
}
```

## 6.6 Board

```text
GET /streams/{stream_id}/board
```

CLI:

```text
board findings --stream demo --open
```

## 6.7 Project

```text
GET /streams/{stream_id}/projection?audience=newcomer
```

CLI:

```text
board project --stream demo --audience newcomer
```

## 6.8 Verify

```text
GET /streams/{stream_id}/verify
```

CLI:

```text
board verify --stream demo
```

---

# 7. Minimal algorithms

## 7.1 Canonical hashing

```python
import hashlib
import json

GENESIS_HASH = "0" * 64


def canonical_json(value):
    return json.dumps(value, sort_keys=True, separators=(",", ":"), ensure_ascii=False)


def hash_value(value):
    return hashlib.sha256(canonical_json(value).encode("utf-8")).hexdigest()
```

## 7.2 Event digest body

```python
def event_digest_body(event):
    return {
        "event_id": event["event_id"],
        "source": event["source"],
        "payload": event["payload"],
        "ts": event["ts"],
        "parent_event": event.get("parent_event"),
    }


def source_digest(events):
    ordered = sorted(events, key=lambda e: (e["ts"], e["event_id"]))
    return hash_value([event_digest_body(e) for e in ordered])
```

## 7.3 Shift identity body

```python
def shift_identity_body(shift):
    evidence_hashes = [hash_value(e) for e in shift["evidence"]]
    return {
        "stream_id": shift["stream_id"],
        "role": shift["role"],
        "kind": shift["kind"],
        "input_hash": shift["input_hash"],
        "actor": shift["actor"],
        "risk_observed": shift["risk_observed"],
        "evidence_hashes": evidence_hashes,
        "opened_at": shift["opened_at"],
        "closed_at": shift["closed_at"],
        "duration_ms": shift["duration_ms"],
        "model_call": shift.get("model_call"),
    }
```

There is no `output_hash` here.

## 7.4 ShiftResult identity body

```python
def shift_result_identity_body(result):
    return {
        "shift_hash": result["shift_hash"],
        "output_kind": result["output_kind"],
        "output_hash": result["output_hash"],
        "output_refs": result["output_refs"],
        "recorded_at": result["recorded_at"],
    }
```

## 7.5 Proposal version hash

```python
def proposal_version_body(proposal):
    return {
        "proposal_id": proposal["proposal_id"],
        "stream_id": proposal["stream_id"],
        "proposed_by": proposal["proposed_by"],
        "source_digest": proposal["source_digest"],
        "slots": proposal["slots"],
        "confidence": proposal["confidence"],
        "status": proposal["status"],
        "opened_at": proposal["opened_at"],
        "ready_at": proposal.get("ready_at"),
        "last_material_edit_at": proposal["last_material_edit_at"],
        "arming_started_at": proposal.get("arming_started_at"),
        "expires_at": proposal.get("expires_at"),
        "version": proposal["version"],
    }
```

## 7.6 Condensation creation routine

```python
def condense(stream_id, events, model_output, now_ms):
    digest = source_digest(events)
    input_body = {
        "stream_id": stream_id,
        "source_digest": digest,
        "events": [event_digest_body(e) for e in sorted(events, key=lambda e: (e["ts"], e["event_id"]))],
        "condenser_version": "membrane-v0.1",
    }
    input_hash = hash_value(input_body)

    evidence = [
        {"kind": "source_digest", "value": digest, "ts": now_ms},
        {"kind": "model_output", "value": hash_value(model_output), "ts": now_ms},
    ]
    shift = {
        "stream_id": stream_id,
        "role": "curator",
        "kind": "condensation",
        "input_hash": input_hash,
        "actor": "model:membrane",
        "risk_observed": model_output["slots"]["risk"],
        "evidence": evidence,
        "opened_at": now_ms,
        "closed_at": now_ms,
        "duration_ms": 0,
        "model_call": model_output.get("model_call"),
    }
    shift_body = shift_identity_body(shift)
    shift_hash = hash_value(shift_body)

    proposal = {
        "proposal_id": new_id("prop"),
        "stream_id": stream_id,
        "proposed_by": shift_hash,
        "source_window": [e["event_id"] for e in events],
        "source_digest": digest,
        "slots": model_output["slots"],
        "confidence": model_output["confidence"],
        "status": "pending",
        "opened_at": now_ms,
        "ready_at": None,
        "last_material_edit_at": now_ms,
        "arming_started_at": None,
        "expires_at": None,
        "version": 1,
        "edit_history": [],
    }
    proposal["version_hash"] = hash_value(proposal_version_body(proposal))

    shift["shift_hash"] = shift_hash
    result_body = {
        "shift_hash": shift_hash,
        "output_kind": "proposal",
        "output_hash": proposal["version_hash"],
        "output_refs": [proposal["proposal_id"]],
        "recorded_at": now_ms,
    }
    result = {"result_hash": hash_value(result_body), **result_body}

    store_shift(shift)
    store_proposal(proposal)
    store_shift_result(result)
    return proposal
```

## 7.7 Mark ready

```python
def mark_ready(proposal, now_ms):
    if proposal["status"] not in ("pending", "editing"):
        raise ValueError("proposal_not_readyable")
    proposal["status"] = "ready"
    proposal["ready_at"] = now_ms
    proposal["arming_started_at"] = now_ms
    proposal["version"] += 1
    proposal["version_hash"] = hash_value(proposal_version_body(proposal))
    save_proposal(proposal)
    return proposal
```

## 7.8 Material edit after ready

```python
def material_edit(proposal, new_slots, now_ms):
    old_hash = proposal["version_hash"]
    proposal["edit_history"].append(old_hash)
    proposal["slots"] = new_slots
    proposal["status"] = "editing"
    proposal["ready_at"] = None
    proposal["arming_started_at"] = None
    proposal["last_material_edit_at"] = now_ms
    proposal["version"] += 1
    proposal["version_hash"] = hash_value(proposal_version_body(proposal))
    save_proposal(proposal)
    return proposal
```

## 7.9 BoardCommit

```python
RISK_RANK = {"L0": 0, "L1": 1, "L2": 2, "L3": 3, "L4": 4}
ARMING_DELAY_MS = {"L0": 5000, "L1": 5000, "L2": 10000, "L3": 20000, "L4": 60000}


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
        shift = {
            "stream_id": proposal["stream_id"],
            "role": "confirmer",
            "kind": "confirmation",
            "input_hash": proposal["version_hash"],
            "actor": actor["actor_id"],
            "risk_observed": risk,
            "evidence": evidence,
            "opened_at": proposal["arming_started_at"],
            "closed_at": now_ms,
            "duration_ms": actual_delay,
            "model_call": None,
        }
        shift_body = shift_identity_body(shift)
        confirmation_shift_hash = hash_value(shift_body)

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

        shift["shift_hash"] = confirmation_shift_hash
        board_act = {"board_act_hash": board_act_hash, **board_act_identity}
        result_body = {
            "shift_hash": confirmation_shift_hash,
            "output_kind": "board_act",
            "output_hash": board_act_hash,
            "output_refs": [board_act_hash],
            "recorded_at": now_ms,
        }
        result = {"result_hash": hash_value(result_body), **result_body}

        store_shift(shift)
        store_board_act(board_act)
        store_shift_result(result)
        mark_proposal_board_committed(proposal_id, board_act_hash)

    run_post_board_commit_jobs_idempotently(board_act_hash)
    return {"ok": True, "board_act_hash": board_act_hash, "seq": seq}
```

## 7.10 Minimal matcher

```python
EXPECTED = ["start_engine", "verify_engine", "taxi"]


def match_workflow(board_acts):
    observed = [a["slots"]["what"] for a in board_acts]
    findings = []

    for i, what in enumerate(observed):
        if what not in EXPECTED:
            findings.append(("extra", i, what))

    positions = [EXPECTED.index(w) for w in observed if w in EXPECTED]
    if positions != sorted(positions):
        findings.append(("reordered", None, observed))

    if positions:
        frontier = max(positions)
        for expected_what in EXPECTED[:frontier]:
            if expected_what not in observed:
                findings.append(("missing", None, expected_what))

    return findings
```

## 7.11 Gap check

For the demo, create an immediate gap if a BoardAct creates an expectation and no later BoardAct fulfills it yet. In production, honor deadlines.

```python
def check_gaps(board_acts, expectations):
    observed = {(a["slots"]["what"], a["slots"].get("object")) for a in board_acts}
    findings = []

    for exp in expectations:
        key = (exp["expected_what"], exp.get("expected_object"))
        if key not in observed:
            findings.append({
                "kind": "gap",
                "severity": exp.get("severity_on_miss", "warn"),
                "detail": f"Expected {exp['expected_what']} has not been board_committed.",
            })

    return findings
```

Production policy: create `Expectation(status="open")` first, and create a `gap` Finding only once the expectation is late or immediately blocking.

## 7.12 Projection source validity

```python
def projection_source(stream_id, as_of_seq, partial=False, selection_reason=None):
    board_acts = load_board_acts(stream_id, seq_lte=as_of_seq)
    return {
        "stream_id": stream_id,
        "as_of_seq": as_of_seq,
        "board_act_hashes": [a["board_act_hash"] for a in board_acts],
        "finding_ids": [f["finding_id"] for f in unresolved_findings(stream_id, as_of_seq)],
        "partial": partial,
        "selection_reason": selection_reason,
    }


def validate_projection_source(projection):
    source = projection["source"]
    if source["partial"]:
        return bool(source["selection_reason"])
    expected = [a["board_act_hash"] for a in load_board_acts(source["stream_id"], seq_lte=source["as_of_seq"])]
    return source["board_act_hashes"] == expected
```

---

# 8. Milestones

## M0: Project skeleton and hashing

Deliverables:

1. `canonical_json` and `hash_value`.
2. `GENESIS_HASH`.
3. Stream config with fixed canonicalization.
4. Object validators.
5. SQLite connection and migrations.
6. Basic CLI or API shell.

Acceptance test:

```text
Hashing the same object with different key order produces the same hash.
Stream canonicalization cannot be changed after creation.
```

## M1: Fast loop Events

Deliverables:

1. Push Event.
2. List Events by stream.
3. TTL zone transition job or simulated command.
4. Source digest computation.

Acceptance test:

```text
Create Event -> compute source_digest -> change zone -> source_digest stays unchanged.
```

## M2: Condensation Shift and Proposal

Deliverables:

1. LLM gateway or strict stub behind the same interface.
2. Condensation Shift record without output_hash.
3. Proposal record with slots, source window, digest, confidence, and arming fields.
4. ShiftResult binding Condensation Shift to Proposal version hash.
5. Proposal JSON validation.

Acceptance tests:

```text
Event "Start engine A" produces Proposal what=start_engine object=engine:A risk=L2.
Proposal.proposed_by points to existing Condensation Shift.
Condensation Shift hash does not change when ShiftResult is recorded.
```

Use a real LLM call for the main demo. Keep a deterministic stub for tests.

## M3: Confirmation gate and BoardAct ledger

Deliverables:

1. Mark Proposal ready.
2. Enforce arming delay from `arming_started_at`.
3. Record attention latency, arming delay, and authority check.
4. Record Confirmation Shift without output_hash.
5. Append BoardAct with `seq`, `prev_board_act_hash`, and `board_act_hash`.
6. ShiftResult binding Confirmation Shift to BoardAct hash.

Acceptance tests:

```text
Attempt board_commit before arming delay -> rejected.
Attempt board_commit after delay -> BoardAct seq=1 appended.
Second BoardAct points to first BoardAct hash.
BoardAct.provenance.confirmation_shift points to existing Confirmation Shift.
Confirmation Shift hash does not change when ShiftResult is recorded.
```

## M4: Findings and Board

Deliverables:

1. Minimal workflow expected sequence.
2. Gap check from expectations.
3. Finding store with stable identity.
4. ShiftResult for Finding or output set.
5. Board endpoint or command showing open Findings.

Acceptance test:

```text
After start_engine, Board shows gap: verify_engine pending.
Consequential Finding resolution must cite a BoardAct or Shift.
```

## M5: Projection

Deliverables:

1. Projection builder for `audience = newcomer`.
2. Projection Shift record without output_hash.
3. Markdown or JSON output.
4. Include BoardActs and open Findings.
5. Projection source prefix with BoardActs seq 1..as_of_seq.
6. ShiftResult binding Projection Shift to Projection hash.

Acceptance tests:

```text
Projection explains that engine A was started and verification remains open.
Projection.source.board_act_hashes equals ledger seq 1..as_of_seq.
Projection.built_by points to existing Projection Shift.
Projection Shift hash does not change when ShiftResult is recorded.
```

## M6: Verification and tamper test

Deliverables:

1. Verify command recomputes BoardAct hashes.
2. Verify command checks `prev_board_act_hash` chain.
3. Verify command checks ShiftResult links.
4. Verify command checks Projection source prefix validity.
5. Tamper command or manual test.
6. `tamper_detected` Finding on failure.

Acceptance tests:

```text
Verify clean ledger -> ok.
Modify BoardAct slot in database -> verify fails.
Restore BoardAct -> verify ok.
Remove ShiftResult for BoardAct -> verify fails.
Add output_hash to Shift identity body -> test fails.
```

---

# 9. End-to-end demo script

Assume CLI name is `board`.

```bash
board init --db board_demo.sqlite
board stream create demo --canonicalization board-json-v0 --hash-algorithm sha256

board event push \
  --stream demo \
  --source human:pilot \
  --payload "Start engine A before taxi."

board condense --stream demo --last 1
board proposal list --stream demo
board proposal ready prop_001 --actor human:pilot

# Before arming delay expires, this should fail.
board commit prop_001 --actor human:pilot

# After delay expires, this should pass.
board commit prop_001 --actor human:pilot

board findings --stream demo --open
board project --stream demo --audience newcomer --format markdown
board verify --stream demo
```

Expected Projection excerpt:

```text
As of sequence 1, one BoardAct has been board_committed.

1. Start engine A before taxi
   The pilot committed an L2 BoardAct to start engine A. The BoardAct was produced from a condensed source window and confirmed after the arming delay.

Open Board Findings

- Gap: verify_engine
  The workflow expects engine A to be verified before taxi. No matching BoardAct has been board_committed yet.

Verification

- Chain status: ok
- Latest sequence: 1
- ShiftResult receipts: ok
- Projection source prefix: ok
```

---

# 10. Verification test details

## 10.1 Clean chain

Input:

```json
[
  {
    "seq": 1,
    "prev_board_act_hash": "0000000000000000000000000000000000000000000000000000000000000000",
    "board_act_hash": "computed_hash_1"
  },
  {
    "seq": 2,
    "prev_board_act_hash": "computed_hash_1",
    "board_act_hash": "computed_hash_2"
  }
]
```

Expected:

```json
{
  "ok": true,
  "latest_seq": 2,
  "latest_hash": "computed_hash_2"
}
```

## 10.2 Tampered BoardAct

Change one byte in `slots_json` for sequence 1.

Expected:

```json
{
  "ok": false,
  "reason": "board_act_hash_mismatch",
  "seq": 1
}
```

## 10.3 Broken ShiftResult

Delete the ShiftResult for the Confirmation Shift that produced sequence 1.

Expected:

```json
{
  "ok": false,
  "reason": "missing_shift_result",
  "output_kind": "board_act",
  "output_hash": "computed_hash_1"
}
```

## 10.4 Projection prefix violation

Build a Projection with `as_of_seq = 2` but include only BoardAct 2 in `source.board_act_hashes`, with `partial = false`.

Expected:

```json
{
  "ok": false,
  "reason": "projection_source_prefix_mismatch",
  "projection_hash": "..."
}
```

---

# 11. Hard invariant test suite

Add these tests before product work expands beyond the slice:

1. Shift hash does not change when ShiftResult is recorded.
2. Proposal can cite Condensation Shift without circular hashing.
3. BoardAct can cite Confirmation Shift without circular hashing.
4. Projection can cite Projection Shift without circular hashing.
5. Every produced Proposal, BoardAct, Finding, Projection, or output set has a ShiftResult.
6. Proposal edit after ready clears `ready_at` and `arming_started_at`.
7. Confirmation `input_hash` equals the current Proposal version hash.
8. Projection source `board_act_hashes` equal ledger seq `1..as_of_seq` unless `partial = true`.
9. Consequential Finding dismissal or resolution has a BoardAct or Shift reference.
10. Stream canonicalization version is fixed at stream creation.
11. `ShiftIdentityBody` never includes `output_hash`.
12. No implementation path uses `PENDING`, `PROVISIONAL`, or placeholder output hashes.

---

# 12. Minimal implementation order

Build in this exact order:

1. Hashing and stream config.
2. Event store and source digest.
3. Shift store without `output_hash`.
4. ShiftResult store.
5. Condensation Shift -> Proposal -> ShiftResult.
6. Proposal ready and edit invalidation.
7. Confirmation Shift -> BoardAct -> ShiftResult.
8. Workflow and immediate demo gaps.
9. Projection Shift -> Projection -> ShiftResult.
10. Verify chain and receipts.
11. Tamper tests.

At the end of step 10, the vertical slice proves the Board spine.
