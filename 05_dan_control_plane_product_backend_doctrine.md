# 05 — Dan Control Plane Product + Backend Doctrine

**Status:** Canon draft  
**Layer:** Product surface, UX doctrine, backend boundary, command bridge  
**Parent:** 01 — Minilab Constitution / Strategic Architecture; 02 — Characters / Ontology Canon; 03 — Authority / Power Doctrine; 04 — Evidence / Receipt Doctrine  
**Scope:** Dan Control Plane as the operational app for `minilab.work`; product surfaces, UI language, backend layers, API boundaries, Authority Core bridge, read/write/secret paths, reports, Code Lab, Registry, Settings, and proof-aware UI behavior  
**Downstream documents:** Database Projection Canon, Doppler / Config Canon, App Park Doctrine, UI Manifest Contracts, API Contracts, Action Contracts

---

## 0. Purpose

This document defines the Dan Control Plane as a product and backend system.

It answers:

```txt
What is the Dan Control Plane?
What is it not?
What does Dan see?
What can the UI compose?
What must the backend resolve?
What may mutate state?
Where do secrets live?
Where does evidence appear?
Where do reports stop?
Where do receipts close?
How do commands cross from UI into Authority?
```

The Dan Control Plane is the cockpit for Dan’s Minilab universe. It is not the institution, not the source of truth, not a direct executor, not a database admin UI, and not a chatbot with logs.

It is the place where Dan observes, asks, decides, codes with discipline, receives reports, inspects evidence, registers the world, and governs access/policies.

---

## 1. Product Definition

The Dan Control Plane is the operational app of `minilab.work`.

It is:

```txt
a physical-digital observability app
an operational intelligence surface
a disciplined Code Lab
an evidence-aware report surface
a recognized-world Registry
an authority/settings surface
an Inspector for proof, context, relations, trace, and raw JSON
```

It is not:

```txt
admin panel
generic dashboard
isolated IDE
CRUD frontend for Registry
Devin clone
Vercel UI clone
chatbot with logs
source of truth
executor
canon
```

Operational definition:

```txt
Dan Control Plane is Daniel’s local surface for observing, asking, coding, receiving reports, registering entities, inspecting evidence, and governing access/policies for the physical-digital laboratory `minilab.work`.
```

---

## 2. Product Law

```txt
UI composes.
Backend resolves.
Authority Core admits.
LAB executes.
Supabase remembers.
Doppler supplies material.
Report synthesizes.
Receipt closes.
Ghost preserves absence.
```

Expanded:

```txt
The UI may ask, inspect, prepare, propose, explain, and render.
The UI may not dispatch, mutate providers, write semantic database state, or close evidence directly.

The backend may validate, consult, transform, authorize, register, prove, block, and call controlled services.
The backend may not bypass Authority Core for semantic writes.

Authority Core is the write kernel.
LABs and host runtimes execute admitted work.
Supabase/Postgres stores projections and indexes.
Doppler provides config/secrets without becoming authority.
Reports synthesize evidence and missing proof.
Receipts close scoped acts.
Ghosts preserve missing proof.
```

---

## 3. Product Feeling

Opening the app should feel like:

```txt
The Lab is speaking to me.
I see the physical state.
I see the digital state.
I know what needs me.
I can ask for a report.
I can code with discipline.
I can inspect what exists in the world.
I can govern access and rules.
```

It should not feel like:

```txt
I am in a backend panel.
I am in a random app generator.
I am in a CRM.
I am in a generic analytics dashboard.
I am in a log dump.
I am inside a shell pretending to be a product.
```

---

## 4. Primary Surfaces

The product has six primary surfaces:

```txt
Hello Daniel
Observe
Code Lab
Lab Reports
Registry
Settings
```

These replace older ontology-first navigation such as:

```txt
Characters
Workorders
Candidates
Gates
Receipts
Policies
Actors
Engines
Memory
Local LLM Findings
```

Those objects still exist. They appear inside the right surface, Inspector, Trace, JSON, Audit, Registry, or Settings. They do not become primary navigation merely because they are backend objects.

The sidebar is routine. It is not ontology.

---

## 5. Shell

Sidebar v0:

```txt
Dan Control Plane
minilab.work
+ New

Hello Daniel
Observe
Code Lab
Lab Reports
Registry
Settings
```

Footer status:

```txt
Current LAB
Mode
Needs Daniel
Premium calls
Runtime status
```

Example:

```txt
Current LAB
LAB-256 · workbench

Mode
Safe · dry-run

Needs Daniel
2

Premium calls
3 left
```

The shell must include:

```txt
Universal Composer
Universal Inspector
Current LAB context
Mode/safety context
Source/evidence affordances
Trace/JSON affordances for advanced inspection
```

---

## 6. Universal Composer

The Universal Composer is always available.

Placeholder:

```txt
Ask, inspect, prepare, or propose work...
```

Allowed input examples:

```txt
Generate today’s Supabase report.
Check whether LAB-256 is healthy.
Prepare a Code Lab change.
Register this NFC sensor.
Why did LAB-8GB heartbeat disappear?
Review premium call costs today.
Prepare an App Park deploy plan.
```

Allowed outputs:

```txt
Answer
Clarification request
Lab Report request
Proposed change
Registry candidate
Code Lab work
Review item
Missing proof
Decision request
```

Forbidden outputs:

```txt
direct dispatch
direct database mutation
direct provider mutation
direct receipt closure
protected execution
secret value display
```

The Composer may create candidates. It may not execute natural language.

---

## 7. Universal Inspector

The Inspector exists in every surface.

Tabs:

```txt
Context
Sources
Relations
Evidence
Reports
Review
Trace
JSON
```

The Inspector opens for any important object:

```txt
LAB
sensor
observation
heartbeat
host runtime
config doctor
code file
command output
benchmark
report
registry entity
policy
gate
receipt
ghost
package
repo
agent
app
route
webhook delivery
work item
```

Inspector order must respect proof discipline:

```txt
human decision context first
evidence before raw
relations before raw
JSON/raw as disclosure, not primary truth
secret values never visible
```

Example object view:

```txt
Title
Kind
Status
Evidence status
Why it matters
Can
Cannot
Relations
Evidence
Missing proof
Trace
JSON
```

---

## 8. UI Language Rule

The main UI should not expose raw backend language unless in Inspector, Trace, or JSON.

Mapping:

```txt
CommandEnvelope   → Request
ActionRequest     → Proposed action
PolicyDecision    → Decision
GateRequest       → Approval request
ExecutionWindow   → One-time access
EvidenceRef       → Evidence
ReceiptRef        → Receipt
Ghost             → Missing proof / Unknown / Blocker
AuthorityResponse → Result
```

Other mappings:

```txt
ops.logline_acts       → Acts / Trace
registry.entities      → Entities
registry.links         → Relations
lab_observability.*    → Observations / Health / Audit
config_registry.*      → Config requirements / Doctor
release_registry.*     → Releases / Installations
reports.*              → Reports
code_lab.*             → Code Lab sessions / Evidence
```

Internal names may appear in:

```txt
Inspector → Trace
Inspector → JSON
Developer mode
Audit detail
```

---

## 9. Evidence Language in UI

Allowed labels:

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

The UI may use `verified` only when the displayed claim has scoped receipt or conformance support.

A green UI state must not hide missing proof.

---

## 10. Surface: Hello Daniel

### 10.1 Function

Hello Daniel is the home surface.

It is the first contact with Lab intelligence.

It answers:

```txt
What does the Lab know now?
What changed?
What is normal?
What is degraded?
What needs Daniel?
What can wait?
What should become a report?
What should become work?
```

Hello Daniel is not a dashboard.
It is a **Lab shift report**.

### 10.2 Layout

```txt
Hello Daniel

Lab Report Card
Needs Daniel
Pulse Grid
  Physical
  Digital
  Code
  Maintenance
  Audit
  Runway
Recent Activity
  observations
  reports
  work
  receipts
  missing proof
Suggested Next
```

### 10.3 Report Card

Example:

```txt
Good morning, Daniel.

LAB-256 is online.
LAB-8GB has no recent heartbeat.
Physical presence is unknown.
One code experiment is ready to continue.
Two settings items need review.

Recommended mode:
Light build work + digital maintenance.
```

View model:

```ts
type HelloDanielReportView = {
  greeting: string;
  generatedAt: string;
  severity: "quiet" | "normal" | "notice" | "warn" | "critical";
  summary: string;
  recommendedMode:
    | "quiet"
    | "light_work"
    | "deep_work"
    | "maintenance"
    | "review_only"
    | "stop_and_recover";
  needsDaniel: NeedDanielItemView[];
  pulses: LabPulseView[];
  suggestedNext: SuggestedActionView[];
};
```

### 10.4 Needs Daniel

Types:

```txt
decision
approval
missing context
risk acceptance
budget decision
physical attention
report requested
code review
registry ambiguity
```

### 10.5 No Forced Insight

The Lab may say:

```txt
Nothing needs Daniel now.
Today is quiet.
No new insight.
Insufficient data to conclude.
```

Quiet is valid. Banal truth beats fake urgency.

---

## 11. Surface: Observe

### 11.1 Function

Observe is physical-digital observability.

It covers:

```txt
physical state
digital state
timeline
maintenance
audit
```

Observation is not verification.

### 11.2 Tabs

```txt
Physical
Digital
Timeline
Maintenance
Audit
```

### 11.3 Physical

Signal classes:

```txt
presence
rfid
wifi
audio
nfc
camera
scale
power
temperature
ventilation
```

States:

```txt
live
stale
offline
degraded
conflicting
disabled
unknown
```

Copy rules:

```txt
Sensor observed.
Sensor inferred.
Sensor conflicts.
Sensor did not prove.
```

Never:

```txt
Sensor verified Dan.
Camera proves truth.
WiFi proves identity.
```

### 11.4 Digital

Includes:

```txt
LAB heartbeats
host runtime status
MCP status
Supabase health
Doppler/config doctor
package versions
release installations
repo sync
CI/build status
LLM/model availability
background jobs
App Park status, when available
```

Primary cards:

```txt
LAB-8GB
LAB-256
LAB-512
Supabase
Doppler
Host Runtime
Release Registry
Config Doctor
Local Models
Premium Models
App Park
```

### 11.5 Timeline

Cross-domain timeline of physical/digital/code/report/audit events.

### 11.6 Maintenance

Maintenance states:

```txt
planned
needed
running
blocked
closed
ghosted
```

Maintenance types:

```txt
host runtime update
sensor calibration
disk pressure
memory pressure
package drift
config drift
repo drift
credential staleness
care routine
physical risk
```

### 11.7 Audit

Audit answers:

```txt
Who asked?
What changed?
Which system executed?
What evidence exists?
Which receipt closed it?
What remains missing?
```

---

## 12. Surface: Code Lab

### 12.1 Function

Code Lab is the experimental bench for making code with:

```txt
LogLine discipline
local coder
premium calls
sandbox / preview
files
logs / evidence
benchmarks
reports
review
```

It is not an isolated IDE.
It is not unconstrained agent coding.
It is not “generated equals done.”

### 12.2 Tabs

```txt
Workbench
Plan
Files
Preview
Logs
Benchmarks
Review
```

### 12.3 Existing White UI Mapping

If a previous white coding UI exists, it becomes:

```txt
Code Lab → Workbench
```

Renamings:

```txt
CHAT                       → Work Chat
SANDBOX REMOTE FILESYSTEM  → Files
SANDBOX REMOTE OUTPUT      → Logs / Evidence
Preview                    → Preview
```

### 12.4 Model Router

Visible but not theatrical.

Cards:

```txt
Local coder
  default / cheap / private / limited

Premium architect
  scarce / expensive / high reasoning

Premium reviewer
  final review / hard bug / architecture check

LogLine planner
  decomposes / tracks evidence / prevents false closure
```

Policy shape:

```ts
type ModelRoutingPolicy = {
  defaultCoder: "local";
  premiumBudgetDaily: number;
  premiumBudgetRemaining: number;
  escalateWhen: Array<
    | "architecture_unclear"
    | "repeated_local_failure"
    | "security_risk"
    | "release_decision"
    | "complex_debugging"
  >;
  localMay: Array<
    | "edit_files"
    | "summarize_logs"
    | "propose_small_patch"
    | "run_safe_tests"
  >;
  localMayNot: Array<
    | "dispatch_protected"
    | "close_receipt"
    | "mutate_provider"
    | "approve_release"
  >;
};
```

### 12.5 Code Evidence Semantics

Generated code is candidate/artifact.
Logs are evidence candidates.
Tests/benchmarks do not auto-close.
Premium calls are explicit and budgeted.
Local coder cannot dispatch protected actions.

---

## 13. Surface: Lab Reports

### 13.1 Function

Lab Reports is structured intelligence on demand.

Reports are interpretation.
Reports are not receipts.

### 13.2 Report Kinds

```txt
Hello Daniel Report
Daily Lab Report
Supabase State Report
Physical Lab Report
Digital Health Report
Maintenance Report
Audit Report
Code Experiment Report
Benchmark Report
Decision Packet
Financial / Services Report
App Park Report, when available
```

### 13.3 Report View

```ts
type LabReportView = {
  id: string;
  title: string;
  kind:
    | "hello_daniel"
    | "daily"
    | "supabase_state"
    | "physical"
    | "digital"
    | "maintenance"
    | "audit"
    | "code_experiment"
    | "benchmark"
    | "decision_packet"
    | "financial_services"
    | "app_park";
  requestedBy: string;
  preparedBy: string;
  generatedAt: string;
  timeWindow: { from: string; to: string };
  sourceRefs: string[];
  findings: ReportFindingView[];
  interpretation: string;
  confidence: "low" | "medium" | "high" | "insufficient";
  missingData: string[];
  suggestedDecisions: SuggestedDecisionView[];
  suggestedWork: SuggestedWorkView[];
};
```

### 13.4 Report Actions

```txt
Generate report
Ask follow-up
Compare with previous
Send to Review
Create Work
Create Registry candidate
Mark quiet
Archive
```

Reports may recommend action. They do not perform it.

---

## 14. Surface: Registry

### 14.1 Function

Registry is the recognized world.

It answers:

```txt
What exists?
Where is it?
Who/what may do what?
What evidence supports existence?
What relationships exist?
What is ghost/unverified?
```

Registry is not a CRUD table.

### 14.2 Tabs

```txt
Entities
Places
Machines
Sensors
Agents
Repos
Packages
Services
Relations
```

### 14.3 Entity Pattern

Example:

```txt
LAB-256

Kind
Machine

Role
Workbench

Status
active

Evidence
observed

Can
run approved probes
capture evidence

Cannot
govern
approve itself
authorize execution

Relations
located in H1 HUB
emits heartbeat
executes approved jobs
```

Every entity has:

```txt
identity
kind
role
status
evidence status
can/cannot
relations
source refs
missing proof
```

### 14.4 Registry Mutation

Registry reads may be server-side read services.
Registry writes always go through Authority Core.

Unknown observations may become registry candidates, not immediate entities.

---

## 15. Surface: Settings

### 15.1 Function

Settings governs authority and infrastructure.

It is not a dumping ground.

### 15.2 Tabs

```txt
Policies
Gates
Rules
Access
Models
Budgets
Connections
Secrets
Runtimes
Releases
Developer
```

### 15.3 Critical Cards

```txt
Supabase Security
Doppler Config
Authority Core
Host Runtimes
Model Routing
Premium Budget
Protected Actions
Gate Rules
Release Registry
Developer Mode
App Park, when available
```

### 15.4 Settings Mutation

All mutations go through:

```txt
POST /api/authority/command
```

Examples:

```txt
policy.rule.update
gate.rule.update
access.actor.register
model.routing.update
budget.limit.update
connection.check
secret.requirement.register
runtime.register
release.publish.prepare
```

---

## 16. Backend Principle

The backend serves product capabilities, not raw tables.

Layering:

```txt
Next UI
↓
Next API routes / server actions
↓
Read services / command services
↓
Authority Core Rust or local authority service
↓
Supabase / Doppler / Host Runtime / Model Router / Providers
```

Separation:

```txt
Read path      = server-side controlled reads
Write path     = Authority Core only
Secret path    = Doppler / server env only
Execution      = Host Runtime / MCP / LAB only under admission
Reports        = Report Engine + model routing + source refs
Code           = Code Lab orchestration
```

---

## 17. Supabase Access Rule

The browser does not access custom schemas directly.

Especially protected schemas:

```txt
ops
registry
lab_observability
config_registry
release_registry
reports
code_lab
model_ops
app_park, when available
```

Rules:

```txt
UI/browser never writes Supabase directly.
UI/browser never receives service_role or sb_secret key.
All custom schema reads go through server routes/read services.
All semantic writes go through Authority Core.
All write behavior must be dry-run capable.
```

If anon/authenticated grants exist for custom schemas, they must be revoked or controlled by RLS and documented.

---

## 18. Secret Path Rule

Secret values live only in approved secret/config material paths.

Allowed:

```txt
Doppler
server process env
runtime-injected env
redacted metadata
secret requirement names
presence/absence checks
```

Forbidden:

```txt
secret values in browser
secret values in logs
secret values in reports
secret values in receipts
secret values in database values
secret values in raw Inspector payload
unredacted env dumps
```

Settings → Secrets shows metadata only.

---

## 19. Backend by Surface

## 19.1 Hello Daniel Backend

Responsibilities:

```txt
generate first report of the shift
compute pulses
identify Needs Daniel
reuse recent valid report
return banal truth when nothing is happening
mark missing data as missing proof
```

Inputs:

```txt
lab_observability.current_state
lab_observability.events
lab_observability.ghosts
lab_observability.receipts
ops.receipt_index
registry.entities
registry.runtimes
config_registry.doctor_runs
release_registry.installations
Code Lab state
Model budget state
Financial/service state when available
```

APIs:

```txt
GET  /api/hello
POST /api/hello/report
POST /api/hello/refresh
```

## 19.2 Observe Backend

Responsibilities:

```txt
physical/digital observability
timeline
maintenance
audit
object inspection
```

APIs:

```txt
GET /api/observe/physical
GET /api/observe/digital
GET /api/observe/timeline
GET /api/observe/maintenance
GET /api/observe/audit
GET /api/observe/object/:ref
POST /api/ingest/lab-event
```

Lab event ingest shape:

```ts
type LabEventIngestRequest = {
  host_id?: string;
  source_ref: string;
  event_kind: string;
  component: string;
  severity: "debug" | "info" | "warn" | "error" | "critical";
  status: "ok" | "degraded" | "failed" | "ghost";
  observed_at: string;
  evidence?: Record<string, unknown>;
  secret_redacted: true;
};
```

Ingestion must:

```txt
validate known source or create unknown observation ghost
redact sensitive fields
write through Authority Core or controlled ingest path
update current_state projection
return evidence/ghost/result
```

## 19.3 Code Lab Backend

Responsibilities:

```txt
code work sessions
LogLine planning
model routing
sandbox adapter
file adapter
command runner
benchmark runner
evidence collector
report generator
review bridge
```

APIs:

```txt
GET  /api/code-lab/sessions
POST /api/code-lab/sessions
GET  /api/code-lab/sessions/:id
POST /api/code-lab/sessions/:id/message
POST /api/code-lab/sessions/:id/plan
POST /api/code-lab/sessions/:id/run-local-coder
POST /api/code-lab/sessions/:id/escalate-premium
POST /api/code-lab/sessions/:id/run-command
POST /api/code-lab/sessions/:id/run-benchmark
POST /api/code-lab/sessions/:id/generate-report
POST /api/code-lab/sessions/:id/send-review
```

Session shape:

```ts
type CodeLabSession = {
  id: string;
  workRef: string;
  sandboxId?: string;
  status: "draft" | "planning" | "running" | "waiting_review" | "reported" | "closed" | "ghost";
  previewUrl?: string;
  files: CodeFileRef[];
  commands: CodeCommandRun[];
  benchmarks: BenchmarkRun[];
  evidenceRefs: string[];
  reportRefs: string[];
  modelUsage: ModelUsageSummary;
};
```

Semantic mapping:

```txt
create-sandbox    → code_experiment_started
generating-files  → code_artifact_generated
run-command       → command_observed
get-sandbox-url   → preview_observed
report-errors     → missing_proof / error_observed
```

## 19.4 Lab Reports Backend

Responsibilities:

```txt
resolve report request
resolve source refs
fetch data server-side
build context pack
select model route
generate report
attach source refs
mark confidence/missing data
store report
return report view
```

APIs:

```txt
GET  /api/reports
GET  /api/reports/:id
POST /api/reports/generate
POST /api/reports/:id/follow-up
POST /api/reports/:id/send-review
POST /api/reports/:id/create-work
```

Request shape:

```ts
type GenerateLabReportRequest = {
  reportKind:
    | "hello_daniel"
    | "daily"
    | "supabase_state"
    | "physical"
    | "digital"
    | "maintenance"
    | "audit"
    | "code_experiment"
    | "benchmark"
    | "decision_packet"
    | "app_park";
  requestedBy: PrincipalRef;
  timeWindow?: { from: string; to: string };
  sourceRefs?: string[];
  question?: string;
  dry_run: boolean;
};
```

Reports may produce:

```txt
no action
suggested work
decision requested
missing proof
registry candidate
settings warning
code lab task
```

## 19.5 Registry Backend

Responsibilities:

```txt
read named world
search entities/relations
create candidates
route mutations through Authority Core
```

APIs:

```txt
GET  /api/registry/entities
GET  /api/registry/entities/:id
GET  /api/registry/relations?ref=...
GET  /api/registry/search?q=...
POST /api/registry/candidates
POST /api/authority/command
```

Write commands:

```txt
registry.entity.new
registry.entity.update
registry.link.new
registry.runtime.new
registry.config_requirement.new
release.artifact.register
release.installation.record
```

## 19.6 Settings Backend

Responsibilities:

```txt
manage policy, gates, rules, access, models, budgets, connections, secrets metadata, runtimes, releases, developer mode
```

APIs:

```txt
GET /api/settings/policies
GET /api/settings/gates
GET /api/settings/rules
GET /api/settings/access
GET /api/settings/models
GET /api/settings/budgets
GET /api/settings/connections
GET /api/settings/secrets
GET /api/settings/runtimes
GET /api/settings/releases
POST /api/authority/command
```

Mutation examples:

```txt
policy.rule.update
gate.rule.update
access.actor.register
model.routing.update
budget.limit.update
connection.check
secret.requirement.register
runtime.register
release.register
```

---

## 20. Authority Command Bridge

Endpoint:

```txt
POST /api/authority/command
```

Responsibilities:

```txt
validate request envelope
avoid logging raw tokens/secrets
build CommandEnvelope
call Rust Authority Core or local authority service
capture stdout/stderr or HTTP response safely
parse AuthorityResponse
map to product view model
return safe UI response
record invalid response as ghost/error
```

Bridge must support:

```txt
dry_run default
apply only when admitted
protected commands requiring gate/window
needs_approval
ghost
denied
error
receipt/evidence refs
```

Bridge must not become arbitrary shell-as-a-service.

---

## 21. View Model Rule

UI consumes product view models, not raw tables.

Core LabObject shape:

```ts
type LabObject = {
  ref: string;
  kind:
    | "place"
    | "machine"
    | "sensor"
    | "observation"
    | "agent"
    | "work"
    | "code_experiment"
    | "benchmark_run"
    | "report"
    | "decision"
    | "receipt"
    | "ghost"
    | "repository"
    | "package"
    | "automation"
    | "setting"
    | "app"
    | "route"
    | "webhook"
    | "work_item";
  title: string;
  subtitle?: string;
  status:
    | "active"
    | "running"
    | "waiting"
    | "blocked"
    | "degraded"
    | "closed"
    | "ghost"
    | "unknown";
  evidenceStatus:
    | "declared"
    | "observed"
    | "verified"
    | "unverified";
  sourceRefs: string[];
  relatedRefs: string[];
  updatedAt?: string;
  payload?: unknown;
};
```

Refs examples:

```txt
lab://machine/lab256
lab://sensor/hub-nfc
lab://observation/evt_123
lab://report/rpt_123
lab://receipt/sha256...
lab://ghost/ghost_123
registry://entity/dan
release://artifact/minilab-host-runtime@0.1.0
code://session/codelab_123
app://minicontratos-admin
route://minicontratos.apps.minilab.work
```

---

## 22. LabRelation

```ts
type LabRelation = {
  id: string;
  fromRef: string;
  relation:
    | "observed_by"
    | "located_in"
    | "generated"
    | "prepared_by"
    | "requested_by"
    | "depends_on"
    | "blocks"
    | "supports"
    | "contradicts"
    | "summarizes"
    | "cites"
    | "requires_decision"
    | "closed_by"
    | "produced_evidence"
    | "derived_from"
    | "triggered"
    | "mentions";
  toRef: string;
  status: "active" | "planned" | "partial" | "ghost" | "retired";
  evidenceStatus: "declared" | "observed" | "verified" | "unverified";
};
```

Relations are product projections of the ontology graph. They are not authority by themselves.

---

## 23. Ghost Behavior

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
```

UI language:

```txt
Missing proof
Unknown
Blocker
```

Backend object:

```txt
Ghost
```

Ghosts must show:

```txt
what is missing
why it matters
what cannot proceed
smallest next probe/input/signature
```

---

## 24. Report vs Receipt Enforcement

Backend and UI must distinguish:

```txt
Report
  LLM-generated or system-generated synthesis
  interpretation
  decision support
  cites sources
  can be wrong
  may request decision

Receipt
  proof closure
  scoped
  evidence-backed
  conformance-bound
  not narrative
```

Forbidden:

```txt
report → receipt closure
provider success → verified
sandbox command → done
LLM summary → evidence
```

---

## 25. Supabase Safety Baseline

First technical baseline before real writes:

```txt
PR 0 — Supabase Safety Baseline
```

Tasks:

```txt
audit grants for custom schemas
audit RLS for custom schemas
create server-only Supabase clients
guarantee service_role/sb_secret never reaches browser
document mutation path
block browser direct writes
create Settings → Supabase Security card
```

Schemas to audit:

```txt
ops
registry
lab_observability
config_registry
release_registry
reports
code_lab
model_ops
app_park, when introduced
```

Acceptance:

```txt
service_role never leaves server
browser has no custom schema write path
RLS warning is fixed or explicitly contained
all future writes declared Authority-only
```

---

## 26. Model Routing and Premium Budget

Settings must administer:

```txt
local coder default
premium budget
escalation rules
call logs
quality gates
```

Policy example:

```ts
type ModelRoutingPolicy = {
  id: string;
  defaultCoder: "local";
  premiumBudgetDaily: number;
  premiumBudgetRemaining: number;
  requireReasonForPremium: true;
  escalationRules: Array<
    | "architecture_unclear"
    | "security_sensitive"
    | "local_failed_twice"
    | "release_decision"
    | "hard_debugging"
  >;
};
```

Premium calls are scarce operational resources. They should be visible, intentional, and auditable.

---

## 27. Host Runtime Integration

Host Runtime lifecycle:

```txt
published artifact
bootstrap
doctor
smoke
heartbeat
self-update
events
receipts
```

UI locations:

```txt
Observe → Digital → Host Runtime status
Registry → Machines / Runtimes
Settings → Runtimes / Releases
Hello Daniel → heartbeat/report summary
```

Update flow:

```txt
Observe shows update pill
Daniel requests update
Authority Core prepares
Gate required
ExecutionWindow opened
Host runtime MCP called
LAB self-updates
events emitted
report/receipt generated
```

---

## 28. App Park Placement

App Park is not a primary surface in this doctrine yet, but it is anticipated.

It should appear through existing surfaces:

```txt
Hello Daniel
  App Park needs Daniel, degraded apps, failed deploys

Observe
  App Park runtime health, shared infra, events, work queues

Registry
  App Park members, services, routes, relations

Settings
  App Park capabilities, providers, policies, routes, secrets metadata

Lab Reports
  App Park reports and decision packets

Inspector
  App manifest, capabilities, evidence, receipts, raw JSON
```

If App Park becomes large enough, it may receive a Place-level view. It still must not become Authority.

---

## 29. Testing / Verification Requirements

Test categories:

```txt
view model tests
API contract tests
Authority command dry-run tests
Supabase access tests
secret redaction tests
report generation tests
Code Lab model routing tests
RLS/grants tests
host runtime event ingest tests
UI evidence-label tests
```

Critical tests:

```txt
browser cannot call Supabase custom schemas directly
UI mutation cannot bypass /api/authority/command
report cannot become receipt
provider success cannot mark verified
natural language cannot dispatch
local coder cannot execute protected action
premium call requires budget/reason
secret values never appear in UI/log/report/receipt
Ghosts appear for missing proof/config/runtime
```

---

## 30. Roadmap Strategy

Build in vertical slices.

Do not start by refactoring everything.
Do not start by building the whole backend.
Do not start by real sensors.
Do not start by App Park mini-cloud.

Recommended sequence:

```txt
PR 0 — Supabase Safety Baseline
PR 1 — Shell Convergence
PR 2 — Hello Daniel v0
PR 3 — Observe v0
PR 4 — Code Lab v0
PR 5 — Lab Reports v0
PR 6 — Registry v0
PR 7 — Settings v0
PR 8 — Authority Bridge v0
```

Each PR must preserve proof discipline and dry-run-first behavior.

---

## 31. Anti-Patterns

Reject designs where:

```txt
UI writes Supabase directly
browser receives service_role key
LLM mutates database
report closes receipt
provider success marks verified
sandbox command equals done
secret value appears in log/report/receipt/browser
policy lives only in prompt
cron becomes authority
Settings becomes dumping ground
Registry becomes CRUD frontend
Hello Daniel becomes dashboard
Observe becomes analytics-only
Code Lab becomes unconstrained IDE
Lab Reports become fake receipts
Inspector starts with raw JSON
App Park hides under random runtime tabs without membership/evidence model
```

---

## 32. Machine-Readable Product Docs

The Control Plane should use machine-readable product docs where possible:

```txt
navigation manifests
page manifests
component catalog
action contracts
API contracts
registry type schemas
report intelligence schema
sensor ingestion schema
quality rules
acceptance criteria
operational temperature map
Inspector grammar
```

These product docs generate UI metadata, validation, acceptance maps, and implementation matrices.

They are product contracts, not proof of backend/runtime behavior.

A UI build passing does not prove backend truth.

---

## 33. Amendment Rule

Changes to this doctrine require a Product / Backend Amendment.

```txt
Product / Backend Amendment

Proposed change:
Why current doctrine fails:
Which surface changes:
Which backend path changes:
Does it affect Authority Core? yes/no
Does it affect database writes? yes/no
Does it affect secret path? yes/no
Does it affect evidence/receipt boundary? yes/no
Which tests/probes are required:
Dan approval required: yes/no
Foundation governance required: yes/no
```

Foundation governance is required only if the change touches LogLine canon, receipt semantics, hash semantics, slot grammar, or conformance.

---

## 34. Current Honest State

```txt
State:
  Canon draft.

Grounded by:
  Existing Dan Control Plane final formal plan, Minilab Constitution, Authority / Power Doctrine, Evidence / Receipt Doctrine, Database Projection Canon, Doppler Canon, and UI machine-doc package inspection.

Verified:
  Product/backend doctrine is specified.

Unverified:
  It does not prove current implementation matches this doctrine.
  It does not prove Supabase grants/RLS safety.
  It does not prove /api/authority/command implementation.
  It does not prove Code Lab integration.
  It does not prove host runtime integration.
  It does not prove App Park placement.

Ghosts:
  Need PR 0 Supabase Safety Baseline receipt.
  Need route/API inventory.
  Need UI manifest alignment with this final IA.
  Need Authority Bridge implementation receipt.
  Need product acceptance tests.
```

---

## 35. Canonical Closing

```txt
Dan Control Plane is the local operational app of minilab.work.

It lets Dan observe, ask, decide, code, inspect, report, register, and govern.

The UI composes.
The backend resolves.
Authority Core admits.
LABs execute.
Supabase remembers.
Doppler supplies material.
Reports synthesize.
Receipts close.
Ghosts preserve absence.

The Control Plane is a cockpit, not the throne.
```

