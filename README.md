# Festival Internacional de Artes Vivas de Loja (FIAVL)

Repositorio del proyecto universitario en equipo (**3 integrantes**) regido por:

- **Specification-Driven Development (SDD)** con `github/spec-kit`.
- **Modelo de Vistas 4+1 de Kruchten** documentado bajo **UML 2.5.1**.
- **ISO/IEC/IEEE 42010** (descripción arquitectónica, interesados, preocupaciones, puntos de vista, vistas, ADRs y correspondencias).
- **ISO/IEC 25010:2023** (modelo de calidad de producto de 9 pilares, redactado con *Condición + Medida*).
- **Diagramación en Mermaid** versionada en Markdown y compatible con **DeepWiki** y GitHub.

> **Constitución obligatoria:** Toda sesión humana o asistida por agentes IA debe respetar [AGENTS.md](./AGENTS.md), [CLAUDE.md](./CLAUDE.md), [.specify/memory/constitution.md](./.specify/memory/constitution.md) y la guía operativa [docs/architecture/GUIA_METODOLOGICA_DAS.md](./docs/architecture/GUIA_METODOLOGICA_DAS.md).

---

## Estructura base del repositorio

```text
FIAVL/
├── AGENTS.md                              # Constitución canónica (reglas R1–R6, ISO 42010, ISO 25010:2023)
├── CLAUDE.md                              # Entrada Claude Code (importa @AGENTS.md)
├── README.md                              # Flujo SDD y convención colaborativa de ramas
├── .gitignore                             # Exclusiones estrictas (OneDrive, secretos, IDEs, entornos, build)
├── .specify/                              # Infraestructura y constitución sincronizada de github/spec-kit
│   └── memory/constitution.md
├── .agents/skills/                        # Skills del proyecto para Antigravity (speckit-* + frontend-design)
├── .claude/skills/                        # Skills del proyecto para Claude Code (speckit-*)
├── specs/                                 # Especificaciones SDD habilitantes (compuerta previa a src/)
│   └── 000-tdr/                           # TDR del cliente, vacíos, preguntas, supuestos y RNF iniciales
├── docs/
│   └── architecture/                      # DAS, vistas 4+1 en Mermaid (DeepWiki) y referencia metodológica
│       ├── GUIA_METODOLOGICA_DAS.md       # Síntesis operativa de las guías oficiales de la asignatura
│       └── adr/                           # Architecture Decision Records (ADR-001, ...)
├── src/                                   # Esqueleto 1:1 de 3 capas (subpaquetes pendientes de Vista de Desarrollo)
│   ├── domain/                            # Capa superior: negocio puro, entidades, control ECB y puertos
│   ├── utility/                           # Capa intermedia: servicios transversales (Prueba 3 preguntas)
│   └── platform/                          # Capa inferior: persistencia, frameworks, red, UI/adaptadores
└── tests/                                 # Pruebas de aceptación (specs/) y verificación arquitectónica
```

---

## Flujo SDD y convención de ramas (3 integrantes)

### Compuerta SDD (`github/spec-kit`)
1. **Especificación (`specs/`):** redactar y aprobar requisitos, criterios de aceptación y atributos ISO/IEC 25010:2023 (`Condición + Medida`).
2. **Arquitectura 4+1 (`docs/architecture/`):** modelar/actualizar en Mermaid Escenarios (+1), Vista Lógica, Vista de Procesos (ECB), Vista de Desarrollo (3 capas 1:1) y Vista de Despliegue (UML 2.5.1).
3. **Plan y Tareas (`/speckit-plan`, `/speckit-tasks`):** desglosar trabajo por capas/paquetes sin violar `Domain -> Utility -> Platform`.
4. **Implementación y Verificación (`/speckit-implement`):** construir `src/` y `tests/` trazados 1:1 contra la especificación aprobada.

### Reglas Git y formato de ramas
- **`main` protegida:** prohibido hacer commits directos o `git push` directo tras el commit inicial. Toda integración entra mediante **Pull Request (PR)**.
- **Revisión obligatoria:** cada PR requiere al menos **1 aprobación de otro integrante** validando trazabilidad SDD, reglas R1–R6 y sintaxis Mermaid limpia.
- **Convención de nombres:** `<tipo>/<integrante>/<referencia-kebab>`
  - `spec/<integrante>/<NNN-tema>` — especificaciones en `specs/` (ej. `spec/carlos/000-tdr`).
  - `arch/<integrante>/<vista-o-adr>` — vistas 4+1 en Mermaid y ADRs en `docs/architecture/`.
  - `feat/<integrante>/<NNN-capacidad>` — implementación autorizada por una spec `Approved`.
  - `fix/<integrante>/<NNN-ajuste>` — correcciones trazadas a defectos o diagramas.
  - `chore/<integrante>/<tema>` — configuración, CI o tooling del repositorio.
- **Commits sugeridos (Conventional Commits):** `docs(spec): ...`, `docs(arch): ...`, `feat(domain): ...`, `test(spec-001): ...`, `chore(repo): ...`.
