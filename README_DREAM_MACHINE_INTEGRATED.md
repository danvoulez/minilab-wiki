# Dream Machine integrated package

This package integrates the three project folders as one runnable Dream Machine workspace:

- `Dream-Machine-LogLine-Acts/` — LogLine authority/kernel and receipt/runtime tests.
- `Dream-Machine-Envelope-Ledger/` — Envelope additive spine and projection primitives.
- `Dream-Machine-Processual-UI/` — Eve/Nuxt Processual UI portal.

## Objective

The current integrated objective is **Dynamic Projections as UI**:

> The Eve portal renders bounded, source-backed, non-authoritative projection cards over runtime/ledger material. A Dynamic Projection is an operator surface for inspection and narration. It is not a backend workflow, not a registration path, not a commit path, and not a hidden approval ceremony.

## Commands

```bash
./dream doctor
./dream prepare
./dream validate
./dream run
```

`./dream validate` runs the concrete checks across the three folders:

- LogLine Python test suite.
- Envelope TypeScript type surfaces.
- Processual UI contract validation.
- Processual UI Vue/TypeScript typecheck.

## UI entry points

- Chat tool renderer: `Dream-Machine-Processual-UI/app/components/chat/tool/RuntimeProjection.vue`
- Projection surface component: `Dream-Machine-Processual-UI/app/components/projections/DynamicProjectionSurface.vue`
- Demo route: `Dream-Machine-Processual-UI/app/pages/projections/index.vue`
- Tool adapter: `Dream-Machine-Processual-UI/agent/tools/runtime_projection.ts`
- Normalizer: `Dream-Machine-Processual-UI/agent/lib/projection-normalizer.ts`
- Contract: `Dream-Machine-Processual-UI/docs/dream-machine-projections.v0.yml`

## Runtime connection

If a runtime projection endpoint exists, set:

```bash
export DREAM_MACHINE_RUNTIME_PROJECTION_URL="http://127.0.0.1:PORT/runtime_projection"
./dream run
```

Without that endpoint, the tool still renders a local read-only projection surface so the UI path is alive and typechecked.
