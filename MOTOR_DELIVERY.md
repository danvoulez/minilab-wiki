# Motor delivery: Dynamic Projections UI

## Delivered

The delivered motor is the **Dynamic Projection UI surface inside Eve / Processual UI**.

It now has:

- `runtime_projection` Eve tool for requesting a bounded read-only projection.
- projection normalizer that rejects authority-claiming projections and requires source refs.
- `DynamicProjectionSurface.vue` as the reusable UI card for projection rendering.
- chat tool wiring so Eve dynamic tool parts render as projection cards.
- `/projections` demo route to show the projection UI outside a chat turn.
- root `./dream` commands to inspect, prepare, validate, and run the three-folder workspace.

## Explicit boundary

Dynamic Projections are UI/read surfaces.

They do not:

- register receipts;
- dispatch executors;
- mutate LogLine;
- mutate Envelope;
- treat a projection as truth;
- perform runtime actions.

## Validated

`./dream validate` passed locally:

- LogLine: `265 passed, 1 skipped`.
- Envelope typecheck: passed.
- Envelope test typecheck: passed.
- Processual UI contract validation: `15 contracts` passed.
- Processual UI Vue/TypeScript typecheck: passed.

The Envelope Vitest runtime check is skipped on this Linux validation host because the bundled pnpm tree came without the Linux Rollup optional native package. The type surfaces pass; running `./dream prepare --install` on the target host refreshes platform optional dependencies.
