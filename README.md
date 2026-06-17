
# 🚀 Composable Data Stack (CDS)

> **Terraform for data platforms.**  
> Build, validate, and evolve data stacks using modular components and explicit contracts.

---

## 🧠 What is CDS (in 1 minute)

Composable Data Stack (CDS) is a framework for defining and assembling data platforms from reusable modules such as orchestrators, warehouses, BI tools, and secrets providers.

Instead of hardcoding integrations or relying on fragile pipelines, CDS introduces:

- 🔧 **Modules** → reusable components (Dagster, Postgres, Superset)  
- 🔗 **Contracts** → explicit interfaces between components  
- 🧩 **Profiles** → fully composed, runnable stacks  

> Think: **Infrastructure-as-Code, but for data platforms**

✅ Swap tools without rewrites  
✅ Avoid hidden coupling  
✅ Build reproducible stacks  

---

## ⚡ Why CDS?

Modern data platforms force a trade-off:

| Approach | Problem |
|----------|--------|
| Monolithic stack | Rigid, hard to evolve |
| Custom pipelines | Flexible but fragile and inconsistent |

👉 CDS gives you the best of both:

- composability **without chaos**
- flexibility **with guarantees**
- modularity **with structure**

---

## 🎯 When to use CDS

Use CDS if you:

- want to swap tools (Airflow ↔ Dagster, Superset ↔ Metabase)
- need reproducible environments across dev / CI / prod
- are building a platform for multiple teams
- want contract-driven integration instead of implicit coupling

CDS may be overkill if:

- you only run a single-tool stack
- you don't need interchangeable components

---

## 🏗️ Example

The `local-dagster-postgres-superset` profile defines:

- Dagster -> orchestration  
- Postgres -> storage  
- Superset -> BI  

### What CDS does:

1. Validates module definitions  
2. Resolves contract bindings  
3. Checks compatibility and security constraints  
4. Produces a fully wired stack definition  

You can replace components without changing the system behavior:

```
Dagster -> Airflow
Superset -> Metabase
Postgres -> MariaDB
```

---

## 🗺️ Architecture Overview

CDS wires modules through **contracts**, not direct dependencies:

```
        +-----------+
        |  Dagster  |
        +-----------+
             |
   (transformation-runner)
             |
             v
        +-----------+
        | Postgres  |
        +-----------+
             |
      (warehouse-query)
             |
             v
        +-----------+
        | Superset  |
        +-----------+
```

---

## 📦 What you get

When you run CDS:

- validated module graph  
- resolved contract bindings  
- dependency-aware execution plan  
- generated Docker Compose configuration  
- reproducible stack definition  

This allows you to go from a declarative profile to a runnable local data stack.

Coming next:

- Docker / Kubernetes generation  
- one-command stack bootstrap  
- automated health checks  

---

## 🚀 Quickstart

### 1. Clone

```bash
git clone https://github.com/RonaldHensbergen/composable-data-stack.git
cd composable-data-stack
```

### 2. Setup environment

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -e .
```

### 3. Configure environment

```bash
cp .env.example .env
```

Set:

```
CDS_POSTGRES_PASSWORD
CDS_SUPERSET_SECRET_KEY
CDS_SUPERSET_ADMIN_PASSWORD
```

### 4. Validate a stack

```bash
cds validate local-dagster-postgres-superset
```

Expected output:

```text
Profile is valid.
```text

### 5. Render the stack

```bash
cds render local-dagster-postgres-superset
```

This generates:

- docker-compose.yml
- service definitions
- fully wired module configuration

---

## 🧩 Core Concepts

### Modules

Reusable building blocks:

- orchestration (Dagster, Airflow)
- warehouse (Postgres, MariaDB)
- BI (Superset, Metabase)
- secrets (env, vault)

Structure:

```
modules/<category>/<name>/
├── module.yaml
├── defaults.yaml
├── compose.yaml
├── scripts/
└── tests/
```

---

### Contracts

Contracts define how modules interact.

Examples:

| Contract | Purpose |
|----------|--------|
| sql-database | database interface |
| http-service | service exposure |
| secrets-provider | secret resolution |

Example binding:

```yaml
dagster.database -> postgres.sql-database
superset.database -> postgres.sql-database
```

No implicit dependencies — everything is explicit.

---

### Profiles

Profiles define supported stacks:

```
local-dagster-postgres-superset
local-airflow-postgres-superset
integration-airflow-postgres-dbt
```

Structure:

```
profiles/<profile>/
├── profile.yaml
├── values.yaml
└── README.md
```

---

## ⚙️ CLI

| Command | Description |
|--------|------------|
| cds validate <profile> | Validate modules and contracts |
| cds plan <profile> | Generate execution plan (planned) |
| cds render <profile> | Generate Docker Compose configuration from a resolved plan |
| cds up <profile> | Start services (planned) |
| cds test <profile> | Run health checks (planned) |

---

## 🔄 Workflow

```
1. Validate modules
2. Resolve contracts
3. Generate plan
4. Render runtime assets
5. Start services
6. Run health checks
7. Produce environment report
```

---

## 📂 Repository Structure

```
.
├── cli/
├── modules/
│   ├── bi/
│   ├── orchestration/
│   ├── secrets/
│   └── warehouse/
├── profiles/
├── docs/
├── pyproject.toml
└── Makefile
```

---

## 📌 Status

MVP ready:

- module validation  
- contract resolution  
- security checks  
- profile composition  

Next:

- rendering  
- secrets integration  
- runtime generation  
- full stack bootstrap  
- smoke tests  
