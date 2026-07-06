# Dream Machine — Atlas

*One system, three organs, mid-consolidation. Written 2026-06-26.*

---

## 1. The one calming truth

This is **one system**, not many. It is a ledger-backed constitutional runtime: every arrival is registered into append-only memory, only complete records matching registered contracts activate, and consequence flows through a governed selector/executor tree — with cryptographic receipts as identity. The Python kernel (LogLine-Acts) is the authority; the TypeScript Envelope-Ledger is the additive transport-and-custody layer that wraps those acts without ever mutating them; the UI is the visible board that humans and the model will manipulate in real time. The spine, in one line: **content-addressed acts, append-only, hashed as identity, activated only by contract, wrapped additively — add, never subtract.**

---

## 2. The three organs

### Organ I — `Dream-Machine-LogLine-Acts` (the kernel / authority)
*Path: `/Users/ubl-ops/Projetos/Dream-Machine-LogLine-Acts`*

A zero-dependency Python runtime where arrivals register into an append-only SQLite+Postgres ledger, complete records matching process contracts activate, and consequence flows through a selector → executor → adapter tree. Identity is a nine-slot receipt hashed with canonical RFC 8785 JSON; dangerous (L4/L5) operations fail closed without verified WebAuthn/passkey signoff. It exports a `lab` CLI (`doctor`, `foundation suite`, `dream verify`, `harness`, `fleet audit`). **Build state: green — all 242 tests pass in 2.11s**, covering JCS conformance vectors, receipt hardening, append-only invariants, danger tiers, authority boundaries, contract activation, adapters, projections, and the new oauth-client process (12 tests). This is the constitutional heart and it is sound.

### Organ II — `Dream-Machine-Envelope-Ledger` (the additive spine / transport)
*Path: `/Users/ubl-ops/Projetos/Dream-Machine-Envelope-Ledger`* — `@tabuleiro/board-core v0.1.0`

A TypeScript ledger runtime ("Tabuleiro") modeling work as append-only chains of BoardActs, Shifts, Findings, and Projections, all backed by deterministic `board-json-v0` sha256 hashing and prefix-valid state reconstruction. Its just-completed foundation is the **Envelope**: it wraps an intact LogLine act with custody, transport, and shift observations under a strict additive law — `createEnvelope`/`appendShift` accrete custody and shifts but never mutate content (dual-hash: thin skin-only vs full bundle). **Build state: green — 18 test files, 103 passing tests in 54.53s**, including envelope dual-hash, Act-chain verification, admission delay/authority, ShiftResult binding, Finding lifecycle, and 100-run fuzzing for bit-flip / row-drop / tamper detection.

### Organ III — `Dream-Machine-Processual-UI` (the visible board / face)
*Path: `/Users/ubl-ops/Projetos/Dream-Machine-Processual-UI`*

The third organ exists on disk but was not part of the two repo maps audited. Honestly: this is the **"visible board that humans and the model manipulate in real-time"** that both specs name as the open question (BOARD SPEC §16 q5) and explicitly defer. Treat it as **scaffolded, not yet load-bearing** — the canvas/tabling UX is a separate phase that comes after the spine is proven. Do not claim it works until it has been opened and verified; it is named here for completeness, not certified.

---

## 3. What is actually DONE

Consolidated and honest across the kernel and the spine:

- **Core kernel (LogLine):** receipt, store, runtime, contracts, authority, grants — complete and tested.
- **Determinism (LogLine):** RFC 8785 canonicalization proven against official conformance vectors; `content_hash` = identity.
- **Signature boundary (LogLine):** WebAuthn/passkey verification for L4/L5; fails closed gracefully.
- **CLI suite (LogLine):** `doctor`, `foundation suite`, `dream verify`, `harness`, `fleet audit` fully functional.
- **Contract system (LogLine):** 11 process contracts defined and governing activation, danger tiers, and evidence obligations.
- **Adapters & projections (LogLine):** 5 non-authoritative adapters registered; read-only attention/projection mechanisms; Dream Machine schema validators.
- **Append-only enforcement (LogLine):** SQLite + Postgres insert-only DDL, no updates/deletes, RLS on all tables.
- **Receipt hardening (LogLine):** commit `c4e2d59` closed a real authority gap (`verify()` had accepted extra `envelope_hash` fields) per LIP-0007.
- **Envelope foundation (Envelope-Ledger):** dual-hash Envelope, append-only constructors, additive-law validation, content-intact spec + tests.
- **Core ledger lifecycle (Envelope-Ledger):** Act/Candidate/Shift/ShiftResult with hashing, arming delay, admission authority, edit audits, material-edit reset, ShiftResult binding.
- **Integrity verification (Envelope-Ledger):** `verify()` detects bit-flips, missing rows, tampering; prefix-valid Projection checks; 100-run fuzzing.
- **Persistence (Envelope-Ledger):** SQLite/pglite/Neon via a common Db interface, append-only guards, migrations; event zoning with 90s TTL + evaporation.
- **Board facade & type safety (Envelope-Ledger):** single public entry point; branded immutable types; RiskTier/Severity systems.
- **Doctrine (both):** LAB specs + ADRs 0001–0003, SECURITY.md, CONTRIBUTING.md; BOARD SPEC v0.2 + Envelope Additive Spine design with invariants 1–10.

---

## 4. What is half-done

- **OAuth client (LogLine, `oauth-client.v1`):** kernel adapter, dry-run boundary, deterministic RFC 7591 request building, and 12 passing tests are *done*. What remains: the edge-effect tool (`tools/register_oauth_client.py`) is written but not deployed, and the Supabase auth-admin API integration is designed but not live.
- **Canyon / Universal Inbox (LogLine, Slice 1):** design spec complete; HTTP registrar endpoint, inbox table schema, and port-7010 Cloudflare ingress designed. Not yet: the Python 3.13 door implementation, table creation, or launchd/Manhattan supervision.
- **Envelope queryability index (Envelope-Ledger):** derived, non-authoritative observability dimensions (semantic, custody, boundary, outcome, lineage, zone, salience). Spec written; implementation deferred.
- **Scene & bounded model perception (Envelope-Ledger):** per-Shift hashable, prefix-valid Projection so models read a Scene, never raw ledger. Spec written; deferred.
- **Attention governance (Envelope-Ledger):** scarce budget, anomaly/loop-risk detection. Shadow-mode only in v0.1.
- **Dynamic authority scoring (Envelope-Ledger):** `effective_authority = role_ceiling * score_factor`, deny high-risk below threshold. Shadow-mode tracking only; enforcement deferred.
- **OAuth crossing (Envelope-Ledger):** the first real client of the Envelope model (edge TS records a Shift across the Supabase boundary). Spec written; deferred.

---

## 5. What is at risk / uncommitted

**Uncommitted in LogLine-Acts (this is the fragile part — it lives only on disk):**

- **MODIFIED (5 tracked files):** `docs/DANGER_TIERS.md`, `lab/adapters.py`, `processes/CURRENT_RUNNABLE_PROCESSES.md`, `processes/PROCESS_CATALOG.md`, `tests/test_grants_danger_tiers.py` — all the oauth-client.v1 wiring.
- **UNTRACKED (not yet under version control at all):** `lab/oauth.py` (150 lines, the kernel adapter), `processes/oauth-client.v1.yml`, `tests/test_oauth_client.py` (145 lines, 12 tests), `tools/register_oauth_client.py`, `docs/CANYON.md`, `docs/decisions/` (three ADR PDFs), and `docs/superpowers/specs/2026-06-25-universal-inbox-door-design.md`.

**Envelope-Ledger:** git status clean — all work committed atomically (envelope foundation in 5 commits). Nothing at risk here.

**Known risks / honest caveats (not bugs, but do not pretend otherwise):**

- **Canonicalization mismatch (spec erratum 2026-06-26):** Envelope-Ledger uses `board-json-v0`, **not** RFC 8785 JCS like LogLine. Cross-system parity is *not* guaranteed and is not yet harmonized with LogLine's `verify-receipt.mjs`. Implementers must not assume parity.
- **No RBAC enforcement yet (Envelope-Ledger):** authority checks exist but denied ops are not locked — shadow-mode only.
- **Model input guardrails not enforced (Envelope-Ledger):** Scene perception is spec-only; models currently see raw propositions.
- **Kernel (LogLine):** no broken or risky items detected — all core invariants enforced.

---

## 6. Prioritized next actions

Ordered cheapest-and-most-at-risk first, so nothing valuable is lost to a disk mishap before it earns a commit.

1. **Commit the OAuth slice in LogLine-Acts — today.** This is the single most urgent item: ~545 lines of *passing, tested* work (`lab/oauth.py`, `oauth-client.v1.yml`, `test_oauth_client.py`, plus the 5 tracked edits) exist only as uncommitted/untracked files. Commit them as one cohesive slice; note in the message that the kernel builds the request deterministically (dry-run, RFC 7591 validated, evidence = `request_hash` + `client_metadata_hash`) and that the edge effect (`tools/register_oauth_client.py`) lives outside the kernel boundary by design.

2. **Track the institutional memory still floating.** Add the three ADR PDFs in `docs/decisions/`, `docs/CANYON.md`, and the Universal-Inbox design spec to version control (same or a follow-up commit). These are high-signal and currently untracked — one `rm -rf` away from gone.

3. **Re-run `lab foundation suite` after committing.** Confirm RFC 8785 conformance vectors and golden receipt digests are still stable post-oauth. Cheap, and it certifies the green state you just committed.

4. **Stage Canyon Slice 1 in thin steps.** Create the Supabase `inbox` table (RLS, realtime=false) → write the zero-dep `lab/inbox/door.py` health stub on port 7010 → curl test → only then wire Cloudflare ingress and launchd/Manhattan supervision. Scope-locked to registrar only; routing/sweep/wake/passport stay in slices 2–4.

5. **Harmonize or formally fork the canonicalization story.** Write the cross-system conformance plan: re-derive a single act body under both `lab/receipt.py` (JCS) and Envelope-Ledger `board-json-v0`, decide the convergence target, and either prove parity or document the fork loudly. This is the one cross-organ correctness question that could bite later.

6. **Build the Envelope queryability index (pglite/pg).** Materialize the derived observability dimensions from Shifts — kept non-authoritative and outside `envelope_hash`. Unblocks semantic/anomaly search without touching the authoritative chain.

7. **Open and verify Organ III (`Dream-Machine-Processual-UI`).** Establish its real state honestly before claiming anything about it. Once the spine is committed and conformance is settled, the visible board is the next phase that turns this from a proven runtime into a usable one.

8. **Begin the Envelope guardrail trio (Scene → attention → dynamic authority) in shadow-mode.** Lowest urgency: log without denying, gather evidence, and only later promote to enforcement. Order matters — Scene first (bounds what models see), then attention, then authority scoring.
