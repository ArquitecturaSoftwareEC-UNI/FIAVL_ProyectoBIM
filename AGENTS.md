# Constitución y Reglas del Proyecto — FIAVL

Este documento constituye la normativa arquitectónica y operativa **obligatoria e innegociable** para todas las sesiones de desarrollo (humanas o asistidas por agentes de IA) de los **3 integrantes** del equipo del proyecto **Festival Internacional de Artes Vivas de Loja (FIAVL)**.

Ningún integrante ni agente puede omitir, relajar o posponer estas reglas. Si una instrucción o cambio propuesto contradice esta constitución, **se debe detener la ejecución y corregir el diseño antes de continuar**.

---

## 1. Marco Metodológico y Estándares

El proyecto se rige por:
- **Specification-Driven Development (SDD)** compatible con `github/spec-kit`.
- **Modelo de Vistas 4+1 de Kruchten** documentado bajo **UML 2.5.1**.
- **ISO/IEC/IEEE 42010** para la descripción arquitectónica (*stakeholders*, *concerns*, *viewpoints*, *views*, decisiones arquitectónicas/ADR y reglas de correspondencia entre vistas).
- **ISO/IEC 25010:2023** como modelo de calidad de producto (9 características: *Functional Suitability*, *Performance Efficiency*, *Compatibility*, *Interaction Capability*, *Reliability*, *Security*, *Maintainability*, *Flexibility* y *Safety*).
- **Diagramación en Mermaid** versionada en el repositorio y compatible con **DeepWiki** y GitHub.

---

## 2. Reglas Innegociables

### R1. Compuerta Estricta de Specification-Driven Development (SDD)
1. **Queda terminantemente prohibido implementar código de aplicación** (en `src/`, scripts productivos o pruebas ligadas a implementación) sin una **especificación aprobada** ubicada en la carpeta `specs/`.
2. Una especificación se considera habilitante solo cuando contiene alcance, requisitos funcionales, criterios de aceptación verificables, atributos de calidad trazados a **ISO/IEC 25010:2023**, impacto en las vistas 4+1 y estado explícito `Approved` (revisada en PR).
3. El flujo obligatorio para toda capacidad es:
   `Especificación (specs/) -> Plan y Diseño 4+1 (docs/architecture/) -> Desglose de Tareas -> Implementación -> Verificación contra Spec`.
4. Al inicializar `github/spec-kit`, `.specify/memory/constitution.md` deberá sincronizarse con las reglas de este archivo; en caso de conflicto, **prevalece esta constitución**.

### R2. Vista de Desarrollo — Tres Capas Descendentes Acíclicas y Correspondencia 1:1
1. Todo el código fuente y su modelado en la Vista de Desarrollo se organizan en exactamente **tres capas descendentes acíclicas (DAG)**:
   - **`Domain` (Capa superior):** reglas de negocio, invariantes, entidades conceptuales, casos de uso/control de dominio y contratos/puertos requeridos.
   - **`Utility` (Capa intermedia):** servicios transversales reutilizables de aplicación o soporte técnico sin lógica de negocio específica de un dominio ni acoplamiento a proveedores concretos.
   - **`Platform` (Capa inferior):** adaptadores de infraestructura, persistencia física, interfaces de usuario/entrega HTTP, librerías de terceros, clientes de red y configuración del entorno.
2. **Dirección única de dependencias:**
   - `Domain -> Utility -> Platform` únicamente en sentido descendente.
   - **Prohibido que cualquier módulo de `Domain` importe, referencie o dependa directa o transitivamente de `Platform`.**
   - **Prohibido** todo ciclo entre capas (`Platform -> Utility`, `Platform -> Domain`, `Utility -> Domain`) y todo ciclo entre módulos dentro de una misma capa.
3. **Prohibición de carpetas cajón de sastre:** quedan prohibidas carpetas o paquetes genéricos como `Common`, `Común`, `General`, `Shared`, `Utils`, `Helpers` o `Misc`. Cada paquete o directorio debe responder a un nombre de dominio, capacidad utilitaria o adaptador de plataforma explícito y cohesivo.
4. **Correspondencia 1:1:** la estructura física de directorios y módulos en `src/` debe coincidir 1:1 con los paquetes y dependencias declarados en los diagramas de la Vista de Desarrollo. No puede existir un paquete en código sin representación en la Vista de Desarrollo ni viceversa.

### R3. Vista de Procesos — Patrón Entidad-Borde-Control (ECB) Estricto
1. Todo flujo dinámico y diagrama de interacción (secuencia/comunicación en la Vista de Procesos) y su traducción al comportamiento en tiempo de ejecución debe aplicar explícitamente el patrón **Entidad-Borde-Control (ECB)** con estereotipos UML `«boundary»`, `«control»` y `«entity»`.
2. **Regla de aislamiento Borde–Entidad:**
   - **Queda prohibida la comunicación directa entre `«boundary»` y `«entity»`** en ambas direcciones.
   - Todo mensaje entre un elemento de Borde (UI, controlador HTTP/CLI, adaptador de entrada/salida) y una Entidad debe atravesar obligatoriamente un componente `«control»`.
3. Responsabilidades permitidas:
   - `«boundary»`: capturar entrada externa, validar formato sintáctico de transporte y presentar respuestas. Sin reglas de negocio ni acceso directo a entidades.
   - `«control»`: orquestar el caso de uso, coordinar validaciones de negocio, invocar entidades y delegar puertos.
   - `«entity»`: encapsular estado e invariantes de negocio puros. Nunca invoca bordes ni infraestructura.

### R4. Vista Lógica — Pureza Conceptual y Libre de Infraestructura
1. Toda la **Vista Lógica** (diagramas de clases conceptuales/dominio, máquinas de estado de dominio y contratos lógicos) debe permanecer **100 % libre de detalles de almacenamiento o interfaz**.
2. **Queda estrictamente prohibido incluir en la Vista Lógica:**
   - Tablas SQL, esquemas relacionales, vistas SQL o índices.
   - Claves primarias/foráneas (`PK`, `FK`), columnas físicas o tipos dependientes de motor (`VARCHAR`, `SERIAL`, `UUID` de BD, `JSONB`, etc.).
   - Detalles de interfaz gráfica (pantallas, botones, rutas web, formularios, DTOs HTTP/JSON, componentes UI) o anotaciones de ORM/frameworks.
3. El modelo físico de persistencia (MER/tablas/migraciones) y los contratos de interfaz pertenecen exclusivamente a artefactos técnicos separados, trazados mediante correspondencia hacia la Vista Lógica, nunca mezclados dentro de ella.

### R5. Vista de Despliegue — Rigor UML 2.5.1
1. Todo diagrama y descripción de la **Vista de Despliegue** debe respetar la semántica oficial de **UML 2.5.1**, diferenciando de forma explícita e inequívoca los cinco elementos siguientes:
   - **Dispositivos (`«device»`):** nodos físicos o infraestructura computacional virtualizada que aloja ejecución.
   - **Entornos de Ejecución (`«executionEnvironment»`):** contenedores lógicos/runtimes anidados en dispositivos (p. ej. sistema operativo, runtime de aplicación, servidor web, motor de base de datos, contenedor).
   - **Artefactos (`«artifact»`):** unidades físicas desplegables producto de empaquetado/compilación (binarios, bundles, imágenes, scripts ejecutables).
   - **Especificaciones de Despliegue (`«deploymentSpec»`):** descriptores parametrizados que configuran el despliegue de un artefacto sobre un entorno (variables de entorno, puertos, perfiles, políticas).
   - **Relación `«manifest»`:** relación explícita desde cada `«artifact»` hacia los componentes/paquetes de la Vista de Desarrollo que materializa, además de la relación `«deploy»` hacia su nodo o entorno destino.
2. Queda prohibido representar el despliegue mediante cajas genéricas sin estereotipos UML 2.5.1 ni omitir qué elementos lógicos/de desarrollo manifiesta cada artefacto.

### R6. Diagramas del Modelo 4+1 Versionados en Mermaid Compatible con DeepWiki
1. **Fuente única de verdad en el repositorio:** todos los diagramas del Modelo 4+1 (`+1 Escenarios/Casos de Uso`, `Lógica`, `Procesos`, `Desarrollo` y `Despliegue`) deben vivir como **código Mermaid versionado en Markdown** dentro del repositorio (`docs/architecture/`).
2. No se aceptan diagramas arquitectónicos mantenidos únicamente como imágenes binarias (`.png`, `.jpg`, `.svg` exportados manualmente) ni en herramientas externas no versionadas como fuente primaria.
3. **Compatibilidad estricta con DeepWiki y Mermaid estándar:**
   - Utilizar sintaxis Mermaid estándar soportada de forma estable (`flowchart`, `sequenceDiagram`, `classDiagram`, `stateDiagram-v2`).
   - Encerrar entre comillas dobles toda etiqueta que contenga espacios, paréntesis, corchetes, dos puntos o estereotipos (`«boundary»`, `«control»`, `«entity»`, `«device»`, `«executionEnvironment»`, `«artifact»`, `«deploymentSpec»` en `flowchart`/`sequenceDiagram`, reservando `<<...>>` solo para el bloque nativo de estereotipos en `classDiagram`), evitando HTML crudo o etiquetas multilínea problemáticas en aristas.
   - Evitar acentos o caracteres especiales en identificadores de nodos (`id`) aunque el texto visible entre comillas sí esté en español claro.
   - Nota de notación UML en Mermaid: cuando un diagrama de despliegue o casos de uso requiera nodos anidados o estereotipos que `classDiagram` no soporte directamente, usar `flowchart` con `subgraph` semánticos y estereotipos explícitos en las etiquetas de nodo y aristas (`-->|"«manifest»"|`, `-->|"«deploy»"|`), documentando en el texto la convención UML 2.5.1 equivalente.
4. Todo cambio en requisitos o diseño que altere la arquitectura exige actualizar los diagramas Mermaid y la documentación ISO/IEC/IEEE 42010 **en el mismo Pull Request**.

---

## 3. Estándar de Documentación Arquitectónica (ISO/IEC/IEEE 42010) y Calidad (ISO/IEC 25010:2023)

1. La documentación en `docs/architecture/` debe explicitar:
   - **Stakeholders** del FIAVL y sus **concerns** arquitectónicos.
   - **Viewpoints** alineados al Modelo 4+1 (definiendo notación UML 2.5.1 / Mermaid y reglas de consistencia).
   - **Views** concretas (cada vista versionada con sus diagramas Mermaid y explicación textual).
   - **Architecture Decisions (ADR)** numeradas con contexto, opciones descartadas, decisión y consecuencias.
   - **Reglas de Correspondencia** entre vistas (trazabilidad Caso de Uso `+1` ↔ Secuencia ECB ↔ Clases Lógicas ↔ Paquetes Desarrollo ↔ Artefactos/Despliegue).
2. Cada requisito no funcional y criterio de aceptación debe clasificarse bajo una de las 9 características de **ISO/IEC 25010:2023**:
   `Functional Suitability` | `Performance Efficiency` | `Compatibility` | `Interaction Capability` | `Reliability` | `Security` | `Maintainability` | `Flexibility` | `Safety`.

---

## 4. Flujo Colaborativo en GitHub (Equipo de 3 Integrantes)

1. **Rama principal protegida (`main`):** prohibido hacer commits directos o `git push` directo a `main` una vez establecido el commit base del proyecto. Toda integración ocurre mediante **Pull Request (PR)**.
2. **Revisión obligatoria entre pares:** cada PR debe ser aprobado por al menos **1 integrante distinto al autor**, verificando trazabilidad SDD (`specs/`), cumplimiento de las reglas R1–R6 y renderizado limpio de Mermaid.
3. **Integración continua de trabajo:** ramas de corta duración enfocadas en una sola especificación, vista arquitectónica o tarea delimitada. Prohibido mezclar múltiples features sin relación en un mismo PR.
4. **Higiene del repositorio:** nunca versionar secretos (`.env`, tokens, llaves), artefactos locales de IDE propios ni carpetas de dependencias generadas.

---

## 5. Lista de Verificación Obligatoria para Sesiones de Agentes IA

Antes de responder con propuestas de diseño o modificar cualquier archivo, todo agente debe comprobar:
1. **Gate SDD (R1):** Si la tarea implica código en `src/`, ¿existe ya una especificación aprobada en `specs/` que la autorice? Si no existe, **detenerse y exigir la especificación primero**.
2. **Capas 1:1 (R2):** ¿Respeta `Domain -> Utility -> Platform` sin ciclos, sin carpetas genéricas (`Common`/`General`/`Shared`/`Utils`) y sin que `Domain` importe `Platform`?
3. **ECB (R3):** En la Vista de Procesos y el flujo de ejecución, ¿está completamente bloqueado el contacto directo `«boundary» <-> «entity»`?
4. **Pureza Lógica (R4):** ¿Está la Vista Lógica libre de tablas SQL, PK/FK, DTOs de red o componentes de interfaz?
5. **UML 2.5.1 Despliegue (R5):** ¿Distingue el despliegue `«device»`, `«executionEnvironment»`, `«artifact»`, `«deploymentSpec»` y relaciones `«manifest»` / `«deploy»`?
6. **Mermaid + DeepWiki (R6):** ¿Todos los diagramas modificados están en código Mermaid válido dentro de `docs/architecture/`?
