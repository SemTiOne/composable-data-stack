# The composable data stack

A modular, self-hosted data platform repository for assembling reproducible data stack distributions from explicit, composable building blocks.

This repository is designed around **modules** and **profiles**:

- **Modules** are reusable platform components such as orchestrators, warehouses, BI tools, secrets providers, and ingress.
- **Profiles** are supported runnable compositions of modules, with explicit bindings and minimal hidden dependencies.

The long-term target is a production-ready distribution for CloudStack-based environments, while preserving a clean path from local development to integration environments and eventually hardened deployments.

## Goals

- Build a **composable** data platform instead of a fixed demo stack
- Support **interchangeable modules**:
  - Airflow or Dagster
  - Postgres, MariaDB, or Spark-oriented backends
  - Superset, Metabase, and other BI options
  - dbt, Great Expectations, Soda, Vault, and more
- Keep **module contracts explicit**
- Make **profiles the unit of support**
- Avoid **implicit coupling** and hidden dependencies
- Evolve from:
  - local development
  - reproducible integration environments
  - production-ready deployments

## Design principles

### Contract-first modules
Each module should declare:

- what it **provides**
- what it **requires**
- configuration inputs
- health checks
- lifecycle hooks
- operational responsibilities

### Profile-driven composition
Profiles define supported combinations of modules.

If a combination is not represented as a profile, it should be treated as experimental rather than supported.

### Minimal hidden dependencies
Modules should interact through declared contracts and bindings, not through undocumented environment variables, cross-folder assumptions, or shared mutable state.

### One architecture, multiple stages
The platform should preserve the same logical composition model across:

- local development
- CI or integration environments
- production deployment targets

Only the runtime packaging and operational controls should change.

## Repository structure

```text
composable-data-stack/
├── LICENSE
├── README.md
├── docs/
├── examples/
├── modules/
├── profiles/
├── scripts/
├── shared/
└── tooling/
```
## Key directories
| Path |	Purpose |
| ---- | ---------- |
| `modules/` |	Deployable, reusable building blocks |
| `profiles/`|	Supported runnable combinations of modules |
| `shared/contracts/`|	Reusable contract definitions between modules |
| `shared/templates/`|	Shared rendering and generation templates |
| `shared/compose/`|	Shared Compose fragments or helpers |
| `tooling/`	|Validation, CLI, linting, and automation tools |
| `docs/`|	Architecture, contracts, modules, operations, and profiles |
| `examples/`|	Example projects, datasets, and sample workloads |

## Modules

Modules are the core building blocks of the platform.

Examples include:

-    `modules/orchestration/airflow`
-    `modules/orchestration/dagster`
-    `modules/warehouse/postgres`
-    `modules/warehouse/mariadb`
-    `modules/transform/dbt`
-    `modules/bi/superset`
-    `modules/quality/great-expectations`
-    `modules/secrets/env`
-    `modules/secrets/vault`

A module should eventually contain:
```text
modules/<category>/<name>/
├── module.yaml
├── defaults.yaml
├── compose.yaml
├── README.md
├── scripts/
└── tests/
```

## Profiles

Profiles are supported compositions of modules.

A profile should define:

-    selected module instances
-    bindings between provided and required contracts
-    stage-specific values
-    supported runtime expectations

Example profile names:

-    `local-airflow-postgres-superset`
-    `local-dagster-postgres-superset`
-    `integration-airflow-postgres-dbt`
-    `cloudstack-airflow-postgres-ha`

A profile should eventually contain:
```text
profiles/<profile-name>/
├── profile.yaml
├── values.yaml
└── README.md
```
## Contracts

Contracts provide the interface layer between modules.

Examples:

-    `sql-database`
-    `secrets-provider`
-    `http-service`
-    `warehouse-query`
-    `transformation-runner`

A module should never depend on another module implicitly. Instead, it should require a contract and receive it through profile bindings.

## Current direction

The repository is being shaped toward:

- **A normalized module layout**
- **Declarative profile definitions**
- **Machine-validated module and profile schemas**
-    **A planner/renderer flow for environment bootstrap**
-    **Portable local and integration runtimes first**
-    **Production-oriented deployment packaging later**

## Planned bootstrap flow

The intended workflow is:

1.    Validate module manifests
1.    Validate profile definitions
1.    Resolve dependencies and bindings
1.    Generate a composition plan
1.    Render runtime assets
1.    Start services in dependency order
1.    Run initialization and health checks
1.    Produce an environment report

Example future commands:
```bash
platform validate
platform plan --profile local-airflow-postgres-superset
platform render --profile local-airflow-postgres-superset
platform up --profile local-airflow-postgres-superset
platform test --profile local-airflow-postgres-superset
```
## Initial implementation priorities

The near-term focus is:

1.    Normalize the repository around modules/ and profiles/
1.    Define shared module and profile schemas
1.    Establish initial contract definitions
1.    Make one profile fully declarative and runnable
1.    Build validation and planning tooling
1.    Add health checks and smoke tests

## MVP target

The initial MVP should center on a single supported local profile:
```text

local-airflow-postgres-superset
```
This is intended to validate:

-    module manifests
-    contract binding rules
-    profile composition
-    local rendering/bootstrap flow
-    health and smoke test behavior

Non-goals for the initial phase

The following are intentionally deferred:

-    full production HA deployment logic
-    advanced CloudStack packaging
-    full upgrade orchestration
-    backup scheduling
-    multi-node Spark orchestration
-    complex secrets management automation

## Documentation

Current and planned documentation lives under `docs/`:

-    `architecture.md`
-    `modules.md`
-    `contracts.md`
-    `profiles.md`
-    `bootstrap.md`
-    `operations.md`

## Status

This repository is under active design and implementation.

The current focus is on establishing:

-    a stable repo shape
-    explicit contracts
-    profile-driven composition
-    a minimal but real bootstrap path

## License

See `LICENSE`.
