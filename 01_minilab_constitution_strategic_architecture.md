# 01 — Minilab Constitution / Strategic Architecture

**Status:** Canon draft  
**Layer:** Minilab constitutional umbrella  
**Scope:** Dan’s Minilab operational universe  
**Depends on:** LogLine Foundation canon, conformance, engine, governance, and constitutional runtime primitives  
**Downstream documents:** Characters / Ontology Canon, Authority / Power Doctrine, Evidence / Receipt Doctrine, Dan Control Plane Doctrine, Database Projection Canon, Doppler / Config Canon, App Park Doctrine

---

## 0. Purpose

This document defines the constitutional architecture of **Minilab**.

It exists to prevent the system from becoming a pile of dashboards, scripts, agents, cron jobs, prompts, SDKs, tunnels, databases, containers, and confident stories.

Minilab is not a dashboard.
Minilab is not a chatbot.
Minilab is not a CRM.
Minilab is not a TypeScript orchestration layer.
Minilab is not a cloud control panel.
Minilab is not a generic automation stack.

Minilab is **Dan’s Rust-native constitutional laboratory**: an operational institution where LogLine gives form, the executable body materializes acts, the Runtime admits meaning, Tower governs power, LABs execute, databases project state, Doppler supplies material, receipts close scope, and Dan signs consequence.

This document defines the hierarchy.
Other documents may specify product surfaces, database schemas, config keys, UI language, app membership, provider adapters, or runtime procedures. They may not invert this hierarchy.

---

## 1. Canonical Sentence

```txt
Minilab is Dan’s constitutional laboratory:
a Rust-native LogLine institution where language becomes candidate,
grammar becomes admissibility,
Tower governs power,
LABs execute,
and receipts beat stories.
```

Portuguese form:

```txt
Minilab é o laboratório constitucional do Dan:
uma instituição Rust-native em LogLine onde linguagem vira candidato,
gramática vira admissibilidade,
a Torre governa poder,
os LABs executam,
e recibos vencem histórias.
```

Short form:

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

---

## 2. Parent Canon

Minilab does not define the universal LogLine canon.

Minilab depends on the LogLine Foundation stack:

```txt
LogLine Foundation Canon
  defines the receipt/act mold

LogLine Conformance
  proves emitted artifacts obey canon

LogLine Engine
  implements the slot grammar and CLI behavior

LogLine Governance
  evolves canon through accepted change process

Constitutional Runtime
  provides primitives for IR, policy, capability, evidence, admission, and decision
```

Minilab may specialize the canon into operational domains. It may not redefine foundational receipt semantics, hash semantics, slot grammar, or canon governance locally.

If Minilab needs domain-specific metadata, it uses domain-specific auxiliary fields or external evidence records. If Minilab needs to change foundational semantics, it proposes a Foundation governance change.

---

## 3. Constitutional Hierarchy

The control hierarchy is:

```txt
LogLine
  constitutional grammar

Rust CLI / Rust institutional body
  executable institutional body

Constitutional Runtime
  admissibility, semantic authority, evidence, closure

Tower / minilab.work
  operational power, machines, heartbeats, Passkey windows, routing, consequence

LABs
  local execution environments

Cloudflare Gateway
  public ingress / edge projection

MCP Local / Host Runtime
  governed host execution surface

SDKs
  rented machinery

Databases
  projection and operational memory

Doppler
  operational delegation vault

ChatGPT / LLMs
  conversational face, translators, planners, reviewers

Receipts
  proof and scoped closure

Dan
  source of intent, approver, doubt arbiter, consequence signer
```

No lower layer may impersonate a higher layer.

A UI may request. It may not govern.
A database may remember. It may not decide.
Doppler may supply material. It may not authorize.
Cloudflare may expose ingress. It may not become institution.
LABs may execute. They may not govern.
LLMs may translate. They may not execute or close evidence.
Provider success may inform evidence. It may not close proof.

---

## 4. Institutional Slot Grammar

Every consequential act must be expressible through the LogLine slot grammar.

Minimum human-readable questions:

```txt
Who did this?
What was done?
About what?
When?
Who or what confirmed it?
If OK, what path follows?
If doubt, where is suspension or simulation recorded?
If not, what rejection or alternative path follows?
What is the current state?
```

Canonical slot names:

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

Rule:

```txt
Mouth is not a tenth semantic slot.
Mouth is the executable jurisdiction that invokes the nine-slot grammar.
```

The executable body speaks the grammar. It does not create a parallel grammar.

---

## 5. The Three Bodies

Minilab is separated into three strict bodies so execution never becomes authority.

### 5.1 Constitutional Runtime

Role:

```txt
admissibility
semantic authority
evidence discipline
closure boundary
IR / policy / capability / evidence planning
```

It decides whether a proposed act is admissible under grammar, policy, capability, and evidence requirements.

It is not:

```txt
Mac admin
cloud dashboard
host runtime
UI shell
Doppler config
```

### 5.2 Tower / minilab.work

Role:

```txt
operational power
machines
heartbeats
Passkey windows
protected command routing
execution consequence
```

Tower governs power. It opens or denies operational windows. It distinguishes normal from protected commands.

It is not:

```txt
semantic runtime
LogLine canon
receipt canon
```

### 5.3 MCP Host Runtime / Local Execution Surface

Role:

```txt
local host execution
tool access
safe host operations
MCP bridge
runtime observation
```

It executes under admitted scope. It does not authorize itself.

It is not:

```txt
authority
policy source
canon
governor
```

---

## 6. Physical Distribution of LABs

LABs are execution places. They are not governors.

```txt
LAB-8GB
  role: supervisor
  function: health loops, recovery checks, stable evidence bridge, control-plane-adjacent supervision

LAB-512
  role: inference / heavy execution
  function: private inference, heavy jobs, model health, App Park runtime candidate

LAB-256
  role: workbench
  function: manual intervention, browser/macOS automation, tests, debugging, experimental work
```

Hard rule:

```txt
LABs execute.
They do not govern.
```

A LAB identity identifies an execution environment. It does not authorize execution.

A LAB must be represented by stable identity:

```json
{
  "lab_id": "stable-lab-id",
  "lab_name": "LAB-512",
  "lab_type": "physical_lab",
  "status": "active",
  "runner_id": "stable-runner-id",
  "entity_id": "stable-registry-entity-id",
  "capability_scope": "local_execution"
}
```

Changing a LAB identity silently is forbidden.

---

## 7. The Operational Laws

These laws are not features. They are constitutional constraints.

### 7.1 Law of External Canon

```txt
Local snapshots may assist.
External canon governs.
```

Minilab obeys the LogLine Foundation canon. It does not create a local constitution to bypass parent law.

### 7.2 Law of LAB Identity

```txt
LAB ID identifies execution environment.
It does not authorize execution.
```

Machine identity is attribution, not permission.

### 7.3 Law of Physical Separation

```txt
Repo describes.
Config instantiates.
Secrets authorize materially.
Data records.
```

Forbidden in repository:

```txt
secret values
tokens
runtime ledgers
heartbeats
machine state
logs
host-local truth
```

### 7.4 Law of Tower Command

Commands are:

```txt
normal
protected
```

There is no third hidden category.

Protected commands require Tower, Passkey or equivalent scoped human power window, execution window, and receipt path.

### 7.5 Law of Language Ingress

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
```

Natural language may not produce:

```txt
dispatch
provider mutation
database mutation
evidence closure
direct runtime action
```

### 7.6 Law of Artifact Honesty

No-op, placeholder, mock success, superficial green tests, and scaffold-only output are not delivery.

```txt
Generated is not run.
Run is not passed.
Passed is not production-observed.
```

If behavior is not real, it must be declared as simulation, draft, ghost, or error.

### 7.7 Law of Strong Grammar

Production code does not call consequential tools directly.

```txt
Strong Grammar does not execute.
Strong Grammar compiles to IR.
IR is admitted or rejected.
```

### 7.8 Law of Constitutional Evidence

```txt
Provider success is evidence input.
It is not closure.
```

Evidence must be scoped, captured, referenced, and closed by an admissible receipt.

### 7.9 Law of Ontology Projection

Relationship tools, SaaS systems, dashboards, and external stores may project or assist ontology. They do not create constitutional truth.

```txt
Entity exists is not authority.
Role exists is not permission.
Permission exists is not execution.
Execution exists is not closure.
Closure requires receipt.
```

### 7.10 Law of Gateway

Every external command must pass through admission before reaching LABs.

```txt
Gateway is ingress/admission projection.
It is not the root institution.
```

Cloudflare exposes an edge. It does not define meaning.

---

## 8. Separation of Talents

### 8.1 Dan

Dan is:

```txt
source of intent
owner of consequence
approver
doubt arbiter
risk accepter
human signer
```

Dan may approve, deny, defer, ask for proof, revoke authority, or sign consequence.

Dan may not honestly replace missing evidence with preference.

### 8.2 Pocket Runtime

The Pocket Runtime governs language state inside the assistant/operator loop.

Its role is to prevent:

```txt
thought becoming action
plausibility becoming verification
candidate becoming dispatch
summary becoming receipt
confidence becoming authority
```

### 8.3 LLMs

LLMs may:

```txt
classify
translate
propose
draft
criticize
summarize
explain
lower intent into structure
produce candidate acts
produce candidate probes
```

LLMs may not:

```txt
execute commands directly
hold dispatch power
mutate provider/database state
declare verified truth without receipt
sign consequence they do not bear
sit behind the dispatcher
```

Doctrine:

```txt
LLMs do not usually fail because they are evil.
They fail when the runtime asks them to impersonate truth.

LogLine gives them the right job:
translate, propose, structure, explain.
Receipts prove.
Gates decide.
Humans sign consequence.
```

### 8.4 Machines / LABs

Machines execute admitted work. They emit observations and evidence. They do not decide whether the work should exist.

### 8.5 SDKs and Providers

SDKs are machinery. Providers are machinery. Provider APIs are not law.

They may be used only inside admitted scope, with explicit evidence capture and receipt boundaries.

---

## 9. Rust-Native Executable Body

The Minilab executable body is Rust-native.

The Rust CLI / Rust institutional body is:

```txt
local body of the institution
executable surface of grammar
first mouth of LogLine inside Minilab
adoption lane
receipt emitter
probe runner
workorder path
interface to gates, runtime, ledger, and canon
```

Rule:

```txt
If it defines institutional meaning, it must be Rust.
If it exposes or hosts institutional meaning, it may be SDK/transport.
If it only renders institutional meaning, it may be UI.
```

The Rust body may call other machinery. It may not disappear behind that machinery.

---

## 10. Cloudflare / Gateway / Edge

Cloudflare Gateway is:

```txt
edge transport
public ingress
hosted projection
deployment target
```

It is not:

```txt
throne
root institution
semantic authority
constitutional runtime
Tower
LAB
```

Gateway flow:

```txt
external agent / ChatGPT / API
→ Cloudflare Gateway
→ normalize request
→ project into LogLine-shaped candidate
→ gate / policy admission
→ ok / denied / needs_approval / ghost / error
→ receipt or receipt request
→ optional job / LAB handoff
```

Gateway receives external intent and projects it into institutional grammar. It does not become the grammar.

---

## 11. Databases and Memory

Databases are machinery.

They provide:

```txt
projection
queryability
indexes
audit views
current-state views
receipt indexes
artifact pointers
operational memory
```

They do not provide:

```txt
canon
authority
executor
gate
semantic legitimacy by themselves
```

The central database law is:

```txt
All semantic database state is projected from LogLine-shaped acts.
```

Supabase/Postgres may store projections and indexes. It does not redefine receipts. It does not become the institution.

Canonical legitimacy comes from:

```txt
content-addressed evidence
admitted acts
contracts
runtime observation
replayable receipts
human signature when consequence requires it
```

---

## 12. Doppler and Configuration

Doppler is Dan’s operational delegation vault.

It stores:

```txt
secrets
configs
flags
brakes
pointers
technical identities
windows
provider credentials
```

It may equip machinery.
It may not define meaning.

Doppler may provide material only inside admitted scope.

Doppler is not:

```txt
authority
receipt canon
database canon
policy substitute
human signature
proof closure
```

Secret values must not appear in:

```txt
repo
database values
receipts
logs
reports
browser payloads
```

The database may index config requirements by name. Doppler owns values.

---

## 13. Receipts and Evidence

Receipts beat stories.

A receipt is scoped proof closure. It is not a narrative summary. It is not provider success. It is not a report. It is not a green terminal line by itself.

Minilab receipts must conform to the parent LogLine Foundation receipt canon.

Evidence and receipts are distinct:

```txt
Evidence
  observed output, diff, status, stdout/stderr, hash, screenshot, HTTP result, probe output

Receipt
  scoped closure over a declared act, referencing evidence as needed
```

Reports may interpret evidence. Reports do not close evidence.

A receipt must distinguish:

```txt
planning text
compiler-only proof
simulation
ingress classification
runtime observation
provider response
local behavior
production-observed behavior
```

A narrow receipt closes only its declared scope.

---

## 14. Ghosts

A Ghost is structured absence.

Ghosts record missing:

```txt
proof
context
runtime
authority
signature
secret
configuration
schema
receipt
observation
ownership
world-fit
```

Weak data becomes a ghost or review queue. It does not become truth.

```txt
if_doubt never becomes hidden OK.
```

Ghosts are not failure. They are honest boundary markers.

---

## 15. Observability, Expeditions, and Maintenance

Observability is institutional routine, not decorative analytics.

### 15.1 Observability

Minilab must observe physical and digital reality:

```txt
LAB heartbeats
host runtimes
machine health
network state
models
providers
configs
release installations
sensor signals
events
receipts
ghosts
```

Observation is not automatic verification.

### 15.2 Expeditions

Expeditions observe. They do not govern.

They may produce:

```txt
candidate findings
ghosts
risk notes
maintenance suggestions
communication topics
receipt requests
```

### 15.3 Snapshots

Snapshots compare intended state to physical/digital reality.

They may compare:

```txt
Gateway DB
LAB local stores
receipt stores
Tower ledger
Runtime evidence
Supabase projections
Doppler requirement names
```

A backup without a restore check is hope.

### 15.4 Maintenance

Manual maintenance is an exception path.

It must be:

```txt
planned
authorized
observable
auditable
closed by receipt or ghosted
```

---

## 16. Product Growth Spiral

Minilab evolves through a spiral, not a giant rewrite.

Rule:

```txt
Each functional advance pays an automation toll.
```

The pattern:

```txt
Product advances
→ repeated pattern is extracted
→ automation hardens
→ next advance becomes cheaper
```

Growth vectors:

```txt
Governance
  static rules → automated policies/gates

Technical capacity
  read-only tools → protected execution behind scoped power windows

Agent autonomy
  translators → supervised maintenance loops under doubt-as-auditable-simulation

Ambiguity economy
  stronger grammar → fewer narrative tokens
  better receipts → less persuasion
  clearer authority → less anxious orchestration
```

---

## 17. Implementation Spine

The implementation spine follows this sequence:

```txt
1. Rust CLI / Canon Contact
2. Local Receipts
3. Tower / Gateway Minimum Gate
4. LAB Heartbeats
5. Protected Command Physics
6. Evidence Closure Spine
7. Expedition Loop
8. Gateway Public Demo
9. Strong Grammar / IR
10. Platform Applications
```

The sequence is not merely chronological. It is constitutional dependency.

No real dispatch before admission.
No protected command before Tower/Passkey/window.
No database semantic write outside LogLine-shaped act.
No platform app governance before Tower and Runtime boundaries are preserved.

Platform applications are payloads.
Tower remains operational authority.
Runtime remains semantic authority.

---

## 18. Non-Negotiable Invariants

1. Natural language never executes.
2. Strong Grammar compiles to IR; it never executes.
3. No material act bypasses IR/admission.
4. No LLM sits behind the dispatcher.
5. Runtime governs semantic admissibility.
6. Tower governs operational power.
7. LABs execute; they do not govern.
8. Cloudflare is public edge projection, not a second institution.
9. Rust is the executable institutional body.
10. SDKs provide machinery; they do not define meaning.
11. Provider success is evidence input, not closure.
12. Confidence is not permission.
13. LAB ID is execution identity, not business identity.
14. Weak data becomes Ghost or review queue, not truth.
15. `if_doubt` never becomes hidden OK.
16. Generated is not run.
17. Run is not passed.
18. Passed is not production-observed.
19. Receipts close only within declared scope.
20. No fake implementation, placeholder, no-op, or superficial green test is accepted as delivery.
21. Database projects. It does not govern.
22. Doppler supplies material. It does not authorize.
23. Reports synthesize. They do not close.
24. UI composes. It does not dispatch directly.
25. Dan signs consequence.

---

## 19. Anti-Patterns

Stop immediately if implementation introduces:

```txt
dashboard as institutional center
cockpit as source of truth
CRM clone as ontology
Attio or external SaaS as hot queue
provider success as constitutional success
policy in prompt
prompt soup hidden as governance
cron as authority
LLM behind dispatcher
natural language direct action
Cloudflare as institution
TypeScript as institutional body
SDK as law
fake receipt
placeholder presented as delivery
simulation reported as production
LAB ID used as customer/business identity
generic MCP break-glass as default
secrets in logs / repo / database / receipts
Supabase-as-brain
Doppler-as-authority
report-as-receipt
```

---

## 20. Corrective Principles

When uncertain:

```txt
Not UI. Tower.
Not social rule. Physics.
Not provider success. Evidence closure.
Not hidden heuristics. Contracts.
Not one giant agent. Supervised loops and scoped tools.
Not confidence as permission. Confidence as input.
Not chat as management. Typed continuation.
Not natural language as act. Natural language as ingress material.
Not Strong Grammar as executor. Strong Grammar as compiler.
Not Tower as semantic runtime. Tower as operational governor.
Not Runtime as Mac admin. Runtime as constitutional pipeline.
Not LAB identity as authority. LAB identity as execution attribution.
Not Cloudflare as institution. Cloudflare as edge projection.
Not SDK as law. SDK as machinery.
Not placeholder. Real behavior or honest ghost.
```

---

## 21. Downstream Document Boundaries

This constitution is the umbrella.

Downstream documents refine it as follows:

```txt
02 Characters / Ontology Canon
  defines named characters, entity kinds, relations, families, and identity boundaries

03 Authority / Power Doctrine
  defines admission, gates, commands, execution windows, protected power, approval, rejection, ghosting

04 Evidence / Receipt Doctrine
  defines evidence records, receipt usage, closure vocabulary, proof boundaries, artifact honesty

05 Dan Control Plane Product + Backend Doctrine
  defines product surfaces, UI language, backend layers, command bridge, server-only reads, Authority Core writes

06 Database Projection Canon
  defines ops/registry/audit/app schemas and projection rules

07 Doppler / Config Canon
  defines config classes, brakes, flags, secrets, safe print, doctor, digest, operational delegation
```

No downstream document may overturn this constitution without an explicit constitutional amendment.

---

## 22. Amendment Rule

Any proposed change to this document must state:

```txt
Proposed change:
Why current constitution fails:
Which invariant remains protected:
Which downstream docs change:
Runtime or operational risk:
Receipt/probe required:
Dan approval required: yes/no
Foundation governance required: yes/no
```

If a change touches LogLine receipt canon, hash semantics, slot grammar, or Foundation conformance, it is not a local Minilab amendment. It requires the appropriate Foundation governance path.

---

## 23. Current Honest State

```txt
State:
  Canon draft.

Grounded by:
  Existing Minilab strategic architecture, Control Plane doctrine, Database Plan, Doppler Canon, Characters/Ontology notes, and inspected LogLine Foundation structure.

Verified:
  This document is a synthesis of existing plans.

Unverified:
  It does not prove current LAB runtime state.
  It does not prove current Supabase schema safety.
  It does not prove Doppler config reality.
  It does not prove Rust crates build.
  It does not deploy anything.

Ghosts:
  Current runtime receipts still required.
  Current database/RLS receipt still required.
  Current Doppler inventory receipt still required.
  Authority Core implementation state still required.
```

---

## 24. Canonical Closing

```txt
Minilab is Dan’s Rust-native constitutional laboratory.

LogLine is the grammar.
The Rust body materializes.
The Runtime admits.
Tower governs power.
LABs execute.
Cloudflare exposes ingress.
SDKs provide machinery.
Supabase projects memory.
Doppler supplies material.
ChatGPT translates.
Reports synthesize.
Ghosts preserve absence.
Receipts prove.
Dan signs consequence.
```

