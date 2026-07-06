# Envelope Additive Spine — Design

- **Date:** 2026-06-26
- **Status:** draft for review
- **Authors:** Dan + Claude
- **Repos in scope:** `Dream-Machine-Envelope-Ledger` (primary), with binding references to `Dream-Machine-LogLine-Acts` (receipt authority) and `Dream-Machine-Processual-UI/docs` (jurisdiction contracts)
- **Grounded in:** `docs/board/` (BOARD SPEC v0.2 §0/§3/§10, BOARD_DECISIONS/OBJECTS/LIFECYCLE/VERTICAL_SLICE) and LogLine ADR-0001 (digestion, attention v5) / ADR-0002 (risk, attention saturation, enzyme boundary)

## Problem

The Envelope definition was settled late and the two halves — LogLine (Python kernel) and the Envelope-Ledger (TypeScript) — were built separately. That produced **subtraction oversteps**: the Envelope-Ledger grew its own canon (`board_act`, `proposal`, renamed slots) that *competes with or replaces* the LogLine act instead of *wrapping* it. Symptoms:

- "Envelope" names two different things — the LogLine **transport-wire envelope** (`{ content, transport, envelope_hash }`, today living in `verify-receipt.mjs`) and the **Envelope-Ledger** (a cognition runtime). Neither jurisdiction contract claims "transport".
- The transport-wire envelope appeared jurisdictionally homeless.

This spec resolves the definition so transport, custody, and observability share one coherent model, and so the OAuth crossing (and every future crossing) has a place to land.

## The law (settled)

| Concept | Biology | Rule |
|---|---|---|
| LogLine act | polar amino acid (rich monomer) | frozen, content-addressed; backbone fixed; `if_ok/if_doubt/if_not` is the reactive side chain; **never rewritten** |
| Process | enzyme | recognizes acts by reading the side chain through *its own* contract; **recognition = movement** |
| Movement | catalysis | emergent, not declared → **unknowable from the act; only observable** |
| Envelope | the observed-reaction record | **additive** — accretes around the intact act, never subtracts |
| Shift | one catalytic event | "process P recognized act A, moved it, custodian C, here are the hashes" |

`if_ok / if_doubt / if_not` are **not a routing table.** Every process interprets them through its own contract; the same fields mean different things to different processes, and that interpretation *is* how acts are recognized and moved. Because movement is emergent, an act's path cannot be read off the act — it can only be **observed**. That emergence is the reason the observability ledger exists (many hashes, frenetic queryability); a routing table would not need one.

## The canonical Envelope object

```
Envelope = {
  content:            <LogLine act, byte-for-byte; content_hash preserved & re-derivable>,
  shifts:             [ Shift, ... ],     // each recognition-movement; each has its own shift_hash
  custody:            [ ... confirmed_by handoffs, observed ... ],
  transport:          { sent_by, sent_to, channel, ... } | null,  // present only when a Shift crosses a boundary
  thin_envelope_hash: <hash over the SKIN: transport + custody + shift refs + content_hash reference; NOT the content body>,
  envelope_hash:      <hash over the WHOLE bundle, including the content body>
}
```

- A **Shift** is the canonical additive unit. Identity (`shift_hash`) **never includes `output_hash`** (already enforced in the ledger). A `shift_result` binds `shift_hash → output_hash` once the output exists. Transport = movement-observed; custody = `confirmed_by`-observed.

### Dual envelope hash

Mirrors the receipt's `tuple_hash` (slots-only) vs `content_hash` (everything), lifted to the envelope layer:

- **`thin_envelope_hash`** — hashes the envelope *skin* only: `transport`, `custody`, the list of `shift_hash` references, and a **reference** to the act's `content_hash`. It deliberately excludes the content body, so the movement/transport layer can be addressed, verified, and queried **without carrying the payload**. This is the queryability enabler.
- **`envelope_hash`** — hashes the whole bundle including the content body. Full tamper-evidence.

Both are carried on every Envelope, `sha256` over canonical bytes.

> **Erratum (2026-06-26, post-implementation):** the Envelope-Ledger computes these hashes over its own **`board-json-v0`** canonicalization (sorted keys, no whitespace, `JSON.stringify` string/number rules), *not* true RFC 8785 (JCS). The two agree for ASCII/string payloads but differ on number serialization. `board-json-v0` is the Envelope's own canon and the correct authority here. A cross-system implementer (e.g. the OAuth crossing) MUST NOT assume JCS parity until a dedicated conformance step establishes it — that re-derivation/parity is explicitly out of scope for this spine (see *Out of scope*).

## Invariants (the additive law, made checkable)

1. **Content intact.** `content_hash(content)` is unchanged and re-derivable via the receipt authority (`lab/receipt.py` / `tests/test_receipt_hardening.py` parity). Constructing or extending an Envelope never mutates `content`.
2. **Append-only.** Shifts/custody/observations accrete; no field inside `content` is ever altered.
3. **No slot rename.** No envelope-local object may rename or redefine a LogLine slot.
4. **No authority inversion.** No envelope-local object (`board_act`, `proposal`, etc.) may be authoritative *over* the act for LogLine; it is `authoritative_for_logline: false`.
5. **Binding reference.** Any envelope-canon object that asserts about an act MUST carry that act's `content_hash`.
6. **Hash discipline.** `thin_envelope_hash` excludes the content body; `envelope_hash` includes it; both are deterministic JCS+sha256.

## Queryability & Attention

The ledger exists for *frenetic queryability* — but **queryability is not access, and attention is the one thing that bounds it.** Grounded in BOARD SPEC §10 (Scenes), ADR-0001 (Attention v5 — scarce, governed presence; not an error queue), and ADR-0002 (attention saturation as a first-class loop-risk).

### The queryability layer (additive, derived, non-authoritative)

Each Shift carries denormalized, indexed observability dimensions, materialized in the pglite/pg store for query and semantic search. This layer is **derived** from the authoritative Shifts, freely rebuildable, and **never part of `envelope_hash`** (cf. BOARD_OBJECTS §1: database indexes and rendered fields are not identity).

| Dimension | Fields | Query it answers |
|---|---|---|
| Recognition | process_id, process_contract_hash, branch_fired (ok / doubt / not), role, kind | "every act process P took the *doubt* branch on" |
| Actors & custody | actor, custodian (confirmed_by), custody_from → custody_to | "the custody chain of cargo X" |
| Boundary / transport | crossed_boundary, sent_to, channel, internal / external | "all crossings to Supabase" |
| Outcome | status, severity, finding_raised | "shifts that closed in doubt" |
| Lineage edges | act_content_hash, prev_shift_hash, source_digest, parent_envelope_hash | movement-graph traversal |
| Freshness / zone | zone (live / buffered / evaporated), as_of_seq, observed_at, ttl | "what's stale / still live" |
| Salience | open_findings, attention / anomaly flags | marks where attention *could* be spent |
| Semantic | per-shift gloss / narrative block | vector / natural-language search |

### The three budgets

Frenetic queryability splits into budgets the first pass blurred:

| Concern | Budget | Governed by |
|---|---|---|
| Indexing + cheap broad projection | **unlimited** — additive, L0, rebuildable | nothing; grow freely |
| Model perception | **bounded per Shift** | the **Scene** (a hashable, prefix-valid Projection) |
| Attention / raising presence | **scarce** | anomaly detection, loop-risk, integrity score |

Salience is indexed richly so attention can be spent *well* — but spending attention is a separate, bounded act, never a column you flood.

### Invariants (extend the additive law)

7. **Query is cheap and broad.** The observability index is derived, non-authoritative, freely rebuildable, and never part of `envelope_hash`; it may grow without limit (L0 projections).
8. **Model access is Scene-mediated.** A model reads a bounded, hashable Scene per Shift — never raw frenetic access to the index. The Scene is itself a governed, prefix-valid Projection.
9. **Attention is scarce and governed.** Raising presence is subject to attention-anomaly and loop-risk governance and is *never* auto-escalated from a query. The index *marks* salience; *spending* attention is separate and bounded.

## Reconciliation approach: B — constrain-in-place

Chosen over A (purist collapse) and C (split/rename) because it is the only approach that obeys the additive law *during the migration itself* — it adds binding constraints rather than deleting primitives.

- **Keep** every existing primitive: `event`, `source_digest`, `proposal`, `board_act`, `shift`, `shift_result`, `finding`, `projection`, `projection_pin`.
- **`shift` / `shift_result`** — promote to the canonical movement primitives. Already shaped correctly. No structural change.
- **`board_act`** — keep the name, enforce invariants 4 & 5: it is an envelope-local annotation that wraps an act *by content_hash* and is never authoritative for LogLine.
- **`proposal`** — structured possibility; must reference act/source by hash; non-sovereign (invariants 4 & 5).
- **Enforcement** lives in the Envelope's own validation (`src/validate.ts` / `src/verify.ts`) plus vitest. **Do not** replace Envelope validation with LogLine/JCS, and **do not** make JSON Schema the canonical authority — JSON Schema stays an auxiliary shape artifact. Envelope validation is its own jurisdiction; this spec only *adds* the additive-binding checks and the dual-hash computation.

## Testing strategy

New/extended vitest suites:

- **content preservation** — round-trip an act through Envelope construction; assert `content_hash` is byte-for-byte preserved and re-derivable.
- **dual hash** — `thin_envelope_hash` and `envelope_hash` are distinct, deterministic, order-independent (JCS); changing the content body changes `envelope_hash` but **not** `thin_envelope_hash`; changing transport changes both.
- **append-only** — extending an Envelope with a new Shift leaves `content` and prior shift hashes unchanged.
- **additive-law rejection** — a `board_act`/`proposal` lacking the act `content_hash`, or attempting a slot rename / authority inversion, is rejected by `validate`/`verify`.
- **index is derived** — rebuilding the queryability index from the authoritative Shifts reproduces it byte-for-byte; mutating/dropping the index never changes any `envelope_hash` or Shift identity.
- **Scene is bounded & prefix-valid** — a Scene built for a Shift is a Projection that passes source-prefix validity; a model is never handed the raw index.
- **attention is not auto-raised** — a query that surfaces salient rows does not, by itself, raise attention; raising presence requires a separate governed step (anomaly/loop-risk checked).

## Out of scope (separate specs)

- The **OAuth crossing** itself (edge TS performing the Supabase POST and recording the resulting Shift). It is the *first client* of this model; it gets its own spec once this spine lands.
- Removing/renaming any primitive (explicitly rejected — that would be a subtraction).
- The LogLine `verify-receipt.mjs` transport-envelope tooling — it remains, and stays in parity; convergence onto this object model is a later step.

## First client (informative, not built here)

When the `oauth-client` process recognizes the act `requested_oauth_client` (`if_ok: oauth-client.v1`) and moves it across the Supabase boundary, the edge TS records that as a **Shift** on the intact act: custody handoff to Supabase, `transport` populated, `shift_result` binding the returned `client_id`. The act is never subtracted. This validates the spine end-to-end.
