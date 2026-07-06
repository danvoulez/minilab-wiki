# 02 — Characters / Ontology Canon

**Status:** Canon draft  
**Layer:** Minilab named-world ontology  
**Parent:** 01 — Minilab Constitution / Strategic Architecture  
**Scope:** Characters, entities, families, roles, permissions boundaries, relations, evidence status, and UI projection vocabulary for `minilab.work`  
**Downstream documents:** Authority / Power Doctrine, Evidence / Receipt Doctrine, Dan Control Plane Doctrine, Database Projection Canon, Doppler / Config Canon, App Park Doctrine

---

## 0. Purpose

This document defines the named-world ontology of Minilab.

It answers:

```txt
What exists?
What kind of thing is it?
What may it do?
What may it not do?
What is it related to?
What evidence supports its current state?
What must never be confused with authority?
```

This document exists to prevent ontology drift.

Without it, dashboards become truth, databases become brains, LAB IDs become authority, apps invent identity, LLMs impersonate operators, secrets become policy, and receipts become stories.

The ontology canon gives Minilab a stable vocabulary of characters and relations. It does not itself grant power. It names the world so Authority, Registry, UI, Database, App Park, and Reports can refer to the same things without reinventing identity.

---

## 1. Core Ontology Law

```txt
Everything important gets identity.
Not every identity gets power.
```

Identity is addressability.
Role is classification.
Capability is shape.
Permission is authority.
Execution is runtime contact.
Closure is receipt.

These states must never collapse:

```txt
entity exists ≠ authority
role exists ≠ permission
permission exists ≠ execution
execution exists ≠ closure
closure requires receipt
```

An entity can be known and still be powerless.
A runtime can exist and still be unauthorized.
A LAB can execute and still not govern.
An app can be registered and still not be admitted.
A receipt can be indexed and still not prove more than its scope.

---

## 2. Product Context

The root context is:

```txt
LogLine               = technology / canon / grammar
LogLine               = company / steward / institutional brand
LogLine Automation    = product line
minilab.work          = Dan’s operational universe
Dan Control Plane     = cockpit / local UI for this instance
```

This document describes the characters of the **Minilab universe**, including the adjacent LogLine, Dan, and operational Minilab domains.

Important boundary:

```txt
LogLine Company is not the operational authority of Minilab.
LogLine Technology is the parent grammar/canon.
LogLine Automation is the product line.
LogLine Automation for minilab.work is the instance.
minilab.work is Dan’s operational universe.
Dan Local is the immediate authority context.
```

Operational authority belongs to the Minilab instance under Dan’s consequence boundary, not to the company name as an entity.

---

## 3. Families

Every character belongs to one primary family.

```ts
type CharacterFamily =
  | "logline"
  | "dan"
  | "minilab"
  | "authority"
  | "lab"
  | "agent"
  | "store"
  | "edge"
  | "engine"
  | "work"
  | "proof"
  | "app"
  | "provider"
  | "physical"
  | "ui";
```

Family is not permission. It is a top-level ontology grouping.

---

## 4. Evidence Status

Every character and relation carries an evidence status.

```ts
type EvidenceStatus =
  | "declared"
  | "observed"
  | "verified"
  | "unverified";
```

### declared

The system has a canonical declaration, plan, registry seed, manifest, or doctrine entry.

### observed

The system has seen runtime evidence or operational signal, but not enough to close broad claims.

### verified

A scoped receipt or conformance proof supports the claim within declared scope.

### unverified

The system lacks sufficient proof, runtime contact, or receipt.

Rule:

```txt
Do not upgrade evidenceStatus by narrative.
Only probes, observations, receipts, or conformance results may upgrade evidence status.
```

---

## 5. Lifecycle Status

Every character and relation also carries lifecycle status.

```ts
type CharacterStatus =
  | "planned"
  | "active"
  | "partial"
  | "suspended"
  | "ghost"
  | "retired";
```

### planned

Known and intended, but not yet operationally active.

### active

Operationally present and currently part of the world.

### partial

Partly implemented, observed, or connected, but not complete across intended scope.

### suspended

Temporarily prevented from acting or being used.

### ghost

Identity or relation is blocked by missing proof/context/runtime/authority.

### retired

No longer active but preserved historically.

---

## 6. Base Character Shape

```ts
type Character = {
  id: string;
  name: string;
  family: CharacterFamily;
  kind: string;
  role: string;
  may: string[];
  mayNot: string[];
  status: CharacterStatus;
  evidenceStatus: EvidenceStatus;
  notes?: string[];
  sourceRefs?: string[];
  metadata?: Record<string, unknown>;
};
```

Norms:

```txt
id is stable.
name is human-readable.
family is ontology grouping.
kind is specific type.
role is operational meaning.
may lists possible allowed behavior, not unconditional authority.
mayNot lists hard prohibitions.
status is lifecycle.
evidenceStatus is proof state.
```

`may` is not direct permission. It is capability vocabulary. Authority still comes from Gate, Policy, Tower, ExecutionWindow, Visa, or Dan signature where required.

---

## 7. Base Relation Shape

```ts
type CharacterRelation = {
  id: string;
  from: string;
  relation: RelationKind;
  to: string;
  status: "active" | "planned" | "partial" | "ghost" | "retired";
  evidenceStatus: EvidenceStatus;
  sourceRefs?: string[];
  metadata?: Record<string, unknown>;
};
```

Relations are projected statements about the named world.

A relation is not authority unless an Authority document explicitly defines it as part of an admitted rule.

---

## 8. Relation Kinds

Core relation vocabulary:

```ts
type RelationKind =
  | "stewards"
  | "defines"
  | "powers"
  | "instantiated_as"
  | "operates"
  | "controls"
  | "contains"
  | "speaks"
  | "calls"
  | "emits"
  | "runs"
  | "prepares"
  | "governs"
  | "authorizes"
  | "requires"
  | "executes"
  | "translates"
  | "proposes"
  | "produces"
  | "closes"
  | "projects_to"
  | "stores"
  | "serves"
  | "exposes_via"
  | "located_in"
  | "depends_on"
  | "configured_by"
  | "represented_by"
  | "observed_by"
  | "triggered"
  | "summarizes"
  | "cites"
  | "blocks"
  | "supports"
  | "contradicts";
```

Relation meaning:

```txt
stewards       caretaker relationship, not necessarily operational control
defines        semantic/canonical definition
powers         enables or provides grammar/capability foundation
instantiated_as product or technology realized as specific instance
operates       acts as operational surface for a universe
controls       human/authority control relation
contains       membership/containment relation
speaks         executable body speaks grammar/canon
calls          invokes another component
emits          produces receipts/events/evidence
runs           runs probes/jobs/workloads
prepares       prepares but does not necessarily execute
governs        semantic or operational governance relation
authorizes     grants permission/power under doctrine
requires       dependency or precondition
executes       performs admitted work
translates     converts intent/context into candidate structure
proposes       suggests candidate acts/work/policies
produces       outputs event/evidence/report/receipt
closes         establishes scoped proof closure
projects_to    maps proof/event/state into projection
stores         stores records, not necessarily truth
serves         exposes views or read surfaces
exposes_via    uses an edge/surface for ingress/presentation
```

---

## 9. Family: LogLine

These characters belong to the parent technology/canon layer.

```ts
type LogLineCharacterKind =
  | "company"
  | "technology"
  | "canon"
  | "grammar"
  | "runtime"
  | "receipt_profile"
  | "product_line";
```

### 9.1 LogLine Company

```txt
kind: company
role: steward / product company
not: runtime, Minilab authority, customer universe
```

May:

```txt
steward canon
ship products
maintain Foundation governance
```

May not:

```txt
operate Minilab power by name alone
replace Dan’s consequence boundary
act as LAB authority
```

### 9.2 LogLine Technology

```txt
kind: technology
role: grammar for consequential acts
not: app, dashboard, Minilab database
```

May:

```txt
define act shape
provide grammar
support conformance
```

May not:

```txt
be customer database
be App Park domain schema
be Control Plane UI
```

### 9.3 LogLine Canon

```txt
kind: canon
role: minimum grammar / shape / conformance
not: every app schema
```

May:

```txt
define receipt and act mold
anchor conformance
```

May not:

```txt
be rewritten by Minilab locally
be extended by hidden operational shortcuts
```

### 9.4 LogLine Grammar

```txt
kind: grammar
slots:
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

May:

```txt
shape consequential acts
force explicit doubt/not paths
```

May not:

```txt
execute by itself
replace runtime observation
```

### 9.5 LogLine Runtime / Walk

```txt
kind: runtime
role: shape, walk, digest, lifecycle, doubt/status
not: Minilab Tower, business authority
```

May:

```txt
walk act lifecycle
support digest/lifecycle behavior
```

May not:

```txt
authorize operational power
become LAB governor
```

### 9.6 LogLine Receipt Profile

```txt
kind: receipt_profile
role: semantic receipt/hash conventions
not: storage backend
```

May:

```txt
define receipt conformance
provide hash semantics
```

May not:

```txt
store every receipt
replace evidence record stores
```

### 9.7 LogLine Automation

```txt
kind: product_line
role: applies LogLine-shaped acts to operational universes
instances:
  LogLine Automation for minilab.work
```

May:

```txt
apply LogLine to specific worlds
provide product pattern
```

May not:

```txt
replace LogLine canon
collapse all instances into one authority
```

---

## 10. Family: Dan

These characters carry human authority, intention, signature, and consequence.

```ts
type DanCharacterKind =
  | "human"
  | "authority_context"
  | "signature"
  | "intent_source"
  | "operator_surface";
```

### 10.1 Dan

```txt
kind: human
role: owner, intent origin, approver, doubt arbiter, consequence signer
```

May:

```txt
declare intent
approve gate
deny gate
ghost gate
issue consequential visa
revoke visa
sign receipt where human consequence matters
accept risk
```

May not:

```txt
pretend evidence
make missing proof true by preference
turn provider success into closure
```

### 10.2 Dan Local

```txt
kind: authority_context
role: local authority context for minilab.work
```

Controls:

```txt
current policy profile
current LAB context
local cockpit actions
mode / safety context
```

Is not:

```txt
global LogLine authority
Foundation governance
unbounded superuser exception
```

### 10.3 Dan Signature

```txt
kind: signature
role: consequence signature
```

Signs:

```txt
approvals
consequential visas
risk acceptance
receipt closure where human consequence matters
```

Does not sign:

```txt
runtime truth without evidence
provider claims without probe
broad safety from narrow evidence
```

### 10.4 Dan Control Plane

```txt
kind: operator_surface
role: cockpit / UI for LogLine Automation for minilab.work
```

Renders:

```txt
authority
places
gates
evidence
receipts
ghosts
reports
relations
settings
```

May:

```txt
compose requests
show evidence
invoke typed actions through backend/Authority Core
render Registry
```

May not:

```txt
be source of truth
execute directly
be canon
mutate database directly from browser
close receipts by UI action alone
```

---

## 11. Family: minilab.work

This is the operational universe.

```ts
type MinilabCharacterKind =
  | "platform"
  | "institution"
  | "authority_runtime"
  | "power_layer"
  | "execution_place"
  | "projection_place"
  | "material_store"
  | "edge"
  | "engine"
  | "app"
  | "resource"
  | "proof_object";
```

### 11.1 minilab.work

```txt
kind: platform / operational_universe
role: Dan’s Minilab universe; operational home for LogLine Automation
```

Contains:

```txt
LABs
Tower
Constitutional Runtime
Supabase/Postgres
Doppler
Gate machinery
Receipts
Workorders
Agents
App Park, when admitted
```

May:

```txt
host LABs
route power through Tower
index acts
project receipts
present operational world to Dan
```

May not:

```txt
be universal LogLine registry
replace Foundation canon
make local shortcuts into law
```

### 11.2 Minilab Constitutional Runtime

```txt
kind: authority_runtime
role: admissibility, semantic authority, evidence closure, consequence lifecycle
```

Governs:

```txt
shape
policy/gate admissibility
receipt closure boundary
IR/capability/evidence discipline
```

May:

```txt
evaluate candidates
govern admissibility
plan evidence requirements
close semantic evidence within scope
```

May not:

```txt
be Mac admin
be cloud dashboard
be execution host
replace Tower for operational power
```

### 11.3 Rust Minilab CLI / Rust Institutional Body

```txt
kind: executable_body
role: first executable institutional body; mouth of the institution
```

Does:

```txt
materializes acts
validates grammar
calls canon/runtime/gate/ledger crates
emits receipts
runs probes
structures workorders
```

May:

```txt
speak LogLine canon
run admitted probes
emit receipts
prepare Tower operations
```

May not:

```txt
invent LogLine canon
become all organs
create runtime parallel
create ledger parallel
create gate parallel
```

---

## 12. Family: Authority / Power

These characters represent power, admissibility, and scoped authority.

### 12.1 Tower

```txt
kind: power_layer
role: operational power, protected commands, Passkey, scoped windows, heartbeats, consequential routing
```

Authorizes:

```txt
protected power
scoped execution windows
operational handoff
```

May:

```txt
authorize protected commands
open execution windows
require passkey windows
route consequential power
```

May not:

```txt
define semantic runtime
replace LogLine canon
close evidence without receipt
```

### 12.2 Gate

```txt
kind: authority_boundary
role: pre-effect boundary
```

Decides:

```txt
ok
denied
needs_approval
ghost
error
```

Requires:

```txt
policy
scope
evidence path
sometimes visa/passkey/window
```

May:

```txt
block unsafe work
require approval
turn missing proof into ghost
```

May not:

```txt
be bypassed by UI
be replaced by prompt
turn confidence into permission
```

### 12.3 Policy

```txt
kind: executable_rule
role: machine-evaluable rule derived from authority
```

May:

```txt
decide admitted capability under declared conditions
explain denial/blocking
feed Gate decisions
```

May not:

```txt
exist only as prompt
be hidden social convention
mutate itself without authority
```

### 12.4 ExecutionWindow

```txt
kind: scoped_permission
role: one-time scoped execution permission
```

Attributes:

```txt
actor
action
resource
expires_at
consumed_at
scope_hash
```

May:

```txt
allow one scoped execution
expire
be consumed
produce receipt requirement
```

May not:

```txt
be general approval
be reused silently
expand its own scope
```

### 12.5 PasskeyWindow

```txt
kind: human_power_window
role: human-approved protected power interval
```

May:

```txt
prove human presence/approval for protected power
bound execution in time
```

May not:

```txt
authorize unrelated actions
replace receipt closure
```

### 12.6 Visa

```txt
kind: scoped_authorization
role: authorization for a passport/use/scope
```

May:

```txt
authorize a specific use under scope
be revoked
expire
```

May not:

```txt
be boolean permission
become unbounded platform access
```

### 12.7 Passport

```txt
kind: registration_receipt
role: recognized existence
```

May:

```txt
register an entity’s known existence
support addressability
```

May not:

```txt
grant permission
imply authority
```

---

## 13. Family: LABs / Machines

LABs are local execution environments. They do not govern.

### 13.1 LAB-8GB

```txt
kind: execution_place / machine
role: supervisor, health loops, recovery checks, stable evidence bridge
```

May:

```txt
observe health
emit heartbeat receipt
run supervisor loops under admitted scope
support control-plane-adjacent checks
```

May not:

```txt
govern
authorize execution
sign consequence
```

### 13.2 LAB-256

```txt
kind: execution_place / machine
role: workbench, manual intervention, browser/macOS automation, tests
```

May:

```txt
run approved probes
capture evidence
support manual intervention
run browser/macOS automation under gate
```

May not:

```txt
govern
approve itself
authorize execution
be hidden dependency for core uptime
```

### 13.3 LAB-512

```txt
kind: execution_place / machine
role: inference, private inference, heavy jobs, model health, App Park runtime candidate
```

May:

```txt
run inference jobs with visa/admission
host heavy jobs
host App Park workloads when admitted
emit model/host health evidence
```

May not:

```txt
govern
authorize execution
become Control Plane authority
```

### 13.4 LAB Runner

```txt
kind: runner
role: executes scoped jobs on LAB identity
```

Requires:

```txt
runner_id
capability_scope
heartbeat
job receipt
```

May:

```txt
execute admitted jobs
emit job evidence
```

May not:

```txt
invent scope
execute without admission
self-authorize
```

---

## 14. Family: Agents / LLMs / Translators

Agents translate, propose, summarize, and assist. They do not govern or execute directly.

### 14.1 ChatGPT

```txt
kind: agent_surface
role: natural-language face, translator, review assistant, operator surface, MCP client
```

May:

```txt
propose
translate
summarize
call tools through gates
produce candidate workorders
review receipts
```

May not:

```txt
replace grammar
execute directly
hold critical credential
close evidence
sit behind dispatcher
```

### 14.2 ChatGPT Agent

```txt
kind: agent
role: workflow/operator layer
```

May:

```txt
create candidate
draft workorder
summarize receipt
prepare report
```

May not:

```txt
sit behind dispatcher
mutate providers directly
sign consequence
```

### 14.3 Local LLM

```txt
kind: llm
role: classify, fill slots, summarize receipts, propose candidates
```

May:

```txt
classify
fill slots
summarize receipts
propose candidates
run local reasoning inside bounds
```

May not:

```txt
execute
approve
mutate
close receipt
```

### 14.4 Cloud LLM

```txt
kind: llm
role: ambiguity, synthesis, planning, comparison
```

Gets:

```txt
context-pack
workorder
ghosts
probes
```

May:

```txt
synthesize
compare
review
plan
```

May not:

```txt
discover workspace freely
execute
hold critical credentials
mutate state
```

### 14.5 Pocket Runtime

```txt
kind: language_state_runtime
role: prevents thought from becoming action without grammar/gate/receipt
```

May:

```txt
maintain claim/probe/evidence separation
record ghosts
reject false closure
```

May not:

```txt
simulate runtime evidence
claim receipts it does not have
```

---

## 15. Family: Stores / Projection / Material

Stores provide memory, projection, and material. They do not govern.

### 15.1 Supabase/Postgres for minilab.work

```txt
kind: operational_postgres
role: Postgres for Minilab LogLine-shaped acts, registry, projections, audit views, receipt index
```

Stores:

```txt
ops.logline_acts
registry.entities
registry.links
registry.runtimes
registry.config_requirements
ops.receipt_index
lab_observability events/projections
release/config projections
```

May:

```txt
store LogLine-shaped acts
project registry
serve audit views
index receipts
support UI read models
```

May not:

```txt
be universal LogLine registry
be semantic canon
govern
execute
act as gate by itself
store secret values
```

### 15.2 Doppler

```txt
kind: material_store
role: secrets, configs, brakes, windows, provider credentials
```

May:

```txt
store material
supply material after gate/engine validation
provide config values to runtime
hold brakes and flags
```

May not:

```txt
authorize
decide gate
close evidence
expose secret values
define institutional meaning
```

### 15.3 Receipt Store / Ledger

```txt
kind: proof_store
role: stores/links receipts and evidence
```

May:

```txt
store content-addressed receipts
link evidence records
support verification
```

May not:

```txt
be mere logs
overclaim receipt scope
store secret values
```

### 15.4 Registry

```txt
kind: projection
role: named current world
```

Includes:

```txt
entities
links
runtimes
config requirements
mandates
app members when admitted
```

May:

```txt
name current world
provide stable identity
support relations
feed UI and reports
```

May not:

```txt
be canon
be CRUD pretending to be constitution
own domain tables for every app
```

---

## 16. Family: Edge / Gateway / SDKs

These are surfaces and machinery.

### 16.1 Cloudflare Gateway

```txt
kind: edge_projection
role: public ingress, edge transport, hosted projection, deployment target
```

Flow:

```txt
external request
→ normalize
→ project to LogLine-shaped candidate
→ gate/policy admission
→ response/receipt
```

May:

```txt
normalize external requests
project to candidate
return admission response
expose edge routes
```

May not:

```txt
be throne
be root institution
define semantics
mutate LABs directly
```

### 16.2 MCP Local

```txt
kind: governed_host_surface
role: local host tools under gates
```

May:

```txt
expose local tools under admission
execute scoped host tasks
capture observations
```

May not:

```txt
be unmanaged break-glass default
execute natural language directly
```

### 16.3 SDKs

```txt
kind: rented_machinery
role: tested external machinery
```

Examples:

```txt
GitHub
Vercel Sandbox
browser automation
provider SDKs
cloud provider clients
```

May:

```txt
perform provider operations under admitted scope
supply provider responses as evidence inputs
```

May not:

```txt
be institution
be law
be semantic authority
turn provider success into closure
```

### 16.4 Vercel Sandbox

```txt
kind: sandbox_engine
role: isolated execution adapter
```

Requires:

```txt
gate
execution window
evidence capture
```

### 16.5 GitHub

```txt
kind: repo_provider
role: repo read/write provider
```

Mutations require:

```txt
gate
provider mutation visa/window
receipt path
```

---

## 17. Family: Work / Proof / Operations

These are operational objects.

### 17.1 Workorder

```txt
kind: work_object
role: structured intent / build unit
```

Can be:

```txt
draft
candidate
to-logline
probed
closed
ghosted
```

### 17.2 Candidate

```txt
kind: proposed_act
role: pre-admission object
```

Sources:

```txt
human
LLM
expedition
ghost
report
```

May:

```txt
be evaluated
be rejected
be admitted
be ghosted
```

May not:

```txt
execute by existing
mutate as proposal
```

### 17.3 Probe

```txt
kind: reality_contact
role: smallest meaningful test/query/command
```

May:

```txt
produce observation
collapse uncertainty
support evidence record
```

May not:

```txt
be tautological
avoid risky path
prove more than it observed
```

### 17.4 Evidence

```txt
kind: observed_output
role: stdout/stderr/diff/test/status/hash/screenshot/HTTP/database result
```

May:

```txt
support receipt
feed reports
be indexed
```

May not:

```txt
be closure by itself
contain secrets
claim broad truth without scope
```

### 17.5 Receipt

```txt
kind: proof_closure
role: scoped memory of what happened
```

May:

```txt
close scope
prove observation within scope
index evidence
project to database
```

May not:

```txt
be story
be provider success
contain secret values
overclaim
```

### 17.6 Ghost

```txt
kind: structured_absence
role: missing proof/context/runtime/authority
```

May:

```txt
preserve missing proof
block false closure
request context/probe/signature
```

May not:

```txt
be hidden OK
be ignored by UI
be rewritten as success
```

### 17.7 Expedition

```txt
kind: observation_loop
role: observes drift
```

Produces:

```txt
candidate findings
ghosts
risk notes
maintenance suggestions
receipt requests
```

May not:

```txt
govern
mutate directly
close receipts without evidence
```

### 17.8 Incident

```txt
kind: containment_record
role: records/contains operational problem
```

### 17.9 MaintenancePlan

```txt
kind: planned_intervention
role: authorized observable auditable maintenance
```

### 17.10 Snapshot

```txt
kind: state_freeze
role: backup/recovery proof surface
```

### 17.11 RestoreCheck

```txt
kind: recovery_probe
role: proves backup is not just hope
```

---

## 18. Family: Apps / App Park Extension Point

App Park is downstream of this ontology.

This document does not yet define App Park membership in full. It reserves the ontology shape.

### 18.1 App

```txt
kind: app
role: domain responsibility and runtime payload
```

May:

```txt
own app-domain behavior
reference registry entities
consume admitted capabilities
emit events/receipts/evidence through contracts
```

May not:

```txt
invent parallel identity
write canon directly
self-grant capabilities
own platform authority
```

### 18.2 App Park

```txt
kind: managed_application_place
role: admitted application execution zone, primarily on LAB-512
```

May:

```txt
host admitted member apps
provide shared infra
project app state
emit App Park evidence and receipts
```

May not:

```txt
be Control Plane authority
define receipt canon
mutate outside admitted capabilities
```

### 18.3 App Park Member

```txt
kind: app_member
role: admitted runtime citizen with declared capabilities
```

May:

```txt
run under manifest
consume granted shared infra
emit events
receive routes
use storage/database/secrets under contract
```

May not:

```txt
be merely a running container
bypass manifest
self-escalate
access another app’s data without grant
```

Detailed App Park membership is defined in the future App Park Membership Canon.

---

## 19. Core Graph

The core graph is:

```txt
LogLine Company
  stewards → LogLine Technology

LogLine Technology
  defines → LogLine Canon
  powers → LogLine Automation

LogLine Automation
  instantiated_as → LogLine Automation for minilab.work

LogLine Automation for minilab.work
  operates → minilab.work
```

```txt
Dan
  controls → Dan Local
  signs → consequence
  approves → Gates
  issues/revokes → Visas
```

```txt
minilab.work
  contains → LAB-8GB
  contains → LAB-256
  contains → LAB-512
  contains → Tower
  contains → Constitutional Runtime
  contains → Supabase/Postgres
  contains → Doppler
  exposes_via → Cloudflare Gateway
```

```txt
LogLine Canon
  defines → 9-slot grammar

Rust Minilab CLI
  speaks → LogLine Canon
  calls → Constitutional Runtime
  emits → Receipts
  runs → Probes
  prepares → Tower
```

```txt
Constitutional Runtime
  governs → admissibility
  evaluates → candidate acts
  closes → semantic evidence

Tower
  authorizes → protected power
  opens → ExecutionWindow
  requires → PasskeyWindow

LABs
  execute → approved jobs
  emit → execution receipts
```

```txt
ChatGPT / LLMs
  translate → intent into candidates
  propose → workorders/policies/receipts
  never_execute → commands

Gate
  requires → policy/scope/evidence path
  produces → decision

Decision
  requires → enforcement

Execution
  produces → evidence

Evidence
  closes → receipt

Receipt
  projects_to → Supabase/Postgres

Supabase/Postgres
  stores → ops.logline_acts
  projects → registry/entities/links/runtimes
  serves → audit views
```

---

## 20. Machine-Readable Universe Shape

```ts
type MinilabUniverse = {
  version: "minilab-universe.v0";

  productContext: {
    company: "LogLine";
    technology: "LogLine";
    productLine: "LogLine Automation";
    instance: "LogLine Automation for minilab.work";
    cockpit: "Dan Control Plane";
  };

  characters: Character[];
  links: CharacterRelation[];
};
```

Minimum seed characters:

```txt
logline_company
logline_technology
logline_canon
logline_grammar
logline_automation
minilab_work
dan
dan_local
dan_signature
dan_control_plane
rust_minilab_cli
constitutional_runtime
tower
gate
policy
execution_window
passkey_window
visa
passport
lab_8gb
lab_256
lab_512
lab_runner
chatgpt
local_llm
cloud_llm
pocket_runtime
supabase_minilab
doppler
receipt_store
registry
cloudflare_gateway
mcp_local
sdk_provider
workorder
candidate
probe
evidence
receipt
ghost
expedition
incident
maintenance_plan
snapshot
restore_check
app_park
```

---

## 21. Registry Projection Rules

Characters and relations should project into Registry.

Minimum rules:

```txt
1. Every important character becomes a registry entity.
2. Every important relation becomes a registry link.
3. No app may invent parallel identity.
4. App/domain schemas may specialize, but must reference registry entities.
5. Evidence status must be visible.
6. Lifecycle status must be visible.
7. UI may render Registry; UI does not own Registry truth.
8. Semantic writes must come from admitted LogLine-shaped acts.
```

Registry is a named-world projection. It is not the canon.

---

## 22. UI Projection Rules

The Control Plane should present this ontology as a world map, not as raw backend ontology.

Primary UI locations:

```txt
Registry → Entities
Registry → Places
Registry → Machines
Registry → Sensors
Registry → Agents
Registry → Repos
Registry → Packages
Registry → Services
Registry → Relations
Observe → Physical/Digital object details
Settings → Runtimes / Policies / Secrets / Access
Inspector → Context / Relations / Evidence / Trace / JSON
```

Right rail pattern for any character:

```txt
Title
Kind
Role
Status
Evidence status
Can
Cannot
Relations
Evidence
Missing proof
Raw JSON
```

Example:

```txt
LAB-256

Kind
machine / execution place

Role
workbench

Status
active

Evidence status
observed

Can
run approved probes
capture evidence

Cannot
govern
approve itself
authorize execution

Relations
contained by minilab.work
authorized by Tower
emits execution receipts
```

The UI must not expose ontology as a sidebar taxonomy dump. The sidebar is routine; the Registry/Inspector reveal ontology.

---

## 23. Anti-Patterns

Reject designs where:

```txt
LogLine Company becomes Minilab operational authority
LogLine Technology becomes an app schema
LAB ID becomes permission
Role becomes authorization
Registry becomes CRUD constitution
Supabase becomes semantic canon
Doppler becomes authority
Cloudflare becomes throne
ChatGPT executes directly
LLM closes evidence
SDK success becomes proof
Receipt becomes story
Report becomes receipt
Passport becomes permission
Visa becomes unbounded power
ExecutionWindow becomes reusable approval
App invents parallel identity
App Park member is just a running container
```

---

## 24. Amendment Rule

Changes to this ontology require an explicit amendment.

```txt
Ontology Amendment

Proposed character/relation change:
Why current ontology fails:
What family/kind is affected:
What may/mayNot changes:
What authority boundary is protected:
What registry projection changes:
What UI projection changes:
What evidence/probe is required:
Dan approval required: yes/no
Foundation governance required: yes/no
```

Foundation governance is required if the change attempts to redefine LogLine canon, receipt semantics, slot grammar, or conformance behavior.

---

## 25. Current Honest State

```txt
State:
  Canon draft.

Grounded by:
  Existing Characters document, Minilab Constitution / Strategic Architecture, Dan Control Plane product doctrine, Database Projection Canon, Doppler Canon, and inspected Foundation structure.

Verified:
  Character and relation vocabulary is derived from existing plans.

Unverified:
  Current runtime status of every character is not proven here.
  LAB states require runtime receipts.
  Supabase/Doppler/Cloudflare states require inventory receipts.
  App Park characters are reserved extension points, not implemented members.

Ghosts:
  Need registry seed generation.
  Need current-state reconciliation against live systems.
  Need App Park Membership Canon before app member states become normative.
```

---

## 26. Canonical Closing

```txt
Minilab is a named world.

LogLine gives grammar.
Dan bears consequence.
Runtime admits meaning.
Tower governs power.
LABs execute.
Agents translate.
Stores remember.
Edges expose.
Work creates candidates.
Proof closes scope.
Ghosts preserve absence.
Registry names the current world.

Identity is not authority.
Role is not permission.
Execution is not closure.
Receipts close only within scope.
```

