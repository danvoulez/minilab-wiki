# Santo André Laboratory — Modus Operandi

Document 11

## Status

```yaml
id: sal-doc-011
title: Santo André Laboratory Modus Operandi
status: draft
scope: research doctrine
authority: Santo André Laboratory
```

---

## 1. Identity

Santo André Laboratory is a receipted research institution.

Its purpose is not merely to build software. Its purpose is to discover latent structures inside executable doctrine, formalize them as falsifiable invariants, test them against runtime or evidence, and promote only scoped, receipted discoveries into Minilab doctrine.

Canonical sentence:

```txt
Santo André Laboratory discovers structures hidden in executable doctrine,
turns them into falsifiable invariants,
tests them against runtime,
and promotes only receipted discoveries into Minilab doctrine.
```

Sharper sentence:

```txt
Santo André Laboratory is where philosophy is forced to produce probes.
```

---

## 2. Prime Directive

```txt
Never let beauty outrun evidence.
```

The lab may begin with intuition, metaphor, philosophy, or architecture. But no discovery is counted until it becomes an invariant and survives contact with a probe, a runtime observation, evidence, or a falsifiable model.

The lab rejects this path:

```txt
idea
→ essay
→ product claim
```

The lab follows this path:

```txt
intuition
→ thesis
→ invariant
→ probe
→ runtime observation
→ receipt
→ doctrine
→ system
```

---

## 3. Operating Law

```txt
No discovery without an invariant.
No invariant without a probe.
No probe without evidence.
No evidence without scoped claim.
No scoped claim without ghosts named.
```

A discovery is not valid because it is beautiful, coherent, or convincing. It becomes valid only when its claim is scoped, its ghosts are named, and its evidence is preserved.

---

## 4. Standard Experiment Lifecycle

Every Santo André experiment should follow this shape:

```yaml
id: sal-exp-000
title: Experiment Title
status: proposed | probing | evidence_captured | scoped_commit | promoted | rejected

intuition:
  - The initial pattern, suspicion, or hidden structure.

thesis:
  - The precise conceptual claim.

invariants:
  - name: invariant_name
    claim: falsifiable statement
    expected_observation: what should be observed if true
    forbidden_observation: what would falsify or weaken it

probe:
  kind: test | command | inspection | model | conformance_vector
  command_or_path:
  fixture:
  expected_observation:
  forbidden_observation:

runtime_observation:
  command:
  output_summary:
  status:

receipt:
  emitted: true | false
  scope:
  artifact:
  commit:
  branch:
  test_output:

scoped_claim:
  - What is now allowed to be said, narrowly.

ghosts:
  - Missing evidence, untested path, unavailable runtime, absent authority, unclear scope.

promotion:
  level: none | repeat_probe | doctrine_note | governance_lip | engine_test | conformance_vector | minilab_feature
```

---

## 5. Evidence Discipline

The lab must never collapse these states:

```txt
intuition ≠ thesis
thesis ≠ invariant
invariant ≠ probe
probe ≠ observation
observation ≠ receipt
receipt ≠ broad truth
scoped commit ≠ canon
canon ≠ production behavior
```

The lab may say:

```txt
This invariant passed under this probe.
```

The lab may not say:

```txt
The entire doctrine is proven.
```

unless the evidence scope actually supports it.

---

## 6. Roles

One person or model may play multiple roles, but the roles must remain conceptually separate.

```txt
Architect / Theorist
  Finds hidden structure.
  Example: "if_doubt is non-releasing computation."

Invariant Hunter
  Converts thesis into falsifiable claims.
  Example: Absorption, Non-extraction, Modal non-collapse.

Runtime Surgeon
  Opens engines, SDKs, protocols, crates, schemas, and runtimes.
  Finds the seam where the invariant can be tested.

Probe Operator
  Runs tests, captures output, refuses overclaim.

Ghost Keeper
  Names what is missing and prevents false closure.

Governance Scribe
  Turns receipts into doctrine, LIPs, ADRs, and papers.

Product Cartographer
  Maps receipted discoveries into Minilab, App Park, Control Plane, Intelligence App, or other system surfaces.

Builder
  Implements only after invariants, contracts, and probes exist.
```

The purpose of explicit roles is to prevent:

```txt
smart person said convincing thing
```

from becoming doctrine.

---

## 7. Experiment Classes

### Class A — Runtime Invariant

Tests actual engine behavior.

Example:

```txt
if_doubt cannot self-release.
```

Outputs:

```txt
engine test
conformance vector
runtime receipt
```

### Class B — Formal Model

Defines a calculus, algebra, or state model before implementation.

Examples:

```txt
Ghost Formation
Consequence lattice
Passage friction model
Constructive Release Semantics
```

Outputs:

```txt
model note
invariants
falsification criteria
```

### Class C — SDK Surgery

Investigates commodity machinery and determines whether Minilab should adopt, slice, wrap, or reject it.

Examples:

```txt
Supabase Auth boundary
OAuth/OIDC as evidence source
WebAuthn/passkey windows
CLI SDK adapters
```

Output:

```txt
candidate record
slice report
adapter boundary
probe
```

Rule:

```txt
SDKs carry. Minilab decides.
```

### Class D — Product Embodiment

Turns a receipted discovery into a system surface.

Examples:

```txt
Ghost Hall in Control Plane
Research Registry
Experiment Intelligence App
Admission Friction Dashboard
```

Outputs:

```txt
manifest
schema
UI projection
probe
```

### Class E — Governance Promotion

Moves a discovery into Foundation or Minilab doctrine.

Examples:

```txt
Constructive Release Semantics note
Ghost Trust doctrine
Post-OAuth Native Passage note
```

Outputs:

```txt
governance note
LIP draft
ADR
canonical amendment proposal
```

---

## 8. Promotion Ladder

A discovery must not jump directly from insight to canon.

Use this ladder:

```txt
hunch
→ named thesis
→ invariant candidate
→ probe proposed
→ probe observed
→ scoped commit
→ repeated probe / conformance
→ doctrine note
→ governance proposal
→ implementation dependency
→ product surface
```

Current calibration examples:

```txt
Experiment 001 — Constructive Release Semantics
  probe observed
  scoped commit
  ready for governance note

Ghost Trust
  named thesis
  invariant candidates
  probe proposed

Native Passage / Post-OAuth
  architecture thesis
  evidence/admission boundary probes proposed
```

---

## 9. Research Tracks

### Track 1 — Constructive Release Semantics

Question:

```txt
What can compute without releasing?
```

Known result:

```txt
Experiment 001 passed:
- Absorption
- Non-extraction
- Modal non-collapse
```

Next candidates:

```txt
Trace composition
Constructive soundness
Governance note
Conformance vectors
```

### Track 2 — Ghost Trust

Question:

```txt
Can remembered menace reduce friction without granting authority?
```

Next experiment:

```txt
Experiment 002 — Ghost Formation
```

Core invariant candidates:

```txt
Ghost memory may alter friction.
Ghost memory may not authorize execution.
Ghost identity must be stable for the same menace shape.
```

### Track 3 — Native Passage / Post-OAuth

Question:

```txt
What comes after authentication?
```

Key distinction:

```txt
Authentication is evidence about the principal.
Admission is a constitutional decision about the action.
```

Near-term work:

```txt
ExternalAuthObservation schema
Supabase Auth as identity machinery
CRT admission boundary
Passkey-bound execution windows
```

### Track 4 — Solved Machinery Surgery

Question:

```txt
Which solved systems should Minilab adopt, slice, wrap, or reject?
```

Rule:

```txt
Do not rebuild solved machinery unless the invariant requires it.
```

### Track 5 — Intelligence App / Slow Cognition

Question:

```txt
Can small models continuously inspect ghosts, receipts, experiments, and next probes without executing consequence?
```

Purpose:

```txt
memory
inspection
summarization
next-probe proposal
ghost tracking
```

Constraint:

```txt
The Intelligence App may observe and propose.
It must not release consequence.
```

---

## 10. Standard Artifact Set

Recommended repository structure:

```txt
research/
  santo-andre-lab/
    README.md
    modus-operandi.md

    experiments/
      001-constructive-release-semantics.md
      002-ghost-formation.md

    discoveries/
      constructive-release-semantics.md
      ghost-trust.md
      auth-as-evidence.md

    invariants/
      if-doubt-absorption.yaml
      doubt-non-extraction.yaml
      modal-non-collapse.yaml
      ghost-formation.yaml

    probes/
      engine/
      conformance/
      minilab/
      auth/
      sdk/

    evidence/
      examples/
      indexes/

    receipts/
      examples/
      indexes/

    ghosts/
      open-ghosts.yaml

    governance/
      lip-drafts/
      adr-drafts/
```

Live evidence may remain outside Git when repository policy requires it. In that case, sanitized summaries and indexes should remain in the lab tree.

---

## 11. Weekly Operating Rhythm

A healthy lab cadence:

```txt
Monday:
  choose one thesis and one ghost

Tuesday:
  formulate invariant

Wednesday:
  write or adapt probe

Thursday:
  run probe and capture evidence

Friday:
  write scoped receipt and decide promotion:
    discard
    repeat
    strengthen
    doctrine note
    implementation task
```

The lab wins by producing durable receipts, not by producing grand theories every day.

---

## 12. Wall Rules

```txt
A structure is not discovered when it is named.
It is discovered when it resists falsification.
```

```txt
A Ghost is not failure.
A Ghost is the exact shape of what must not be lied about.
```

```txt
Receipts beat charisma.
```

```txt
A beautiful theory with no probe is still only a hunch.
```

---

## 13. Experiment 001 as Template

Experiment 001 established the first official Santo André Laboratory pattern.

```yaml
id: sal-exp-001
title: Constructive Release Semantics
status: scoped_commit

repo: LogLine-Foundation/engine
branch: test/constructive-release-semantics
commit: 1664cde

probe:
  command: cargo test -p logline-status --test core_tests

observed:
  result: 72 passed; 0 failed

scoped_discovery:
  if_doubt is a non-releasing computational region.

invariants:
  - absorption
  - non-extraction
  - modal non-collapse

ghosts:
  - not yet promoted to governance note
  - not yet represented as conformance vectors
  - constructive soundness not yet tested
  - trace composition not yet tested
```

Experiment 001 does not prove all of Ghost Trust. It proves the release-safety primitive required for Ghost Trust:

```txt
safe unreleased computation with memory
```

That primitive enables later research into:

```txt
memory without release
simulation without fact
trace without consequence
counterfactual without execution
```

---

## 14. Immediate Next Moves

```txt
1. Record Experiment 001 as a formal lab artifact.

2. Draft Experiment 002 — Ghost Formation.

3. Promote Constructive Release Semantics into a governance note only after the engine PR is reviewed.

4. Add conformance vectors for the three tested invariants.

5. Continue with Trace Composition and Constructive Soundness.
```

No further abstraction should be promoted until these files exist and the receipts are indexed.

---

## 15. Final Formulation

The modus operandi of Santo André Laboratory:

```txt
Find the hidden structure.
Name it carefully.
Turn it into an invariant.
Attack it with a probe.
Capture evidence.
Close narrowly.
Promote only what survived.
Keep ghosts visible.
Split labour by talent.
Let receipts, not charisma, govern discovery.
```
