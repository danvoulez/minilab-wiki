# 07 — Doppler / Config Canon

**Status:** Canon draft  
**Layer:** Operational delegation, configuration, secrets, brakes, provider material, runtime environment doctrine  
**Parent:** 01 — Minilab Constitution / Strategic Architecture; 03 — Authority / Power Doctrine; 04 — Evidence / Receipt Doctrine; 06 — Database Projection Canon  
**Scope:** Doppler project/config structure, environment variables, config classes, secret policy, hard brakes, doctor/digest/receipt commands, database projection of config requirements, runtime read rules, and anti-sprawl rules for `minilab.work`  
**Downstream documents:** App Park Config Contract, Provider Adapter Contracts, Runtime Config Doctor, Supabase Safety Baseline, Deployment Plans

---

## 0. Purpose

This document defines how Minilab treats Doppler and runtime configuration.

It exists to prevent:

```txt
secret leakage
config sprawl
environment drift
Doppler-as-authority
Doppler-as-canon
feature flags as hidden law
brakes bypassed by scripts
secret values in database/logs/receipts/reports
provider credentials becoming permission
```

Doppler is Dan’s operational delegation vault.

It stores material required for machinery to run. It does not define institutional meaning. It does not authorize consequence. It does not close proof.

---

## 1. Main Law

```txt
Doppler stores operational delegation.
Doppler does not define institutional meaning.
```

Portuguese form:

```txt
Doppler guarda delegação operacional.
Doppler não define significado institucional.
```

Expanded:

```txt
Doppler may hold secrets.
Doppler may hold config.
Doppler may hold flags.
Doppler may hold brakes.
Doppler may hold provider credentials.
Doppler may hold runtime pointers.

Doppler may not define LogLine canon.
Doppler may not approve protected power.
Doppler may not close receipts.
Doppler may not override Authority.
Doppler may not make provider success into proof.
```

---

## 2. Project Structure

Canonical Doppler project:

```txt
minilab
```

Canonical configs:

```txt
dev_local
lab8gb
lab512
lab256
gateway_staging
production
```

Rules:

```txt
One project.
Multiple configs.
No duplicate projects by whim.
No app-specific project sprawl without explicit amendment.
No environment names that hide authority or production meaning.
```

If additional configs are needed, they must be proposed through a Config Canon Amendment.

Examples of possible future additions requiring amendment:

```txt
app_park_staging
app_park_production
lab512_app_park
control_plane_staging
```

These may be valid, but not by accident.

---

## 3. Config Classes

Every key belongs to exactly one primary class.

```txt
secret
config
flag
brake
pointer
identity
window
provider
```

### 3.1 secret

Sensitive runtime value.

Examples:

```txt
SUPABASE_SERVICE_ROLE_KEY
OPENAI_API_KEY
GITHUB_TOKEN
CLOUDFLARE_API_TOKEN
DATABASE_URL when it contains credentials
```

### 3.2 config

Non-secret operational setting.

Examples:

```txt
MINILAB_ENV
MINILAB_LOG_LEVEL
RECEIPT_PROFILE
DATABASE_SCHEMA_VERSION
```

### 3.3 flag

Behavior toggle.

Examples:

```txt
DATABASE_ENABLED
GATEWAY_ENABLED
LAB_ENABLED
OBSERVABILITY_ENABLED
```

### 3.4 brake

Safety override that disables or constrains power.

Examples:

```txt
MINILAB_DRY_RUN
MINILAB_SAFE_MODE
MINILAB_ALLOW_DB_WRITE
MINILAB_ALLOW_PROVIDER_MUTATION
MINILAB_ALLOW_LAB_EXEC
```

### 3.5 pointer

Path, URL, ID, ref, or location.

Examples:

```txt
MINILAB_HOME
MINILAB_WORKSPACE_ROOT
SUPABASE_URL
GATEWAY_BASE_URL
ARTIFACT_STORAGE_ROOT
```

### 3.6 identity

Technical or human identity.

Examples:

```txt
HUMAN_OPERATOR_ID
LAB512_ID
LAB8GB_ENTITY_ID
DOPPLER_MANAGED_BY
```

### 3.7 window

Temporal authorization/config interval.

Examples:

```txt
TOWER_PASSKEY_WINDOW_SECONDS
MAINTENANCE_WINDOW_START
MAINTENANCE_WINDOW_END
```

### 3.8 provider

External SDK/cloud/app credential or provider-specific config.

Examples:

```txt
OPENAI_*
ANTHROPIC_*
GITHUB_*
CLOUDFLARE_*
SUPABASE_*
SLACK_*
ATTIO_*
GOOGLE_*
```

---

## 4. Universal Secret Rules

Secret values must never appear in:

```txt
repo
database values
receipts
reports
logs
browser payloads
Inspector raw payloads
screenshots unless explicitly redacted
```

Allowed:

```txt
secret key name
secret class
presence/absence
required_for list
Doppler project/config name
redacted digest of schema/key list when safe
secret_values_printed: false
```

Forbidden:

```txt
SUPABASE_SERVICE_ROLE_KEY=eyJ...
Authorization: Bearer ...
DATABASE_URL=postgres://user:password@...
.env dump with values
secret in receipt metadata
secret in report finding
```

---

## 5. Config Is Not Authority

Config may shape runtime behavior, but cannot replace authority.

Forbidden meanings:

```txt
DOPPLER_CONFIG=production means action is authorized
MINILAB_ALLOW_DB_WRITE=true means semantic write is admitted
OPENAI_ENABLED=true means LLM may mutate
LAB_ENABLED=true means LAB may execute any command
CLOUDFLARE_ENABLED=true means Gateway is institution
SUPABASE_ENABLED=true means Supabase is canon
```

Correct meaning:

```txt
Config enables machinery surface.
Authority decides whether a specific action may use that machinery.
Receipts close scoped claims.
```

---

## 6. Hard Brakes

Hard brakes are safety keys that default to denial.

Core brakes:

```txt
MINILAB_SAFE_MODE=true
MINILAB_DRY_RUN=true
MINILAB_ALLOW_DB_WRITE=false
MINILAB_ALLOW_GATEWAY=false
MINILAB_ALLOW_LAB_EXEC=false
MINILAB_ALLOW_PROTECTED_COMMANDS=false
MINILAB_ALLOW_PROVIDER_MUTATION=false
MINILAB_ALLOW_REAL_LEDGER_APPEND=false
MINILAB_ALLOW_CLOUDFLARE_DEPLOY=false
MINILAB_ALLOW_SUPABASE_MUTATION=false
MINILAB_ALLOW_NEON_MUTATION=false
MINILAB_ALLOW_CALENDAR_WRITE=false
MINILAB_ALLOW_ATTIO_WRITE=false
MINILAB_ALLOW_SLACK_WRITE=false
MINILAB_ALLOW_GITHUB_WRITE=false
```

Secret safety brakes:

```txt
MINILAB_FORBID_SECRET_LOGGING=true
MINILAB_FORBID_SECRET_RECEIPTS=true
MINILAB_FORBID_SECRET_DATABASE_STORAGE=true
MINILAB_FORBID_SECRET_REPO_STORAGE=true
SECRET_REDACTION_ENABLED=true
SECRET_REDACTION_MODE=strict
SECRET_SCAN_BEFORE_RECEIPT=true
SECRET_SCAN_BEFORE_LOG=true
SECRET_SCAN_BEFORE_DB_WRITE=true
SECRET_SCAN_BEFORE_ARTIFACT_WRITE=true
ALLOW_ENV_DUMP=false
ALLOW_PRINT_SECRET_NAMES=true
ALLOW_PRINT_SECRET_VALUES=false
```

Rule:

```txt
Brakes are checked before execution.
Brake failure denies or ghosts.
A script may not bypass a brake because it has access to a secret.
```

---

## 7. Current Safe Default State

For early or uncertain phases, the safe state is:

```txt
draft_only
dry_run
safe_mode
receipt doctor allowed
temp ledger allowed
no DB write
no Gateway mutation
no LAB execution
no Tower protected command
no provider mutation
no real ledger append
```

Keys expected `true` in safe early state:

```txt
MINILAB_SAFE_MODE=true
MINILAB_DRY_RUN=true
CLI_REQUIRE_RECEIPT_FOR_IMPORTANT_COMMANDS=true
CLI_REQUIRE_DRY_RUN_FOR_MUTATIONS=true
RECEIPT_DOCTOR_ENABLED=true
RECEIPT_DOCTOR_TEMP_ONLY=true
RECEIPT_DOCTOR_ALLOW_JSON_ATOMIC=true
RECEIPT_DOCTOR_ALLOW_UBL_LEDGER_TEMP=true
RECEIPT_DOCTOR_ALLOW_LLLV_CORE=true
RECEIPT_TEMP_LEDGER_ENABLED=true
RECEIPT_EXISTING_HASH_IMMUTABLE=true
RECEIPT_TAMPER_CHECK_REQUIRED=true
OBSERVABILITY_ENABLED=true
SECRET_REDACTION_ENABLED=true
```

Keys expected `false` until gates prove readiness:

```txt
DATABASE_ENABLED=false
GATEWAY_ENABLED=false
LAB_ENABLED=false
TOWER_ENABLED=false
GATE_ENABLED=false
ADMISSION_ENABLED=false
SUPABASE_ENABLED=false
NEON_ENABLED=false
CLOUDFLARE_ENABLED=false
MINILAB_ALLOW_DB_WRITE=false
MINILAB_ALLOW_GATEWAY=false
MINILAB_ALLOW_LAB_EXEC=false
MINILAB_ALLOW_PROTECTED_COMMANDS=false
MINILAB_ALLOW_PROVIDER_MUTATION=false
MINILAB_ALLOW_REAL_LEDGER_APPEND=false
```

A complete schema is allowed.
A complete power surface is not automatically allowed.

```txt
Schema complete is good.
Execution without gate is mud.
```

---

## 8. Canonical Environment Namespaces

Namespaces:

```txt
MINILAB_*
HUMAN_*
DOPPLER_*
LOGLINE_*
CLI_*
WORKORDER_*
RECEIPT_*
GATE_*
ADMISSION_*
TOWER_*
LAB_*
LAB8GB_*
LAB512_*
LAB256_*
MCP_LOCAL_*
GATEWAY_*
CLOUDFLARE_*
DATABASE_*
SUPABASE_*
NEON_*
ARTIFACT_STORAGE_*
SNAPSHOT_*
OPENAI_*
ANTHROPIC_*
GOOGLE_AI_*
OLLAMA_*
GITHUB_*
GOOGLE_CALENDAR_*
GOOGLE_DRIVE_*
ATTIO_*
SLACK_*
BROWSER_AUTOMATION_*
MACOS_AUTOMATION_*
EXPEDITION_*
MAINTENANCE_*
INCIDENT_*
BUILD_*
RELEASE_*
OBSERVABILITY_*
SECRET_REDACTION_*
APP_PARK_* when introduced
```

Namespace ownership must be documented.

Apps may have app-specific prefixes only when admitted:

```txt
APP_<APP_ID>_*
APP_PARK_<SERVICE>_*
```

But app-specific keys must still classify as secret/config/flag/brake/pointer/identity/window/provider.

---

## 9. Canonical Schema File

A machine-readable schema must exist in the repo without secret values.

Path:

```txt
config/doppler.schema.yaml
```

Schema must define:

```txt
version
status
evidence_status
doctrine
invariants
classes
rules
keys
```

Minimum structure:

```yaml
version: minilab-doppler-v0.1
status: draft
evidence_status: architecture-aligned / implementation-unverified

doctrine:
  sentence: >
    Doppler is Dan's operational delegation vault. It stores secrets,
    config, flags, windows, pointers, and brakes. It enables machinery
    without becoming authority.
  invariants:
    - no_secret_value_in_repo
    - no_secret_value_in_database
    - no_secret_value_in_receipt
    - no_secret_value_in_logs
    - no_doppler_flag_defines_logline_canon
    - no_doppler_flag_changes_receipt_hash_semantics
    - no_protected_command_without_tower_passkey_window_and_receipt
    - no_semantic_database_write_without_logline_act
    - no_gateway_as_institution
    - no_lab_identity_as_authority
    - no_provider_success_as_closure

classes:
  secret: sensitive runtime value
  config: non-secret operational setting
  flag: behavior toggle
  brake: safety override that disables power
  pointer: path, URL, ID, or reference
  identity: human, LAB, runtime, app, or entity identity
  window: bounded authorization period
  provider: external SDK/cloud/app credential
```

---

## 10. Key Schema Requirements

Every key entry must define:

```txt
class
required
default when applicable
allowed values when applicable
purpose
provider when applicable
secret policy when applicable
gate or brake relation when applicable
```

Example:

```yaml
MINILAB_ENV:
  class: config
  required: true
  default: dev_local
  allowed: [dev_local, lab8gb, lab512, lab256, gateway_staging, production]
  purpose: logical environment name

MINILAB_SAFE_MODE:
  class: brake
  required: true
  default: "true"
  purpose: global safety brake

SUPABASE_SERVICE_ROLE_KEY:
  class: secret
  required: false
  provider: supabase
  purpose: privileged Supabase key; forbidden until database gate
```

Key schema may contain names and metadata. It must not contain secret values.

---

## 11. Human Delegation Fields

Doppler represents delegated operational context, not final authority.

Canonical fields:

```txt
HUMAN_OPERATOR_ID=dan
HUMAN_OPERATOR_NAME=Dan
HUMAN_OPERATOR_EMAIL=
HUMAN_OPERATOR_HANDLE=
HUMAN_TIMEZONE=Europe/Lisbon

HUMAN_APPROVAL_MODE=manual
HUMAN_APPROVAL_REQUIRED_FOR_PROTECTED=true
HUMAN_APPROVAL_REQUIRED_FOR_DB_WRITE=true
HUMAN_APPROVAL_REQUIRED_FOR_PROVIDER_MUTATION=true
HUMAN_APPROVAL_REQUIRED_FOR_LAB_EXEC=true
```

Meaning:

```txt
This environment carries operational delegation for Dan.
It may use secrets/configs to keep machinery functioning.
Consequential action still requires Authority, Gate, Tower, Passkey, Receipt, or Dan approval according to command type.
```

Doppler is the hand on the electrical panel.
It is not the final signature on the contract.

---

## 12. LogLine / Receipt Fields

Canonical examples:

```txt
LOGLINE_CANON_MODE=local
LOGLINE_CANON_SOURCE=local_workspace
LOGLINE_CANON_PATH=
LOGLINE_CANON_VERSION=
LOGLINE_CANON_DIGEST_EXPECTED=

LOGLINE_TUPLE_FIELDS=9
LOGLINE_RECEIPT_PROFILE=
LOGLINE_STRONG_GRAMMAR_ENABLED=false
LOGLINE_IR_ENABLED=false
LOGLINE_RUNTIME_WALK_ENABLED=true
LOGLINE_RUNTIME_EXECUTION_ENABLED=false

RECEIPT_PROFILE=
RECEIPT_HASH_OWNER=
RECEIPT_ROOT=receipts
RECEIPT_STORAGE_MODE=local
RECEIPT_EXISTING_HASH_IMMUTABLE=true
RECEIPT_TAMPER_CHECK_REQUIRED=true
RECEIPT_REQUIRE_SCOPE=true
RECEIPT_REQUIRE_EVIDENCE_MODE=true
RECEIPT_REQUIRE_COMMAND_OUTPUT=true
RECEIPT_ALLOW_SECRET_VALUES=false
```

Rules:

```txt
Doppler may point to canon/profile.
Doppler may not redefine canon/profile semantics.
A mismatched receipt profile must block or ghost receipt emission.
```

---

## 13. Gate / Tower / Authority Fields

Examples:

```txt
GATE_ENABLED=false
GATE_MODE=disabled
GATE_PROVIDER=none
GATE_REQUIRE_LOGLINE_ACT=true
GATE_REQUIRE_RECEIPT_PATH=true
GATE_ALLOW_COMMIT=false
GATE_ALLOW_GHOST=true
GATE_ALLOW_REJECT=true
GATE_ALLOW_NEEDS_APPROVAL=true
GATE_DEFAULT_DECISION=needs_approval

ADMISSION_ENABLED=false
ADMISSION_CACHE_ENABLED=false
ADMISSION_EXPLAIN_ENABLED=true

TOWER_ENABLED=false
TOWER_BASE_URL=
TOWER_ENTITY_ID=
TOWER_COMMAND_MODE=disabled
TOWER_PROTECTED_COMMANDS_ENABLED=false
TOWER_NORMAL_COMMANDS_ENABLED=false
TOWER_PASSKEY_REQUIRED=true
TOWER_PASSKEY_RP_ID=
TOWER_PASSKEY_ORIGIN=
TOWER_PASSKEY_WINDOW_SECONDS=0
TOWER_LEDGER_REQUIRED=true
TOWER_RECEIPT_REQUIRED=true
```

Meaning:

```txt
Config can enable authority machinery.
Authority still evaluates each action.
Tower still requires scoped power discipline.
```

---

## 14. LAB Fields

Common:

```txt
LAB_ENABLED=false
LAB_REGISTRY_ENABLED=false
LAB_HEARTBEATS_ENABLED=false
LAB_EXECUTION_ENABLED=false
LAB_RECEIPTS_REQUIRED=true
LAB_IDENTITY_REQUIRED=true
LAB_CAPABILITY_SCOPE=local_execution
LAB_ALLOWED_JOB_CLASSES=none
```

LAB-8GB:

```txt
LAB8GB_ENABLED=false
LAB8GB_ID=
LAB8GB_NAME=LAB-8GB
LAB8GB_TYPE=physical_lab
LAB8GB_RUNNER_ID=
LAB8GB_ENTITY_ID=
LAB8GB_HOSTNAME=
LAB8GB_HEARTBEAT_INTERVAL_SECONDS=60
LAB8GB_EXECUTION_ENABLED=false
LAB8GB_ROLE=supervisor
```

LAB-512:

```txt
LAB512_ENABLED=false
LAB512_ID=
LAB512_NAME=LAB-512
LAB512_TYPE=physical_lab
LAB512_RUNNER_ID=
LAB512_ENTITY_ID=
LAB512_HOSTNAME=
LAB512_HEARTBEAT_INTERVAL_SECONDS=60
LAB512_EXECUTION_ENABLED=false
LAB512_ROLE=inference
LAB512_OLLAMA_ENABLED=false
LAB512_OLLAMA_BASE_URL=http://localhost:11434
```

LAB-256:

```txt
LAB256_ENABLED=false
LAB256_ID=
LAB256_NAME=LAB-256
LAB256_TYPE=physical_lab
LAB256_RUNNER_ID=
LAB256_ENTITY_ID=
LAB256_HOSTNAME=
LAB256_HEARTBEAT_INTERVAL_SECONDS=60
LAB256_EXECUTION_ENABLED=false
LAB256_ROLE=workbench
LAB256_BROWSER_AUTOMATION_ENABLED=false
LAB256_MACOS_AUTOMATION_ENABLED=false
```

Rule:

```txt
LAB identity is execution attribution.
It is not authority.
```

---

## 15. Database / Supabase Fields

Database common:

```txt
DATABASE_ENABLED=false
DATABASE_PROVIDER=none
DATABASE_URL=
DATABASE_SCHEMA_VERSION=minilab-db-v0.3
DATABASE_DRY_RUN=true
DATABASE_SEMANTIC_WRITES_REQUIRE_LOGLINE=true
DATABASE_STORE_SECRET_VALUES=false
DATABASE_ALLOW_DIRECT_CRUD=false
DATABASE_ALLOW_REGISTRY_MUTATION=false
DATABASE_ALLOW_OPS_LOGINE_ACT_INSERT=false
DATABASE_ALLOW_RECEIPT_INDEX_WRITE=false
DATABASE_ALLOW_SNAPSHOT_INDEX_WRITE=false
DATABASE_ALLOW_EXPEDITION_INDEX_WRITE=false
```

Supabase:

```txt
SUPABASE_ENABLED=false
SUPABASE_URL=
SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
SUPABASE_PROJECT_REF=
SUPABASE_SCHEMA=minilab
SUPABASE_DRY_RUN=true
SUPABASE_ALLOW_MIGRATIONS=false
SUPABASE_ALLOW_RLS_CHANGES=false
SUPABASE_ALLOW_SERVICE_ROLE=false
```

Rules:

```txt
DATABASE_ENABLED enables database machinery, not semantic permission.
MINILAB_ALLOW_DB_WRITE gates database mutation.
Semantic writes require LogLine-shaped acts.
Service role must remain server-only.
Secret values must not be stored.
```

---

## 16. Gateway / Cloudflare Fields

Gateway:

```txt
GATEWAY_ENABLED=false
GATEWAY_MODE=disabled
GATEWAY_BASE_URL=
GATEWAY_ENTITY_ID=
GATEWAY_MCP_ENABLED=false
GATEWAY_PUBLIC_INGRESS_ENABLED=false
GATEWAY_ADMISSION_REQUIRED=true
GATEWAY_RECEIPTS_REQUIRED=true
GATEWAY_DIRECT_LAB_MUTATION_ALLOWED=false
GATEWAY_ALLOW_PROVIDER_MUTATION=false
GATEWAY_ALLOW_DB_MUTATION=false
```

Cloudflare:

```txt
CLOUDFLARE_ENABLED=false
CLOUDFLARE_ACCOUNT_ID=
CLOUDFLARE_API_TOKEN=
CLOUDFLARE_WORKER_NAME=minilab-gateway
CLOUDFLARE_WORKER_ENV=staging
CLOUDFLARE_DEPLOY_ENABLED=false
CLOUDFLARE_DRY_RUN=true
```

Rules:

```txt
Gateway is edge projection.
Cloudflare is door, not throne.
Gateway cannot mutate LABs directly without Tower/admission.
Cloudflare deploy requires explicit brake release and receipt path.
```

---

## 17. Provider Fields

Provider namespaces include:

```txt
OPENAI_*
ANTHROPIC_*
GOOGLE_AI_*
OLLAMA_*
GITHUB_*
GOOGLE_CALENDAR_*
GOOGLE_DRIVE_*
ATTIO_*
SLACK_*
```

Examples:

```txt
OPENAI_ENABLED=false
OPENAI_API_KEY=
OPENAI_DEFAULT_MODEL=
OPENAI_ALLOW_TOOL_CALLS=false
OPENAI_ALLOW_MUTATION=false

GITHUB_ENABLED=false
GITHUB_TOKEN=
GITHUB_OWNER=
GITHUB_DEFAULT_REPO=
GITHUB_ALLOW_READ=true
GITHUB_ALLOW_WRITE=false
GITHUB_ALLOW_PR_CREATE=false
GITHUB_ALLOW_COMMIT=false
```

Rules:

```txt
Provider enabled means machinery is available.
Provider mutation requires Authority and mutation brake.
Provider response is evidence input, not closure.
```

---

## 18. App Park Config Extension

App Park may extend this canon when App Park contracts are introduced.

Likely keys:

```txt
APP_PARK_ENABLED=false
APP_PARK_HOST=lab512
APP_PARK_RUNTIME_PROVIDER=docker_compose
APP_PARK_PROXY_PROVIDER=caddy
APP_PARK_EVENT_PROVIDER=supabase_postgres
APP_PARK_QUEUE_PROVIDER=supabase_postgres
APP_PARK_STORAGE_PROVIDER=local_fs
APP_PARK_WEBHOOK_GATEWAY_ENABLED=false
APP_PARK_WEBSOCKET_GATEWAY_ENABLED=false
APP_PARK_DEFAULT_EXPOSURE=private
APP_PARK_ALLOW_PUBLIC_ROUTES=false
APP_PARK_REQUIRE_HEALTH=true
APP_PARK_REQUIRE_RECEIPTS=true
APP_PARK_REQUIRE_BACKUP_FOR_STATEFUL=true
```

Rules:

```txt
App Park config declares providers and defaults.
App Park authority still comes from Control Plane / Authority Core.
App Park members still need manifests and admitted capabilities.
```

App Park config extension requires App Park Config Contract.

---

## 19. Forbidden Variables

These variables must never exist.

```txt
LOGLINE_CANON_OVERRIDE=
RECEIPT_HASH_OVERRIDE=
FORCE_VERIFIED=true
SKIP_GATE=true
ALLOW_UNRECEIPTED_MUTATION=true
ALLOW_SECRET_LOGGING=true
ALLOW_PROVIDER_SUCCESS_AS_CLOSURE=true
CLOUDFLARE_IS_AUTHORITY=true
SUPABASE_IS_CANON=true
LAB_ID_IS_AUTHORITY=true
LLM_CAN_DISPATCH=true
NATURAL_LANGUAGE_EXECUTES=true
```

If any key like this appears, config doctor must fail.

---

## 20. Runtime Read Rules

Every sensitive command must begin by reading config safely.

Flow:

```txt
command starts
→ load env / Doppler material
→ validate schema
→ redact secrets
→ classify execution mode
→ apply brakes
→ check command class
→ check Authority requirements
→ run or reject/ghost
→ emit receipt/ghost when relevant
```

No sensitive command may run before config doctor passes for the relevant scope.

No command may print secret values in normal operation.

---

## 21. CLI Config Commands

The CLI requires a config surface.

Commands:

```txt
minilab config doctor
minilab config print-safe
minilab config schema
minilab config requirements
minilab config digest
minilab config receipt
```

### 21.1 `minilab config doctor`

Checks:

```txt
required variables present
boolean values valid
execution mode valid
receipt profile coherent
receipt hash immutability enabled
sensitive brakes false unless gate allows
secret values absent from output
gateway/lab/db/tower disabled if current gate forbids them
forbidden variables absent
schema version matches expected
```

### 21.2 `minilab config print-safe`

Prints only non-secret/safe values.

Example:

```json
{
  "minilab_env": "dev_local",
  "execution_mode": "draft_only",
  "safe_mode": true,
  "dry_run": true,
  "receipt_profile": "logline.receipt.v0",
  "brakes": {
    "db_write": false,
    "gateway": false,
    "lab_exec": false,
    "protected_commands": false,
    "provider_mutation": false,
    "real_ledger_append": false
  },
  "secret_values_printed": false,
  "state": "safe"
}
```

### 21.3 `minilab config digest`

Generates digest of schema and key names, not secret values.

Example:

```json
{
  "schema_version": "minilab-doppler-v0.1",
  "doppler_project": "minilab",
  "doppler_config": "dev_local",
  "key_count": 180,
  "secret_key_count": 24,
  "secret_values_included": false,
  "schema_digest": "..."
}
```

### 21.4 `minilab config receipt`

Emits a Foundation-conformant receipt or configured receipt profile saying:

```txt
which schema was observed
which Doppler project/config was used
which environment was loaded
which brakes were active
which surfaces were disabled/enabled
no secret values were printed
```

---

## 22. Database Projection of Config Requirements

The database does not receive secret values.

It receives requirement names.

Target table:

```txt
registry.config_requirements
```

Example projection:

```sql
insert into registry.config_requirements (
  runtime_entity_id,
  config_key,
  required_for,
  is_secret,
  status,
  description
) values
(
  :cli_runtime_entity_id,
  'RECEIPT_PROFILE',
  array['receipts.verify', 'receipts.doctor_machinery'],
  false,
  'active',
  'Receipt profile expected by Minilab receipt machinery'
),
(
  :database_runtime_entity_id,
  'SUPABASE_SERVICE_ROLE_KEY',
  array['database.projection.write'],
  true,
  'planned',
  'Privileged Supabase key; value lives only in Doppler'
);
```

Rule:

```txt
Doppler owns values.
Database indexes requirements.
Receipts prove usage/readiness.
```

---

## 23. Config Receipt Scope

A config receipt may close narrow claims such as:

```txt
schema minilab-doppler-v0.1 was loaded
dev_local config was inspected
required key names were present
secret values were not printed
brakes were observed false/true
forbidden variables were absent
```

It may not close broad claims such as:

```txt
all secrets are correct
all providers will work
production is safe
all runtime dependencies are configured
all credentials are valid for mutation
```

Credential validity requires provider-specific probes.

---

## 24. Permission Matrix

Practical command type matrix:

```txt
receipt verify                allowed by default if safe
receipt doctor temp           RECEIPT_DOCTOR_ENABLED=true
real ledger append            MINILAB_ALLOW_REAL_LEDGER_APPEND=true

database render dry-run       DATABASE_DRY_RUN=true
database write                MINILAB_ALLOW_DB_WRITE=true
supabase migration            SUPABASE_ALLOW_MIGRATIONS=true

gateway status                GATEWAY_ENABLED=true
gateway deploy dry-run        CLOUDFLARE_DRY_RUN=true
gateway deploy real           MINILAB_ALLOW_CLOUDFLARE_DEPLOY=true

lab status                    LAB_ENABLED=true
lab heartbeat                 LAB_HEARTBEATS_ENABLED=true
lab job run                   MINILAB_ALLOW_LAB_EXEC=true

provider read                 PROVIDER_ENABLED=true
provider mutation             MINILAB_ALLOW_PROVIDER_MUTATION=true

protected command             MINILAB_ALLOW_PROTECTED_COMMANDS=true
```

Even when a flag allows machinery, Authority may still deny a specific action.

---

## 25. Drift and Sprawl Rules

Config drift must be visible.

Doctor should detect:

```txt
unexpected projects
unexpected configs
unexpected keys
missing required keys
forbidden keys
schema version mismatch
secret classified as config
config classified as secret
brake unexpectedly enabled/disabled
environment/config mismatch
production-like config without production brakes
lab config with wrong LAB role
```

Config sprawl must produce a report/ghost, not silent acceptance.

Canonical reconciliation process:

```txt
1. inventory current Doppler projects/configs/keys
2. classify each config/key
3. compare against schema
4. mark canonical / duplicate / obsolete / unknown / app-specific
5. propose amendments or cleanup
6. emit receipt/ghost
```

---

## 26. Environment-Specific Rules

### 26.1 `dev_local`

Expected:

```txt
dry_run=true
safe_mode=true
protected=false
provider mutation=false
DB write=false unless explicit local dry-run path
```

### 26.2 `lab8gb`

Expected role:

```txt
supervisor
control-plane-adjacent
health loops
recovery checks
```

### 26.3 `lab512`

Expected role:

```txt
inference
heavy jobs
App Park runtime candidate
```

### 26.4 `lab256`

Expected role:

```txt
workbench
manual intervention
testing
browser/macOS automation when admitted
```

### 26.5 `gateway_staging`

Expected:

```txt
edge/gateway staging
admission required
no direct LAB mutation
```

### 26.6 `production`

Expected:

```txt
strong brakes
explicit approval
receipts required
no debug leakage
no dry-run confusion
```

---

## 27. UI Projection

Settings → Secrets should show:

```txt
key name
class
required_for
status
source config
last doctor result
secret value printed: false
missing/present
related runtime
related entity
```

Settings → Connections should show:

```txt
provider enabled
read allowed
write allowed
mutation brake
last probe
last receipt
missing proof
```

Settings → Runtimes should show:

```txt
runtime entity
required config keys
present/missing
brakes
last config receipt
```

No UI surface may reveal secret values.

---

## 28. App Park Config Policy Preview

When App Park lands, every App Park member must declare:

```txt
required secrets
optional secrets
runtime provider
route provider
database needs
storage needs
event/work/webhook needs
LLM profile needs
```

Doppler supplies member material only after:

```txt
manifest validated
capability admitted
config requirements projected
secret readiness checked
receipt path exists
```

App Park member config must not become hidden `.env` folklore.

---

## 29. Testing Requirements

Config discipline requires tests/probes for:

```txt
config schema validates
config doctor catches missing required keys
config doctor catches forbidden variables
config doctor redacts secret values
print-safe includes no secrets
digest excludes secret values
database projection contains key names only
secret values not in database
secret values not in receipt
secret values not in report
secret values not in browser payload
brakes block commands
provider mutation denied when brake false
DB write denied when brake false
LAB exec denied when brake false
config sprawl produces report/ghost
```

---

## 30. Anti-Patterns

Reject designs where:

```txt
Doppler config name authorizes action
Doppler flag changes receipt semantics
secret value is stored in database
secret value appears in receipt/report/log/UI
.env file copied by hand becomes source of truth
app-specific configs sprawl without schema
production config has unsafe defaults
script bypasses config doctor
provider key presence equals provider permission
provider success equals closure
LAB config role overrides Authority
Gateway config makes Cloudflare the institution
Supabase config makes Supabase canon
forced variable like FORCE_VERIFIED exists
SKIP_GATE exists
ALLOW_UNRECEIPTED_MUTATION exists
```

---

## 31. Amendment Rule

Changes to this doctrine require a Config Canon Amendment.

```txt
Config Canon Amendment

Proposed change:
Why current config canon fails:
Which key/classes/configs change:
Does it introduce a new Doppler config? yes/no
Does it introduce a new namespace? yes/no
Does it affect secret policy? yes/no
Does it affect brakes? yes/no
Does it affect Authority behavior? yes/no
Does it affect receipt/conformance behavior? yes/no
Migration risk:
Rollback path:
Doctor/digest/receipt changes required:
Dan approval required: yes/no
Foundation governance required: yes/no
```

Foundation governance is required if a change affects LogLine canon, receipt semantics, hash semantics, slot grammar, or conformance behavior.

---

## 32. Current Honest State

```txt
State:
  Canon draft.

Grounded by:
  Existing Doppler Canon v0.1, Minilab Constitution, Authority / Power Doctrine, Evidence / Receipt Doctrine, Database Projection Canon, and Dan Control Plane Doctrine.

Verified:
  This document defines intended config architecture and safety doctrine.

Unverified:
  It does not prove current Doppler project/config inventory.
  It does not prove current key values or classifications.
  It does not prove config doctor implementation.
  It does not prove print-safe/digest/receipt commands exist.
  It does not prove secret values are absent from all logs/db/receipts.

Ghosts:
  Need Doppler inventory receipt.
  Need schema file generation.
  Need config doctor implementation receipt.
  Need print-safe/digest probes.
  Need database config requirement projection dry-run.
  Need App Park config extension after App Park contracts are written.
```

---

## 33. Canonical Closing

```txt
Doppler supplies material.
It does not govern meaning.

Config enables machinery.
It does not authorize consequence.

Brakes stop power.
They are checked before action.

Secrets stay secret.
They are never proof.

The database indexes requirements.
It never stores values.

Receipts prove readiness or use within scope.
They do not expose secrets.

Authority decides.
Tower powers.
LABs execute.
Doppler equips.
```

