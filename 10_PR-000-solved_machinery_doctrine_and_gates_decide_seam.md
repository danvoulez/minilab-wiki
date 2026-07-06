# PR-000.6 — Solved Machinery Doctrine + `gates.decide` First Seam

**Status:** architecture / probe draft  
**Layer:** Layer 4 preparation — SDK adoption discipline and first ops-backend seam  
**Parent:** `docs/build-equation.md`, Minilab canon docs 01–07, App Park Primordial Packet v0.1, Layer 4 Ops Backend Blueprint  
**Evidence status:** architecture placed, runtime-unverified  
**Runtime mutation:** none  
**World-touch:** none

---

## 0. Purpose

This document introduces the doctrine for adopting open-source SDKs and defines the first tiny seam probe: `POST /api/gates/decide`.

The purpose is not to build the backend.
The purpose is not to vendor a large dependency stack.
The purpose is not to adopt a policy brain.

The purpose is to prove the boundary:

```txt
SDKs solve machinery.
Minilab preserves meaning.
```

The first proof is deliberately small:

```txt
axum + tokio + serde
→ HTTP transport seam
→ mocked constitutional-runtime admission boundary
→ AuthorityResponse-like JSON
→ no Dispatcher
→ no database write
→ no receipt claim
→ no provider call
→ no LAB execution
```

---

## 1. Canonical Sentence

```txt
Minilab adopts mature open-source SDKs for solved machinery,
but never outsources authority, canon, evidence closure, receipt semantics,
or consequence.
```

Short form:

```txt
Use the world’s machinery.
Keep Minilab’s meaning.
```

---

## 2. Solved Machinery Doctrine

### 2.1 What counts as solved machinery

Solved machinery is infrastructure whose mechanism is commodity and not unique to Minilab’s institutional purpose.

Examples:

```txt
HTTP routing
async runtime
serialization
Postgres client
HTTP client
WebSocket transport
WebAuthn/passkey protocol implementation
CLI parsing
config loading
schema validation
OpenAPI generation
logging/tracing transport
provider SDK calls
```

These problems are not where Minilab should spend its originality.

### 2.2 What does not count as solved machinery

These are Minilab-owned meaning surfaces:

```txt
LogLine canon
constitutional hierarchy
Authority Core
admission semantics
policy class meaning
capability boundaries
evidence discipline
receipt semantics
projection law
App Park membership
Intelligence App authority limits
Dan consequence signature
```

Do not outsource these to commodity SDKs.

---

## 3. SDK Boundary Law

```txt
SDKs may carry requests.
SDKs may call providers.
SDKs may serialize data.
SDKs may store projected rows.
SDKs may implement commodity protocols.
SDKs may return observations.

SDKs may not govern.
SDKs may not define canon.
SDKs may not authorize.
SDKs may not close evidence.
SDKs may not turn provider success into truth.
SDKs may not replace receipts.
SDKs may not mutate semantic state outside the admitted spine.
```

The correct path is:

```txt
CommandEnvelope
→ constitutional-runtime admission / planning
→ Minilab seam
→ SDK / provider / runtime call
→ observation
→ evidence record
→ Foundation-conformant LogLine receipt when closure is allowed
→ projection
```

Forbidden path:

```txt
SDK success
→ truth
```

---

## 4. Candidate Classes

### 4.1 P0 — first seam commodities

Use only the smallest set needed for `gates.decide`:

```txt
tokio
axum
serde
serde_json
```

Purpose:

```txt
host one JSON endpoint
parse one request
return one response
prove HTTP transport is not authority
```

### 4.2 P1 — later backend commodities

Evaluate and pin only after the first seam works:

```txt
tower / tower-http
tracing / tracing-subscriber
clap
reqwest
sqlx or PostgREST/reqwest path
webauthn-rs
jsonschema
tokio-tungstenite
figment or config
utoipa or aide
```

### 4.3 P2 — research-only / pressure-driven commodities

Do not adopt until a real pressure exists:

```txt
cedar-policy
object_store or opendal
NATS / Redis clients
OpenTelemetry stack
async-nats
```

P2 candidates require special doctrine review before adoption.

---

## 5. Cedar Demotion Rule

`cedar-policy` is not P0 or ordinary P1.

It touches policy meaning, not just commodity machinery.

Therefore:

```txt
Cedar is research-only until proven subordinate.
```

Allowed future role:

```txt
doctrine invariant
→ doctrine IR
→ generated policy data
→ CRT-governed policy seam
→ optional Cedar evaluation
→ comparison with CRT-native decision
→ mismatch becomes ghost/failure
```

Forbidden role:

```txt
Cedar decision
→ Minilab authority truth
```

A future Cedar probe must prove:

```txt
same generated policy fixtures
→ CRT-native evaluator
→ Cedar evaluator
→ same decision
→ same explanation class
→ no divergence
```

Until then, Cedar remains:

```txt
status: evaluate, do not adopt
class: P2
risk: second policy brain
```

---

## 6. Bus / Realtime / Queue Rule

Any bus-like SDK is notification transport unless explicitly admitted otherwise.

If NATS, Redis, Supabase Realtime, WebSocket transport, or another bus appears, default rule is:

```txt
Postgres/Supabase durable rows are coordination/projection.
Bus/realtime/websocket is notification or live transport.
Workers execute.
Receipts close.
```

Forbidden:

```txt
bus message as canonical truth
websocket message as durable command
realtime payload as only copy of event
queue ack as receipt
provider delivery success as closure
```

---

## 7. Solved Machinery Adoption Pipeline

The full pipeline exists, but it should not be built before the first seam breathes.

Target pipeline:

```txt
1. Problem map
2. Candidate research
3. Candidate decision
4. Version pin
5. Download/vendor/import
6. Inspect
7. Slice allowed/denied surface
8. Wrap behind Minilab adapter
9. Probe
10. Narrow receipt
```

But bootstrap sequence is:

```txt
first seam
→ second SDK pressure
→ third SDK pressure
→ then build third_party registry/vendor/slice machinery
```

Do not create a dependency-governance cathedral before the first endpoint proves the seam.

---

## 8. Future Third-Party Layout

Only after pressure exists, create:

```txt
third_party/
  registry/
    sdk-problem-map.yaml
    sdk-candidates.yaml
    sdk-decisions.yaml
    sdk-import-receipts/
    sdk-audit-reports/

  audit/
    cargo-metadata.json
    cargo-tree-features.txt
    cargo-tree-build.txt

  vendor/
    rust/

  slices/
    axum/
      allowed_surface.md
      denied_surface.md
      adapter_contract.md
    tokio/
    serde/
    sqlx/
    webauthn-rs/
```

Rules:

```txt
Vendor/imported code is not edited.
Patch only with manifest.
Directory/vendor source is treated as imported machinery.
A slice describes allowed use; it does not invent authority.
```

---

# 9. First Seam: `POST /api/gates/decide`

## 9.0 Why this seam comes first

`gates.decide` is the smallest useful backend seam because it does not need:

```txt
Dispatcher
provider mutation
LAB execution
Supabase write
Doppler secret values
receipt emission
App Park deployment
Intelligence worker
```

It proves only:

```txt
HTTP handler
→ request parsing
→ proposed act / command envelope surface
→ admission seam
→ AuthorityResponse-like result
→ no world-touch
```

This is enough to validate SDK use as transport without confusing it with authority.

## 9.1 Claim

```txt
A minimal axum/tokio/serde endpoint can receive a gate-decision request,
call a mocked constitutional-runtime admission seam,
and return an AuthorityResponse-like JSON without touching the world.
```

## 9.2 Scope

In scope:

```txt
one route
one request shape
one response shape
mock admission seam
status mapping
JSON parse / serialize
negative malformed JSON test
assert no Dispatcher invoked
assert no DB write path exists
assert no receipt closure claim appears
```

Out of scope:

```txt
real constitutional-runtime integration
real policy evaluation
real Foundation receipt validation
real evidence store
real Supabase projection
real Doppler config
real LAB execution
real App Park admission
real Intelligence classification
```

## 9.3 Endpoint

```txt
POST /api/gates/decide
```

## 9.4 Request Draft

```json
{
  "request_version": "minilab.gates_decide_request.v0",
  "correlation_id": "corr_demo_001",
  "principal": {
    "id": "dan",
    "kind": "human"
  },
  "auth_context": {
    "mode": "dry_run",
    "environment": "dev_local",
    "current_lab": "none",
    "safe_mode": true
  },
  "action": {
    "kind": "registry.entity.new",
    "command_class": "normal",
    "risk_class": "low"
  },
  "resource": {
    "ref": "registry://entity/example"
  },
  "scope": {
    "dry_run": true,
    "world_touch": false
  }
}
```

## 9.5 Response Draft

```json
{
  "response_version": "minilab.authority_response.v0",
  "correlation_id": "corr_demo_001",
  "status": "ok",
  "decision": {
    "decision_status": "allowed",
    "reason": "Mock admission allowed dry-run normal command."
  },
  "gate": {
    "status": "ok",
    "world_touch": false
  },
  "execution_window": null,
  "evidence_refs": [],
  "receipt_refs": [],
  "ghosts": [],
  "safe_message": "Request is allowed for dry-run planning only. No execution occurred.",
  "runtime_claims": {
    "dispatcher_invoked": false,
    "database_written": false,
    "provider_called": false,
    "receipt_emitted": false
  }
}
```

## 9.6 Status Mapping

The mocked admission seam may return:

```txt
ok
denied
needs_approval
ghost
error
```

Mapping:

```txt
ok:
  dry-run request admitted for planning only

denied:
  request not allowed under mock policy

needs_approval:
  protected or consequential request requires Dan/higher approval

ghost:
  required context missing

error:
  seam failed to evaluate
```

No status implies execution.

## 9.7 Negative Cases

The first seam must reject or ghost:

```txt
missing principal
missing action
mode=apply
world_touch=true
command_class=protected
risk_class=high without approval
request containing secret-looking values
request attempting direct_row_edit
request claiming receipt_emitted=true
```

## 9.8 Forbidden Strings

The response body must not contain ungrounded closure language:

```txt
done
verified
production ready
works
safe
confirmed
executed
receipt closed
```

Allowed:

```txt
allowed for dry-run planning only
no execution occurred
mock admission only
runtime unverified
```

---

# 10. Minimal Rust Sketch

This is a shape sketch, not implementation truth.

```rust
use axum::{routing::post, Json, Router};
use serde::{Deserialize, Serialize};
use std::net::SocketAddr;

#[derive(Debug, Deserialize)]
struct GatesDecideRequest {
    request_version: String,
    correlation_id: String,
    principal: PrincipalRef,
    auth_context: AuthContext,
    action: ActionRequest,
    resource: ResourceRef,
    scope: CommandScope,
}

#[derive(Debug, Deserialize)]
struct PrincipalRef {
    id: String,
    kind: String,
}

#[derive(Debug, Deserialize)]
struct AuthContext {
    mode: String,
    environment: String,
    current_lab: String,
    safe_mode: bool,
}

#[derive(Debug, Deserialize)]
struct ActionRequest {
    kind: String,
    command_class: String,
    risk_class: String,
}

#[derive(Debug, Deserialize)]
struct ResourceRef {
    r#ref: String,
}

#[derive(Debug, Deserialize)]
struct CommandScope {
    dry_run: bool,
    world_touch: bool,
}

#[derive(Debug, Serialize)]
struct AuthorityResponse {
    response_version: &'static str,
    correlation_id: String,
    status: String,
    decision: PolicyDecisionView,
    gate: GateDecisionView,
    execution_window: Option<serde_json::Value>,
    evidence_refs: Vec<String>,
    receipt_refs: Vec<String>,
    ghosts: Vec<GhostView>,
    safe_message: String,
    runtime_claims: RuntimeClaims,
}

#[derive(Debug, Serialize)]
struct PolicyDecisionView {
    decision_status: String,
    reason: String,
}

#[derive(Debug, Serialize)]
struct GateDecisionView {
    status: String,
    world_touch: bool,
}

#[derive(Debug, Serialize)]
struct GhostView {
    what_missing: String,
    why_it_matters: String,
    next_probe: String,
}

#[derive(Debug, Serialize)]
struct RuntimeClaims {
    dispatcher_invoked: bool,
    database_written: bool,
    provider_called: bool,
    receipt_emitted: bool,
}

async fn gates_decide(Json(req): Json<GatesDecideRequest>) -> Json<AuthorityResponse> {
    let response = mock_admission(req);
    Json(response)
}

fn mock_admission(req: GatesDecideRequest) -> AuthorityResponse {
    let mut ghosts = Vec::new();

    if req.auth_context.mode != "dry_run" || !req.scope.dry_run || req.scope.world_touch {
        ghosts.push(GhostView {
            what_missing: "dry-run-only boundary".to_string(),
            why_it_matters: "First seam cannot touch the world.".to_string(),
            next_probe: "Re-submit with mode=dry_run and world_touch=false.".to_string(),
        });
    }

    if req.action.command_class == "protected" || req.action.risk_class == "high" || req.action.risk_class == "critical" {
        ghosts.push(GhostView {
            what_missing: "approval/execution-window path".to_string(),
            why_it_matters: "Protected commands require the real authority path, not the mock seam.".to_string(),
            next_probe: "Route this later through real CRT admission with ExecutionWindow rules.".to_string(),
        });
    }

    let status = if ghosts.is_empty() { "ok" } else { "ghost" };

    AuthorityResponse {
        response_version: "minilab.authority_response.v0",
        correlation_id: req.correlation_id,
        status: status.to_string(),
        decision: PolicyDecisionView {
            decision_status: if ghosts.is_empty() { "allowed" } else { "ghosted" }.to_string(),
            reason: if ghosts.is_empty() {
                "Mock admission allowed dry-run normal command.".to_string()
            } else {
                "Mock admission found missing requirements.".to_string()
            },
        },
        gate: GateDecisionView {
            status: status.to_string(),
            world_touch: false,
        },
        execution_window: None,
        evidence_refs: vec![],
        receipt_refs: vec![],
        ghosts,
        safe_message: if status == "ok" {
            "Request is allowed for dry-run planning only. No execution occurred.".to_string()
        } else {
            "Request is ghosted. No execution occurred.".to_string()
        },
        runtime_claims: RuntimeClaims {
            dispatcher_invoked: false,
            database_written: false,
            provider_called: false,
            receipt_emitted: false,
        },
    }
}

#[tokio::main]
async fn main() {
    let app = Router::new().route("/api/gates/decide", post(gates_decide));
    let addr = SocketAddr::from(([127, 0, 0, 1], 3000));
    axum::serve(tokio::net::TcpListener::bind(addr).await.unwrap(), app)
        .await
        .unwrap();
}
```

## 10.1 Why this sketch is not enough

This sketch is not completion.

It lacks:

```txt
real CRT API integration
tests
schema validation
secret-like-value scanner
forbidden string scanner
cargo metadata receipt
version/license/advisory verification
real endpoint execution evidence
```

It is useful only as a seam target.

---

# 11. Probe Definition

## 11.1 Probe command candidates

```bash
cargo test -p minilab-gates-decide-seam
cargo run -p minilab-gates-decide-seam
curl -sS -X POST http://127.0.0.1:3000/api/gates/decide \
  -H 'content-type: application/json' \
  --data @fixtures/gates/decide.normal-dry-run.json
```

## 11.2 Expected evidence

```txt
server starts locally
valid dry-run request returns status=ok
protected/high-risk request returns status=ghost or needs_approval
world_touch=true returns ghost
runtime_claims.dispatcher_invoked=false
runtime_claims.database_written=false
runtime_claims.provider_called=false
runtime_claims.receipt_emitted=false
response contains no closure overclaim strings
```

## 11.3 Receipt scope, when run

A future receipt may close only:

```txt
The gates.decide seam accepted and returned mocked admission responses locally for the tested fixtures.
No Dispatcher, DB write, provider call, receipt emission, or LAB execution occurred in those fixtures.
```

It may not close:

```txt
real CRT integration
real policy correctness
production readiness
App Park admission
Intelligence App behavior
Foundation receipt conformance
```

---

# 12. PR-000.6 Acceptance Criteria

This PR is accepted when it adds:

```txt
docs/solved-machinery-doctrine.md
docs/gates-decide-first-seam.md
minimal seam crate or fixture plan
first request/response fixtures
negative cases
artifact honesty receipt
```

And does not add:

```txt
full third_party registry
vendor tree
runtime mutation
Dispatcher implementation
Supabase writes
Doppler secret reads
real provider calls
App Park deploy
Intelligence worker loop
receipt closure claim
```

---

# 13. Follow-up Sequence

After PR-000.6:

```txt
1. Run the seam probe.
2. Capture evidence.
3. Emit narrow receipt if the receipt path exists; otherwise record evidence and ghost receipt closure.
4. Verify current versions/licenses/advisories for P0/P1 SDKs.
5. Create SDK candidate/decision records.
6. Add third_party registry only after at least two SDK seams justify it.
7. Re-cut Layer 4 Ops Backend Blueprint against build-equation and solved-machinery doctrine.
8. Return to App Park PR-001.
```

---

# Artifact Honesty Receipt

Intent Coverage:

```txt
Created the doctrine and first seam definition for adopting open-source SDKs as solved machinery without letting SDKs become authority.
Defined gates.decide as the first tiny proof seam.
Demoted Cedar to research-only until proven subordinate to CRT.
Preserved no-world-touch boundaries.
```

Implemented:

```txt
Solved Machinery Doctrine draft.
P0/P1/P2 SDK categorization.
Cedar demotion rule.
Bus/realtime/queue rule.
SDK adoption pipeline.
Future third_party layout.
gates.decide seam definition.
Request/response drafts.
Minimal Rust sketch.
Probe definition.
PR-000.6 acceptance criteria.
```

Changed:

```txt
No repo, runtime, database, provider, Doppler, LAB, route, or app state changed.
Canvas document only.
```

Stubbed:

```txt
Rust sketch is not compiled.
Endpoint is not run.
CRT seam is mocked.
No SDK versions are pinned.
No dependency advisories checked.
No cargo metadata collected.
No receipt emitted.
```

Unverified:

```txt
P0 crate versions.
P0 licenses/advisories.
Exact CRT API integration.
Target repo package layout.
Whether axum/tokio/serde compile in the eventual repo.
```

Probes:

```txt
None run in this turn.
Probe proposed only.
```

Ghosts:

```txt
SDK version verification ghost.
Real CRT integration ghost.
Endpoint execution ghost.
Receipt path ghost.
Target repo ghost.
```

Desk Clean:

```txt
status: yes
next: implement or hand this PR-000.6 seam to Codex/Claude in the target repo.
```

