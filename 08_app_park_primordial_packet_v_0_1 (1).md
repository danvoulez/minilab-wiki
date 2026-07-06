# App Park Primordial Packet v0.1

**Status:** Canon draft  
**Layer:** App Park primordial doctrine and downstream Minilab domain canon  
**Parent:** Minilab Constitution, Characters/Ontology Canon, Authority/Power Doctrine, Evidence/Receipt Doctrine, Dan Control Plane Doctrine, Database Projection Canon, Doppler/Config Canon  
**Foundation dependency:** LogLine Foundation canon, conformance, engine, governance, constitutional-runtime primitives  
**First member candidate:** Intelligence App — mini-LLMs layer of slow continuous cognitive work  
**Evidence status:** doctrine-grounded, runtime-unverified

---

## 0. Purpose of this Packet

This packet creates the first App Park canon layer.

It does not implement App Park.  
It does not claim LAB512 is ready.  
It does not create a parallel receipt canon.  
It does not authorize deployment.  
It does not assume Supabase, Doppler, Docker, proxy, routes, or runtime state are already correct.

It defines the missing primordial documents required before App Park can become a lawful Minilab domain.

The operating sequence is:

```txt
Doctrine
→ Canon
→ Contract
→ Plan
→ Probe
→ Evidence
→ Foundation-conformant Receipt
→ Projection
```

Not:

```txt
Idea
→ Code
→ Hope
```

---

## 1. Parent Invariants Imported

App Park inherits these laws from parent canon.

### 1.1 Minilab hierarchy

```txt
LogLine gives form.
Rust materializes.
Runtime admits.
Tower authorizes power.
LABs execute.
Supabase projects.
Doppler supplies material.
Receipts prove.
Dan signs consequence.
```

No App Park component may invert this hierarchy.

### 1.2 App Park is downstream

App Park may specialize Minilab into an app-runtime domain.

It may not redefine:

```txt
LogLine canon
receipt semantics
hash semantics
slot grammar
conformance governance
Authority Core
Tower power
Dan signature
```

### 1.3 Natural language boundary

```txt
Natural language never executes.
```

Natural language may produce candidate manifests, plans, workorders, ghosts, reports, or decision requests.

Natural language may not dispatch App Park deploys, mutate providers, create routes, restart services, write semantic database state, close evidence, or mark an app healthy.

### 1.4 LAB boundary

```txt
LABs execute.
They do not govern.
```

LAB512 may host App Park workloads when admitted.

LAB512 does not authorize App Park.

### 1.5 Database boundary

```txt
All semantic database state is projected from LogLine-shaped acts.
```

Supabase/Postgres may store App Park projections, events, work items, receipt indexes, current state, and app-domain tables.

It does not become canon or authority.

### 1.6 Doppler boundary

```txt
Doppler stores operational delegation.
Doppler does not define institutional meaning.
```

Doppler may supply secret/config material after admission.

It does not authorize an app, route, deployment, webhook, model call, database write, or provider mutation.

### 1.7 Receipt boundary

App Park must use Foundation-conformant receipts.

App Park may define receipt profiles.

App Park may not define a new receipt format.

Correct:

```txt
app_park.deploy.receipt_profile.v1
app_park.health.receipt_profile.v1
app_park.work_attempt.receipt_profile.v1
```

Forbidden:

```txt
app_park.receipt_format.v1
```

Evidence records remain separate from receipts.

---

# 08 — App Park Doctrine

## 08.0 Purpose

App Park Doctrine defines what App Park is and what it is not.

It prevents App Park from becoming:

```txt
random apps on LAB512
Docker graveyard
mini-AWS cosplay
Control Plane replacement
Supabase-as-platform-brain
LLM worker with hidden power
natural-language deployment surface
```

## 08.1 Canonical Sentence

```txt
App Park is Minilab’s managed application execution zone:
a downstream domain where admitted apps run inside declared capability boundaries,
shared infrastructure makes apps smaller,
probes contact reality,
Foundation receipts close scoped acts,
and the Control Plane projects current state to Dan.
```

Short form:

```txt
Apps execute.
Authority admits.
Providers materialize.
Probes observe.
Receipts close.
Supabase projects.
Dan signs consequence.
```

## 08.2 What App Park Is

App Park is:

```txt
managed application execution zone
registry place
membership domain
shared infrastructure layer
capability boundary
provider-adapter domain
evidence-producing runtime surface
Control Plane projection domain
```

It may host:

```txt
web apps
internal APIs
workers
job runners
dashboards
communication gateways
webhook handlers
realtime adapters
websocket sessions
inference-adjacent services
local proprietary tools
slow cognition workers
```

## 08.3 What App Park Is Not

App Park is not:

```txt
Control Plane
Authority Core
Tower
Constitutional Runtime
LogLine Foundation
canonical database
Doppler meaning
policy root
receipt canon
natural-language executor
unbounded cloud platform
```

## 08.4 Placement

Default placement:

```txt
LAB8GB:
  control/admission-facing services
  route authority
  public ingress where appropriate
  LLM Gateway front door
  supervisor loops

LAB512:
  App Park runtime
  app services
  workers
  shared infra providers
  dispatchers
  heavy jobs
  inference-adjacent services

LAB256:
  workbench
  testing
  manual intervention
  non-core dependency
```

This placement is declared, not runtime-verified by this packet.

## 08.5 App Park as Place

App Park must appear as a first-class Place in Registry and Control Plane.

A Place has:

```txt
identity
host
role
members
routes
health
shared services
receipts
current state projection
operational history
missing proof
```

App Park is not a folder path.

---

# 09 — App Park Membership Canon

## 09.0 Purpose

Membership Canon defines what it means for an app to be part of App Park.

A process can be running and still not be a member.

A container can exist and still not be a member.

An app becomes a member only when it is known, admitted, bounded, observable, recoverable, and receiptable.

## 09.1 Canonical Sentence

```txt
An App Park member is an admitted runtime citizen:
identified by manifest, bounded by capabilities,
served by shared infrastructure, observed by probes,
and represented in Minilab by projections and receipts.
```

## 09.2 Member Shape

A member has:

```txt
stable app id
registry entity
owner
kind
version
host placement
declared runtime
declared routes
declared secrets
declared database access
declared storage access
declared event permissions
declared work item permissions
declared webhook permissions
declared realtime/websocket permissions
declared LLM/external capabilities
declared health probe
declared backup method if stateful
declared rollback method
receipt profile bindings
current state projection
```

## 09.3 Member States

```txt
proposed
admitted
provisioning
deployed
healthy
degraded
blocked
suspended
retired
```

### proposed

Manifest exists but has not been admitted.

### admitted

Control Plane / Authority accepted the member under declared scope.

### provisioning

Shared infra is being prepared.

### deployed

Runtime materialization occurred and deployment evidence exists.

### healthy

Declared health probe currently passes within scope.

### degraded

Runtime exists but one or more declared probes fail.

### blocked

Required capability, secret, route, database, provider, receipt path, or runtime condition is missing.

### suspended

Policy prevents app execution or communication.

### retired

App is no longer active, but history, receipts, and projections remain.

## 09.4 Member Rights

Rights are available only when declared and admitted.

### Runtime rights

```txt
run as managed service
run as managed worker
run as finite job runner
restart through App Park
request deployment
request rollback
publish health status
```

### Routing rights

```txt
receive internal route
receive private route
receive public route if policy allows
receive route aliases
receive TLS termination through shared proxy
receive Cloudflare Access or equivalent protection where applicable
```

### Data rights

```txt
receive app-owned schema
run app-scoped migrations
read/write app-owned tables
emit events to shared bus
read allowed event streams
enqueue allowed work items
consume allowed work items
```

### Storage rights

```txt
receive app-scoped bucket/prefix
read/write declared storage classes
participate in backup schedule
restore from app-scoped backup with admission
```

### Communication rights

```txt
receive inbound webhooks through shared gateway
send outbound webhooks through shared dispatcher
publish allowed event types
subscribe to allowed streams
open allowed realtime channels
use WebSocket sessions for declared live use cases
```

### Secrets rights

```txt
receive declared secrets at runtime
validate secret readiness
rotate/reload secrets through platform flow
never inspect or exfiltrate secret values through receipts/logs/reports/UI
```

### LLM / external capability rights

```txt
call LLM Gateway under declared profile
call external APIs under declared capability
consume shared notification/email service if admitted
```

### Observability rights

```txt
publish health
publish logs
publish metrics
publish evidence records
publish receipt references
appear in Control Plane UI
```

## 09.5 Member Duties

A member must:

```txt
provide manifest
provide health contract
declare all ports
declare all routes
declare required secrets
declare database/schema use
declare storage use
declare events it publishes
declare streams it subscribes to
declare webhook providers and outbound targets
declare stateful/stateless nature
declare backup requirements if stateful
expose logs through supported runtime mechanism
avoid logging secrets
support graceful shutdown if long-running
support idempotency for retries
produce or allow evidence records for operations
avoid direct canon mutation
avoid hidden dependencies outside manifest
not bind arbitrary public ports
not use undeclared credentials
not write another app’s schema/storage without explicit grant
not bypass shared webhook/event/work infrastructure for platform-visible operations
```

---

# 10 — App Park Shared Infra Canon

## 10.0 Purpose

Shared Infra Canon defines what App Park provides to members so apps do not reimplement operational machinery.

Shared infra makes apps smaller, safer, more observable, and more recoverable.

## 10.1 Shared Infra Law

```txt
Shared infra grants capability surfaces.
It does not grant authority.
```

## 10.2 v1 Shared Infra Classes

App Park v1 may provide these classes:

```txt
runtime manager
reverse proxy / routing
auth boundary
secrets injection
database access
event bus
work queue
webhook gateway
realtime layer
websocket gateway
object storage
logging / observability
backup / restore
LLM access
notification / communication service
```

## 10.3 Required v1 Philosophy

App Park v1 is:

```txt
more than vanilla Docker Compose
less than mini-AWS
substantial enough for real proprietary apps
small enough to inspect, replay, and recover
```

## 10.4 Default v1 Provider Candidates

These are candidates, not verified facts:

```txt
runtime manager: Docker Compose
reverse proxy: Caddy or Traefik
secrets: Doppler-generated runtime env / platform injection
database: Supabase/Postgres app schemas
event bus: Supabase/Postgres durable events
queue: Supabase/Postgres work_items with leases
storage: local filesystem first, external durable copy later
realtime: Supabase Realtime over persisted rows
websocket: custom gateway only for live sessions when needed
LLM access: LLM Gateway, not direct model endpoint
```

Provider choice requires runtime inventory receipts before implementation.

## 10.5 Shared Infra Must Not Become Shared Blast Radius

Forbidden:

```txt
one app reading another app’s secrets
one app writing another app’s schema/storage by default
public route creation without approval
webhook replay with side effect without admission
secret values in logs/receipts/reports/db/browser
LLM profile escalation without declared capability
health failure hidden behind green UI
backup marked safe without restore-check path
```

---

# 11 — App Park Capability Canon

## 11.0 Purpose

Capability Canon defines the vocabulary of what an App Park member may request or consume.

Capability is not permission. It is the shape of a possible allowed behavior.

Permission comes from Authority, Policy, Gate, ExecutionWindow, Visa, PasskeyWindow, and receipt path where required.

## 11.1 Capability Classes

```txt
runtime.service
runtime.worker
runtime.job_runner
route.internal
route.private
route.public
secret.readiness
database.schema_owned
database.migration_app_scoped
storage.bucket_owned
event.publish
event.subscribe
work.enqueue
work.consume
webhook.inbound
webhook.outbound
realtime.channel
websocket.session
llm.profile
external_api.call
notification.send
backup.create
restore.plan
restore.apply
health.probe
logs.read
metrics.publish
receipt.emit_profile
```

## 11.2 Capability Lifecycle

```txt
declared
requested
admitted
denied
ghosted
active
suspended
revoked
retired
```

## 11.3 Capability Grant Rule

A capability grant must be:

```txt
declared in manifest
validated by schema
evaluated by policy
admitted by Authority when needed
visible in Control Plane
revocable
receiptable when used for consequential operations
```

## 11.4 Dangerous Capabilities

These are protected by default:

```txt
route.public
webhook.outbound
restore.apply
database.migration_app_scoped
external_api.call
llm.profile above local-small
runtime.restart
runtime.deploy
runtime.rollback
provider.mutation
```

## 11.5 Intelligence App Initial Capability Envelope

The first member candidate, Intelligence App, should start with low-authority capabilities:

```txt
event.subscribe
event.publish
work.consume
work.enqueue_proposed
llm.profile.local_small
health.probe
receipt.emit_profile
logs.read_own
```

Explicitly denied in its first form:

```txt
shell.execute
service.restart
runtime.deploy
runtime.rollback
provider.mutation
database.semantic_write
canon.mutate
secret.value_read
public_route.create
external_webhook.dispatch
```

---

# 12 — App Park Contract Catalog

## 12.0 Purpose

Contract Catalog defines the machine-readable contracts App Park must eventually provide.

Schemas arrive after doctrine/canon. Otherwise schemas become typed vibes.

## 12.1 Contract Ladder

```txt
Foundation Contract:
  logline.receipt.v0
  conformance verifier
  constitutional runtime primitives

Minilab Contract:
  entity/runtime/config/receipt projection
  Authority Core CommandEnvelope
  Doppler config doctrine
  Supabase projection doctrine

App Park Contract:
  membership
  manifest
  capabilities
  providers
  runtime
  routes
  secrets
  database
  storage
  communication
  probes
  evidence records
  receipt profiles
  UI projections
```

## 12.2 Machine-Readable Contract Set

Required conceptual contracts:

```txt
app-park.manifest.v1
app-park.member.v1
app-park.capability.v1
app-park.provider.v1
app-park.runtime.v1
app-park.route.v1
app-park.secret.v1
app-park.database.v1
app-park.storage.v1
app-park.event.v1
app-park.work-item.v1
app-park.webhook.v1
app-park.realtime.v1
app-park.websocket.v1
app-park.health.v1
app-park.backup.v1
app-park.rollback.v1
app-park.suspension.v1
app-park.evidence-record.v1
app-park.probe.v1
app-park.receipt-profile.v1
app-park.projection.v1
app-park.policy-pack.v1
```

Forbidden contract:

```txt
app-park.receipt-format.v1
```

## 12.3 Minimal First Contract Set

For the Intelligence App vertical slice, start with:

```txt
app-park.manifest.v1
app-park.member.v1
app-park.capability.v1
app-park.event.v1
app-park.work-item.v1
app-park.evidence-record.v1
app-park.probe.v1
app-park.receipt-profile.v1
app-park.projection.v1
intelligence.job.v1
intelligence.finding.v1
```

---

# 13 — App Park Provider Adapter Canon

## 13.0 Purpose

Provider Adapter Canon defines how App Park touches external or local machinery without confusing provider success with institutional closure.

Adapters materialize plans. They do not decide meaning.

## 13.1 Provider Law

```txt
Contracts declare desired state.
Policy admits.
Adapters materialize.
Probes observe.
Evidence records capture.
Receipts close.
```

## 13.2 Adapter Classes

```txt
runtime_adapter
proxy_adapter
secret_adapter
database_adapter
storage_adapter
event_adapter
queue_adapter
webhook_adapter
realtime_adapter
websocket_adapter
llm_gateway_adapter
notification_adapter
backup_adapter
observability_adapter
```

## 13.3 Adapter Output

An adapter may output:

```txt
plan
applied operation result
provider response
probe command/request
raw observation
evidence record candidate
ghost
```

An adapter may not output:

```txt
final truth
broad verification
receipt closure without conformance path
authority approval
secret values
```

## 13.4 Provider Choice Status

Current provider choices are ghosts until runtime inventory receipts exist.

Open ghosts:

```txt
Docker Compose availability on LAB512
Caddy vs Traefik decision
Supabase schema/RLS/grants reality
Doppler current config reality
storage provider choice
Cloudflare Access vs Control Plane token for private routes
WebSocket gateway placement
LLM Gateway route/profile availability
```

---

# 14 — App Park Communication Canon

## 14.0 Purpose

Communication Canon defines events, work, webhooks, realtime, and websocket boundaries.

It prevents “message” from becoming conceptual mush.

## 14.1 Vocabulary

```txt
Event:
  something happened.

Command / Work Item:
  something should happen.

Webhook:
  HTTP delivery or intake from outside the system.

Realtime Signal:
  someone should refresh or wake.

WebSocket Message:
  live session transport.

Receipt:
  scoped closure over an admitted act.
```

## 14.2 Communication Law

```txt
Postgres/Supabase rows are durable coordination/projection.
Realtime is notification.
Webhooks are edge transport.
WebSockets are live session transport.
Workers execute.
Receipts close.
```

## 14.3 Event Bus

App Park events are durable rows.

Initial event families:

```txt
app.*
route.*
health.*
secret.*
deploy.*
rollback.*
work.*
webhook.*
realtime.*
websocket.*
backup.*
receipt.*
intelligence.*
```

Initial examples:

```txt
app.proposed
app.admitted
app.blocked
app.suspended
app.retired
deploy.requested
deploy.planned
deploy.started
deploy.failed
deploy.succeeded
health.probe.requested
health.probe.observed
health.failed
health.ok
secret.missing
secret.ready
work.requested
work.claimed
work.completed
work.failed
webhook.received
webhook.dispatched
webhook.dead_lettered
receipt.indexed
intelligence.classification.completed
intelligence.ghost.detected
intelligence.probe.suggested
intelligence.decision.requested
```

## 14.4 Work Items

A work item is a durable command candidate or task.

Work item must support:

```txt
kind
target app
payload
status
lease owner
lease expiration
attempts
max attempts
idempotency key
result event
receipt path
```

Work status:

```txt
pending
leased
running
completed
failed
dead_lettered
ghosted
cancelled
```

## 14.5 Webhooks

Inbound webhooks must be received through a shared gateway unless explicitly internal and admitted otherwise.

Inbound gateway must:

```txt
identify provider
verify signature where required
normalize safely
redact secrets
persist delivery row
emit event
enqueue work if needed
produce evidence/receipt path
```

Outbound webhooks must:

```txt
use declared target reference
respect policy
sign where required
retry under policy
dead-letter after exhaustion
emit delivery evidence
```

## 14.6 Realtime

Realtime must point to persisted rows or explicitly declare itself ephemeral.

Forbidden:

```txt
realtime payload as only copy of command
realtime message as canonical event
realtime signal as receipt
```

## 14.7 WebSocket

WebSocket v1 is allowed only for live sessions:

```txt
live logs
LLM token streaming
interactive session status
presence
browser/app interaction stream
terminal-like session under protected admission
```

WebSocket may not become durable bus or hidden mutation path.

---

# 15 — App Park Business Logic Canon

## 15.0 Purpose

Business Logic Canon defines the opinionated defaults that stop App Park from becoming unsafe infrastructure with nice schemas.

This is the most important App Park-specific doctrine after Membership.

## 15.1 Default Policy Stance

```txt
default exposure: private
default authority: deny mutation
default LLM access: denied
default public route: needs approval
default deploy: requires health probe
default stateful app: requires backup plan
default webhook inbound: signature required unless internal
default outbound webhook: retry + dead-letter required
default app-to-app access: denied unless granted
default cross-schema access: denied
default secret value visibility: forbidden
default self-escalation: forbidden
default natural-language action: candidate only
```

## 15.2 App Cannot Write Canon

Apps may own app-domain data.

Apps may emit events, findings, proposed acts, and receipts.

Apps may not directly mutate:

```txt
Foundation canon
Minilab constitution
Authority rules
Tower gates
Doppler canon
Database projection law
receipt semantics
another app’s ownership boundary
```

## 15.3 Public Exposure Rule

Public route creation is protected.

Requires:

```txt
manifest declaration
policy allow
Dan approval when risk demands
route provider plan
secret/config readiness
health probe path
rollback/disable route path
receipt profile
```

## 15.4 Stateful App Rule

Stateful apps must declare:

```txt
database schema or storage namespace
backup scope
backup cadence
restore-check path
owner
retention
```

A backup without restore-check is hope.

## 15.5 Intelligence App Business Defaults

The Intelligence App is slow cognition, not authority.

It may:

```txt
observe declared streams
classify
summarize
detect ghosts
detect stale work
suggest probes
suggest decision requests
write intelligence findings
enqueue proposed work
call declared local-small LLM profiles
```

It may not:

```txt
execute shell
restart service
deploy app
rollback app
write canon
approve action
close broad claims
read secret values
call undeclared model profiles
send outbound webhooks
create public routes
mutate provider state
```

## 15.6 First Slice Business Rule

The first lawful member ceremony should prove:

```txt
one member manifest validates
capabilities are bounded
plan is generated without mutation
seeded events are classified
findings are written to test/projection store
evidence record is produced
receipt profile references Foundation receipt canon
no consequence action occurs
Control Plane can project member state
```

---

# 16 — First Member Candidate: Intelligence App

## 16.0 Definition

```txt
Intelligence App is the slow cognition layer of App Park.
It runs small, constant, low-authority reasoning jobs that observe,
classify, summarize, compare, suggest, and prepare — without executing
consequences directly.
```

## 16.1 Role

```txt
mini-LLMs layer
slow continuous work
LAB cognition
inference orchestration consumer
receipt/report summarizer
ghost detector
probe suggester
decision-request preparer
```

## 16.2 First Loop

`intelligence.classify_once`

Reads:

```txt
seeded events or recent App Park/host events
```

Does:

```txt
classify each event as normal | warn | failure | ghost | decision_needed
```

Writes:

```txt
intelligence.finding
intelligence.classification.completed event
evidence record
receipt profile candidate
current state projection
```

Does not:

```txt
restart services
execute shell
mutate canon
write another schema
call undeclared model
close broad claims
```

## 16.3 First Probe

Claim:

```txt
Intelligence App can read five events, classify them, write findings, and produce evidence/receipt-profile output without executing consequences.
```

Probe:

```txt
Seed events:
  heartbeat.ok
  health.failed
  receipt.missing
  work_item.stale
  decision.pending

Run:
  intelligence classify-once --source seeded --dry-run=false-in-test-store

Expected:
  five classifications
  at least one ghost or stale finding
  one evidence record
  one receipt-profile candidate
  zero forbidden actions attempted
```

Evidence status now:

```txt
probe-proposed
```

---

# 17 — Open Ghosts

This packet deliberately leaves these ghosts open.

## 17.1 Runtime ghosts

```txt
LAB512 Docker availability unknown in this packet
Docker Compose version unknown
Caddy/Traefik availability unknown
open ports/routes unknown
current launchctl services not freshly observed here
```

## 17.2 Supabase ghosts

```txt
current schemas unknown
RLS/grants unknown
service role path unknown
app_park schema absent/unverified
intelligence schema absent/unverified
```

## 17.3 Doppler ghosts

```txt
current project/config inventory unknown
canonical config drift unresolved
APP_PARK_* keys not implemented
secret readiness unknown
```

## 17.4 Repo/code ghosts

```txt
target repo unknown
package manager unknown
existing Control Plane package unknown
existing Authority Core package unknown
existing LogLine Foundation package paths unknown in implementation repo
```

## 17.5 Provider ghosts

```txt
runtime provider undecided
proxy provider undecided
storage provider undecided
queue implementation undecided
websocket gateway need undecided
policy engine implementation undecided
```

---

# 18 — Next Document Set

After this packet, author machine-readable documents in this order:

```txt
16 app-park.manifest.v1.schema
17 app-park.contract-schemas.v1
18 app-park.policy-pack.v1
19 app-park.sql-migration-pack.v1 draft
20 app-park.doppler-extension.v1
21 app-park.probe-catalog.v1
22 app-park.receipt-profiles.v1
23 app-park.ui-projection-contract.v1
24 lab512-runtime-inventory-receipt
25 supabase-current-schema-receipt
26 doppler-current-config-receipt
27 repo-codebase-inventory-receipt
28 network-route-inventory-receipt
29 existing-apps-inventory
30 intelligence-app-onboarding-packet
```

Do not implement deploy until at least 16, 18, 21, 22, and one runtime inventory receipt exist.

---

# 19 — First Implementation Strategy

Use the coding-making protocol:

```txt
ChatGPT:
  doctrine, task briefs, acceptance criteria, risk map

Codex:
  bounded implementation slices

Claude Code / Devin Terminal:
  repo inspection, plan mode, runtime inventory, debugging

Devin Cloud:
  later ticket-sized PRs after contracts stabilize
```

First PR should not be “build App Park.”

First PR should be:

```txt
PR-001: App Park Primordial Contracts + Intelligence App classify-once plan
```

Scope:

```txt
docs 08-15
manifest schema draft
Intelligence App manifest draft
parkctl validate/plan only
seeded classify-once test plan
no deployment
no runtime mutation
no public route
no provider mutation
```

---

# 20 — app-park.manifest.v1.schema Draft

## 20.0 Purpose

The App Park manifest schema defines the passport shape for an App Park member candidate.

It is not admission by itself.

A valid manifest means:

```txt
This app is structurally describable.
```

It does not mean:

```txt
This app is authorized.
This app is deployed.
This app is healthy.
This app is safe.
```

Admission requires policy evaluation, authority response, provider plan, probes, evidence, and receipt path.

## 20.1 Schema Principles

The manifest must:

```txt
identify the app
declare ownership
declare runtime shape
declare capabilities
declare routes
declare secrets
declare database/storage boundaries
declare communications
declare health
declare backup/rollback where applicable
declare authority prohibitions
declare receipt profiles
```

The manifest must not contain:

```txt
secret values
provider tokens
raw database credentials
unbounded shell commands
hidden natural-language dispatch
implicit public exposure
undeclared capabilities
```

## 20.2 JSON Schema Draft

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "app-park.manifest.v1",
  "title": "App Park Manifest v1",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "schema_version",
    "id",
    "name",
    "kind",
    "owner",
    "host",
    "version",
    "purpose",
    "runtime",
    "capabilities",
    "authority",
    "health",
    "receipts"
  ],
  "properties": {
    "schema_version": {
      "const": "app-park.manifest.v1"
    },
    "id": {
      "type": "string",
      "pattern": "^[a-z][a-z0-9-]{2,63}$"
    },
    "name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 120
    },
    "kind": {
      "enum": [
        "web_app",
        "api",
        "worker",
        "job_runner",
        "dashboard",
        "desktop_bridge",
        "inference_adjacent_service",
        "internal_service",
        "communication_gateway",
        "intelligence_worker"
      ]
    },
    "owner": {
      "type": "string",
      "minLength": 1
    },
    "host": {
      "enum": ["lab512", "lab8gb", "lab256", "unassigned"]
    },
    "version": {
      "type": "string",
      "minLength": 1
    },
    "status": {
      "enum": [
        "proposed",
        "admitted",
        "provisioning",
        "deployed",
        "healthy",
        "degraded",
        "blocked",
        "suspended",
        "retired"
      ],
      "default": "proposed"
    },
    "purpose": {
      "type": "object",
      "additionalProperties": false,
      "required": ["statement"],
      "properties": {
        "statement": { "type": "string", "minLength": 1 },
        "non_goals": {
          "type": "array",
          "items": { "type": "string" },
          "default": []
        }
      }
    },
    "runtime": {
      "$ref": "#/$defs/runtime"
    },
    "routes": {
      "type": "array",
      "items": { "$ref": "#/$defs/route" },
      "default": []
    },
    "capabilities": {
      "type": "array",
      "items": { "$ref": "#/$defs/capabilityGrant" },
      "minItems": 1
    },
    "secrets": {
      "$ref": "#/$defs/secrets",
      "default": { "required": [], "optional": [] }
    },
    "database": {
      "$ref": "#/$defs/database",
      "default": { "uses_database": false }
    },
    "storage": {
      "$ref": "#/$defs/storage",
      "default": { "uses_storage": false }
    },
    "communications": {
      "$ref": "#/$defs/communications",
      "default": {}
    },
    "llm": {
      "$ref": "#/$defs/llm",
      "default": { "allowed": false }
    },
    "health": {
      "$ref": "#/$defs/health"
    },
    "backup": {
      "$ref": "#/$defs/backup",
      "default": { "required": false }
    },
    "rollback": {
      "$ref": "#/$defs/rollback",
      "default": { "required": false }
    },
    "authority": {
      "$ref": "#/$defs/authority"
    },
    "receipts": {
      "$ref": "#/$defs/receipts"
    },
    "metadata": {
      "type": "object",
      "additionalProperties": true,
      "default": {}
    }
  },
  "$defs": {
    "runtime": {
      "type": "object",
      "additionalProperties": false,
      "required": ["type", "mode"],
      "properties": {
        "type": {
          "enum": [
            "docker_compose",
            "process",
            "app_park_worker",
            "scheduled_worker",
            "external_adapter",
            "none"
          ]
        },
        "mode": {
          "enum": [
            "long_running",
            "scheduled",
            "event_driven",
            "scheduled_and_event_driven",
            "finite_job",
            "plan_only"
          ]
        },
        "compose_file": { "type": "string" },
        "entrypoint": { "type": "string" },
        "services": {
          "type": "array",
          "items": { "type": "string" },
          "default": []
        },
        "ports": {
          "type": "array",
          "items": {
            "type": "object",
            "additionalProperties": false,
            "required": ["name", "container"],
            "properties": {
              "name": { "type": "string" },
              "container": { "type": "integer", "minimum": 1, "maximum": 65535 },
              "host": { "type": "integer", "minimum": 1, "maximum": 65535 },
              "protocol": { "enum": ["tcp", "udp"], "default": "tcp" }
            }
          },
          "default": []
        },
        "expected_cadence": {
          "type": "object",
          "additionalProperties": true,
          "default": {}
        }
      }
    },
    "route": {
      "type": "object",
      "additionalProperties": false,
      "required": ["hostname", "target", "exposure", "auth"],
      "properties": {
        "hostname": { "type": "string" },
        "target": { "type": "string" },
        "exposure": { "enum": ["internal", "private", "public"] },
        "auth": {
          "enum": [
            "cloudflare_access",
            "control_plane_token",
            "app_owned",
            "none"
          ]
        },
        "tls": { "type": "boolean", "default": true }
      }
    },
    "capabilityGrant": {
      "type": "object",
      "additionalProperties": false,
      "required": ["capability", "status"],
      "properties": {
        "capability": {
          "type": "string",
          "pattern": "^[a-z][a-z0-9_]*(\.[a-z][a-z0-9_]*)+$"
        },
        "status": {
          "enum": ["declared", "requested", "admitted", "denied", "ghosted", "active", "suspended", "revoked", "retired"]
        },
        "scope": {
          "type": "object",
          "additionalProperties": true,
          "default": {}
        },
        "requires_approval": { "type": "boolean", "default": false },
        "requires_receipt": { "type": "boolean", "default": false }
      }
    },
    "secrets": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "required": {
          "type": "array",
          "items": { "type": "string", "pattern": "^[A-Z0-9_]+$" },
          "default": []
        },
        "optional": {
          "type": "array",
          "items": { "type": "string", "pattern": "^[A-Z0-9_]+$" },
          "default": []
        },
        "source": {
          "enum": ["doppler", "runtime_env", "none"],
          "default": "doppler"
        },
        "values_forbidden_in_manifest": { "const": true, "default": true }
      }
    },
    "database": {
      "type": "object",
      "additionalProperties": false,
      "required": ["uses_database"],
      "properties": {
        "uses_database": { "type": "boolean" },
        "schema": { "type": "string" },
        "role": { "type": "string" },
        "migrations": { "type": "boolean", "default": false },
        "semantic_writes": { "type": "boolean", "default": false }
      }
    },
    "storage": {
      "type": "object",
      "additionalProperties": false,
      "required": ["uses_storage"],
      "properties": {
        "uses_storage": { "type": "boolean" },
        "namespaces": {
          "type": "array",
          "items": { "type": "string" },
          "default": []
        },
        "backup_included": { "type": "boolean", "default": false }
      }
    },
    "communications": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "events": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "publish": { "type": "array", "items": { "type": "string" }, "default": [] },
            "subscribe": { "type": "array", "items": { "type": "string" }, "default": [] }
          },
          "default": {}
        },
        "work_items": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "enqueue": { "type": "array", "items": { "type": "string" }, "default": [] },
            "consume": { "type": "array", "items": { "type": "string" }, "default": [] }
          },
          "default": {}
        },
        "webhooks": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "inbound": { "type": "array", "items": { "type": "object" }, "default": [] },
            "outbound": { "type": "array", "items": { "type": "object" }, "default": [] }
          },
          "default": {}
        },
        "realtime": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "channels": { "type": "array", "items": { "type": "string" }, "default": [] }
          },
          "default": {}
        },
        "websocket": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "enabled": { "type": "boolean", "default": false },
            "use_cases": { "type": "array", "items": { "type": "string" }, "default": [] }
          },
          "default": {}
        }
      }
    },
    "llm": {
      "type": "object",
      "additionalProperties": false,
      "required": ["allowed"],
      "properties": {
        "allowed": { "type": "boolean" },
        "gateway": { "type": "string" },
        "profiles": { "type": "array", "items": { "type": "string" }, "default": [] },
        "default_profile": { "type": "string" },
        "cloud_llm_allowed": { "type": "boolean", "default": false }
      }
    },
    "health": {
      "type": "object",
      "additionalProperties": false,
      "required": ["kind"],
      "properties": {
        "kind": { "enum": ["http", "tcp", "command", "composite", "manual"] },
        "url": { "type": "string" },
        "interval_seconds": { "type": "integer", "minimum": 5, "default": 60 },
        "timeout_seconds": { "type": "integer", "minimum": 1, "default": 5 },
        "expected_status": { "type": "integer", "default": 200 },
        "checks": { "type": "array", "items": { "type": "string" }, "default": [] }
      }
    },
    "backup": {
      "type": "object",
      "additionalProperties": false,
      "required": ["required"],
      "properties": {
        "required": { "type": "boolean" },
        "scope": { "type": "array", "items": { "type": "string" }, "default": [] },
        "restore_check_required": { "type": "boolean", "default": true }
      }
    },
    "rollback": {
      "type": "object",
      "additionalProperties": false,
      "required": ["required"],
      "properties": {
        "required": { "type": "boolean" },
        "strategy": { "enum": ["previous_deployment", "manual", "not_applicable"], "default": "not_applicable" }
      }
    },
    "authority": {
      "type": "object",
      "additionalProperties": false,
      "required": [
        "may_execute",
        "may_mutate_canon",
        "may_read_secret_values",
        "consequence_requires_gate"
      ],
      "properties": {
        "may_execute": { "type": "boolean" },
        "may_mutate_canon": { "type": "boolean" },
        "may_read_secret_values": { "type": "boolean" },
        "may_write_app_state": { "type": "boolean", "default": true },
        "may_emit_events": { "type": "boolean", "default": true },
        "may_enqueue_proposed_work": { "type": "boolean", "default": false },
        "consequence_requires_gate": { "type": "boolean" },
        "forbidden_actions": {
          "type": "array",
          "items": { "type": "string" },
          "default": []
        }
      }
    },
    "receipts": {
      "type": "object",
      "additionalProperties": false,
      "required": ["foundation_profile", "app_park_profiles"],
      "properties": {
        "foundation_profile": { "const": "logline.receipt.v0" },
        "app_park_profiles": {
          "type": "array",
          "items": { "type": "string", "pattern": "^app_park\.[a-z0-9_]+\.receipt_profile\.v1$" }
        },
        "evidence_required": { "type": "boolean", "default": true },
        "secret_values_forbidden": { "const": true, "default": true }
      }
    }
  }
}
```

## 20.3 Schema Ghosts

This schema is a draft until:

```txt
Foundation receipt schema is available in target repo
JSON Schema validator is selected
policy pack validates against it
sample Intelligence manifest validates
negative fixtures fail
```

---

# 21 — app-park.policy-pack.v1 Draft

## 21.0 Purpose

The policy pack defines initial machine-evaluable App Park defaults.

Policy is not a prompt.

This draft is intentionally expressed as declarative rules first. It can later lower into Rust policy code, Rego, Cedar, or Constitutional Runtime policy primitives.

## 21.1 Default Decision

```yaml
policy_pack_version: app-park.policy-pack.v1
status: draft
default_decision: denied
explain_required: true
secret_values_forbidden: true
receipt_profile_required: true
```

## 21.2 Invariants

```yaml
invariants:
  - id: app_park_no_local_receipt_canon
    rule: App Park may define receipt profiles but must not define receipt formats.
    decision_on_violation: denied

  - id: natural_language_never_executes
    rule: Natural language may create candidates/plans, not runtime effects.
    decision_on_violation: denied

  - id: apps_do_not_govern
    rule: App Park members may execute admitted work but may not govern authority/canon.
    decision_on_violation: denied

  - id: no_secret_values
    rule: Manifests, evidence, receipts, logs, reports, DB projections, and browser payloads must not contain secret values.
    decision_on_violation: denied

  - id: evidence_before_closure
    rule: Closure requires evidence refs and Foundation-conformant receipt path.
    decision_on_violation: ghosted

  - id: supabase_projects_not_governs
    rule: Supabase rows may coordinate/project; they do not authorize.
    decision_on_violation: denied

  - id: doppler_supplies_not_authorizes
    rule: Doppler key presence does not authorize use.
    decision_on_violation: denied
```

## 21.3 Manifest Validation Rules

```yaml
manifest_rules:
  - id: manifest_schema_required
    when: manifest.submitted
    require:
      - schema_version == app-park.manifest.v1
      - id.present
      - owner.present
      - kind.present
      - runtime.present
      - health.present
      - authority.present
      - receipts.foundation_profile == logline.receipt.v0
    decision_on_failure: ghosted

  - id: no_secret_values_in_manifest
    when: manifest.submitted
    forbid:
      - literal_secret_values
      - auth_headers
      - provider_tokens
      - database_urls_with_credentials
    decision_on_failure: denied

  - id: app_id_stable_format
    when: manifest.submitted
    require:
      - id.matches_slug
    decision_on_failure: denied
```

## 21.4 Route Rules

```yaml
route_rules:
  - id: default_private_exposure
    when: route.declared_without_exposure
    set:
      exposure: private

  - id: public_route_requires_approval
    when: route.exposure == public
    require:
      - dan_approval
      - route_provider_plan
      - health_probe_path
      - disable_route_path
      - receipt_profile
    command_class: protected
    decision_without_requirements: needs_approval

  - id: no_route_without_health
    when: route.declared
    require:
      - health.kind.present
    decision_without_requirements: ghosted
```

## 21.5 Runtime Rules

```yaml
runtime_rules:
  - id: deploy_is_protected
    when: action in [runtime.deploy, runtime.rollback, runtime.restart]
    command_class: protected
    require:
      - authority_core
      - gate_decision
      - execution_window_when_required
      - evidence_path
      - receipt_profile

  - id: plan_only_allowed_without_runtime
    when: action == app.plan
    command_class: normal
    allow_if:
      - manifest_valid
      - no_mutation

  - id: no_unbounded_shell
    when: runtime.entrypoint_or_command_declared
    forbid:
      - unbounded_shell_string_as_authority
      - natural_language_command
    decision_on_failure: denied
```

## 21.6 Stateful App Rules

```yaml
stateful_rules:
  - id: stateful_requires_backup
    when: database.uses_database == true or storage.uses_storage == true
    require:
      - backup.required == true
      - backup.restore_check_required == true
    decision_without_requirements: ghosted

  - id: app_schema_boundary
    when: database.uses_database == true
    require:
      - database.schema.present
      - schema_is_app_scoped
    forbid:
      - direct_registry_write
      - direct_ops_logline_act_write
      - direct_receipt_index_write_without_authority
```

## 21.7 Communication Rules

```yaml
communication_rules:
  - id: event_publish_must_be_declared
    when: event.publish_requested
    require:
      - event_type in manifest.communications.events.publish
    decision_on_failure: denied

  - id: event_subscribe_must_be_declared
    when: event.subscribe_requested
    require:
      - stream in manifest.communications.events.subscribe
    decision_on_failure: denied

  - id: work_consume_requires_declared_kind
    when: work.consume_requested
    require:
      - work_kind in manifest.communications.work_items.consume
    decision_on_failure: denied

  - id: inbound_webhook_requires_signature_by_default
    when: webhook.inbound_declared
    default:
      requires_signature: true
    decision_without_signature_unless_internal: ghosted

  - id: outbound_webhook_requires_retry_dead_letter
    when: webhook.outbound_declared
    require:
      - retry_policy
      - dead_letter_policy
      - target_ref_not_secret_value
    decision_without_requirements: ghosted

  - id: realtime_is_not_truth
    when: realtime.channel_declared
    require:
      - source_table_or_persisted_event
    forbid:
      - realtime_only_command
      - realtime_only_event

  - id: websocket_not_durable_bus
    when: websocket.enabled == true
    require:
      - declared_live_use_case
      - auth_scope
      - persistence_rule_for_important_messages
    forbid:
      - websocket_as_canonical_event_store
```

## 21.8 LLM Rules

```yaml
llm_rules:
  - id: llm_denied_by_default
    when: llm.allowed not present
    set:
      llm.allowed: false

  - id: llm_gateway_required
    when: llm.allowed == true
    require:
      - llm.gateway.present
      - llm.profiles.non_empty
      - capability.llm_profile_declared
    decision_without_requirements: ghosted

  - id: cloud_llm_escalation_protected
    when: llm.cloud_llm_allowed == true
    command_class: protected
    require:
      - explicit_policy
      - budget_scope
      - dan_approval_when_required
      - receipt_profile

  - id: llm_cannot_execute_consequence
    when: app.kind in [intelligence_worker, inference_adjacent_service]
    forbid:
      - service_restart
      - shell_execute
      - provider_mutation
      - canon_mutation
      - receipt_closure_as_authority
```

## 21.9 Intelligence App Rules

```yaml
intelligence_app_rules:
  - id: intelligence_low_authority_start
    when: manifest.id == intelligence
    require:
      - kind == intelligence_worker
      - authority.may_execute == false
      - authority.may_mutate_canon == false
      - authority.may_read_secret_values == false
      - authority.consequence_requires_gate == true

  - id: intelligence_allowed_capabilities_v0
    when: manifest.id == intelligence
    allow_capabilities:
      - event.subscribe
      - event.publish
      - work.consume
      - work.enqueue
      - llm.profile
      - health.probe
      - receipt.emit_profile
      - logs.read

  - id: intelligence_forbidden_capabilities_v0
    when: manifest.id == intelligence
    deny_capabilities:
      - shell.execute
      - service.restart
      - runtime.deploy
      - runtime.rollback
      - provider.mutation
      - database.semantic_write
      - canon.mutate
      - secret.value_read
      - route.public
      - webhook.outbound
```

---

# 22 — Intelligence App Manifest Draft

## 22.0 Purpose

This manifest describes the first candidate App Park member.

It is proposed, not admitted.

It is used to test App Park contracts, policy pack, and `parkctl validate/plan`.

## 22.1 Manifest YAML

```yaml
schema_version: app-park.manifest.v1
id: intelligence
name: Intelligence App
kind: intelligence_worker
owner: dan
host: lab512
version: 0.1.0
status: proposed

purpose:
  statement: >
    Slow continuous cognitive layer for App Park and Minilab. Observes declared
    event streams, work items, receipts, health records, and model status;
    classifies, summarizes, detects ghosts, suggests probes, and prepares
    decision requests without executing consequences directly.
  non_goals:
    - Do not execute shell commands.
    - Do not restart services.
    - Do not deploy or roll back apps.
    - Do not mutate canon or authority rules.
    - Do not read secret values.
    - Do not close broad claims.

runtime:
  type: app_park_worker
  mode: scheduled_and_event_driven
  expected_cadence:
    fast_loop_seconds: 300
    normal_loop_seconds: 900
    daily_loop: "09:00 Europe/Lisbon"

routes: []

capabilities:
  - capability: event.subscribe
    status: requested
    scope:
      streams:
        - app_park.events
        - app_park.work_items
        - app_park.health_checks
        - app_park.receipts
        - registry.runtimes

  - capability: event.publish
    status: requested
    requires_receipt: true
    scope:
      event_types:
        - intelligence.signal.created
        - intelligence.classification.completed
        - intelligence.summary.completed
        - intelligence.anomaly.detected
        - intelligence.ghost.detected
        - intelligence.probe.suggested
        - intelligence.decision.requested

  - capability: work.consume
    status: requested
    scope:
      work_kinds:
        - intelligence.classify_once
        - intelligence.summarize_window
        - intelligence.detect_ghosts
        - intelligence.prepare_daily_brief_fragment

  - capability: work.enqueue
    status: requested
    requires_receipt: true
    scope:
      work_kinds:
        - proposed_probe
        - proposed_maintenance_check
        - proposed_decision_request

  - capability: llm.profile
    status: requested
    requires_receipt: true
    scope:
      profiles:
        - mini_local_classifier
        - mini_local_summarizer
        - local_reasoning_small

  - capability: health.probe
    status: requested
    scope:
      checks:
        - worker_loop_freshness
        - llm_gateway_reachable
        - model_profile_available
        - projection_write_available
        - no_forbidden_actions_attempted

  - capability: receipt.emit_profile
    status: requested
    scope:
      profiles:
        - app_park.work_attempt.receipt_profile.v1
        - app_park.health.receipt_profile.v1
        - app_park.intelligence_finding.receipt_profile.v1

  - capability: logs.read
    status: requested
    scope:
      own_logs_only: true

secrets:
  required:
    - LLM_GATEWAY_TOKEN
    - APP_PARK_DATABASE_URL
  optional: []
  source: doppler
  values_forbidden_in_manifest: true

database:
  uses_database: true
  schema: intelligence
  role: intelligence_app
  migrations: true
  semantic_writes: false

storage:
  uses_storage: false
  namespaces: []
  backup_included: false

communications:
  events:
    publish:
      - intelligence.signal.created
      - intelligence.classification.completed
      - intelligence.summary.completed
      - intelligence.anomaly.detected
      - intelligence.ghost.detected
      - intelligence.probe.suggested
      - intelligence.decision.requested
    subscribe:
      - app_park.events
      - app_park.work_items
      - app_park.health_checks
      - app_park.receipts
      - registry.runtimes
  work_items:
    consume:
      - intelligence.classify_once
      - intelligence.summarize_window
      - intelligence.detect_ghosts
      - intelligence.prepare_daily_brief_fragment
    enqueue:
      - proposed_probe
      - proposed_maintenance_check
      - proposed_decision_request
  webhooks:
    inbound: []
    outbound: []
  realtime:
    channels:
      - app_park.events
      - app_park.work_items
      - intelligence.findings
  websocket:
    enabled: false
    use_cases: []

llm:
  allowed: true
  gateway: llm_gateway
  profiles:
    - mini_local_classifier
    - mini_local_summarizer
    - local_reasoning_small
  default_profile: mini_local_classifier
  cloud_llm_allowed: false

health:
  kind: composite
  interval_seconds: 300
  timeout_seconds: 10
  checks:
    - worker_loop_freshness
    - llm_gateway_reachable
    - model_profile_available
    - projection_write_available
    - no_forbidden_actions_attempted

backup:
  required: true
  scope:
    - intelligence.cursors
    - intelligence.findings
  restore_check_required: true

rollback:
  required: false
  strategy: not_applicable

authority:
  may_execute: false
  may_mutate_canon: false
  may_read_secret_values: false
  may_write_app_state: true
  may_emit_events: true
  may_enqueue_proposed_work: true
  consequence_requires_gate: true
  forbidden_actions:
    - shell.execute
    - service.restart
    - runtime.deploy
    - runtime.rollback
    - provider.mutation
    - database.semantic_write
    - canon.mutate
    - secret.value_read
    - route.public
    - webhook.outbound

receipts:
  foundation_profile: logline.receipt.v0
  app_park_profiles:
    - app_park.work_attempt.receipt_profile.v1
    - app_park.health.receipt_profile.v1
    - app_park.intelligence_finding.receipt_profile.v1
  evidence_required: true
  secret_values_forbidden: true

metadata:
  evidence_status: declared
  first_member_candidate: true
  runtime_verified: false
```

## 22.2 Manifest Ghosts

```txt
LLM_GATEWAY_TOKEN presence unknown
APP_PARK_DATABASE_URL presence unknown
llm_gateway availability unknown
mini_local_classifier profile unknown
Supabase intelligence schema absent/unverified
backup/restore path absent/unverified
```

---

# 23 — parkctl validate/plan Contract Draft

## 23.0 Purpose

`parkctl` is the typed App Park control CLI/API surface.

The first version is intentionally non-mutating.

It validates and plans. It does not deploy.

## 23.1 Commands v0

```bash
parkctl validate <manifest>
parkctl plan <manifest>
parkctl explain <plan-id-or-file>
```

Forbidden in v0:

```bash
parkctl deploy
parkctl rollback
parkctl restart
parkctl route-create
parkctl webhook-replay --apply
```

## 23.2 `parkctl validate`

Input:

```txt
manifest path
schema path
policy pack path
```

Checks:

```txt
JSON/YAML parse
schema_version
required fields
no secret values
capability vocabulary
receipt profile references
authority prohibitions
stateful backup rule
communication declarations
LLM rules
Intelligence App special rules when id=intelligence
```

Output shape:

```json
{
  "command": "parkctl.validate",
  "status": "valid|invalid|ghosted",
  "manifest_id": "intelligence",
  "schema_version": "app-park.manifest.v1",
  "errors": [],
  "ghosts": [],
  "warnings": [],
  "secret_values_printed": false
}
```

## 23.3 `parkctl plan`

Input:

```txt
valid manifest
policy pack
provider registry draft
current inventory receipts when available
```

Produces:

```txt
membership plan
capability plan
provider plan
required config requirements
required database schemas
required events/work tables
required probes
required evidence records
required receipt profiles
ghosts
forbidden actions confirmed
```

Output shape:

```json
{
  "command": "parkctl.plan",
  "status": "planned|blocked|ghosted",
  "manifest_id": "intelligence",
  "plan_id": "plan_intelligence_001",
  "mutation": false,
  "membership": {
    "state": "proposed",
    "target_state": "admitted"
  },
  "capabilities": {
    "requested": [],
    "allowed_by_policy": [],
    "needs_approval": [],
    "denied": [],
    "ghosted": []
  },
  "provider_requirements": [],
  "config_requirements": [],
  "database_requirements": [],
  "probe_requirements": [],
  "evidence_requirements": [],
  "receipt_profiles": [],
  "forbidden_actions": [],
  "ghosts": [],
  "secret_values_printed": false
}
```

## 23.4 Intelligence App Expected Plan

For the Intelligence App manifest, `parkctl plan` should produce:

```txt
proposed member: intelligence
host candidate: lab512
runtime type: app_park_worker
no routes
no webhooks
no websocket
LLM Gateway required
local-small model profiles required
intelligence schema required
app_park.events/work_items/receipts projections required
backup required for intelligence.cursors and intelligence.findings
forbidden actions confirmed
classify-once probe required
Foundation receipt profile required
```

## 23.5 Negative Test Cases

`parkctl validate` must reject or ghost:

```txt
manifest with secret value
manifest with public route and no approval requirement
manifest with llm.allowed true but no gateway/profile
manifest with database.uses_database true but no backup
manifest with app_park.receipt_format.v1
manifest with may_mutate_canon true
manifest with Intelligence App shell.execute capability
manifest with outbound webhook target as raw URL secret value
manifest with realtime-only command source
```

---

# 24 — First Probe Catalog Draft

## 24.0 Purpose

Probe Catalog defines the first reality contacts needed before implementation claims.

## 24.1 Contract Probes

```txt
schema.validate.manifest
policy.evaluate.manifest
parkctl.validate.intelligence
parkctl.plan.intelligence
negative.secret_value_rejected
negative.receipt_format_rejected
negative.intelligence_shell_execute_rejected
```

## 24.2 Runtime Inventory Probes

Not run yet:

```bash
hostname
uptime
docker --version
docker compose version
which caddy || true
which traefik || true
launchctl list | grep -Ei 'minilab|docker|caddy|traefik|gateway|mistral'
lsof -iTCP -sTCP:LISTEN -n -P | head -80
```

## 24.3 Supabase Inventory Probes

Not run yet:

```txt
list schemas
list app_park tables if present
list intelligence tables if present
check RLS/grants for custom schemas
check receipt_index existence
check logline_acts existence
```

## 24.4 Doppler Inventory Probes

Not run yet:

```txt
list project/config names
check canonical project minilab
check canonical configs
check APP_PARK_* key names only
check forbidden variables absent
run config doctor when available
```

## 24.5 Intelligence classify-once Probe

Claim:

```txt
Intelligence App can classify seeded events and emit findings/evidence/receipt-profile candidate without consequence execution.
```

Seed:

```json
[
  { "event_type": "heartbeat.ok", "severity": "info", "status": "ok" },
  { "event_type": "health.failed", "severity": "error", "status": "failed" },
  { "event_type": "receipt.missing", "severity": "warn", "status": "ghost" },
  { "event_type": "work_item.stale", "severity": "warn", "status": "blocked" },
  { "event_type": "decision.pending", "severity": "info", "status": "waiting" }
]
```

Expected:

```txt
five classifications
at least one ghost finding
at least one stale/blocked finding
one evidence record
one receipt-profile candidate
zero forbidden actions attempted
```

---

# 25 — PR-001 Implementation Brief Draft

## Goal

Implement the non-mutating App Park machine-contract layer and Intelligence App first-member plan.

## Scope

```txt
docs/app-park/08-15 primordial docs
schemas/app-park.manifest.v1.json
policies/app-park.policy-pack.v1.yaml
apps/intelligence/app.manifest.yaml
parkctl validate
parkctl plan
test fixtures for positive and negative manifests
seeded classify-once probe specification only, or pure test-store implementation if repo already has suitable test harness
```

## Non-goals

```txt
Do not deploy anything.
Do not create public routes.
Do not mutate Supabase production.
Do not read Doppler secret values.
Do not call LAB512 runtime.
Do not call real LLM Gateway unless explicitly in a later runtime probe.
Do not create a new receipt format.
Do not claim Intelligence App exists operationally.
```

## Acceptance Criteria

```txt
Intelligence manifest validates.
Plan output lists capabilities, providers, probes, config requirements, database requirements, and ghosts.
Negative fixtures fail.
No secret values appear in outputs.
app_park.receipt_format.v1 is rejected.
Intelligence App shell.execute capability is rejected.
Stateful database use without backup is ghosted.
LLM access without gateway/profile is ghosted.
Final artifact honesty receipt lists unverified runtime/database/Doppler paths.
```

## Work Style

```txt
Inspect existing repo structure first.
Add smallest coherent docs/schemas/CLI surface.
No runtime side effects.
Run schema/policy tests.
Return changed files, probes run, output summary, ghosts, and next step.
```

---

# Artifact Honesty Receipt

Intent Coverage:

```txt
Extended the App Park primordial packet into the first machine layer: manifest schema draft, policy pack draft, Intelligence App manifest draft, parkctl validate/plan contract, probe catalog, and PR-001 brief.
Preserved the correction that these are prerequisites before implementation/deployment.
```

Implemented:

```txt
app-park.manifest.v1 schema draft.
app-park.policy-pack.v1 draft.
Intelligence App first-member manifest draft.
parkctl validate/plan contract.
Initial probe catalog.
PR-001 implementation brief.
```

Changed:

```txt
Canvas document only.
No repo, runtime, database, Doppler, LAB, provider, route, or app state changed.
```

Stubbed:

```txt
JSON Schema has not been run through a validator.
Policy pack has not been lowered into Rust/Rego/Cedar/runtime policy.
parkctl has not been implemented.
Intelligence classify-once worker has not been implemented.
Supabase migrations have not been authored.
Receipt profiles are named but not yet fully specified.
```

Unverified:

```txt
Target repo structure.
Foundation receipt schema availability.
LAB512 runtime readiness.
Doppler current config state.
Supabase schema/RLS/grants.
LLM Gateway and model profile availability.
```

Probes:

```txt
No runtime/tool probes run in this turn.
This is artifact drafting from uploaded canon and prior App Park dialogue.
```

Ghosts:

```txt
Runtime inventory ghost.
Supabase inventory ghost.
Doppler inventory ghost.
Repo inventory ghost.
Foundation schema integration ghost.
Provider choice ghost.
```

Desk Clean:

```txt
status: yes
notes:
  - Next step is either to package PR-001 as a Codex brief or to write the actual schema/policy files in a target repo once supplied.
  - Do not treat this packet as deployment authorization.
```

Next:

```txt
Choose target repo/path, then run repo inspection and implement PR-001 with no runtime mutation.
```

