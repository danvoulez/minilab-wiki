# 03 — Authority / Power Doctrine

**Status:** Canon draft  
**Layer:** Minilab authority and effect boundary  
**Parent:** 01 — Minilab Constitution / Strategic Architecture; 02 — Characters / Ontology Canon  
**Scope:** Authority, admission, gates, policy, protected power, command envelopes, execution windows, visas, passkeys, decisions, enforcement, and ghost behavior for `minilab.work`  
**Downstream documents:** Evidence / Receipt Doctrine, Dan Control Plane Doctrine, Database Projection Canon, Doppler / Config Canon, App Park Doctrine

---

## 0. Purpose

This document defines how Minilab decides whether an intended effect may happen.

It exists to prevent ontology from becoming permission, UI from becoming dispatch, natural language from becoming execution, provider success from becoming proof, and machine identity from becoming authority.

Authority is the discipline that sits between:

```txt
intent
candidate
request
policy decision
gate decision
execution window
runtime effect
evidence
receipt / ghost
```

The core job of this doctrine is to keep every one of those states separate.

---

## 1. Authority Law

```txt
Identity is not authority.
Role is not permission.
Permission is not execution.
Execution is not closure.
Closure requires receipt.
```

A system may know an entity exists and still deny it action.
A role may describe what an entity is suited for and still grant no permission.
A policy may allow a request and still require a gate.
A gate may approve a request and still require an execution window.
An execution may run and still fail.
A provider may return success and still not close the institutional claim.

Authority exists to protect this separation.

---

## 2. Parent Hierarchy

Authority follows the constitutional hierarchy:

```txt
LogLine Foundation
  defines grammar/canon/conformance

Constitutional Runtime
  governs semantic admissibility, IR, policy, capability, evidence requirements

Tower / minilab.work
  governs operational power and protected execution

Authority Core
  write kernel / command admission interface for the product

LABs / Host Runtimes
  execute admitted scoped work

Supabase/Postgres
  projects acts, receipts, ghosts, decisions, and current state

Doppler
  supplies material after validation; does not authorize

Dan
  signs consequence where human authority is required
```

No lower layer may bypass or impersonate a higher authority.

---

## 3. Authority Vocabulary

### 3.1 Principal

The actor making or carrying a request.

Examples:

```txt
dan
chatgpt
local_llm
control_plane
host_runtime
lab_runner
app_park_member
scheduled_maintenance_agent
```

A principal is identity plus context. It is not automatically permission.

### 3.2 Auth Context

The current authority context around the principal.

Includes:

```txt
session
mode
environment
current LAB
current policy profile
human approval state
passkey state
brake flags
Doppler config scope
runtime availability
```

### 3.3 Action Request

A proposed effect.

Examples:

```txt
registry.entity.new
policy.rule.update
gate.rule.update
runtime.register
release.installation.record
lab.event.ingest
code.command.run
app.deploy
app.rollback
webhook.replay
secret.requirement.register
```

An action request is not execution.

### 3.4 Resource

The thing affected or queried.

Examples:

```txt
registry://entity/lab512
runtime://host/lab512
config://requirement/SUPABASE_SERVICE_ROLE_KEY
app://minicontratos-admin
route://minicontratos.apps.minilab.work
receipt://sha256:...
```

### 3.5 Policy Decision

A machine-evaluable decision derived from authority rules.

Policy answers:

```txt
allowed
denied
needs_approval
requires_gate
requires_window
requires_evidence
requires_receipt
ghost
```

Policy is not a prompt. Policy is executable rule.

### 3.6 Gate Request

A pre-effect request submitted to a gate.

A gate request packages:

```txt
principal
auth context
action
resource
scope
risk class
policy decision
evidence requirements
receipt requirements
requested execution mode
```

### 3.7 Gate Decision

A pre-effect decision.

Allowed operational surface states:

```txt
ok
denied
needs_approval
ghost
error
```

Lower-level or internal state names may include:

```txt
COMMIT
GHOST
REJECT
```

No dispatch occurs merely because a gate decision object exists. Dispatch requires the correct execution path.

### 3.8 Execution Window

A scoped, time-bounded, usually one-time permission to execute an admitted action.

Fields:

```txt
window_id
actor
action
resource
scope_hash
created_at
expires_at
consumed_at
created_by
approved_by
risk_class
receipt_required
```

An ExecutionWindow is not general approval.
It cannot silently expand its scope.
It cannot be reused after consumption.

### 3.9 Passkey Window

A human-approved protected power interval.

It proves human presence or approval for a protected command class.
It does not close evidence.
It does not authorize unrelated scope.

### 3.10 Visa

A scoped authorization for a use, passport, or capability.

A Visa may authorize a specific class of use under conditions. It is not a boolean permission and not a permanent superpower.

### 3.11 Passport

A registration receipt of recognized existence.

A Passport says a thing is known. It does not say the thing may act.

### 3.12 Ghost

A structured absence that blocks honest closure or execution.

Authority ghosts include missing:

```txt
proof
context
identity
authentication
authorization
policy
runtime
secret
receipt path
human signature
scope
evidence
```

A Ghost is not an error to hide. It is a state to expose.

---

## 4. Command Classes

All commands belong to one of two operational classes:

```txt
normal
protected
```

There is no third hidden category.

### 4.1 Normal Commands

Normal commands may be allowed through ordinary policy and context.

Examples:

```txt
read registry projection
read report
inspect receipt
list machines
view health status
run non-mutating doctor
validate manifest
plan dry-run
```

Normal does not mean ungoverned. Normal means it does not require protected power.

### 4.2 Protected Commands

Protected commands require stronger admission.

Examples:

```txt
provider mutation
database semantic write
secret requirement change
policy/gate update
protected host command
release publish
LAB job execution
public route creation
backup restore
app deploy / rollback
external webhook replay with side effect
```

Protected commands require some combination of:

```txt
policy allow
Gate decision
Dan approval
PasskeyWindow
ExecutionWindow
Doppler brake state
runtime availability
evidence path
receipt path
```

Protected commands do not exist outside Tower/Authority discipline.

---

## 5. Natural Language Boundary

```txt
Natural language never executes.
```

Natural language may produce:

```txt
intent
candidate
clarification request
rejection
ghost record
workorder draft
report request
registry candidate
code lab task
review item
```

Natural language may not produce:

```txt
dispatch
provider mutation
database mutation
evidence closure
direct runtime action
protected execution
receipt closure
```

A Universal Composer, chat UI, LLM, or agent may prepare an ActionRequest. It may not bypass Authority Core.

---

## 6. Authority Core

Authority Core is the write kernel.

It is the canonical path for mutations from product surfaces into institutional effect.

Authority Core receives a `CommandEnvelope`, resolves the relevant authority context, evaluates policy/gate requirements, returns an `AuthorityResponse`, and may prepare or trigger execution only through admitted paths.

Authority Core is not the UI.
Authority Core is not a prompt.
Authority Core is not a provider SDK.
Authority Core is not Supabase.
Authority Core is not Doppler.

Authority Core may call those systems under declared scope.

---

## 7. Command Envelope

All mutation requests must be wrapped in a `CommandEnvelope`.

Conceptual shape:

```ts
type CommandEnvelope = {
  envelope_version: "minilab.command.v0";
  command_id: string;
  correlation_id: string;
  requested_at: string;

  principal: PrincipalRef;
  auth_context: AuthContext;
  action: ActionRequest;
  resource: ResourceRef;

  mode: "dry_run" | "apply";
  command_class: "normal" | "protected";
  risk_class: "low" | "medium" | "high" | "critical";

  scope: CommandScope;
  evidence_requirements: EvidenceRequirement[];
  receipt_requirements: ReceiptRequirement[];

  idempotency_key?: string;
  reason?: string;
  source_refs?: string[];
  metadata?: Record<string, unknown>;
};
```

### 7.1 Required Properties

A command envelope must identify:

```txt
who asks
what action is requested
what resource is affected
what mode is requested
whether the command is normal or protected
what scope is claimed
what evidence is required
what receipt path is expected
```

### 7.2 Forbidden Properties

A command envelope must not contain:

```txt
raw secret values
provider tokens
unbounded shell strings as authority
hidden natural language dispatch
implicit scope expansion
silent apply mode
```

---

## 8. Authority Response

Authority Core returns an `AuthorityResponse`.

Conceptual shape:

```ts
type AuthorityResponse = {
  response_version: "minilab.authority_response.v0";
  command_id: string;
  correlation_id: string;
  status:
    | "ok"
    | "denied"
    | "needs_approval"
    | "ghost"
    | "error";

  decision: PolicyDecision;
  gate?: GateDecision;
  execution_window?: ExecutionWindowRef;
  evidence_refs?: EvidenceRef[];
  receipt_refs?: ReceiptRef[];
  ghosts?: Ghost[];
  safe_message: string;
  ui_action?: UIActionHint;
  raw_trace_ref?: string;
};
```

### 8.1 Status Meanings

#### ok

The request is admitted for its declared scope. It may still require execution/probe/receipt to close.

#### denied

The request is not allowed under current authority.

#### needs_approval

The request may proceed only after human or higher authority approval.

#### ghost

The request cannot honestly proceed because required proof, context, runtime, config, secret, policy, signer, or receipt path is missing.

#### error

The authority system failed to evaluate or process the request. Error is not denial and not ghost. It requires repair.

---

## 9. Policy

Policy is executable rule derived from authority.

Policy may be implemented through a Rust policy module, Constitutional Runtime primitive, OPA/Rego, Cedar, or another approved machine-evaluable engine.

Policy may not exist only as prose or prompt.

Policy evaluates:

```txt
principal
auth context
action
resource
scope
command class
risk class
brakes
capabilities
current state
evidence requirements
receipt requirements
```

Policy returns a structured decision with reasons.

### 9.1 Policy Must Be Explainable

Every denial, approval, approval requirement, or ghost must include a reason suitable for UI display and audit.

### 9.2 Policy Must Be Revocable

Authority rules must support removal, suspension, and override through admitted processes.

### 9.3 Policy Must Not Log Secrets

Policy may check the presence or class of a secret. It may not expose secret value.

---

## 10. Gates

A Gate is the pre-effect boundary.

It decides whether a proposed act may advance toward effect.

Inputs:

```txt
CommandEnvelope
PolicyDecision
current registry state
runtime state
config/brake state
evidence availability
human approval state
```

Outputs:

```txt
ok
denied
needs_approval
ghost
error
```

### 10.1 Gate Does Not Execute

A gate admits or blocks. It does not perform the side effect by existing.

### 10.2 Gate Does Not Close

A gate decision is not a receipt. It may require a receipt after execution.

### 10.3 Gate Must Preserve Ghosts

If a required input is missing, the gate must record a Ghost. It must not silently choose OK.

---

## 11. Execution Windows

ExecutionWindow is the physics of scoped permission.

A protected command cannot run merely because the UI button exists or because a policy returned allowed.

Execution requires a window when the command class, risk class, or policy requires it.

### 11.1 Window Creation

Window creation requires:

```txt
admitted CommandEnvelope
policy/gate decision
scope hash
expiry
actor
resource
receipt requirement
```

For protected commands, it may also require:

```txt
Dan approval
PasskeyWindow
Visa
brake state check
```

### 11.2 Window Consumption

A window must be consumed when used.

Consumption records:

```txt
executor
runtime
started_at
consumed_at
correlation_id
result/evidence refs
receipt requirement state
```

### 11.3 Window Expiration

Expired windows cannot run commands. They must create a ghost or denial if attempted.

---

## 12. Dan Approval and Signature

Dan is the human authority for consequence.

Dan may:

```txt
approve
deny
defer
request more evidence
accept risk
revoke approval
sign consequence
```

Dan approval is required when policy or risk class demands it.

Examples:

```txt
protected provider mutation
database write affecting institutional state
secret/config brake change
public exposure
release publish
backup restore
privileged LAB execution
high-cost model escalation
legal/financial consequence
```

Dan approval does not create evidence. It authorizes risk or effect under scope.

Dan cannot honestly sign missing runtime truth. If evidence is missing, the correct output is Ghost or risk acceptance, not verification.

---

## 13. Doppler and Brakes in Authority

Doppler supplies material. It does not authorize.

Authority may check Doppler-derived flags, brakes, pointers, identities, windows, and provider credentials.

Examples:

```txt
MINILAB_ALLOW_DB_WRITE
MINILAB_ALLOW_LAB_EXEC
MINILAB_ALLOW_PROVIDER_MUTATION
MINILAB_ALLOW_PROTECTED_COMMANDS
MINILAB_DRY_RUN
MINILAB_SAFE_MODE
SUPABASE_ALLOW_MIGRATIONS
CLOUDFLARE_DRY_RUN
```

If a brake forbids the action, Authority must deny or ghost the request.

If a required secret is absent, Authority must ghost or block the request.

Secret values must not enter:

```txt
CommandEnvelope logs
AuthorityResponse
Receipt
Report
Database values
Browser payloads
```

---

## 14. Database and Authority

The browser does not write custom schemas directly.

All semantic writes go through Authority Core.

Read path:

```txt
UI
→ server routes / read services
→ controlled Supabase/Postgres reads
→ projected view model
```

Write path:

```txt
UI / agent / report / app
→ CommandEnvelope
→ Authority Core
→ policy/gate/window
→ admitted write or execution
→ evidence/receipt/ghost
→ database projection
```

Forbidden:

```txt
UI → Supabase write directly
browser → custom schema directly
LLM → database mutation
report → database mutation
provider callback → semantic write without admission
```

Technical writes and projection updates may occur under controlled service paths, but semantic writes must trace to admitted acts.

---

## 15. Provider and Runtime Authority

Providers do not define authority.

Provider success is evidence input, not closure.

A provider SDK may perform work only when:

```txt
principal is known
action is declared
resource is scoped
policy allows
gate admits
execution window exists if required
Doppler material is present
receipt path exists
```

Host runtimes and LABs execute. They do not approve themselves.

A host runtime update flow, for example, requires:

```txt
Observe shows update need
Dan requests or approves update
Authority Core prepares command
Gate required
ExecutionWindow opened
Host runtime MCP called
LAB executes update
runtime emits events/evidence
receipt or ghost recorded
```

---

## 16. Reports and Authority

Reports synthesize. They do not authorize by themselves.

A report may produce:

```txt
suggested work
decision request
missing proof
registry candidate
settings warning
code lab task
```

A report may not produce:

```txt
direct dispatch
database mutation
provider mutation
receipt closure
verified state by narrative
```

If a report recommends action, that action becomes a candidate or CommandEnvelope subject to Authority.

---

## 17. Ghost Behavior

Any structural absence becomes a Ghost.

Examples:

```txt
Missing proof
Unknown entity
Unverified auth
Runtime unavailable
Unpublished release
Config missing
Evidence missing
Receipt unverified
Secret missing
Policy missing
Window expired
Signer missing
```

UI label:

```txt
Missing proof
Unknown
Blocker
```

Backend record:

```txt
Ghost
```

Ghosts must include:

```txt
what is missing
why it matters
what cannot proceed
smallest next probe or input required
```

Ghosts must not be swallowed as warnings if they block honest execution or closure.

---

## 18. UI Language Mapping

The primary UI should not expose raw backend language unless in Inspector, Trace, or JSON.

Mapping:

```txt
CommandEnvelope  → Request
ActionRequest    → Proposed action
PolicyDecision   → Decision
GateRequest      → Approval request
ExecutionWindow  → One-time access
EvidenceRef      → Evidence
ReceiptRef       → Receipt
Ghost            → Missing proof / Unknown / Blocker
AuthorityResponse→ Result
```

The UI composes and explains. It does not directly dispatch.

---

## 19. Authority Surfaces in Dan Control Plane

Authority appears in product surfaces as follows:

### Hello Daniel

Shows needs Dan, missing proof, risk, approvals, and important blocked actions.

### Observe

Shows runtime status, host health, heartbeats, missing proof, and maintenance needs.

### Code Lab

Shows plan, model routing, command observations, evidence candidates, and review gates.

### Lab Reports

Generates decision support, not receipts.

### Registry

Shows named world, evidence status, relations, candidates, and mutations through Authority.

### Settings

Holds policies, gates, rules, access, models, budgets, connections, secrets metadata, runtimes, and releases.

### Inspector

Shows authority trace, sources, relations, evidence, reports, review, and JSON.

---

## 20. Minimal Authority State Machine

```txt
intent
  ↓
candidate
  ↓
CommandEnvelope
  ↓
PolicyDecision
  ↓
GateDecision
  ↓
needs_approval? ── yes → Dan approval / denial / ghost
  ↓ no or approved
ExecutionWindow? ── required → create scoped window
  ↓
runtime dispatch
  ↓
evidence record
  ↓
receipt or ghost
  ↓
projection
```

Forbidden jumps:

```txt
intent → dispatch
candidate → database mutation
policy → execution
approval → closure
runtime success → verified
report → receipt
provider success → done
```

---

## 21. Minimum Machine-Readable Contracts

Authority v0 requires these contract shapes:

```txt
PrincipalRef
AuthContext
ActionRequest
ResourceRef
CommandEnvelope
PolicyDecision
GateRequest
GateDecision
ExecutionWindow
PasskeyWindow
Visa
EvidenceRequirement
ReceiptRequirement
Ghost
AuthorityResponse
```

These should become machine-readable schemas and generated types.

They feed:

```txt
Settings
Review
Inspector Trace
JSON
Audit
Authority Core
Command Bridge
App Park admission
Host Runtime execution
```

---

## 22. Command Examples

### 22.1 Registry Candidate Dry Run

```txt
User asks to register sensor NFC.
UI creates Proposed action.
Backend wraps CommandEnvelope with dry_run.
Authority Core evaluates.
Policy allows dry-run candidate.
Gate returns ok.
No database write happens.
UI shows candidate and next approval path.
```

### 22.2 Registry Apply

```txt
User applies registry entity.
CommandEnvelope mode: apply.
Policy checks actor, entity type, scope.
Gate requires authority.
If ok, write goes through Authority Core.
Evidence/receipt path created.
Registry projection updates.
```

### 22.3 Protected Host Runtime Update

```txt
Observe detects update need.
User approves update.
Authority Core prepares protected command.
Gate requires ExecutionWindow.
ExecutionWindow opens with scope hash.
Host runtime executes update.
Events/evidence emitted.
Receipt or ghost closes result.
```

### 22.4 Code Lab Command

```txt
Code Lab requests command run.
Local safe command may run under Code Lab policy.
Protected action is denied or requires gate/window.
Logs become evidence candidates.
Tests do not auto-close receipt.
```

### 22.5 Provider Mutation

```txt
Provider mutation requested.
Policy checks provider enabled, mutation allowed, scope, actor, risk.
Doppler secret presence checked without value exposure.
Gate requires approval/window if protected.
Provider response becomes evidence input.
Receipt closes only declared scope.
```

---

## 23. Testing and Verification Requirements

Authority requires tests for:

```txt
UI mutation cannot bypass /api/authority/command
browser cannot write custom schemas directly
natural language cannot dispatch
report cannot become receipt
provider success cannot mark verified
local coder cannot execute protected action
premium call requires budget/reason
secret values never enter logs/reports/receipts/browser payloads
protected command cannot run without gate/window
expired execution window blocks execution
missing config creates Ghost
missing evidence creates Ghost
```

A green UI test does not prove Authority. Authority requires behavioral probes against command paths.

---

## 24. Anti-Patterns

Reject designs where:

```txt
policy exists only as prompt
cron becomes authority
UI button dispatches protected command directly
LLM mutates database
browser writes custom schema
provider callback mutates semantic state directly
Doppler flag defines institutional meaning
LAB ID authorizes execution
ExecutionWindow is reusable indefinitely
PasskeyWindow authorizes unrelated actions
Visa becomes unbounded admin access
Passport becomes permission
report closes receipt
provider success becomes verified
sandbox command becomes done
secret value appears in trace/log/receipt/report
Gateway mutates LAB directly without Tower
App Park member self-grants capability
```

---

## 25. Amendment Rule

Changes to this doctrine require an Authority Amendment.

```txt
Authority Amendment

Proposed change:
Why current doctrine fails:
Which authority boundary is affected:
Which command classes change:
Which policy/gate/window behavior changes:
Which risks increase:
Which receipts/probes are required:
Which downstream documents change:
Dan approval required: yes/no
Foundation governance required: yes/no
```

Foundation governance is required if a change attempts to redefine LogLine canon, receipt semantics, slot grammar, or conformance behavior.

---

## 26. Current Honest State

```txt
State:
  Canon draft.

Grounded by:
  Minilab Constitution / Strategic Architecture, Characters / Ontology Canon, Dan Control Plane product/backend doctrine, and existing Authority Core contract vocabulary.

Verified:
  This document defines intended authority boundaries.

Unverified:
  It does not prove Authority Core implementation exists.
  It does not prove /api/authority/command is implemented.
  It does not prove current Supabase grants/RLS safety.
  It does not prove protected command physics on LABs.
  It does not prove Passkey or ExecutionWindow runtime behavior.

Ghosts:
  Need machine-readable schemas for CommandEnvelope and AuthorityResponse.
  Need Authority Core implementation receipt.
  Need Supabase Safety Baseline receipt.
  Need protected command probe.
  Need host runtime legalization receipt.
```

---

## 27. Canonical Closing

```txt
Authority is the firewall between intention and effect.

Natural language may propose.
Policy may evaluate.
Gate may admit.
Dan may approve.
Tower may open power.
ExecutionWindow may scope action.
LABs may execute.
Evidence may observe.
Receipts may close.
Ghosts may block false closure.

No identity becomes permission.
No permission becomes execution.
No execution becomes truth without evidence.
No evidence becomes closure without receipt.
```

