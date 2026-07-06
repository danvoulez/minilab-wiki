# 04 — Evidence / Receipt Doctrine

**Status:** Canon draft  
**Layer:** Minilab proof, evidence, receipt, ghost, and closure doctrine  
**Parent:** 01 — Minilab Constitution / Strategic Architecture; 03 — Authority / Power Doctrine  
**Foundation dependency:** LogLine Foundation receipt canon and conformance suite  
**Scope:** Evidence records, receipts, reports, ghosts, proof boundaries, closure vocabulary, artifact honesty, conformance, and projection for `minilab.work`  
**Downstream documents:** Dan Control Plane Doctrine, Database Projection Canon, Doppler / Config Canon, App Park Doctrine, App Park Receipt Profile

---

## 0. Purpose

This document defines how Minilab distinguishes:

```txt
claim
candidate
plan
execution
observation
evidence
result
transport
receipt
report
ghost
closure
```

It exists to prevent narrative from becoming proof, provider success from becoming verification, logs from becoming receipts, reports from becoming closure, and broad claims from narrow evidence.

The central rule:

```txt
Receipts beat stories.
```

A receipt is not a story. It is scoped proof closure over a declared act.

Evidence is not closure by itself. Evidence supports closure.

Reports are not receipts. Reports synthesize evidence for decision.

Ghosts are not failure. Ghosts preserve missing proof.

---

## 1. Parent Canon

Minilab does not define the universal receipt canon.

Minilab depends on the LogLine Foundation receipt canon and conformance layer.

Therefore:

```txt
Minilab receipts must conform to the Foundation receipt profile in force.
App-specific or platform-specific data may live in domain auxiliary fields.
Evidence, result, and transport must remain outside the core receipt shape unless the parent canon explicitly permits otherwise.
Receipt semantics may not be changed locally.
```

If Minilab or App Park needs to change receipt semantics, hash semantics, slot grammar, or conformance behavior, that is a Foundation governance issue, not a local Minilab shortcut.

---

## 2. Core Proof Law

```txt
Claim is not evidence.
Plan is not execution.
Execution is not success.
Provider success is not closure.
Observation is not verification.
Report is not receipt.
Receipt closes only declared scope.
```

Expanded form:

```txt
generated ≠ run
run ≠ passed
passed ≠ reviewed
reviewed ≠ production-observed
provider success ≠ verified
verified locally ≠ production-safe
receipt ≠ signature
signature ≠ evidence
```

Every proof claim must state its scope.

A receipt may close:

```txt
this command ran on this machine at this time and produced this observed output
```

It may not automatically close:

```txt
the whole system works
this is production safe
no regressions exist
all users are protected
future operations will succeed
```

---

## 3. Evidence Vocabulary

### 3.1 Claim

A testable statement.

Example:

```txt
LAB-512 host runtime health endpoint returns ok.
```

A claim is not evidence.

### 3.2 Candidate

A proposed act or interpretation before admission.

Example:

```txt
Register this service as an App Park member.
```

A candidate is not execution.

### 3.3 Plan

A proposed route from intent to effect.

Example:

```txt
Deploy app using Docker Compose, create route, run health probe, emit receipt.
```

A plan is not proof that the effect happened.

### 3.4 Probe

A scoped contact with reality.

Examples:

```txt
HTTP health request
SQL query
shell command
container inspection
route check
webhook replay in dry-run
migration dry-run
backup restore-check
```

A probe must be small enough to be meaningful and specific enough to change the answer.

### 3.5 Observation

Raw or structured output from a probe/runtime/provider.

Examples:

```txt
HTTP 200 with body
exit code 0
stdout/stderr
SQL result rows
Docker container state
launchctl service state
Supabase RLS query result
Doppler key presence check
```

Observation is evidence input, not closure.

### 3.6 Evidence Record

A persisted, referenceable record of observation.

Evidence records may include:

```txt
probe kind
command/request/query
runtime
started_at
finished_at
exit/status code
redacted stdout/stderr
hashes
artifact references
secret redaction flag
scope
```

Evidence records must be content-addressable or otherwise stably referenceable.

### 3.7 Result

The interpreted operational outcome of execution.

Examples:

```txt
health ok
deploy failed
migration blocked
route reachable
secret missing
rollback succeeded within scope
```

Result is not the same as raw evidence. Result is derived from evidence and policy.

### 3.8 Transport

The envelope or carrier through which content moved.

Examples:

```txt
HTTP request envelope
MCP call envelope
Supabase Realtime notification
webhook transport
CLI invocation wrapper
Cloudflare edge request
```

Transport is not the core receipt.

### 3.9 Receipt

A Foundation-conformant scoped closure over a declared act.

A receipt says:

```txt
Within this declared scope, this act has this closure state, supported by these references.
```

A receipt does not contain unlimited proof. It closes only what it names.

### 3.10 Report

A structured synthesis for decision.

Reports may cite evidence and receipts. Reports may produce recommendations, missing proof, or decision requests.

Reports do not close proof.

### 3.11 Ghost

A structured absence.

A ghost records the missing proof, context, authority, runtime, signature, config, secret, schema, evidence path, or receipt path that prevents honest closure.

---

## 4. Receipt Shape Discipline

Minilab receipts must follow the parent Foundation receipt canon.

The canonical slot layer is:

```txt
who
did
this
when
confirmed_by
if_ok
if_doubt
if_not
status
```

A receipt may include domain-specific auxiliary fields when allowed by the Foundation profile and conformance rules.

Domain auxiliary fields may describe Minilab/App Park-specific data such as:

```txt
app_id
deployment_id
manifest_hash
runtime_id
host_id
route_ref
probe_ref
evidence_refs
policy_decision_ref
gate_decision_ref
execution_window_ref
```

But receipt discipline requires:

```txt
Evidence remains separate.
Result remains separate unless represented as status/aux under canon rules.
Transport remains separate.
Secret values never appear.
Broad claims never appear without scoped support.
```

---

## 5. Hash and Conformance Discipline

Receipts must be canonicalized and hashed according to the parent canon in force.

Minilab must preserve these boundaries:

```txt
tuple hash
  hash over the canonical act tuple / slot content as defined by Foundation

content hash
  hash over receipt content as defined by Foundation

envelope hash
  hash over transport/envelope material when applicable, outside core receipt semantics
```

Rules:

```txt
Do not invent local hash semantics.
Do not include forbidden fields in core receipt.
Do not mutate historical receipt content silently.
Do not claim conformance without running the conformance check.
Do not use transport envelope hash as replacement for receipt content hash.
```

A receipt is not valid merely because it looks right. It must pass conformance for the claimed profile.

---

## 6. Evidence Records

Evidence records are separate from receipts.

Conceptual shape:

```ts
type EvidenceRecord = {
  evidence_id: string;
  evidence_version: "minilab.evidence.v0";
  correlation_id: string;
  produced_by: string;
  runtime_ref?: string;
  probe_ref?: string;
  observed_at: string;

  kind:
    | "command_output"
    | "http_response"
    | "sql_result"
    | "file_hash"
    | "container_state"
    | "runtime_health"
    | "webhook_delivery"
    | "work_item_attempt"
    | "screenshot"
    | "manual_observation"
    | "other";

  scope: string;
  status: "observed" | "failed" | "inconclusive" | "redacted";
  summary: string;

  payload_ref?: string;
  stdout_ref?: string;
  stderr_ref?: string;
  artifact_refs?: string[];
  secret_redacted: true;
  hashes?: Record<string, string>;
  metadata?: Record<string, unknown>;
};
```

Evidence payloads may be large, sensitive, or provider-specific. Store them as referenced artifacts, not as uncontrolled receipt blobs.

---

## 7. Receipt Profiles in Minilab

Minilab may define receipt profiles that specify how domains use the Foundation receipt canon.

Examples:

```txt
minilab.command.receipt_profile.v1
minilab.health.receipt_profile.v1
minilab.config.receipt_profile.v1
minilab.database.receipt_profile.v1
app_park.deploy.receipt_profile.v1
app_park.webhook.receipt_profile.v1
app_park.work_attempt.receipt_profile.v1
```

A receipt profile may define:

```txt
required did values
allowed auxiliary fields
required evidence references
required policy/gate refs
required runtime refs
required redaction flags
projection target
conformance check
```

A receipt profile may not redefine:

```txt
Foundation receipt version
hash semantics
slot names
canonicalization rules
forbidden fields
closure semantics
```

---

## 8. Closure States

Minilab uses explicit closure states.

```txt
draft
candidate
observed
inconclusive
ghosted
rejected
closed
superseded
retired
```

### draft

Language or plan exists, but not yet shaped as an admissible act.

### candidate

A proposed act exists and can be evaluated.

### observed

Runtime or tool output exists, but closure has not been granted.

### inconclusive

Evidence exists but does not decide the claim.

### ghosted

Closure is blocked by structured absence.

### rejected

The claim or act was denied or contradicted.

### closed

A scoped receipt closes the declared act.

### superseded

A later receipt/act replaces this state for future interpretation while preserving history.

### retired

No longer active, but historically preserved.

---

## 9. Evidence Status

For UI, Registry, Reports, and Inspector, use evidence status carefully.

```txt
declared
observed
verified
unverified
```

### declared

A plan, manifest, doctrine, or registry seed says the thing exists or should exist.

### observed

Runtime evidence has been seen, but closure is incomplete.

### verified

A scoped receipt or conformance proof supports the claim.

### unverified

No adequate evidence/receipt exists.

Rule:

```txt
Only scoped receipts, conformance results, or explicit runtime probes may upgrade evidenceStatus.
```

---

## 10. What Receipts May Close

Receipts may close narrow claims such as:

```txt
this command ran and produced this output
this health endpoint returned this response at this time
this manifest validated under this schema version
this route was reachable from this runtime
this config doctor observed these key names and brakes without secret values
this database migration dry-run produced this plan
this webhook delivery received this provider response
this backup snapshot was created
this restore-check probe succeeded for this backup
```

Receipts may not automatically close broad claims such as:

```txt
the system is safe
the app works
the deployment is production-ready
all secrets are secure
all policies are correct
no data loss is possible
all users can access correctly
future execution will succeed
```

Broad claims require multiple receipts, explicit scope, and often human review/signature.

---

## 11. Reports Versus Receipts

Reports synthesize. Receipts close.

A report may:

```txt
summarize evidence
compare sources
identify drift
recommend action
request decision
list missing proof
create work candidates
produce review packets
```

A report may not:

```txt
close evidence
mark verified by narrative
replace receipt
replace policy decision
replace Dan approval
mutate database/provider state directly
```

Report confidence is not proof.

Report labels:

```txt
confidence: insufficient | low | medium | high
```

Receipt labels:

```txt
status: closed | ghosted | rejected | superseded | retired
```

These labels must not be confused.

---

## 12. Provider Success and Evidence

Provider success is evidence input.

Examples:

```txt
HTTP 200
Docker compose returned 0
Supabase query succeeded
Cloudflare route API returned success
Doppler key is present
GitHub API returned created
LLM returned answer
```

These are observations.

They do not prove:

```txt
semantic correctness
policy compliance
world-fit
user impact
future safety
production readiness
```

Provider success may support a receipt only when the receipt scope is narrow and the evidence requirements match the claim.

---

## 13. Artifact Honesty

Every meaningful generated or modified artifact must report honestly.

Artifact honesty must include:

```txt
Intent Coverage
Implemented
Changed
Stubbed
Unverified
Probes
Ghosts
Desk Clean
Next
```

### 13.1 Intent Coverage

What part of the human’s actual goal is covered.

### 13.2 Implemented

Real load-bearing behavior added.

### 13.3 Changed

Files, modules, schemas, APIs, configs, prompts, docs, or runtime settings modified.

### 13.4 Stubbed

Placeholders, mocks, hardcoded returns, fake providers, empty files, TODOs, demo-only behavior.

### 13.5 Unverified

Paths, integrations, permissions, edge cases, data shapes, runtime behavior, or world-fit not exercised.

### 13.6 Probes

Commands, tests, queries, HTTP requests, screenshots, inspections, or conformance checks actually run.

### 13.7 Ghosts

Missing context, runtime, integration, authority, receipt, signer, or evidence.

### 13.8 Desk Clean

Enough handoff state for the next human/model/runtime to continue without pretending continuity.

### 13.9 Next

Smallest executable step to collapse the largest remaining ghost.

---

## 14. Ghost Doctrine

A Ghost is structured absence.

Ghosts appear when closure would be dishonest due to missing:

```txt
runtime contact
evidence
context
schema
secret
policy
authority
signer
scope
receipt path
review
world-fit
```

Ghosts must include:

```txt
what is missing
why it matters
what cannot honestly be concluded
what could be harmed by pretending
smallest next probe/input/signature
```

Ghosts are not defects in the system. They are part of the system’s honesty.

Forbidden:

```txt
ghost → silent OK
ghost → green status
ghost → hidden warning
ghost → TODO presented as done
ghost → report confidence
```

---

## 15. Evidence Redaction

Evidence must not leak secrets.

Forbidden in evidence, receipts, reports, logs, UI payloads, and database values:

```txt
secret values
provider tokens
service_role keys
API keys
private credentials
raw auth headers
unredacted env dumps
```

Allowed:

```txt
secret key name
secret class
presence/absence
config scope
redacted hash when safe
Doppler config name
requirement name
```

Examples:

Allowed:

```txt
SUPABASE_SERVICE_ROLE_KEY: present
APP_SECRET: missing
DOPPLER_CONFIG: lab512
secret_values_printed: false
```

Forbidden:

```txt
SUPABASE_SERVICE_ROLE_KEY=eyJ...
Authorization: Bearer ...
DATABASE_URL=postgres://user:password@...
```

---

## 16. Database Projection of Proof

Database stores projections and indexes. It does not redefine receipts.

Proof-related database zones:

```txt
ops.logline_acts
ops.receipt_index
ops.snapshot_index
ops.expedition_index
lab_observability.events
lab_observability.receipts
lab_observability.ghosts
registry.entities
registry.links
audit views
```

Rules:

```txt
Receipts are indexed, not redefined.
Evidence records are referenced, not silently summarized into truth.
Semantic state is projected from admitted acts and receipts.
Audit views are projections, not canon.
Reports may cite receipt/evidence refs.
```

No secret values in database.

---

## 17. UI Projection of Proof

The UI must use proof language carefully.

Allowed UI labels:

```txt
Generated
Observed
Tested locally
Benchmark captured
Report generated
Needs review
Closed with receipt
Missing proof
Unknown
Blocked
```

Forbidden without scoped receipt/proof:

```txt
Done
Verified
Production ready
Works
Safe
Confirmed
```

The UI may show `verified` only when the object has scoped receipt/conformance support for the specific claim being displayed.

The Inspector should prioritize:

```txt
Decision context
Evidence
Relations
Reports
Trace
Raw JSON
```

Raw payloads should not be the primary proof surface.

---

## 18. Code Lab Evidence

Code Lab must not collapse generated artifacts into working software.

Labels:

```txt
code artifact generated
command observed
test run observed
benchmark captured
preview observed
missing proof
review required
closed with receipt
```

Forbidden shortcuts:

```txt
generated file → implemented
command exit 0 → behavior proven
test exists → intent tested
benchmark ran → quality proven
preview loads → production ready
LLM review → verified
```

A Code Lab experiment may close only when:

```txt
intent is preserved
behavior is exercised under meaningful probes
stubs are declared
unverified paths are explicit
evidence refs exist
receipt scope is narrow
```

---

## 19. App Park Evidence Extension

App Park must use this doctrine.

App Park operation examples:

```txt
app.admit
app.deploy
app.route.create
app.health.probe
app.secret_readiness.check
app.database.migrate
app.webhook.receive
app.webhook.dispatch
app.work_item.claim
app.work_item.complete
app.backup.create
app.restore_check.run
app.rollback
app.suspend
```

Each operation should produce:

```txt
plan
policy/gate decision refs
execution window ref if required
evidence records
Foundation-conformant receipt or ghost
projection update
```

App Park must not invent a parallel receipt canon.

---

## 20. Conformance Requirements

Minilab must treat conformance as a runtime/probe requirement where applicable.

Receipt emission pipeline:

```txt
construct receipt content
canonicalize according to Foundation
compute required hashes
validate against conformance suite/schema
store or index receipt
project receipt reference
```

If conformance fails:

```txt
do not index as valid receipt
record ghost or rejection
preserve failure evidence
request repair
```

Conformance passing means the receipt obeys the claimed mold. It does not prove the world claim beyond the evidence and scope inside the receipt.

---

## 21. Minimum Machine-Readable Contracts

This doctrine requires machine-readable definitions for:

```txt
EvidenceRecord
ReceiptRef
EvidenceRef
Ghost
Probe
ReceiptProfile
ArtifactHonestyReceipt
ReportFinding
ClosureState
EvidenceStatus
RedactionPolicy
ConformanceResult
```

These feed:

```txt
Authority Core
Dan Control Plane
Database Projection
App Park
Code Lab
Reports
Registry
Inspector
```

---

## 22. Probe Quality Rules

A good probe is:

```txt
specific
scoped
observable
replayable
safe
non-tautological
connected to the claim
capable of changing the answer
```

Bad probes include:

```txt
assert true
function exists only
mock-only path for integration claim
LLM explanation instead of runtime contact
broad test suite with no claim mapping
manual glance with no record
provider 200 for semantic correctness
```

Probe selection should prefer the smallest meaningful probe:

```txt
single deterministic command
→ targeted unit test
→ targeted integration test
→ focused API/database check
→ full suite
→ manual inspection
```

---

## 23. Closure Examples

### 23.1 Valid Narrow Closure

Claim:

```txt
LAB-512 Mistral health endpoint returned OK at 2026-05-21T18:41Z.
```

Evidence:

```txt
curl http://127.0.0.1:1234/health returned OK
```

Receipt may close:

```txt
Mistral health endpoint was observed OK at that time from that host.
```

Receipt may not close:

```txt
all inference is production ready
all models are correct
LLM gateway routing is safe
```

### 23.2 Invalid Broad Closure

Claim:

```txt
App Park is ready.
```

Evidence:

```txt
Docker Compose deploy returned 0 for one app.
```

Correct state:

```txt
observed deployment success for one app under one provider
broader readiness unverified
ghosts remain
```

### 23.3 Ghost Closure

Claim:

```txt
Supabase security baseline is safe.
```

Missing:

```txt
grants audit
RLS audit
browser path audit
service role leak check
```

Correct state:

```txt
ghosted: security baseline cannot be closed without audits
```

---

## 24. Testing Requirements

Evidence/receipt discipline requires tests for:

```txt
receipt conformance passes for valid receipts
invalid receipts are rejected
forbidden fields are rejected
secret values are redacted before receipt/log/db/report
provider success does not auto-mark verified
report cannot become receipt
Code Lab command output is evidence, not done
UI cannot display verified without scoped receipt
missing proof becomes Ghost
receipt index does not redefine receipt content
app deploy cannot close without health probe when required
backup cannot close without restore-check when required
```

---

## 25. Anti-Patterns

Reject designs where:

```txt
receipt contains raw provider payload as proof without reference boundary
receipt contains secret value
receipt includes transport as if it were core content when canon forbids it
report marks state verified
LLM says tests passed without runtime output
provider success closes semantic claim
health process exists means service works
mock success is presented as production behavior
generated code is marked implemented without probe
wide claim is closed by narrow evidence
receipt profile redefines Foundation hash semantics
Supabase row is treated as proof without receipt/evidence
logs are treated as receipt
screenshot is treated as full verification
```

---

## 26. Amendment Rule

Changes to this doctrine require an Evidence / Receipt Amendment.

```txt
Evidence / Receipt Amendment

Proposed change:
Why current doctrine fails:
Which proof boundary is affected:
Does it touch Foundation canon/conformance? yes/no
Which receipt profile changes:
Which evidence records change:
Which projections change:
Which risks increase:
Which tests/conformance probes are required:
Dan approval required: yes/no
Foundation governance required: yes/no
```

Foundation governance is required if the change affects receipt canon, hash semantics, canonicalization, slot grammar, or conformance behavior.

---

## 27. Current Honest State

```txt
State:
  Canon draft.

Grounded by:
  Minilab Constitution, Authority / Power Doctrine, Database Projection Canon, Doppler / Config Canon, Dan Control Plane Doctrine, and inspected LogLine Foundation canon/conformance structure.

Verified:
  The doctrine defines intended proof boundaries.

Unverified:
  It does not prove current receipt machinery implementation.
  It does not prove Rust engine conformance.
  It does not prove current database receipt indexes.
  It does not prove current EvidenceRecord storage.
  It does not prove UI labels enforce this doctrine.

Ghosts:
  Need machine-readable EvidenceRecord schema.
  Need machine-readable ReceiptProfile schemas.
  Need conformance integration in receipt emitter.
  Need database projection receipt path.
  Need UI evidence-label tests.
```

---

## 28. Canonical Closing

```txt
Evidence is contact with reality.
Reports interpret evidence.
Receipts close scoped acts.
Ghosts preserve missing proof.
Conformance checks the mold.
Authority controls effect.
Dan signs consequence.

Stories do not close.
Provider success does not close.
Confidence does not close.
Only scoped receipts close — and only within scope.
```

