# 06 — Database Projection Canon

**Status:** Canon draft  
**Layer:** Minilab database, projection, registry, audit, app schema, and semantic write doctrine  
**Parent:** 01 — Minilab Constitution / Strategic Architecture; 03 — Authority / Power Doctrine; 04 — Evidence / Receipt Doctrine; 05 — Dan Control Plane Product + Backend Doctrine  
**Scope:** `minilab.database`, Supabase/Postgres usage, semantic writes, LogLine-shaped acts, registry projection, audit views, receipt indexes, app schemas, RLS/security baseline, and database anti-reinvention rules  
**Downstream documents:** Doppler / Config Canon, App Park Doctrine, App Park Database Contract, Supabase Safety Baseline, Migration Plan

---

## 0. Purpose

This document defines the database doctrine for Minilab.

It exists to prevent reinvention, Supabase-as-brain, CRUD-as-constitution, app-specific domain takeover inside Registry, secret leakage, and semantic writes that bypass LogLine-shaped acts.

The database gives ground.

It does not govern the institution.

The institution remains:

```txt
LogLine = grammar.
Rust body / CLI = executable institutional body.
Constitutional Runtime = admissibility, evidence, closure.
Authority Core = write kernel.
Tower = operational power.
LABs = execution.
Receipts = proof.
Database = projection and queryable memory.
Dan = consequence signer.
```

Database design must serve that hierarchy.

---

## 1. Database Law

```txt
All semantic database state is projected from LogLine-shaped acts.
```

Portuguese form:

```txt
Todo estado semântico do banco é projetado de atos escritos em forma LogLine.
```

Correct flow:

```txt
LogLine-shaped act
→ admission / closure / ghost / rejection
→ receipt / evidence refs
→ database projection
→ audit / UI / report view
```

Forbidden flow:

```txt
UI form
→ direct CRUD write
→ pretend the world changed constitutionally
```

The database does not create institutional truth. It stores projections, indexes, and read models derived from admitted acts, receipts, observations, and controlled ingestion paths.

---

## 2. What Counts as Semantic Write

A semantic write is any database mutation that changes the named institutional world.

Examples:

```txt
register_entity
rename_entity
retire_entity
assign_role
link_entities
unlink_entities
register_runtime
update_runtime_status
declare_config_requirement
index_receipt
declare_snapshot
record_expedition
record_ghost
record_maintenance
record_release_installation
register_app_member
grant_capability
revoke_capability
record_public_route
```

Every semantic write must trace to:

```txt
LogLine-shaped act
Authority Core command
policy/gate decision where required
evidence / receipt / ghost path
```

Technical writes may exist without their own semantic act:

```txt
row id
created_at
updated_at
cache timestamp
view refresh metadata
migration bookkeeping
internal index fields
retry counters
lease timestamps
```

But if a field changes institutional meaning, it is semantic.

---

## 3. Two Database Boundary

Minilab recognizes two database domains.

## 3.1 `minilab.database`

Function:

```txt
operate the laboratory
name entities
maintain projected state
index receipts
show status to Dan and agents
support observability, reports, settings, registry, App Park projections
```

Not:

```txt
CRM
permanent personal/business memory
brain
canonical LogLine ledger
executor
strong final ledger
secret store
```

## 3.2 `dan.database`

Function:

```txt
network
clients
sales
commercial relationships
personal/professional durable memory
```

Rule:

```txt
minilab may serve dan.database.
dan.database must survive minilab.
```

This document does not specify `dan.database`. It protects the boundary.

---

## 4. Structure of `minilab.database`

The database has four zones:

```txt
ops.*
  acts, operational indexes, receipts, snapshots, expeditions, evidence pointers

registry.*
  projected current named world

audit.*
  readable projections and product-safe views

app schemas
  specific domains, when they exist
```

Correct hierarchy:

```txt
ops.logline_acts is the writing spine.
registry.* is projected state.
audit.* is readable projection.
app schemas specialize by reference.
```

No app schema may invent a parallel identity universe.

---

## 5. Core Schemas

### 5.1 `ops.*`

Purpose:

```txt
acts
receipt indexes
snapshot indexes
expedition indexes
ghost indexes
evidence refs
operational event indexes
```

`ops.*` is not a domain app schema. It is the operational spine.

### 5.2 `registry.*`

Purpose:

```txt
entities
links
runtimes
config requirements
mandates
capability projections when approved
```

Registry is the current named-world projection.

### 5.3 `audit.*`

Purpose:

```txt
readable views
mobile views
recent activity
unverified proof
runtime inventory
receipt summaries
missing proof
```

Audit views are projections, not truth.

### 5.4 App Schemas

Purpose:

```txt
app-specific domain data
product-specific tables
worker state
communication/event-specific tables
App Park app domains
```

Rules:

```txt
Apps specialize by reference.
Registry names.
Apps do not take over Registry.
Apps do not define canon.
```

---

## 6. Mandatory v0 Tables

Core v0 tables:

```txt
ops.logline_acts

registry.entities
registry.links
registry.runtimes
registry.config_requirements

ops.receipt_index
ops.snapshot_index
ops.expedition_index

audit views
```

Planned but not mandatory until their gates mature:

```txt
registry.tools
registry.actions
registry.capabilities
registry.policies
registry.gates
ops.requests
ops.decisions
ops.workflow_runs
ops.jobs
app_park.*
```

Do not create advanced authority/job abstractions merely because they are attractive. They arrive when their runtime path, authority path, and receipt path exist.

---

## 7. `ops.logline_acts`

`ops.logline_acts` is the writing spine.

```txt
ops.logline_acts stores indexed LogLine-shaped acts.
It does not define the LogLine canon.
```

Conceptual shape:

```sql
ops.logline_acts (
  id bigint generated always as identity primary key,

  tuple_hash text null unique,
  receipt_hash text null,
  correlation_id text null,

  who_entity_id bigint null,
  did text not null,
  this_ref jsonb not null default '{}',
  when_ref timestamptz null,

  confirmed_by_ref jsonb not null default '{}',
  if_ok_ref jsonb not null default '{}',
  if_doubt_ref jsonb not null default '{}',
  if_not_ref jsonb not null default '{}',

  status text not null,
  act_state text not null default 'candidate',
  evidence_mode text not null default 'unverified',

  source text null,
  storage_uri text null,
  metadata_json jsonb not null default '{}',

  created_at timestamptz not null default now()
)
```

Minimum act states:

```txt
candidate
ghosted
rejected
closed
```

Future states may include:

```txt
draft
walked
admitted
committed
superseded
```

But v0 must stay small enough to be operated honestly.

---

## 8. `registry.entities`

Entity is projection, not origin.

```txt
registry.entities = current projection of named things from admitted or closed LogLine-shaped acts.
```

Conceptual shape:

```sql
registry.entities (
  id bigint primary key,
  number bigint not null unique,
  name text not null,
  kind text not null,
  roles text[] not null default '{}',
  can_work boolean not null default false,
  status text not null default 'planned',

  owner_app_entity_id bigint null references registry.entities(id),

  created_by_act_id bigint null references ops.logline_acts(id),
  updated_by_act_id bigint null references ops.logline_acts(id),
  retired_by_act_id bigint null references ops.logline_acts(id),

  metadata_json jsonb not null default '{}',
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
)
```

Kinds v0:

```txt
person
llm
computer
company
app
object
service
place
runtime
workflow
project
snapshot
document
artifact
plan
```

Status v0:

```txt
planned
active
suspended
retired
ghost
```

Sacred rule:

```txt
Entity ID never changes.
Entity number never changes.
```

---

## 9. Work-Capable Identities

Some identities can perform work:

```txt
people
LLMs
computers
companies
apps
agents
```

This is represented by:

```txt
can_work = true
```

But:

```txt
can_work is capability shape.
It is not permission.
```

Permission comes from Authority, Gate, Runtime, Visa, ExecutionWindow, and receipt path.

---

## 10. Addressable-Only Identities

Some things need identity but do not work:

```txt
objects
workflows
projects
snapshots
places
documents
artifacts
plans
routes
policies
receipts
ghosts
```

They have identity because they appear in acts, receipts, reports, links, and views.

Rule:

```txt
Everything important gets identity.
Not every identity gets power.
```

---

## 11. `registry.links`

Links are projected relations.

```sql
registry.links (
  id bigint generated always as identity primary key,

  from_entity_id bigint not null references registry.entities(id),
  relation text not null,
  to_entity_id bigint not null references registry.entities(id),

  status text not null default 'active',

  created_by_act_id bigint null references ops.logline_acts(id),
  retired_by_act_id bigint null references ops.logline_acts(id),

  metadata_json jsonb not null default '{}',
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),

  unique (from_entity_id, relation, to_entity_id)
)
```

Relation kinds v0:

```txt
owns
uses
runs_on
located_at
belongs_to
observes
executes
stores
routes_to
depends_on
represented_by
provides
configured_by
serves
exposes_via
contains
emits
requires
blocks
supports
contradicts
```

Rule:

```txt
Links let future domains grow without schema panic.
```

Links are relation projections, not authority by themselves.

---

## 12. `registry.runtimes`

Runtime is first-class because it joins:

```txt
cost
energy
configuration
data
work
risk
operational responsibility
```

Conceptual shape:

```sql
registry.runtimes (
  id bigint generated always as identity primary key,

  entity_id bigint not null unique references registry.entities(id),
  runtime_class text not null,

  place_entity_id bigint null references registry.entities(id),
  provider_entity_id bigint null references registry.entities(id),

  cost_class text not null default 'unknown',
  energy_class text not null default 'unknown',
  config_scope text not null default 'unknown',

  status text not null default 'planned',

  created_by_act_id bigint null references ops.logline_acts(id),
  updated_by_act_id bigint null references ops.logline_acts(id),
  retired_by_act_id bigint null references ops.logline_acts(id),

  metadata_json jsonb not null default '{}',
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
)
```

Runtime classes v0:

```txt
cli
gateway
edge
lab
mcp
inference
database
storage
auth
config
app
automation
calendar
webhook_gateway
event_bus
queue
proxy
llm_gateway
```

Law:

```txt
Runtime is where cost/config/data/work meet.
App is where domain responsibility lives.
```

---

## 13. `registry.config_requirements`

Doppler stores values.

Database stores requirement names.

```sql
registry.config_requirements (
  id bigint generated always as identity primary key,

  runtime_entity_id bigint not null references registry.entities(id),
  config_key text not null,
  required_for text[] not null default '{}',
  is_secret boolean not null default true,

  status text not null default 'active',
  description text null,

  created_by_act_id bigint null references ops.logline_acts(id),
  updated_by_act_id bigint null references ops.logline_acts(id),
  retired_by_act_id bigint null references ops.logline_acts(id),

  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),

  unique (runtime_entity_id, config_key)
)
```

Rules:

```txt
No secrets in repo.
No secrets in database.
Only config requirement names.
Doppler owns values.
Database indexes requirements.
Receipts prove usage/readiness.
```

Allowed:

```txt
SUPABASE_SERVICE_ROLE_KEY is required for database.projection.write
```

Forbidden:

```txt
SUPABASE_SERVICE_ROLE_KEY value stored in database
```

---

## 14. `ops.receipt_index`

Correct name:

```txt
receipt_index
```

not:

```txt
receipts
```

Reason:

```txt
The database indexes receipts.
It does not redefine receipts.
```

Conceptual shape:

```sql
ops.receipt_index (
  id bigint generated always as identity primary key,

  receipt_hash text not null unique,
  tuple_hash text null,
  semantic_hash_owner text not null,

  correlation_id text null,
  receipt_kind text not null,

  actor_entity_id bigint null references registry.entities(id),
  target_entity_id bigint null references registry.entities(id),
  witness_entity_id bigint null references registry.entities(id),

  evidence_mode text not null default 'unverified',
  receipt_status text not null default 'indexed',

  storage_uri text null,
  created_at timestamptz not null default now()
)
```

Rule:

```txt
Supabase indexes receipts.
It does not redefine receipts.
```

---

## 15. `ops.snapshot_index`

```sql
ops.snapshot_index (
  id bigint generated always as identity primary key,

  snapshot_entity_id bigint not null references registry.entities(id),
  scope text not null,

  manifest_uri text null,
  manifest_hash text null,

  status text not null default 'planned',
  restore_check_status text not null default 'unverified',

  receipt_hash text null,
  created_at timestamptz not null default now()
)
```

Law:

```txt
Backup without restore-check is hope.
```

A snapshot is not trusted merely because it exists.

---

## 16. `ops.expedition_index`

```sql
ops.expedition_index (
  id bigint generated always as identity primary key,

  expedition_entity_id bigint not null references registry.entities(id),
  scope text not null,

  status text not null default 'planned',
  summary text null,
  calendar_ref text null,

  receipt_hash text null,
  created_at timestamptz not null default now()
)
```

Expedition observes.

It does not govern.

It may produce:

```txt
findings
ghosts
risk notes
maintenance suggestions
receipt requests
```

---

## 17. Audit Views

Audit views are projections.

They are not truth.

v0 views:

```txt
audit.v_entity_inventory
audit.v_runtime_inventory
audit.v_runtime_config_requirements
audit.v_recent_logline_acts
audit.v_logline_ghosts
audit.v_recent_receipts
audit.v_unverified_receipts
audit.v_snapshot_status
audit.v_expedition_status
audit.v_mobile_today
```

Most important product view:

```txt
audit.v_mobile_today
```

It should answer:

```txt
what is active?
what needs Dan?
what became ghosted?
what is unverified?
which runtime is strange?
which recent receipt matters?
which snapshot lacks restore-check?
which expedition observed something?
```

---

## 18. App Schemas and Domains

Apps may have their own schemas.

Examples:

```txt
reports.*
code_lab.*
model_ops.*
app_park.*
automation.*
intelligence.*
documents.*
network.*
sales.*
```

But every app must obey:

```txt
No app may invent parallel identity.
Every important app object references registry.entities.id or stable registry refs.
```

Registry does not model every domain detail.

Registry names.
Apps specialize.

---

## 19. Reports Schema

Reports are decision support, not receipts.

Suggested schema:

```txt
reports.lab_reports
reports.report_sources
reports.report_findings
reports.report_decision_requests
```

`reports.lab_reports` fields:

```txt
id
report_ref
kind
title
status
severity
requested_by_entity_id
prepared_by_entity_id
time_window_start
time_window_end
summary
interpretation
confidence
missing_data
source_refs
suggested_decisions
suggested_work
created_at
updated_at
```

Statuses:

```txt
draft
generated
sent_to_review
archived
ghost
```

Confidence:

```txt
insufficient
low
medium
high
```

Report confidence is not proof.

---

## 20. Code Lab Schema

Suggested schema:

```txt
code_lab.sessions
code_lab.messages
code_lab.files
code_lab.command_runs
code_lab.benchmark_runs
code_lab.model_calls
code_lab.experiments
```

`code_lab.sessions` fields:

```txt
id
session_ref
work_ref
title
status
sandbox_id
preview_url
model_policy_id
created_by_entity_id
created_at
updated_at
closed_at
```

Statuses:

```txt
draft
planning
running
waiting_review
reported
closed
ghost
```

`code_lab.command_runs` fields:

```txt
id
session_id
cmd_id
command
args_json
background
status
exit_code
stdout_ref
stderr_ref
started_at
ended_at
evidence_ref
secret_redacted
```

Code Lab output is evidence candidate. It is not automatic completion.

---

## 21. Model Ops Schema

Suggested schema:

```txt
model_ops.providers
model_ops.models
model_ops.routing_policies
model_ops.call_budgets
model_ops.call_events
```

Purpose:

```txt
local coder default
premium budgets
escalation rules
call logs
model availability
cost/routing audit
```

Premium calls are scarce operational resources.

They must be visible, reasoned, and auditable.

---

## 22. Physical Lab Schema

Optional schema:

```txt
physical_lab.*
```

Possible tables:

```txt
physical_lab.places
physical_lab.sensors
physical_lab.assets
physical_lab.risks
physical_lab.care_routines
physical_lab.home_state
```

Preferred v0:

```txt
Represent places/sensors/assets as registry.entities.
Represent observations as lab_observability.events.
Avoid creating physical_lab.* unless Registry/lab_observability cannot support the use case.
```

Avoid premature schema explosion.

---

## 23. LabObject Projection

UI should not consume raw tables.

Product view shape:

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

Refs:

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
```

---

## 24. LabRelation Projection

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

Relations are projected links.

They are not authority by themselves.

---

## 25. Authority Contracts and Commands

Database writes must align with Authority Core contracts.

Contracts P0:

```txt
ServiceContract
CommandEnvelope
PrincipalRef
AuthContext
ActionRequest
ResourceRef
PolicyDecision
GateRequest
ExecutionWindow
ConfigDoctorResult
RegistryEntityRef
ReleaseArtifactRef
LabEvent
EvidenceRef
ReceiptRef
Ghost
AuthorityResponse
```

Command families:

```txt
report.generate
report.refresh
lab.status
lab.event.ingest
lab.ghost.record
maintenance.plan
code.session.create
code.plan.generate
code.local_coder.run
code.premium_call.request
code.command.run
code.benchmark.run
code.report.generate
report.send_to_review
report.create_work
registry.entity.new
registry.entity.update
registry.link.new
registry.runtime.new
registry.sensor.register
registry.place.register
policy.rule.update
gate.rule.update
access.actor.register
model.routing.update
budget.limit.update
connection.check
runtime.register
release.register
```

All mutations:

```txt
dry_run default true
apply requires authority
protected requires gate/window
```

---

## 26. Security / RLS Plan

### 26.1 Immediate Risk

Custom schemas with RLS disabled are acceptable only if:

```txt
anon/authenticated have no direct grants
service_role is server-only
browser cannot reach custom schema tables directly
all writes go through Authority Core
```

Otherwise, revoke grants or enable RLS immediately.

### 26.2 PR 0 Checks

Grants audit:

```sql
select
  table_schema,
  table_name,
  privilege_type,
  grantee
from information_schema.role_table_grants
where table_schema in (
  'ops',
  'registry',
  'lab_observability',
  'config_registry',
  'release_registry',
  'reports',
  'code_lab',
  'model_ops',
  'app_park'
)
order by table_schema, table_name, grantee;
```

RLS audit:

```sql
select
  schemaname,
  tablename,
  rowsecurity
from pg_tables
where schemaname in (
  'ops',
  'registry',
  'lab_observability',
  'config_registry',
  'release_registry',
  'reports',
  'code_lab',
  'model_ops',
  'app_park'
);
```

### 26.3 Policy

If anon/authenticated have grants:

```txt
revoke grants or enable RLS immediately
```

If only service_role has access:

```txt
document server-only access
consider RLS for defense-in-depth
```

---

## 27. Secret Policy

Never store secret values in:

```txt
repo
database values
receipts
reports
logs
browser payloads
Inspector raw payloads
```

Database may store:

```txt
secret key names
config requirement names
is_secret boolean
presence/absence state
Doppler config name
safe digest of schema/key list when appropriate
```

Doppler stores values.

Database indexes requirements.

Receipts prove readiness/usage without exposing values.

---

## 28. Migration Discipline

No real database mutation without:

```txt
plan
dry-run when possible
schema diff
rollback path
receipt/probe requirement
Authority command where semantic
Dan approval if protected
```

Planned CLI commands:

```txt
minilab database plan
minilab database schema render
minilab database doctor --dry-run
minilab database seed --dry-run
minilab database seed
minilab database verify
minilab database snapshot-index
minilab database receipt-index
```

For early gates:

```txt
render
doctor --dry-run
seed --dry-run
```

No Supabase real mutation before its safety gate.

---

## 29. App Park Database Extension

App Park may introduce an app/platform schema, likely:

```txt
app_park.*
```

Potential tables:

```txt
app_park.apps
app_park.app_versions
app_park.app_capabilities
app_park.deployments
app_park.routes
app_park.health_checks
app_park.events
app_park.work_items
app_park.work_item_attempts
app_park.webhook_deliveries
app_park.realtime_channels
app_park.websocket_sessions
app_park.storage_allocations
app_park.backups
app_park.receipts
app_park.dead_letters
app_park.current_app_state
```

Rules:

```txt
App Park apps are Registry entities.
App Park runtime/provider/shared infra are Registry runtimes/entities.
App Park events/work/webhooks may live in app_park.*.
Important app objects reference registry entities/refs.
App Park receipts remain Foundation-conformant and are indexed, not redefined.
```

App Park must not use `app_park.*` as a parallel Registry.

---

## 30. Realtime and Event Bus Discipline

Supabase/Postgres may power durable coordination.

Rules:

```txt
Postgres rows are durable truth for coordination/projection.
Realtime is notification.
Webhooks are transport.
WebSockets are live session transport.
Workers execute.
Receipts close.
```

Realtime must not be the only copy of commands or events.

Correct pattern:

```txt
insert durable event/work row
→ Realtime wakes UI/worker
→ worker reads row
→ worker claims with lease if applicable
→ worker executes
→ evidence/result rows
→ receipt/ghost
→ projection update
```

Forbidden:

```txt
Realtime payload as only command copy
WebSocket message as durable work item
webhook transport as truth
```

---

## 31. Anti-Reinvention Rules for Agents

Any agent working on database must obey:

```txt
1. Read this Database Projection Canon first.
2. Do not create a new database ontology.
3. Do not bypass LogLine-shaped acts for semantic writes.
4. Do not turn Supabase into canon, gate, runtime, or executor.
5. Do not add tools/actions/policies/gates/jobs unless explicitly approved.
6. Do not create app domain tables inside Registry.
7. Do not store secrets.
8. Do not mutate real database before dry-run and receipt plan.
9. Do not call implementation complete without probes.
10. If deviating, write an explicit amendment proposal.
```

---

## 32. Testing Requirements

Database discipline requires tests/probes for:

```txt
browser cannot write custom schemas directly
service_role never reaches browser
anon/authenticated grants audited
RLS status audited
semantic writes route through Authority Core
reports cannot mutate state directly
provider callbacks cannot mutate semantic state directly
secrets are not stored
receipt index does not redefine receipt content
registry entities are projected from acts
app schemas reference registry identities
views are read-only projections where intended
migration dry-run produces plan
rollback path exists for real migrations
```

---

## 33. Anti-Patterns

Reject designs where:

```txt
Supabase becomes brain
CRUD becomes constitution
Registry becomes app domain dump
app creates parallel identity table without Registry refs
browser writes custom schema directly
service_role leaks to client
RLS warning is ignored without containment
report writes semantic state
LLM writes database
provider callback writes semantic state without Authority
receipt is stored as arbitrary report text
secret value stored in database
view treated as truth
audit projection mutates source state
migration has no rollback path
generated SQL from UI manifests is applied as canon without review
```

---

## 34. Amendment Rule

Changes to this doctrine require a Database Plan Amendment.

```txt
Database Plan Amendment

Proposed change:
Why current plan fails:
What invariant remains protected:
What schemas/tables/views change:
Does it affect semantic writes? yes/no
Does it affect Authority Core? yes/no
Does it affect secret policy? yes/no
Migration risk:
Rollback path:
Receipt/probe required:
Dan approval required: yes/no
Foundation governance required: yes/no
```

Foundation governance is required only if a change affects LogLine canon, receipt semantics, hash semantics, slot grammar, or conformance behavior.

---

## 35. Current Honest State

```txt
State:
  Canon draft.

Grounded by:
  Existing Database Plan v0.3, Minilab Constitution, Evidence / Receipt Doctrine, Authority / Power Doctrine, Dan Control Plane Product + Backend Doctrine, and Doppler / Config Canon.

Verified:
  This document defines intended database architecture and projection law.

Unverified:
  It does not prove current Supabase schema exists.
  It does not prove grants/RLS are safe.
  It does not prove existing migrations match this plan.
  It does not prove Authority Core write path is implemented.
  It does not prove app_park.* exists.

Ghosts:
  Need Supabase schema inventory receipt.
  Need grants/RLS receipt.
  Need server-only client proof.
  Need migration dry-run receipt.
  Need App Park database contract before app_park.* implementation.
```

---

## 36. Canonical Closing

```txt
LogLine-shaped acts write the institution.
Receipts prove scoped closure.
The database stores projections.
Entities give stable identity.
Links give relation.
Runtimes expose cost, config, data, work, and risk.
Indexes make evidence findable.
Views make the Lab visible to Dan.
Apps keep their own domains.
Registry names the shared world.
Supabase remembers.
It does not govern.
```

