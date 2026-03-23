# Alpe

**Sovereign Cloud for Europe**

Europe has datacenters, engineers, and world-class networking infrastructure. What it lacks is the platform: the managed services, the operational guarantees, the self-service experience that makes hyperscale cloud providers the default choice.

AWS did not win because of its servers. It won because when you click "create database," you get automated backups, failover, monitoring, scaling, and patching — without hiring a DBA. That operational layer is the product. The servers underneath are a commodity.

Alpe is the operational layer — built in the open, running on European infrastructure, under European law.

---

## What Alpe Is

Alpe is an open-source sovereign cloud platform. European infrastructure providers supply the compute, the network, and the physical security. Alpe supplies everything above that: the control plane, the managed services, the developer experience, the sovereignty guarantees, and the operational accountability.

When you run `alpe deploy` or click "Create Database" in the console, you are provisioning managed services that Alpe operates, on European hardware, under European jurisdiction — with operational guarantees that Alpe is accountable for.

Alpe is not a tool. Infrastructure-as-code tools describe what to deploy. They do not operate anything. They do not wake up at 3 AM when your database runs out of disk. Alpe does.

---

## Product Catalog

Three managed services, two interfaces (CLI and console).

**Alpe Compute** — Run containerized applications with a URL, TLS, and logs. Custom domains, rolling deploys with zero downtime, health checks, auto-restart, horizontal scaling (1–10 replicas), and Prometheus metrics.

**Alpe Database** — Production-ready managed PostgreSQL. Daily automated backups with 7-day retention, point-in-time recovery, connection pooling via PgBouncer, TLS in transit, encryption at rest, automatic minor version upgrades, and alerting. Synchronous read replicas on Medium plans and above.

**Alpe Storage** — S3-compatible object storage. Every tool and SDK that works with S3 works with Alpe Storage. Encryption at rest, versioning, lifecycle policies, and access logging.

For services Alpe does not yet manage natively, deploy any container on Alpe Compute and wire it to your applications. The workload runs on European infrastructure under the same sovereignty guarantees.

---

## Two Interfaces, One API

**The CLI** — A single static binary. Three commands to production:

```
$ alpe db create --name main
$ alpe storage create --name uploads
$ alpe deploy
```

Result: a running app with managed Postgres, S3-compatible storage, auto-TLS, and linked environment variables. No mention of Kubernetes, Helm, pods, or ingress.

**The Console** — A full management interface, not a read-only dashboard. Create databases, deploy applications from Git, scale replicas, trigger restores, stream logs, review audit trails. A developer who has never opened a terminal can go from sign-up to running application entirely through the browser.

The centerpiece is the sovereignty dashboard: a map of Europe showing where resources physically run, which jurisdiction governs them, encryption status, and operator identity. This is the screen that gets screenshotted for compliance reports and projected at board meetings.

Both interfaces consume the same API. Neither is an afterthought.

---

## Sovereignty as Architecture

Every other "sovereign cloud" implements sovereignty as a policy layer: choose a region, sign a contract. The enforcement is legal, not technical.

Alpe implements sovereignty as architecture. The resource model encodes jurisdiction as a first-class type. The federation replication engine checks jurisdiction constraints at the protocol level. A database tagged with German jurisdiction physically cannot replicate to a non-German operator — not because of a policy document, but because the replication protocol rejects it and the Rust compiler ensures every code path checks jurisdiction.

---

## Federation

Alpe is not a company. It is a network. Any European infrastructure provider can run an Alpe instance, pass the automated conformance test suite, and join the federation. If the company behind Alpe disappears, the software and the network survive because every operator has the full stack.

Federation is not a free-for-all. The API contract is versioned and immutable once published. Operators run the same Alpe binary — they configure infrastructure-level parameters but cannot modify the API surface, the resource model, or the sovereignty enforcement layer. Strict conformance, not loose compatibility.

For infrastructure providers, Alpe transforms commodity offerings into platform businesses. A dedicated server becomes a substrate for managed Postgres, object storage, and container workloads that command higher margins and create stickier relationships.

---

## Technical Architecture

The control plane is written in Rust. This is a deliberate choice: a cloud platform must run for decades, and the Rust compiler catches entire categories of bugs at compile time. A single statically-linked binary for every component means the barrier to becoming a federation operator is as low as possible.

**Control plane store:** PostgreSQL (not etcd or CRDs). Rich multi-tenant queries, transactional writes, audit logging, and cross-cluster federation require a real database.

**Workload execution:** Kubernetes, driven through kube-rs. The Rust code bridges both through well-typed interfaces.

**Operators:** Three operators manage the lifecycle of managed services — Database (CloudNativePG), Storage (MinIO), and App Runtime (Buildpacks + cert-manager). Each watches the control plane database for resources needing reconciliation and drives the underlying system through Kubernetes.

**Console:** React + TypeScript with Next.js. Tailwind CSS with a custom design system. D3 for the sovereignty map. SSE subscriptions for real-time provisioning progress and log streaming.

### Workspace

```
alpe/
  crates/
    alpe-core/               # Shared types, resource model, errors
    alpe-api/                # API server (axum + sqlx)
    alpe-auth/               # Authentication + RBAC (JWT)
    alpe-operator-db/        # Postgres lifecycle operator (kube-rs + CloudNativePG)
    alpe-operator-storage/   # Object storage operator (kube-rs + MinIO)
    alpe-operator-app/       # App runtime operator
    alpe-substrate/          # Cluster provisioning (Hetzner API)
    alpe-cli/                # The alpe binary (clap)
    alpe-sdk/                # Client SDK (reqwest)
  console/                   # Web console (React + TypeScript)
```

### Key Libraries

`axum` for HTTP, `sqlx` for compile-time checked Postgres queries, `kube-rs` for Kubernetes, `clap` for CLI, `tokio` everywhere, `tracing` + OpenTelemetry from day one.

---

## Principles

**Open source, no asterisks.** The entire stack — backend, operators, CLI, and console — is open source under permissive licensing. Not "open core." Not "source available." Trust requires verifiability, and verifiability requires access to every line of code.

**Build first, standardize later.** Working software before specifications. Code is the specification.

**Operational excellence is the product.** The difference between a tool and a platform is who wakes up when something breaks. Alpe wakes up.

**Sovereignty is not isolation.** S3-compatible storage, standard Kubernetes APIs, OpenID Connect, OpenTofu. Migration should be a decision, not a prison sentence.

**Developer experience is not a luxury.** A platform that requires a consultant to deploy a database will lose to one that takes thirty seconds.

---

## Roadmap

**MVP** — Alpe Compute, Alpe Database (managed PostgreSQL), Alpe Storage (S3-compatible), CLI, and console. Single operator, single region, containers only.

**Post-MVP** — Managed Redis/Valkey (+3 months), federation protocol v1 (+6–9 months), managed Kafka (+12 months), networking primitives, managed identity, lakehouse, and data orchestration in year 2–3.

---

## The Name

The Alps are the backbone of Europe. They span eight countries, connect cultures, and have endured for millions of years. They are solid, shared, and no one owns them.

Alpe is infrastructure with the same qualities: solid, shared, enduring, and sovereign.

---

## Join Us

If you are an engineer who has worked on cloud infrastructure and wants to build something that matters — contribute. If you are an organization that needs sovereign cloud and is willing to be an early adopter — talk to us. If you believe that Europe's digital autonomy depends on open-source software built by and for Europeans — this is the project.

`alpe deploy` — and begin.