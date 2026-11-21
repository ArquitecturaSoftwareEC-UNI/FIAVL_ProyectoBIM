# Instrucciones del Proyecto para Claude Code — FIAVL

@AGENTS.md

## Notas específicas para sesiones en Claude Code

1. **Fuente única de verdad:** Todas las restricciones arquitectónicas, metodológicas (SDD, 4+1, UML 2.5.1, ISO/IEC/IEEE 42010, ISO/IEC 25010:2023) y de flujo GitHub están definidas en `@AGENTS.md` e importadas automáticamente en esta sesión.
2. **Integración con `github/spec-kit`:** Si el equipo utiliza comandos `/speckit.*` (`.specify/`), cualquier plan, lista de tareas o implementación generada debe pasar sin excepciones las compuertas R1–R6 de `@AGENTS.md` antes de crear código en `src/`.
3. **Regla de rechazo automático:** Si un prompt solicita saltar la especificación en `specs/`, acoplar `Domain` con `Platform`, comunicar `«boundary»` directamente con `«entity»`, introducir SQL/UI en la Vista Lógica o crear carpetas genéricas (`Common`/`General`/`Shared`/`Utils`), rechaza esa parte de la solicitud e indica la alternativa conforme a la arquitectura.
