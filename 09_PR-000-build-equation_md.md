# Build Equation

## Purpose

This document freezes the build equation for Minilab.

It exists to make the repo physically express the constitutional hierarchy:

```txt
doctrine is upstream of code
schemas are executable doctrine
generated files are consequences
runtime is governed
evidence is first-class
receipts close narrowly
apps are payloads, not law
```

This is the rule the repo must show:

```txt
source doctrine
+ extracted canonical blocks
+ doctrine compiler
+ foundation
= doctrine IR

doctrine IR
+ schema compilers
+ contract indexing
= schemas / policies / manifests / probes

schemas / policies / manifests / probes
+ generators
+ templates
= generated code / SQL / validators / contracts

generated
+ hand-written crates
+ build runtime
+ ops runtime
+ adapters
= running Minilab
```

## Constitutional Equation

### Layer 1

```txt
doctrine/source
+ doctrine/blocks
+ compilers/doctrinec
+ foundation/upstream
= doctrine/ir
```

Meaning:

- `doctrine/source` contains the human canonical documents.
- `doctrine/blocks` contains extracted machine-governed canonical blocks.
- `compilers/doctrinec` extracts, validates, and normalizes those blocks.
- `foundation/upstream` supplies the pinned Foundation inputs Minilab depends on.
- `doctrine/ir` is the normalized machine doctrine layer.

### Layer 2

```txt
doctrine/ir
+ schema compilers
+ contract indexing
= schemas + policies + manifests + probes
```

Meaning:

- doctrine IR becomes executable contracts.
- the contract layer is not only JSON Schema; it also includes policy packs, manifests, and probes.
- each emitted contract must be traceable to source doctrine blocks.

### Layer 3

```txt
schemas + policies + manifests + probes
+ generators
+ templates
= generated code + SQL + validators + contracts
```

Meaning:

- formal contracts produce generated implementation material.
- generated outputs may include Rust types, TypeScript contracts, SQL migrations, validators, and projection scaffolds.
- generated files are downstream consequences and must not become hidden law.

### Layer 4

```txt
generated
+ crates
+ runtime/build
+ runtime/ops
+ adapters
= running Minilab
```

Meaning:

- generated outputs are combined with hand-written institutional code.
- build-time and operational runtime share discipline but not necessarily risk surface.
- adapters contact the outside world under governed boundaries.
- the final running system remains downstream of doctrine.

## Repo Shape

The repo should express the equation directly:

```txt
minilab/
  AGENTS.md
  README.md

  doctrine/
    source/
    blocks/
    ir/
    hashes/

  foundation/
    upstream/
    trimmed/
    patches/
    receipts/

  bootstrap/
    hand_seeded/
      schemas/
      policies/
      manifests/
    probes/
    receipts/

  contracts/
    contract-index.yaml

  schemas/
  policies/
  manifests/
  probes/

  compilers/
  generated/

  crates/
  apps/

  runtime/
    shared/
    build/
    ops/

  db/
  fixtures/

  receipts/
    examples/
    fixtures/
    schemas/

  evidence/
    examples/
    fixtures/
    schemas/

  reports/
    artifact-honesty/
    build-reports/
```

## Source Doctrine, Blocks, and IR

The doctrine path must be strict:

```txt
doctrine/source  = human canonical docs
doctrine/blocks  = extracted canonical machine blocks
doctrine/ir      = normalized doctrine IR
```

Rule:

```txt
doctrinec does not compile prose.
doctrinec extracts fenced canonical blocks and validates their relationship to prose.
```

This prevents the compiler layer from pretending to understand free-form narrative.

## Bootstrap Lane

The compiler tower does not start with authority already earned.

The bootstrap path is:

```txt
hand-seeded schemas
-> tests
-> first compiler
-> compiler regenerates same schema
-> diff is zero
-> compiler earns authority
```

Therefore `bootstrap/` must exist to hold:

- hand-seeded schema and policy seeds
- bootstrap probes
- bootstrap receipts

Bootstrap receipts must stay narrow. They should prove things like:

```txt
these doctrine blocks were parsed
this IR was emitted
this golden fixture matched
```

They should not overclaim constitutional correctness.

## Foundation Boundary

The Foundation layer is parent dependency, not local sovereignty.

Rules:

```txt
foundation/upstream is imported, not edited
foundation/trimmed is derived
foundation/patches contains explicit patch manifests
foundation/receipts records import, trim, and patch evidence
```

And:

```txt
No direct edits under foundation/upstream
No local patch without patch manifest
No trim without trim receipt
No Foundation semantic change without Foundation governance path
```

Minilab may specialize downstream behavior.
It may not locally redefine foundational receipt semantics, hash semantics, slot grammar, or canon governance.

## Contracts

The contract layer must stay indexable.

`contracts/contract-index.yaml` should answer:

```txt
contract id
source doctrine section
generated outputs
downstream crates
tests or probes
receipt profile
owner
status
```

This prevents `schemas/` from becoming a junk drawer.

## Generated and Hand-Written Code

The code boundary is:

```txt
generated code handles structure
hand-written code handles institutional intelligence
```

Generated outputs may include:

- types
- validators
- SQL scaffolds
- contract bindings
- policy data structures
- projection scaffolds

Hand-written code should include:

- doctrine compilers
- schema compilers
- generator templates
- authority kernel logic
- runtime glue
- provider adapters
- product behavior

## Build Runtime and Operational Runtime

The runtime boundary must be explicit:

```txt
runtime/shared = common discipline
runtime/build  = compiler and artifact execution
runtime/ops    = operational and LAB execution
```

Build-time and operational execution share:

- admission discipline
- evidence discipline
- receipt discipline
- projection discipline

But they must not blur scope.
A build receipt is not an operational closure claim.

## Evidence and Receipt Storage Boundary

The repo may contain:

- examples
- fixtures
- schemas
- sanitized build reports intentionally committed

The repo must not casually contain live runtime truth.

So these directories are for controlled material only:

```txt
receipts/examples
receipts/fixtures
receipts/schemas

evidence/examples
evidence/fixtures
evidence/schemas
```

Live runtime artifacts should remain outside normal versioned repo content, for example in local runtime storage or an external artifact store.

Rule:

```txt
build receipts may be committed when sanitized and intentional
runtime receipts should be indexed and projected, not casually versioned
```

## LogLine Engine Usage

The LogLine engine must be used in two phases.

### During building

Use LogLine for consequential build acts such as:

- doctrine IR compilation
- schema compilation
- code generation
- SQL generation
- conformance proof runs
- packaging

Build rule:

```txt
every consequential build stage
-> act
-> evidence
-> receipt
```

### After code is ready

Use LogLine for consequential institutional acts such as:

- semantic writes
- entity registration
- policy or gate changes
- deployment and release acts
- maintenance acts
- other consequential world-changing actions

## Constitutional Runtime Usage

The constitutional runtime must also be used in two phases.

### During building

Use it for:

- admitting generation steps
- dry-run versus apply
- capability checks
- deterministic planning and lowering
- evidence closure discipline
- protected operations such as migration apply or package publication

### After code is ready

Use it for:

- action admission
- policy checks
- capability realization checks
- routing and lowering
- execution boundary discipline
- evidence requirement enforcement
- idempotency discipline

## App Position

Apps remain downstream payloads.

They may consume:

- schemas
- policies
- manifests
- probes
- generated contracts
- runtime services

They may not become the source of doctrine or institutional law.

This applies to App Park, Intelligence, and any later app surfaces.

## Non-Negotiable Repo Rules

```txt
doctrinec compiles blocks, not prose
no direct edits in foundation/upstream
no generated file is hand-edited
no live runtime evidence or receipts in git
no schema without source doctrine block
no policy without source invariant
no consequential build step without evidence and receipt
```

## Freeze Condition

The repo equation is considered placed when:

```txt
the directory layout expresses the hierarchy
the doctrine path is strict
the bootstrap lane exists
the foundation boundary is explicit
the contract index exists
the build and ops runtime split is explicit
the evidence and receipt storage boundary is explicit
```

At that point the repo stops being `src/` soup and starts becoming a constitutional build system.