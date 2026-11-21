# Guía Metodológica Operativa del DAS (Referencia para FIAVL)

> Documento de referencia metodológica consolidado a partir de las dos guías oficiales de la asignatura **Arquitectura de Software (DSOF_4069)** (Semanas 1–6 y Semana 7). Establece las plantillas, filtros, reglas de trazabilidad y criterios de auditoría obligatorios para elaborar el Documento de Arquitectura de Software (DAS) y los artefactos del proyecto **FIAVL**, sin incluir ejemplos externos ajenos al proyecto.

---

## 1. Plantilla Consolidada DAS v3.0 (Apéndice A + Cierre Semana 7)

Cada sección del DAS responde a un estándar verificable y alimenta directamente a la siguiente:

| Sección del DAS | Origen | Artefactos Obligatorios y Criterio de Auditoría |
| :--- | :--- | :--- |
| **1. Introducción, TDR y Alcance** | Semanas 1–2 | Resumen del problema, objetivo, límites explícitos (qué entra y qué queda fuera de alcance/plazo), matriz de clasificación del TDR, tabla de vacíos/ambigüedades aclaradas, pliego de preguntas estratégicas al cliente, supuestos documentados (`Assumptions`) y restricciones duras (legales, presupuestarias, de entorno, p. ej. LOPDP 2021). |
| **2. Interesados y Calidad** | Semana 2 | Tabla de **Interesados y Preocupaciones** (ISO/IEC/IEEE 42010) + **3 a 5 Atributos de Calidad priorizados** (ISO/IEC 25010:2023) redactados como escenarios verificables **Condición + Medida** + Matriz de Trazabilidad Holística + Registro de Riesgos Técnicos. |
| **3. Vista de Escenarios (+1)** | Semana 3 | Diagrama de Casos de Uso **UML 2.5.1** (actores primarios a la izquierda incl. `Tiempo`, secundarios a la derecha; `«include»` y `«extend»` justificados), catálogo general filtrado y **≥ 3 especificaciones Cockburn completas** de Casos Arquitectónicamente Significativos. |
| **4. Vista Lógica** | Semana 4 | Extracción gramatical y depuración (4 filtros), **Diagrama de Clases UML** completo (100 % libre de tablas SQL, PK/FK, UI o hilos; con visibilidad y multiplicidades justificadas), **Tarjetas CRC**, **Diccionario de Clases** con invariantes (`RN-xx`), Diagramas de Estado (si aplica) y Matriz de Trazabilidad Escenarios vs. Clases. |
| **5. Vista de Procesos** | Semana 5 | Tablas de identificación **ECB** auditadas contra reglas de robustez, **Diagramas de Secuencia UML 2.5.1** de los casos significativos con fragmentos combinados (`alt`, `opt`, `loop`, `break`, `par`, `critical`) y comunicación síncrona vs. asíncrona, análisis de concurrencia/fallos y **verificación 1:1** contra operaciones del Diagrama de Clases. |
| **6. Vista de Desarrollo** | Semana 6 | Tablas de **Dominios**, **Utilitarios** (auditados con Prueba de las 3 Preguntas) y **Plataformas** (6 niveles, versión, licencia, justificación), Diagrama de Paquetes UML **100 % acíclico y descendente** (`«Domain» -> «Utility» -> «Platform»`), correspondencia **1:1 con carpetas Git**, asignación del equipo y **ADRs** (Contexto, Alternativas, Decisión, Consecuencias). |
| **7. Vista de Despliegue** | Semana 7 | Transformación de paquetes a **Componentes** (interfaces proporcionadas/requeridas) y **Artefactos** (`«manifest»`), Diagramas de Despliegue **UML 2.5.1** por entorno (Dev y Prod; opcional QA/Staging) aplicando el **Algoritmo de 9 Pasos**, Tabla de Nodos / Hardware Base, resultado de las **4 Pruebas de Estrés**, diagnóstico de **SPOF** y Plan de Contingencia obligatorio. |

### Distinción de Niveles de Decisión (Grady Booch) y Normativa Base
- **Arquitectura (estratégico, costo de cambio muy alto):** estilos globales, descomposición en módulos/capas, comunicación síncrona/asíncrona, atributos de calidad, integración entre sistemas. Fórmula de Perry & Wolf: `Arquitectura = {Elementos (procesamiento, datos, conexión), Forma, Justificación (Rationale)}`.
- **Diseño Detallado (táctico, costo medio):** clases internas de un componente, firmas, patrones GoF (*Strategy*, *Observer*, *Factory*), esquemas físicos fuera de la Vista Lógica.
- **Implementación (operativo, costo bajo):** estructuras de datos en memoria, algoritmos locales, sintaxis.
- **Marcos ISO de referencia:** **ISO/IEC/IEEE 12207** (sitúa la Definición de Arquitectura entre Requisitos y Diseño Detallado) e **ISO/IEC/IEEE 15288** (visión sociotécnica integral: software + hardware + redes + procedimientos + personas + normativa legal).

---

## 2. Análisis de TDR, ISO/IEC/IEEE 42010 e ISO/IEC 25010:2023

### Metodología de 4 Pasos frente al TDR
1. **Extraer certezas:** funcionalidades explícitas, atributos implícitos, restricciones duras (*constraints* con cero grados de libertad), actores e interesados.
2. **Detectar los 4 defectos clásicos ("olores"):**
   - *Ambigüedad:* adjetivos subjetivos sin métrica ("rápido", "seguro", "intuitivo", "siempre disponible").
   - *Incompletitud (vacíos):* ausencia de volumetría, concurrencia pico, crecimiento de datos, contratos de integración o tolerancia a fallos de red.
   - *Mezcla de niveles:* requisitos de negocio mezclados con detalles de UI o implementación.
   - *Contradicciones:* exigencias incompatibles con plazo, presupuesto o hardware.
3. **Formular pliego de preguntas estratégicas al cliente** (volumetría/rendimiento, integración/interoperabilidad, seguridad/cumplimiento legal, operación/reglas de negocio).
4. **Documentar Supuestos (`Assumptions`):** condiciones explícitas bajo las cuales se diseña si algún vacío queda pendiente de respuesta.

### Metamodelo ISO/IEC/IEEE 42010 y Principio de Validez
- Cadena formal: `Sistema -> tiene Arquitectura` (abstracta) ↔ `Descripción de Arquitectura (DAS)` (tangible).
- **Interesado (`Stakeholder`)** tiene **Preocupación (`Concern`)** → atendida por una **Vista (`View`)** construida bajo un **Punto de Vista (`Viewpoint`)** y validada mediante **Correspondencias (`Correspondences`)** entre vistas.
- **Regla de Oro de Validez:** toda vista y todo diagrama en el DAS existen únicamente para responder a preocupaciones concretas de interesados identificados. Cualquier diagrama que no responda a una preocupación real debe eliminarse.
- *Complemento C4:* el Modelo C4 (*Context, Containers, Components, Code* + Dinámico) puede complementar las vistas 4+1 manteniendo la regla de adaptar cada vista a su público.

### Atributos de Calidad Verificables: Fórmula `Condición + Medida` e ISO/IEC 25010:2023
Todo requisito no funcional (RNF) debe redactarse de forma comprobable en laboratorio o producción mediante:
`[Condición Operativa / Estímulo / Carga] + [Métrica Cuantitativa / Umbral Verificable]`

Clasificar las métricas según su momento de evaluación (**Calidad Interna** estática sobre arquitectura/código, **Calidad Externa** dinámica en pruebas/estrés, **Calidad en Uso** por usuarios reales en su flujo operativo) y trazarlas a los **9 pilares vigentes de ISO/IEC 25010:2023**:
1. **Adecuación Funcional (*Functional Suitability*):** completitud, corrección, pertinencia funcional.
2. **Compatibilidad (*Compatibility*):** coexistencia e interoperabilidad con sistemas externos.
3. **Seguridad (*Security*):** confidencialidad, integridad, no repudio, responsabilidad (*accountability*), autenticidad y **Resistencia** (operar funciones críticas bajo ataque).
4. **Eficiencia de Desempeño (*Performance Efficiency*):** comportamiento temporal (latencia/percentiles), utilización de recursos y capacidad máxima.
5. **Capacidad de Interacción (*Interaction Capability*):** inclusividad, accesibilidad, reconocimiento de adecuación, aprendibilidad, operabilidad, protección ante errores y *user engagement*.
6. **Fiabilidad (*Reliability*):** ausencia de fallos, disponibilidad, tolerancia a fallos y recuperabilidad.
7. **Mantenibilidad (*Maintainability*):** modularidad, reusabilidad, analizabilidad, modificabilidad y facilidad de prueba (*testability*).
8. **Flexibilidad (*Flexibility*):** adaptabilidad, **escalabilidad** (nube elástica), instalabilidad y reemplazabilidad.
9. **Seguridad Física (*Safety*):** identificación de riesgos operativos, comportamiento a prueba de fallos (*fail-safe*), alerta de peligros e integración segura.

---

## 3. Vista de Escenarios (+1): UML 2.5.1, Plantilla Cockburn y Filtro Significativo

### Reglas UML 2.5.1 del Diagrama de Casos de Uso
- **Caja negra ("nivel del mar"):** describe **QUÉ** valor completo observable obtiene el actor; **prohibido** describir detalles internos, SQL, algoritmos, pantallas, botones o micro-tareas sin valor aislado.
- **Actores:** roles externos (persona, organización, sistema externo o **`Tiempo`** como disparador cron/automático).
  - *Actores Primarios* a la izquierda (inician el caso).
  - *Actores Secundarios* a la derecha (responden a peticiones del sistema).
  - **Prohibido** conectar dos actores entre sí con líneas de asociación directa (salvo generalización explícita de roles).
- **Caso de Uso:** nombrado obligatoriamente con **`Verbo en infinitivo + Objeto directo`**, encerrado dentro del **Límite del Sistema**.
- **Relaciones:**
  - `«include»`: comportamiento **obligatorio y siempre ejecutado**; sin él, el caso base queda incompleto. Flecha discontinua: `Caso Base -.«include».-> Caso Incluido`.
  - `«extend»`: comportamiento **condicional u opcional**; el caso base funciona completo de inicio a fin sin la extensión. Flecha discontinua en sentido inverso: `Caso Extensor -.«extend».-> Caso Base` (anotar condición/punto de extensión).
  - Usar ambas con moderación para evitar descomposición funcional excesiva.

### Plantilla Completa de Alistair Cockburn (8 Secciones)
1. **Identificador, Nombre y Descripción:** código único (`CU-XX`), verbo infinitivo + objeto y resumen de negocio.
2. **Actores:** primario(s) y secundario(s) (revelan fronteras de integración).
3. **Disparador (*Trigger*):** evento exacto que inicia el caso.
4. **Precondiciones:** estado verdadero previo garantizado antes de iniciar (no se revalidan como pasos del flujo básico).
5. **Postcondiciones Verificables:** estado persistente garantizado al finalizar con éxito.
6. **Flujo Básico ("Camino Feliz"):** diálogo limpio "ping-pong" actor–sistema con **3 a 9 pasos cortos**, numerados, en voz activa, con sujeto claro e intención pura (**cero detalles de GUI**).
7. **Flujos Alternos vs. Flujos de Excepción ("Los Arquitectos en la Sombra"):** referenciados al número de paso donde nacen (ej. `4a`, `6a`).
   - *Flujo Alterno:* desvío que resuelve una variación válida y **cumple la postcondición** (se reincorpora al flujo principal o termina con éxito).
   - *Flujo de Excepción:* fallo o rechazo que **impide alcanzar el objetivo**, termina en fracaso (`X`) **sin cumplir la postcondición** y revierte efectos parciales.
   - *Lectura como insights arquitectónicos:* los flujos alternos/excepción de concurrencia (colisión simultánea en un paso crítico) y fallos de terceros externos dictan directamente bloqueos transaccionales/regiones críticas y colas asíncronas en las vistas posteriores.
8. **Reglas de Negocio y Requisitos Especiales:** reglas lógicas y RNF asociados (latencia, concurrencia, seguridad).

### Filtro de Casos Arquitectónicamente Significativos (Mínimo 3)
Documentar con plantilla Cockburn completa únicamente los casos que superen el filtro de relevancia arquitectónica (cumpliendo los 5 criterios):
1. **Núcleo del negocio:** razón de ser central del sistema.
2. **Cobertura extremo a extremo:** atraviesan múltiples capas y componentes.
3. **Prueba de atributos de calidad:** estresan concurrencia, seguridad, rendimiento o disponibilidad.
4. **Fronteras externas:** integran servicios/sistemas externos fuera de control propio.
5. **Alto riesgo técnico:** concentran incertidumbre o severidad ante fallos.

---

## 4. Vista Lógica: Análisis Gramatical, 4 Filtros, Relaciones y Diccionario

### Frontera de Pureza Conceptual
- **SÍ incluye:** clases y enumeraciones del dominio (Nivel Conceptual y Nivel de Especificación con visibilidad, tipos de datos abstractos y operaciones con firma), relaciones estructurales, multiplicidades, paquetes lógicos y diagramas de estado para ciclos de vida complejos.
- **EXCLUYE estrictamente:** tablas SQL, columnas físicas, `PK`/`FK`, tipos de motor BD, pantallas/formularios/botones/DTOs HTTP, hilos concurrentes, carpetas de código o servidores.

### Extracción Gramatical y Árbol de 4 Filtros
1. **Decodificación gramatical sobre los casos significativos:**
   - Sustantivos y frases nominales → candidatos a Clases o Atributos.
   - Verbos → candidatos a Operaciones o nombres de Relaciones.
   - Adjetivos y valores de estado → candidatos a Atributos, `«enumeration»` o Estados.
   - Expresiones de cantidad → Multiplicidades.
2. **Descartes inmediatos:** eliminar sinónimos, actores externos cuya historia no se almacena, elementos de UI/reportes y operaciones disfrazadas de sustantivo (ej. "búsqueda").
3. **Los 4 Filtros Secuenciales por candidato:**
   - *Filtro 1:* ¿El sistema necesita recordar su información a largo plazo? (No → parámetro temporal, descartar).
   - *Filtro 2:* ¿Posee más de un atributo relevante? (No → atributo simple dentro de otra clase).
   - *Filtro 3:* ¿Tiene identidad propia e instancia distinguible con ciclo de vida independiente? (Roles/Personas, Eventos, Tangibles, Catálogos).
   - *Filtro 4:* ¿Participa en al menos un escenario de negocio priorizado? (No → ruido, descartar).
4. **Regla de atributos:** evitar derivables salvo que se marquen con `/` (`/edad`), y jamás simular asociaciones mediante claves foráneas (`idXxxFK`).

### Cohesión, Relaciones UML 2.5.1, Roles y Multiplicidades
- **Tarjetas CRC (Clase – Responsabilidad – Colaboración):** auditan cohesión y acoplamiento antes del UML definitivo; demasiadas responsabilidades delatan *God Class*; demasiados colaboradores delatan alto acoplamiento.
- **Matriz de Trazabilidad Escenarios vs. Clases:** cruza casos significativos contra clases; columnas sin marcas revelan clases sobrantes.
- **Relaciones formales UML:**
  - *Asociación (`—`):* vínculo estructural nombrado con verbo.
  - *Agregación (`<>—`):* Todo–Parte débil (la parte sobrevive si muere el Todo).
  - *Composición (`<*>—`):* Todo–Parte fuerte (dependencia total de ciclo de vida; si se destruye el Todo, mueren sus partes).
  - *Generalización (`—|>`):* herencia taxonómica permanente ("es un tipo de"). **Evitarla cuando un mismo sujeto pueda cambiar o acumular roles dinámicamente**; en su lugar aplicar el **Patrón de Roles por Asociación** (entidad base asociada con multiplicidad `0..1` a cada rol).
  - *Dependencia (`-->`):* uso paramétrico o temporal (ej. uso de `«enumeration»`).
  - *Realización (`--|>`):* implementación de interfaz/contrato.
- **Multiplicidades restrictivas como escudo arquitectónico:** leer y validar obligatoriamente en ambos sentidos (`1`, `0..1`, `0..*` / `*`, `1..*`, `n..m`). Fijar límites exactos (como `0..1`) bloquea estructuralmente inconsistencias de negocio; toda relación `*..*` con datos o eventos propios se descompone en una **Entidad Intermedia**.
- **Visibilidad UML:** `+` Público, `-` Privado (por defecto en atributos), `#` Protegido, `~` Paquete.

### Diccionario de Clases (Eliminando Ambigüedades)
Toda clase del diagrama debe documentarse con:
1. **Nombre y Propósito** (vocabulario idéntico a los casos de uso).
2. **Atributos y Restricciones** (tipo abstracto, obligatoriedad y rangos/reglas de formato).
3. **Operaciones (Entrada / Salida)** (firmas exactas).
4. **Invariantes / Reglas de Negocio (`RN-xx`)** (condiciones que nunca pueden violarse).
5. **Casos de Uso Trazados** (`CU-xx`).

---

## 5. Vista de Procesos: Patrón ECB, Secuencia UML 2.5.1 y Trazabilidad

### Unidades de Ejecución y Comunicación
- **Proceso:** unidad táctica independiente con memoria y ciclo de vida propios, replicable entre nodos. **Tarea:** hilo de control individual dentro de un proceso. *Regla de oro:* las tareas mayores nunca asumen compartir proceso o nodo físico.
- **Síncrona** (flecha continua punta rellena `—▶`): emisor bloqueado esperando respuesta; acoplamiento temporal alto; riesgo de latencia acumulada y fallos en cascada.
- **Asíncrona** (flecha continua punta abierta `—>`): emisor despacha (ej. cola/worker) y continúa sin esperar; acoplamiento temporal bajo; exige gestionar consistencia eventual y reintentos.

### Patrón Entidad–Borde–Control (ECB) y Reglas de Robustez
- **`«boundary»` (Borde):** encapsula interacción con cada actor primario (UI, endpoints API) y cada actor secundario saliente (pasarelas/clientes hacia externos).
- **`«control»` (Control):** orquesta secuencialmente el caso de uso y las transacciones. **Diferencia con MVC:** el controlador ECB coordina activamente el flujo completo del caso de uso, no solo enruta peticiones; **prohibido** convertirlo en *Clase Dios* absorbiendo reglas intrínsecas de los datos, las cuales pertenecen a las `«entity»`.
- **`«entity»` (Entidad):** encapsula estado persistente e invariantes propios del dominio (corresponde a las clases de la Vista Lógica).

**Matriz estricta de comunicación ECB:**
- **PERMITIDO:** `Actor ↔ «boundary»`, `«boundary» ↔ «control»`, `«control» ↔ «control»`, `«control» → «entity»`, `«entity» ↔ «entity»`.
- **PROHIBIDO:** `Actor → «control» / «entity»`, **`«boundary» ↔ «entity»`** (en ambos sentidos), `«entity» → «control» / «boundary»`.

**Procedimiento de 6 pasos:** (1) tomar caso significativo → (2) crear un `«boundary»` por actor primario y secundario → (3) definir el `«control»` coordinador → (4) mapear `«entity»` exactas de Vista Lógica → (5) auditar cero violaciones de robustez → (6) asignar métodos y verificar contra el Diagrama de Clases.

### Fragmentos Combinados de Secuencia UML 2.5.1 y Trazabilidad 1:1
- **`[alt]`:** bifurcación condicional excluyente (`if / else`) según guarda booleana.
- **`[opt]`:** ejecución opcional condicional simple (`if` sin `else`).
- **`[loop]`:** iteración mientras se cumpla guarda o sobre colección.
- **`[break]`:** interrupción crítica que abandona inmediatamente el fragmento contenedor (ej. timeout/error irrecuperable).
- **`[par]`** *(puente a procesos)*: ejecución concurrente en múltiples hilos/procesos sin orden estricto.
- **`[critical]`** *(puente a procesos)*: sección atómica de exclusión mutua para impedir condiciones de carrera.
- **Regla inexcusable de trazabilidad 1:1 (Vista Lógica ↔ Vista de Procesos):** todo mensaje recibido por un participante `«entity»` o `«control»` en el Diagrama de Secuencia **debe existir obligatoriamente** como operación declarada con la misma semántica en su clase del Diagrama de Clases y Diccionario de Clases.

---

## 6. Vista de Desarrollo: 3 Capas, Prueba de Utilitarios, Plataformas y 1:1

### Tres Reglas de Kruchten y Principio Gravitacional (DAG)
1. **Partición** (un paquete cohesionado por área/dominio), **Agrupamiento** (cambia junto → vive junto), **Visibilidad** (dependencias estrictamente descendentes o laterales acíclicas entre dominios).
2. **Jerarquía de 3 capas (`«Domain» -> «Utility» -> «Platform»`):**
   - **`«Domain»` (arriba):** lógica pura de negocio, entidades, casos de uso y controles ECB. Puede depender de otros dominios (sin ciclos) y de `«Utility»`. **Prohibido importar `«Platform»`.**
   - **`«Utility»` (medio):** servicios transversales genéricos. Puede depender de `«Platform»`. **Prohibido conocer o depender de `«Domain»`.**
   - **`«Platform»` (abajo):** tecnología adoptada de terceros. **No conoce ni a `«Domain»` ni a `«Utility»`.**
3. **Derivación de Dominios en 4 pasos:** agrupar entidades fuertemente asociadas → sumar sus casos de uso → ubicar cada `«control»` en el dominio que orquesta → validar nombres con el lenguaje del negocio.
4. **Alertas rojas y ruptura de ciclos:**
   - **Prohibidos nombres cajón de sastre:** `General`, `Común`, `Shared`, `Misceláneo`, `Varios`, `Utils`, `Helpers`.
   - **Ruptura de ciclos (`A <-> B`):** (a) Inversión de Dependencias con interfaz, (b) Extracción de lo compartido a un tercer paquete inferior o eventos asíncronos, o (c) Fusión si las fronteras estaban mal trazadas.

### Prueba del Ácido de las 3 Preguntas para Utilitarios (`«Utility»`)
Todo candidato a Utilitario debe responder **SÍ rotundo a las 3 preguntas** (un solo NO lo devuelve a un Dominio):
1. ¿Lo necesitan **dos o más dominios** diferentes?
2. ¿Podría **copiarse íntegro a otro negocio totalmente distinto** (otra industria) sin modificar su código?
3. ¿**Carece por completo de reglas de negocio** específicas del problema?
*(Utilitarios universales típicos: Seguridad/Auth/Cifrado, Auditoría inmutable, Notificaciones técnicas, Persistencia/Conexiones/Tx, Validaciones sintácticas genéricas, Reportes/Renderizado técnico).*

### 6 Niveles Tecnológicos de Plataforma y Principio de Aislamiento
Toda tecnología adoptada (documentada con **versión exacta, licencia y justificación**) se clasifica en uno de los 6 niveles bajo `«Utility»`:
1. Servicios Externos (pasarelas, proveedores SMTP/SMS, IdPs externos).
2. Software de Infraestructura (RDBMS, servidores web/proxy, brokers).
3. Marcos de Trabajo y Bibliotecas (*Frameworks*, ORMs, UI libs).
4. Lenguaje y Entorno de Ejecución (*Runtime*: JVM, Python, Node.js, CLR).
5. Sistema Operativo.
6. Hardware / Nube IaaS.
- **Principio de Aislamiento:** ningún Dominio importa directamente librerías, drivers SQL o SDKs de Plataforma; toda interacción con Plataforma ocurre a través de un Utilitario o puerto aislado (protege Mantenibilidad y Flexibilidad).
- **Correspondencia 1:1 y ADRs:** los paquetes del Diagrama de Paquetes coinciden 1:1 con las carpetas en `src/` (`domain/`, `utility/`, `platform/`), pueden validarse automáticamente en tests de arquitectura, y las decisiones clave se registran en **ADR de 4 partes** (*Contexto*, *Alternativas evaluadas*, *Decisión tomada*, *Consecuencias*).

---

## 7. Vista de Despliegue UML 2.5.1: Algoritmo de 9 Pasos y Pruebas SPOF

### Triángulo Conceptual y Cambio de Paradigma UML 2.x
- **Paquete:** agrupación estática en tiempo de desarrollo/compilación (no existe en ejecución).
- **Componente (`«component»`):** unidad modular reemplazable en ejecución conectada exclusivamente mediante **Interfaz Proporcionada** (*Lollipop* `○—`) e **Interfaz Requerida** (*Socket* `)—`) bajo la **Ley de Ensamblaje** (*ball-and-socket*).
- **Artefacto (`«artifact»`):** archivo físico concreto instalable (ejecutable, bundle web, imagen OCI, scripts DDL).
- **Cambio de paradigma UML 1.x → UML 2.x (2.5.1):** prohibido dibujar componentes directamente dentro de nodos. En UML 2.5.1:
  1. Los **Nodos/Entornos** alojan **Artefactos** mediante `«deploy»` (o anidamiento visual).
  2. Los **Artefactos** materializan **Componentes** lógicos mediante `«manifest»` (`«artifact» -.«manifest».-> «component»`).
- **Cadena completa de 5 eslabones:** `1. Clase (Lógica) -> 2. Paquete (Desarrollo) -> 3. Componente -> 4. Artefacto («manifest») -> 5. Nodo («deploy»)`.

### Elementos Normativos UML 2.5.1 y Regla de Entornos
- **Regla multi-entorno:** elaborar diagramas separados por entorno operativo (**Desarrollo**, **Producción** y, si aplica, **Pruebas/QA/Staging**) demostrando que los mismos artefactos cambian de topología sin tocar código.
- **Elementos obligatorios:**
  - `«device»`: nodo hardware o máquina virtual con cómputo propio (contenedor exterior).
  - `«executionEnvironment»`: software/runtime/motor/contenedor **siempre anidado** dentro de un `«device»` (o de otro entorno).
  - `«artifact»`: entregable físico anidado dentro de su `«executionEnvironment»`.
  - `«deploymentSpec»`: artefacto de configuración parametrizada por entorno (variables, puertos, *pools*, credenciales externas, `ConfigMap`/`Secret`).
  - **Equivalencia moderna:** imagen Docker/OCI = `«artifact»`; contenedor en ejecución o Pod K8s = `«executionEnvironment»`; Worker Node = `«device»`; archivos IaC/manifiestos = `«deploymentSpec»`.
  - **Rutas de comunicación:** línea continua entre nodos comunicantes con **protocolo explícito obligatorio** (jamás líneas mudas).

### Algoritmo Metodológico de 9 Pasos
1. **Listar artefactos y qué componentes manifiestan** (`«artifact»`, `«deploymentSpec»`, matriz `«manifest»` sin componentes huérfanos).
2. **Identificar nodos físicos/virtuales (`«device»`)** según el entorno.
3. **Anidar entornos de ejecución (`«executionEnvironment»`)** dentro de cada `«device»` (derivados de `«Platform»`).
4. **Asignar artefactos y especificaciones a sus entornos (`«deploy»`)**.
5. **Trazar rutas de comunicación con protocolos explícitos** (`HTTPS`, `HTTP`, `TCP/IP`, `SMTP`, etc.).
6. **Indicar multiplicidad de nodos críticos** (réplicas activas para disponibilidad/balanceo).
7. **Delimitar zonas de red (fronteras de confianza):** Internet (Pública), Red Interna (Privada/Aislada), Externo (Terceros).
8. **Validar topología contra RNF (Pruebas de Estrés y SPOF).**
9. **Documentar la Tabla de Nodos y Hardware Base** (nombre, tipo, entorno anidado, artefactos, réplicas, protocolos entrada/salida, CPU/RAM/disco/SO).

### Las 4 Pruebas de Estrés Arquitectónicas y Plan de Contingencia SPOF
Ningún diagrama de despliegue está terminado hasta superar estas 4 auditorías:
1. **Caída de una instancia del servidor de aplicaciones:** verificar conmutación del balanceador y diseño **sin estado local (*stateless*)**.
2. **Caída o latencia extrema de un servicio externo:** verificar **desacoplamiento asíncrono**, colas de reintento, *timeouts*, *Circuit Breaker* o degradación elegante para no bloquear operaciones críticas.
3. **Escáner de red malicioso desde Internet:** verificar que solo la frontera perimetral (proxy inverso/balanceador por HTTPS) sea visible desde Internet y que nodos de aplicación y datos queden aislados en Red Interna.
4. **Detección de Puntos Únicos de Falla (SPOF) visibles u ocultos:** auditar instancia única de base de datos, nodo de entrada/balanceador/enlace único y dependencias externas síncronas únicas. Todo SPOF detectado exige documentar un **Plan de Contingencia obligatorio** (réplica *Primary–Standby* con *failover*, respaldos automatizados con PITR, redundancia en borde y patrones de resiliencia en integraciones).

---

## 8. Convención Mermaid Compatible con DeepWiki (Norma del Repositorio)

Para cumplir la regla **R6** de `AGENTS.md`:
- Usar únicamente diagramas estándar: `flowchart` (para casos de uso con límite de sistema, desarrollo en capas y despliegue con `subgraph` anidados por zona/nodo/entorno), `classDiagram` (Vista Lógica), `stateDiagram-v2` (ciclos de vida) y `sequenceDiagram` (Vista de Procesos ECB con bloques `alt`, `opt`, `loop`, `break`, `par`, `critical`).
- Encerrar entre comillas dobles toda etiqueta con espacios, paréntesis, dos puntos o estereotipos (`"«device» Servidor Web"`, `"«control» GestorX"`), usando IDs de nodo simples en ASCII sin tildes ni espacios.
- En `flowchart`, etiquetar explícitamente `-->|"«manifest»"|`, `-->|"«deploy»"|` y los protocolos de red (`-->|"HTTPS (443)"|`).
