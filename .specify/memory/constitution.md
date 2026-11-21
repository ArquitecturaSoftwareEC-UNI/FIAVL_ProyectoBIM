# FIAVL Project Constitution

<!-- 100% aligned with root AGENTS.md (R1-R6 canonical constitution). In case of conflict, AGENTS.md prevails. -->

## Core Principles

### R1. Compuerta Estricta de Specification-Driven Development (SDD) (NON-NEGOTIABLE)
Application code (`src/`, runtime scripts, or implementation tests) **MUST NOT** be implemented without an approved specification in `specs/` (`Status: Approved`, including scope, functional requirements, verifiable acceptance criteria, quality attributes traced to ISO/IEC 25010:2023, and 4+1 impact).
Mandatory workflow: `Specification (specs/) -> 4+1 Plan & Design (docs/architecture/) -> Tasks -> Implementation -> Spec Verification`.
If any Spec Kit template diverges from `AGENTS.md`, `AGENTS.md` prevails.

### R2. Vista de Desarrollo — 3 Capas Descendentes Acíclicas y Correspondencia 1:1 (NON-NEGOTIABLE)
All source code and Development View models follow exactly three downward acyclic layers (DAG): `Domain -> Utility -> Platform`.
- **`Domain` (top):** business rules, invariants, conceptual entities, ECB use-case controls, and required ports. **PROHIBITED** from importing, referencing, or depending directly or transitively on `Platform`.
- **`Utility` (middle):** reusable cross-cutting technical/application services without domain-specific business rules or concrete vendor coupling. **PROHIBITED** from depending on `Domain`.
- **`Platform` (bottom):** infrastructure adapters, physical persistence, HTTP/UI delivery, third-party libraries, network clients, and environment configuration. **PROHIBITED** from depending on `Utility` or `Domain`.
- **No generic catch-all folders:** directories or packages named `Common`, `Común`, `General`, `Shared`, `Utils`, `Helpers`, or `Misc` are **STRICTLY FORBIDDEN**.
- **1:1 Correspondence:** physical directories under `src/` must match 1:1 the packages and dependencies modeled in the Development View.

### R3. Vista de Procesos — Patrón Entidad-Borde-Control (ECB) Estricto (NON-NEGOTIABLE)
All dynamic flows, sequence/communication diagrams, and runtime behavior must explicitly enforce the **Entity-Boundary-Control (ECB)** pattern (`«boundary»`, `«control»`, `«entity»`).
- **Boundary–Entity Isolation:** direct communication between `«boundary»` and `«entity»` is **STRICTLY FORBIDDEN** in both directions.
- Every message between a Boundary and an Entity must pass through a `«control»` component. Intrinsic data invariants remain in `«entity»` (never turn `«control»` into a God Class).

### R4. Vista Lógica — Pureza Conceptual Libre de Infraestructura (NON-NEGOTIABLE)
The entire Logical View (conceptual/domain class diagrams, domain state machines, logical contracts) **MUST remain 100% free** of storage or UI details.
SQL tables, relational schemas, SQL views, indexes, primary/foreign keys (`PK`, `FK`), engine-specific types (`VARCHAR`, `SERIAL`, `UUID` of DB, `JSONB`), UI screens/buttons/routes/forms, HTTP/JSON DTOs, and ORM annotations are **STRICTLY FORBIDDEN** in the Logical View.

### R5. Vista de Despliegue — Rigor UML 2.5.1 (NON-NEGOTIABLE)
All Deployment View diagrams and descriptions must strictly adhere to **UML 2.5.1**, explicitly distinguishing:
1. `«device»` (physical hardware or virtualized compute infrastructure).
2. `«executionEnvironment»` (nested OS/runtimes/web servers/RDBMS/containers inside devices).
3. `«artifact»` (deployable physical build outputs/bundles/images/scripts).
4. `«deploymentSpec»` (parameterized deployment configuration descriptors).
5. Explicit `«manifest»` relationships (from each `«artifact»` to Development View components/packages) and `«deploy»` relationships (to target nodes/environments).

### R6. Diagramas del Modelo 4+1 Versionados en Mermaid Compatible con DeepWiki (NON-NEGOTIABLE)
All Kruchten 4+1 views (`+1 Scenarios`, `Logical`, `Process`, `Development`, `Deployment`) must live as standard versioned **Mermaid** Markdown code in `docs/architecture/`, compatible with GitHub and DeepWiki (`flowchart`, `sequenceDiagram`, `classDiagram`, `stateDiagram-v2` with double-quoted labels for spaces/stereotypes and accent-free node IDs). Binary images alone are rejected. Architectural changes must update Mermaid diagrams and ISO/IEC/IEEE 42010 documentation in the same Pull Request.

## Architecture Documentation & Quality Standards

- **ISO/IEC/IEEE 42010:** Document stakeholders, concerns, viewpoints mapped to 4+1, views, numbered Architecture Decision Records (`docs/architecture/adr/`), and cross-view correspondence rules.
- **ISO/IEC 25010:2023:** Classify all non-functional requirements and quality criteria under the 9 quality characteristics (`Functional Suitability`, `Performance Efficiency`, `Compatibility`, `Interaction Capability`, `Reliability`, `Security`, `Maintainability`, `Flexibility`, `Safety`) using verifiable *Condition + Measure* statements.

## Collaborative GitHub Workflow (3-Member Team)

- `main` is protected after the base commit: direct commits or direct pushes to `main` are forbidden.
- All work integrates through focused short-lived branches (`spec/`, `arch/`, `feat/`, `fix/`, `chore/`) and Pull Requests approved by at least **1 peer reviewer** verifying SDD traceability, R1–R6 compliance, and clean Mermaid rendering.
- Never commit secrets (`.env`, tokens, keys), local IDE artifacts, or generated dependency directories.

## Governance

This constitution is synchronized with `AGENTS.md` and governs all `/speckit-*` workflows (`/speckit-specify`, `/speckit-plan`, `/speckit-tasks`, `/speckit-implement`, `/speckit-analyze`, `/speckit-checklist`, `/speckit-clarify`, `/speckit-converge`). Every AI session must verify gates R1–R6 before designing or editing files. Amendments require team consensus and PR review updating `AGENTS.md` and this file together.

**Version**: 1.0.0 | **Ratified**: 2026-11-21 | **Last Amended**: 2026-11-21
