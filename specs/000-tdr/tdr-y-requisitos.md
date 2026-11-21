# Especificación 000-TDR: Análisis Crítico de TDR, Alcance y Requisitos de Usuario — FIAVL

- **ID de Especificación:** `000-tdr`
- **Sistema:** Gestor de Programación e Itinerarios Urbanos del Festival Internacional de Artes Vivas de Loja (FIAVL)
- **Rama de trabajo:** `spec/carlos/000-tdr`
- **Estado:** `Draft` *(pendiente de revisión entre pares y aprobación en Pull Request conforme a `AGENTS.md` R1)*
- **Fecha:** `2026-11-21`
- **Marco normativo:** `AGENTS.md` (R1–R6), `docs/architecture/GUIA_METODOLOGICA_DAS.md`, ISO/IEC/IEEE 12207, ISO/IEC/IEEE 15288, ISO/IEC/IEEE 42010, ISO/IEC 25010:2023 y LOPDP Ecuador (2021).

---

## 1. Contexto de Negocio y Enunciado Formal del Problema

### 1.1 Contexto Operativo del Festival
El **Festival Internacional de Artes Vivas de Loja (FIAVL)** concentra durante **10 días continuos** una cartelera que supera las **400 actividades** artísticas (teatro, danza, música, ópera, circo e intervenciones urbanas), distribuidas simultáneamente en múltiples escenarios de la ciudad:
1. **Espacios públicos de acceso libre y gratuito** (plazas, calles y parques).
2. **Teatros cerrados con funciones seleccionadas**, sujetos a **aforo limitado** y **boleto con tarifa referencial de 5 USD**.

La solución solicitada es un sistema web (**responsive / PWA**) orientado tanto al **uso en calle** por parte de los asistentes desde dispositivos móviles como a la **gestión operativa** por parte de los organizadores del festival.

### 1.2 Enunciado Formal del Problema
Durante los 10 días del FIAVL, la convivencia de más de 400 actividades en escenarios dispersos genera cuatro problemáticas centrales:
1. **Desorientación y fricción en la exploración de cartelera:** los asistentes carecen de un medio unificado y ágil para filtrar la programación por disciplina artística, sede, fecha/horario y modalidad de acceso (gratuito o pagado).
2. **Planificación deficiente de recorridos urbanos:** sin un itinerario personal estructurado y disponible en movilidad, el público incurre en traslapes horarios o desplazamientos ineficientes entre espacios públicos y teatros.
3. **Incertidumbre y riesgo de sobreocupación en funciones con aforo limitado:** la ausencia de gestión controlada de cupos en funciones pagadas de teatro expone a los recintos a sobreventa/aglomeración o a desplazamientos fallidos del público cuando el aforo ya está cubierto.
4. **Desinformación ante cambios operativos de último momento:** cuando la organización modifica una sala, reprograma un horario o alcanza aforo completo, los asistentes afectados no reciben avisos oportunos, provocando confusión en las sedes y sobrecarga en la logística presencial.

*(Bajo ISO/IEC/IEEE 15288, el sistema se concibe como un componente sociotécnico integrado con la operación presencial de sedes, personal de sala/organización y asistentes en vía pública; el software falla operativamente si obliga al personal o al usuario en calle a flujos lentos o burocráticos).*

---

## 2. Objetivos del Sistema

### 2.1 Objetivo General
Proveer una aplicación web *responsive*/PWA para la gestión de la programación oficial e itinerarios urbanos del FIAVL que permita al público explorar y filtrar las más de 400 actividades, construir su itinerario personal, reservar o gestionar cupos en funciones seleccionadas con aforo limitado y recibir avisos oportunos, habilitando a los organizadores mantener la cartelera actualizada y emitir alertas operativas con trazabilidad y cumplimiento de protección de datos.

### 2.2 Objetivos Específicos
1. **OE-01 (Acceso unificado a cartelera):** Centralizar la oferta artística de los 10 días (>400 actividades) con consulta y filtrado por disciplina, sede, fecha/horario y modalidad de acceso (gratuito vs. pagado de 5 USD).
2. **OE-02 (Autonomía en itinerario urbano):** Permitir a cada asistente estructurar y consultar en movilidad su itinerario personal por la ciudad, detectando conflictos horarios entre actividades seleccionadas.
3. **OE-03 (Integridad de aforos):** Garantizar la gestión exacta de cupos disponibles en funciones seleccionadas de teatro cerrado, impidiendo sobreocupación o dobles reservas simultáneas.
4. **OE-04 (Comunicación operativa oportuna):** Permitir a los organizadores actualizar la programación y publicar alertas ante cambios de sala, horario o aforo lleno, entregando avisos oportunos sin bloquear la operación principal.
5. **OE-05 (Cumplimiento normativo y calidad arquitectónica):** Incorporar privacidad desde el diseño (*LOPDP Ecuador 2021*) y habilitar el trabajo paralelo de un equipo de 3 integrantes bajo SDD y el Modelo 4+1.

---

## 3. Límites Explícitos de Alcance (Equipo Académico de 3 Integrantes)

La definición estricta de fronteras evita sobredimensionamiento en un proyecto académico de 3 desarrolladores y previene diagramas o módulos innecesarios (Principio de Validez de ISO/IEC/IEEE 42010):

| Dimensión | Dentro del Alcance (`In-Scope`) | Fuera del Alcance (`Out-of-Scope`) |
| :--- | :--- | :--- |
| **Plataforma de entrega** | Aplicación web *responsive* / PWA accesible desde navegador móvil (uso en calle) y escritorio (gestión), con disponibilidad de lectura en caché local para el itinerario guardado del asistente. | Aplicaciones móviles nativas independientes para iOS/Android o publicación en tiendas App Store / Google Play. |
| **Cartelera e itinerario** | Catálogo completo de actividades (10 días, >400 actividades), filtros combinados, ficha informativa (disciplina, sede, horario, modalidad gratuita o tarifa referencial 5 USD, disponibilidad) e itinerario urbano personal con detección de traslapes horarios. | Mapas con navegación GPS giro a giro en tiempo real, cálculo de tráfico vehicular o integración con apps externas de transporte. |
| **Reservas y boleto de 5 USD** | Reserva y gestión de cupos en funciones pagadas de aforo limitado, generación de código/constancia verificable de reserva, expiración por tiempo límite, cancelación/liberación de cupo y registro de tarifa referencial (5 USD). | **Procesamiento bancario o pasarelas de pago reales**, cobro con tarjetas, facturación electrónica (SRI), conciliación contable o reembolsos financieros reales. |
| **Control en sedes / taquilla** | Consulta de ocupación por función y validación lógica del estado de una reserva o liberación operativa de cupos por el organizador autorizado. | **Hardware físico de taquillas**, impresoras térmicas de boletos, torniquetes, pistolas láser o lectores RFID/NFC dedicados. |
| **Alertas y notificaciones** | Emisión de alertas operativas por cambios de sala, cambio de horario, cancelación o aforo lleno, y recordatorios automáticos previos al inicio de actividades del itinerario mediante canales digitales estándar (notificación en plataforma/Web Push o correo electrónico institucional/usuario). | Centrales telefónicas, campañas masivas de WhatsApp Business/SMS de pago sin presupuesto asignado o mensajería omnicanal con múltiples operadoras locales. |
| **Gestión y operación FIAVL** | Administración de programación, sedes, aforos y alertas por organizadores autenticados, con auditoría de cambios críticos y consentimiento LOPDP. | Gestión contractual de elencos, logística hotelera/viáticos de artistas, venta de *merchandising*, transmisión de video en vivo (*streaming*) o integración con ERPs municipales no especificados. |

---

## 4. Análisis Crítico del TDR en 4 Pasos (Metodología DAS v3.0)

### 4.1 Paso 1 — Matriz de Clasificación del TDR

| Categoría | Elementos Extraídos del Contexto del FIAVL |
| :--- | :--- |
| **Funcionalidades Explícitas** *(Qué debe hacer)* | • Explorar y filtrar cartelera (>400 actividades en 10 días) por disciplina (teatro, danza, música, ópera, circo, intervenciones urbanas), sede, fecha/horario y modalidad de acceso (gratuito o pagado 5 USD).<br>• Armar y gestionar un itinerario urbano personal por la ciudad.<br>• Reservar, consultar y gestionar (cancelar/liberar) cupos en funciones seleccionadas en teatros cerrados con boleto de 5 USD y aforo limitado.<br>• Recibir avisos oportunos ante cambios de sala, horario o aforo lleno.<br>• Mantener actualizada la programación del festival y publicar alertas operativas (Organizadores). |
| **Atributos de Calidad Implícitos** *(Sin métrica inicial)* | • **Uso en calle (*responsive*/PWA):** operabilidad ágil en pantallas móviles y resiliencia ante señal intermitente en plazas/calles.<br>• **Rapidez en consulta (>400 actividades):** filtrado fluido sin demoras perceptibles.<br>• **Integridad y disponibilidad en aforos limitados:** ausencia de dobles reservas bajo demanda concurrente.<br>• **Oportunidad y tolerancia a fallos en avisos:** entrega rápida y desacoplada de alertas operativas.<br>• **Seguridad y privacidad:** control de acceso de organizadores y resguardo de datos personales de asistentes.<br>• **Mantenibilidad:** división limpia para desarrollo paralelo en equipo de 3 integrantes. |
| **Restricciones Duras (*Constraints*)** *(Cero grados de libertad)* | • Modalidad obligatoria: sistema web *responsive* / PWA.<br>• Escala operativa del evento: 10 días continuos, >400 actividades, convivencia de acceso libre gratuito y teatros cerrados con boleto referencial de 5 USD y aforo limitado.<br>• Equipo ejecutor: 3 integrantes en contexto universitario (excluye pasarelas bancarias reales y hardware físico de taquillas).<br>• Cumplimiento legal obligatorio: **LOPDP Ecuador (2021)** para datos personales de asistentes/organizadores.<br>• Normativa arquitectónica innegociable (`AGENTS.md`): SDD previo a código, Modelo 4+1 en Mermaid compatible con DeepWiki, 3 capas acíclicas (`Domain -> Utility -> Platform`), ECB estricto, Vista Lógica pura y Despliegue UML 2.5.1. |

### 4.2 Paso 2 — Defectos, Ambigüedades y Vacíos Críticos Detectados

| Defecto (Categoría TDR) | Hallazgo en el Enunciado | Riesgo Arquitectónico si No se Aclara |
| :--- | :--- | :--- |
| **Ambigüedad** | *"Uso en calle"* y *"avisos oportunos"* | No define si exige lectura *offline* del itinerario ni en cuántos segundos/minutos máximo debe emitirse un aviso tras un cambio operativo. |
| **Ambigüedad / Contradicción aparente** | *"Boleto de cinco dólares"* frente a alcance académico sin procesamiento bancario real | Si no se delimita el mecanismo de reserva sin pasarela bancaria real, se introducen expectativas falsas de cobro en línea o facturación. |
| **Incompletitud (Vacío)** | Volumetría y picos de concurrencia | No especifica cuántos asistentes consultan simultáneamente la cartelera ni cuántos compiten en el mismo segundo por cupos limitados de un teatro. |
| **Incompletitud (Vacío)** | Reglas de caducidad de reserva, aforo lleno e itinerario | No define cuántos cupos máximos puede reservar un asistente por función, cuánto dura una reserva antes del evento, si hay límite de actividades en itinerario o qué ocurre con traslapes horarios. |
| **Incompletitud (Vacío)** | Canal de notificaciones y fallo de proveedor externo | No precisa si los avisos son solo dentro de la web/PWA (Web Push), por correo electrónico o ambos, ni qué ocurre con la operación principal si el servicio de envío cae. |
| **Mezcla de niveles** | Mención directa de *"responsive/PWA"* junto a necesidades del negocio | Mezcla una decisión técnica de entrega/plataforma con requisitos del dominio; debe aislarse en restricciones técnicas y Vista de Desarrollo/Despliegue, sin contaminar los casos de uso ni la Vista Lógica. |

### 4.3 Paso 3 — Pliego de Preguntas Estratégicas al Cliente (4 Ejes)

| Eje de Análisis | Preguntas Estratégicas para Validación |
| :--- | :--- |
| **1. Volumetría, Concurrencia y Uso en Calle** | **P1:** ¿Cuál es el aforo típico y máximo de los teatros cerrados y cuántos usuarios concurrentes se estiman en picos de apertura de cartelera o previo a funciones estelares?<br>**P2:** En escenarios callejeros con cobertura móvil inestable, ¿basta con consultar en modo *offline* la programación en caché y el itinerario personal guardado, dejando las reservas en tiempo real condicionadas a conexión activa? |
| **2. Reglas de Cupos, Boleto de 5 USD e Itinerario** | **P3:** Al excluirse el procesamiento bancario real, ¿la reserva genera un cupo bloqueado nominal con código verificable para pago/ingreso presencial en taquilla o confirmación dentro de un plazo límite antes del inicio de la función?<br>**P4:** ¿Cuántos cupos por función puede reservar una misma persona y con cuántas horas o minutos de anticipación puede cancelar para liberar el cupo a otros asistentes?<br>**P5:** ¿El itinerario urbano permite agregar actividades superpuestas con advertencia explícita al asistente, o debe bloquear estrictamente el traslape horario? |
| **3. Canales y Oportunidad de Alertas** | **P6:** ¿Qué canal oficial se usará para los avisos oportunos (notificaciones en la PWA / Web Push, correo electrónico o ambos) y cuál es la latencia máxima aceptable tras publicar un cambio de sala, horario o aforo lleno?<br>**P7:** ¿Deben emitirse avisos solo cuando la actividad esté en el itinerario o reserva del asistente, o también alertas generales visibles en cartelera? |
| **4. Gobernanza y Cumplimiento Legal (LOPDP 2021)** | **P8:** ¿Qué datos personales mínimos se requieren del asistente para reservar cupos o recibir avisos (ej. nombre, correo electrónico e identificador mínimo) y cómo se registra su consentimiento informado?<br>**P9:** ¿Qué perfiles de organizador existen (ej. editor de programación vs. coordinador operativo en sala) y se requiere bitácora de auditoría sobre modificaciones de horarios, salas y aforos? |

### 4.4 Paso 4 — Supuestos Operativos Cerrados (`Assumptions`)

Mientras el cliente/docente no modifique formalmente estos puntos, toda la arquitectura y especificaciones posteriores del FIAVL se diseñan bajo los siguientes supuestos explícitos:

| ID | Supuesto Operativo Cerrado (`Assumption`) | Impacto en Diseño |
| :--- | :--- | :--- |
| **SUP-01** | **Reserva sin cobro bancario en línea:** En funciones de teatro cerrado con tarifa referencial de 5 USD, la plataforma gestiona la **reserva nominal de cupo** con un límite máximo configurable por asistente (por defecto hasta **2 cupos por función**). El pago o canje del boleto de 5 USD se realiza de forma presencial en taquilla/canal externo antes del cierre de ventana previo al inicio; si el asistente no confirma o canjea dentro de la ventana operativa (ej. 30 minutos antes de la función) o cancela voluntariamente, el cupo vuelve al aforo disponible. | Elimina pasarelas de pago y hardware físico; exige control estricto de estados de reserva y liberación automática por tiempo. |
| **SUP-02** | **Concurrencia de referencia:** Se dimensiona para un pico académico/institucional de **300 usuarios concurrentes** consultando cartelera y compitiendo simultáneamente por cupos en funciones estelares, con un catálogo base de **400 a 600 actividades** distribuidas en **10 días** y **15 a 30 sedes/espacios urbanos**. | Exige exclusión mutua transaccional en reserva de cupos y consultas ágiles indexadas/cacheadas. |
| **SUP-03** | **Uso en calle *offline-first* de lectura (PWA):** La aplicación web *responsive*/PWA almacena en caché local del dispositivo la cartelera consultada recientemente y el **itinerario urbano personal** del asistente para lectura sin conexión en plazas y calles. Toda operación transaccional (reservar o cancelar cupos, actualizar cartelera) requiere conectividad activa para garantizar consistencia de aforo. | Separa operaciones de lectura resiliente en calle de mutaciones consistentes en servidor. |
| **SUP-04** | **Itinerario personal flexible con alerta de conflicto:** El asistente puede incorporar tanto actividades gratuitas como funciones reservadas en su itinerario urbano; si selecciona dos actividades cuyo horario se solapa (incluyendo un margen referencial de traslado entre sedes distintas), el sistema advierte el conflicto sin impedir obligatoriamente guardar la actividad libre, mientras que las reservas pagadas sí validan disponibilidad de cupo. | Define reglas claras de conflicto horario en dominio sin asumir cálculo GPS complejo. |
| **SUP-05** | **Canales y asincronía de avisos:** Las alertas por cambio de sala, horario, cancelación o aforo lleno se registran en la plataforma (bandeja de avisos visibles para el asistente) y se despachan de forma **asíncrona** hacia un canal digital externo (correo electrónico / Web Push). La indisponibilidad temporal del proveedor externo nunca bloquea ni revierte la actualización de cartelera ni la reserva de cupos. | Impone cola/desacoplamiento asíncrono en Vista de Procesos y aislamiento en Utilitario de Notificaciones. |
| **SUP-06** | **Regla de aforo lleno:** Cuando los cupos disponibles de una función de teatro llegan a `0`, la función pasa automáticamente a estado `Aforo lleno`, bloqueando nuevas reservas hasta que exista una cancelación o liberación automática por vencimiento; adicionalmente, los organizadores pueden activar/emitir un aviso operativo si una función gratuita o pagada alcanza saturación reportada. | Protege integridad de recintos (*Safety*) y trazabilidad con alertas de aforo lleno. |
| **SUP-07** | **Privacidad mínima bajo LOPDP Ecuador (2021):** La exploración pública de cartelera es anónima. Para guardar itinerario persistente entre dispositivos, reservar cupos o suscribirse a avisos externos, se solicitan únicamente datos mínimos (identificador/nombre, correo de contacto y credencial de acceso) con aceptación explícita de finalidad y registro de auditoría sobre cambios de programación y reservas. | Reduce superficie de riesgo legal y define requisitos de Seguridad y Auditoría desde el inicio. |
| **SUP-08** | **Alcance de sedes y tiempos de festival:** Todas las actividades pertenecen a una edición activa de 10 días del FIAVL con zona horaria única (`America/Guayaquil`, UTC-5), eliminando ambigüedades de husos horarios en el cálculo de traslapes y temporizadores. | Simplifica invariantes temporales de dominio y reglas de negocio. |

---

## 5. Catálogo Numerado y Trazable de Requisitos de Usuario (`RU-XX`) y Funcionales (`RF-XX`)

> **Regla de pureza conceptual (R4 / Guía Metodológica):** Todos los requisitos están redactados a nivel de **necesidad del negocio y frontera externa (caja negra)**, sin mencionar pantallas, botones, rutas HTTP, tablas SQL ni claves foráneas.

### 5.1 Actor: Asistente (Público General / Turista)

| ID Usuario | Requisito de Usuario (`RU`) | IDs Funcionales Trazados |
| :--- | :--- | :--- |
| **RU-01** | Como **Asistente**, necesito explorar y filtrar la cartelera completa de los 10 días del FIAVL por disciplina, sede, fecha/horario y modalidad de acceso (gratuito o pagado) para descubrir actividades acordes a mi interés y presupuesto. | `RF-01`, `RF-02` |
| **RU-02** | Como **Asistente**, necesito armar, ajustar y consultar en movilidad mi propio itinerario urbano por la ciudad, con advertencias ante cruces de horario, para organizar mi recorrido durante el festival incluso con señal inestable. | `RF-03`, `RF-04`, `RF-05` |
| **RU-03** | Como **Asistente**, necesito reservar, consultar y cancelar cupos en funciones seleccionadas de teatro cerrado (tarifa referencial de 5 USD y aforo limitado) para asegurar mi ingreso sin sobreocupación. | `RF-06`, `RF-07`, `RF-08` |
| **RU-04** | Como **Asistente**, necesito recibir avisos oportunos ante cambios de sala, cambios de horario, cancelaciones o aforo lleno en actividades de mi interés para evitar desplazamientos innecesarios. | `RF-09` |

#### Especificación Funcional y Criterios de Aceptación — Actor: Asistente

- **RF-01 — Filtrado multicriterio de la cartelera oficial** *(Trazado a `RU-01`)*
  - **Descripción:** El sistema debe permitir al Asistente consultar la programación vigente del festival aplicando de forma individual o combinada filtros por disciplina artística (teatro, danza, música, ópera, circo, intervención urbana), sede o tipo de espacio urbano, rango de fecha/horario y modalidad de acceso (gratuito o pagado de 5 USD).
  - **Criterios de Aceptación:**
    1. Dada una cartelera con >400 actividades en 10 días, al aplicar uno o varios filtros combinados el sistema retorna únicamente las actividades activas que satisfacen todos los criterios seleccionados.
    2. Cada actividad devuelta expone su título, disciplina, sede/espacio urbano, fecha, hora de inicio y fin, modalidad de acceso (gratuito o 5 USD), estado operativo vigente y disponibilidad de cupos cuando aplique aforo limitado.
    3. Si la combinación de filtros no produce coincidencias, el sistema informa explícitamente la ausencia de resultados sin error operativo.

- **RF-02 — Consulta de detalle de actividad y disponibilidad de acceso** *(Trazado a `RU-01`)*
  - **Descripción:** El sistema debe permitir al Asistente consultar el detalle completo de una actividad específica, diferenciando espacios públicos de acceso libre y funciones en teatros cerrados con tarifa referencial de 5 USD y cupos restantes actualizados.
  - **Criterios de Aceptación:**
    1. Para actividades en espacios públicos gratuitos, el detalle indica acceso libre sin requerimiento de reserva de cupo.
    2. Para funciones pagadas en teatros cerrados, el detalle muestra el aforo total, los cupos disponibles en ese instante, la tarifa referencial (5 USD) y las condiciones límite de reserva/cancelación.

- **RF-03 — Incorporación y organización de actividades en itinerario urbano personal** *(Trazado a `RU-02`)*
  - **Descripción:** El sistema debe permitir al Asistente agregar o retirar actividades dentro de su itinerario urbano personal ordenado cronológicamente por día de festival y sede.
  - **Criterios de Aceptación:**
    1. Un Asistente puede agregar cualquier actividad vigente (gratuita o con reserva) a su itinerario personal sin duplicar la misma función.
    2. El sistema ordena las actividades del itinerario cronológicamente por fecha y hora de inicio, indicando sede y modalidad de acceso.
    3. El Asistente puede retirar en cualquier momento una actividad libre de su itinerario sin afectar la disponibilidad global del evento.

- **RF-04 — Detección y advertencia de conflictos horarios en itinerario urbano** *(Trazado a `RU-02`)*
  - **Descripción:** Al incorporar una actividad al itinerario personal o cuando una actividad existente cambie de horario/sede, el sistema debe detectar traslapes horarios o cambios entre sedes distintas en horarios contiguos y emitir una advertencia explícita al Asistente.
  - **Criterios de Aceptación:**
    1. Si la franja `[horaInicio, horaFin]` de la nueva actividad se superpone con otra actividad ya presente en el itinerario del mismo día, el sistema identifica exactamente las actividades en conflicto y notifica la superposición.
    2. Si dos actividades consecutivas ocurren en sedes distintas con margen inferior al tiempo mínimo de traslado configurado, el sistema advierte la posible incompatibilidad de desplazamiento urbano.

- **RF-05 — Disponibilidad en movilidad del itinerario guardado** *(Trazado a `RU-02`)*
  - **Descripción:** El sistema debe conservar una copia sincronizada localmente en el dispositivo del Asistente con su itinerario personal y las fichas básicas de dichas actividades para su consulta en calle aunque exista pérdida temporal de conectividad.
  - **Criterios de Aceptación:**
    1. Una vez sincronizado el itinerario con conectividad activa, el Asistente puede consultar las actividades guardadas, sus horarios, sedes y códigos de reserva vigentes aun sin conexión de red.
    2. Cuando el dispositivo recupera conectividad y existen actualizaciones operativas en el servidor, el sistema sincroniza el estado vigente del itinerario e informa los cambios ocurridos.

- **RF-06 — Reserva de cupo en funciones seleccionadas con aforo limitado** *(Trazado a `RU-03`)*
  - **Descripción:** El sistema debe permitir a un Asistente autenticado y con consentimiento LOPDP activo solicitar la reserva de uno o más cupos (hasta el máximo permitido por persona según `SUP-01`) en una función de teatro cerrado pagada que cuente con aforo disponible.
  - **Criterios de Aceptación:**
    1. Antes de confirmar, el sistema verifica en exclusión mutua que el cupo solicitado no excede el aforo restante ni el límite por asistente, y que la función no se encuentra cancelada ni cerrada por tiempo límite.
    2. Al confirmarse la reserva, el sistema descuenta atómicamente los cupos del aforo disponible, emite un identificador único de reserva verificable con su vigencia temporal y vincula la función al itinerario del Asistente.
    3. Si dos o más asistentes solicitan el último cupo disponible en el mismo instante, como máximo una solicitud se confirma y las restantes reciben rechazo controlado por indisponibilidad de cupo (`0` sobreventas).

- **RF-07 — Consulta de estado y comprobante lógico de reserva** *(Trazado a `RU-03`)*
  - **Descripción:** El sistema debe permitir al Asistente consultar el historial y estado actual de sus reservas (ej. reservada/pendiente de canje, confirmada/validada, cancelada, expirada), incluyendo el código único de verificación.
  - **Criterios de Aceptación:**
    1. El Asistente visualiza únicamente sus propias reservas con el estado vigente, cantidad de cupos, tarifa referencial (5 USD por cupo), fecha límite de validez y código de verificación.

- **RF-08 — Cancelación voluntaria de reserva y liberación de cupos** *(Trazado a `RU-03`)*
  - **Descripción:** El sistema debe permitir al Asistente cancelar una reserva activa antes del límite horario operativo previo a la función, restituyendo automáticamente los cupos al aforo disponible.
  - **Criterios de Aceptación:**
    1. Si la cancelación se solicita dentro del plazo permitido y la reserva no fue ya validada/consumida ni cancelada, el sistema cambia la reserva a estado `Cancelada` y suma los cupos liberados al aforo disponible de la función de forma atómica.
    2. Si la función ya inició, venció el plazo límite de cancelación o la reserva ya fue utilizada, el sistema rechaza la cancelación e informa el motivo.

- **RF-09 — Gestión de consentimiento LOPDP y recepción de avisos oportunos** *(Trazado a `RU-04`)*
  - **Descripción:** El sistema debe permitir al Asistente otorgar o revocar su consentimiento explícito para recibir alertas operativas y consultar los avisos emitidos ante cambios de sala, horario, cancelación o aforo lleno que afecten actividades de su itinerario o reserva.
  - **Criterios de Aceptación:**
    1. Ningún envío externo de notificaciones se realiza sin consentimiento explícito registrado conforme a la LOPDP (2021); el Asistente puede revocar dicho consentimiento en cualquier momento.
    2. Todo aviso emitido por cambio en una función del itinerario/reserva o por aforo lleno queda registrado en la bandeja de avisos del Asistente y se encola para entrega por el canal consentido.

---

### 5.2 Actor: Organizador (Gestión del Festival FIAVL)

| ID Usuario | Requisito de Usuario (`RU`) | IDs Funcionales Trazados |
| :--- | :--- | :--- |
| **RU-05** | Como **Organizador**, necesito registrar y mantener actualizada la programación de actividades, disciplinas, sedes y aforos durante los 10 días del FIAVL para ofrecer información oficial confiable al público. | `RF-10`, `RF-11` |
| **RU-06** | Como **Organizador**, necesito aplicar cambios operativos de sala, horario o cancelación y publicar alertas operativas hacia los asistentes afectados para responder oportunamente ante imprevistos en sedes. | `RF-12`, `RF-13` |
| **RU-07** | Como **Organizador**, necesito monitorear la ocupación de funciones con aforo limitado y validar o liberar reservas operativas para controlar el ingreso ordenado en teatros cerrados. | `RF-14`, `RF-15` |

#### Especificación Funcional y Criterios de Aceptación — Actor: Organizador

- **RF-10 — Administración de catálogo base de sedes y programación de actividades** *(Trazado a `RU-05`)*
  - **Descripción:** El sistema debe permitir únicamente a un Organizador autenticado y autorizado registrar, editar o desactivar sedes urbanas (clasificadas en espacio público gratuito o teatro cerrado con capacidad física máxima) y actividades de la cartelera con disciplina, horario, sede, modalidad de acceso y aforo habilitado.
  - **Criterios de Aceptación:**
    1. Solo usuarios con rol autorizado de Organizador pueden ejecutar modificaciones sobre sedes y cartelera; intentos no autorizados son rechazados y auditados.
    2. El sistema valida invariantes de programación: la hora de fin debe ser estrictamente posterior a la hora de inicio, la fecha debe caer dentro de los 10 días de la edición activa, y en teatros cerrados el aforo habilitado no puede superar la capacidad física máxima de la sede ni ser negativo.
    3. Toda actividad nueva o editada queda reflejada de inmediato en las consultas de cartelera (`RF-01`).

- **RF-11 — Validación de no solapamiento operativo en una misma sede cerrada o escenario** *(Trazado a `RU-05`)*
  - **Descripción:** Al programar o reprogramar una actividad en una sede específica, el sistema debe verificar que no exista otra actividad programada en la misma sede cuyo horario se superponga.
  - **Criterios de Aceptación:**
    1. Si se intenta asignar una función en una sede y franja ya ocupada por otra actividad activa en esa misma sede, el sistema rechaza la operación señalando la actividad en conflicto (evitando doble asignación física de un teatro o escenario).

- **RF-12 — Registro auditado de cambio operativo de sala, horario o estado de función** *(Trazado a `RU-06`)*
  - **Descripción:** El sistema debe permitir al Organizador modificar la sede, reasignar la fecha/horario o suspender/cancelar una actividad ya publicada, registrando el motivo del cambio y conservando trazabilidad en bitácora de auditoría.
  - **Criterios de Aceptación:**
    1. Todo cambio de sede, horario o cancelación actualiza inmediatamente la programación oficial y registra en auditoría quién ejecutó el cambio, qué valores cambiaron, el motivo y la marca de tiempo.
    2. Si se suspende/cancela una función con reservas vigentes, el sistema marca dichas reservas como afectadas/canceladas por la organización y dispara la notificación obligatoria hacia los asistentes afectados.
    3. No se permite reducir el aforo máximo habilitado por debajo de la cantidad de cupos ya reservados activos sin gestionar primero las reservas excedentes.

- **RF-13 — Publicación de alertas operativas dirigidas y avisos por aforo lleno** *(Trazado a `RU-06`)*
  - **Descripción:** El sistema debe permitir al Organizador emitir alertas operativas asociadas a una actividad, sede o anuncio del festival (cambio de sala, cambio de horario, aforo lleno o contingencia climática/logística), identificando a los asistentes con esa actividad en su itinerario o con reserva vigente.
  - **Criterios de Aceptación:**
    1. Toda alerta publicada contiene identificador único, tipo de evento (`Cambio de sala`, `Cambio de horario`, `Aforo lleno`, `Suspensión/Aviso operativo`), mensaje claro, alcance (actividad/sede afectada) y marca de tiempo de emisión.
    2. El sistema genera la entrega de la alerta hacia todos los asistentes que tengan la actividad afectada en su itinerario o reserva activa, ejecutando el envío externo de manera asíncrona para que el Organizador reciba confirmación inmediata de publicación.

- **RF-14 — Monitoreo de ocupación de aforos por función** *(Trazado a `RU-07`)*
  - **Descripción:** El sistema debe permitir al Organizador consultar el estado actualizado de cupos totales, reservados, validados/ingresados y disponibles para cada función con aforo limitado o sede.
  - **Criterios de Aceptación:**
    1. La suma de cupos reservados activos + cupos validados + cupos disponibles coincide exactamente con el aforo habilitado de la función en todo instante.

- **RF-15 — Validación operativa de código de reserva en ingreso o liberación manual** *(Trazado a `RU-07`)*
  - **Descripción:** El sistema debe permitir al Organizador verificar un código de reserva emitido para confirmar el ingreso del asistente (sin hardware propietario) o liberar manualmente una reserva cuyo asistente desistió en taquilla.
  - **Criterios de Aceptación:**
    1. Al validar una reserva activa, su estado transiciona a `Validada/Utilizada`, impidiendo reutilizaciones duplicadas del mismo código.
    2. Si un código corresponde a una reserva `Expirada` o `Cancelada`, el sistema rechaza la validación indicando su estado real.

---

### 5.3 Actor: Temporizador / Sistema (`Tiempo` — Procesos Automáticos)

| ID Usuario | Requisito de Usuario (`RU`) | IDs Funcionales Trazados |
| :--- | :--- | :--- |
| **RU-08** | Como **Sistema / Operación por Tiempo**, necesito liberar automáticamente cupos de reservas vencidas antes del inicio de cada función para maximizar la ocupación real de los teatros cerrados. | `RF-16` |
| **RU-09** | Como **Sistema / Operación por Tiempo**, necesito disparar recordatorios programados y procesar asíncronamente las alertas de cambios o aforo lleno para mantener informados a los asistentes sin afectar la disponibilidad de la plataforma. | `RF-17`, `RF-18` |

#### Especificación Funcional y Criterios de Aceptación — Actor: Temporizador / Sistema

- **RF-16 — Liberación automática por vencimiento de ventana de reserva** *(Trazado a `RU-08`)*
  - **Descripción:** Al cumplirse el tiempo límite de vigencia de una reserva pendiente de confirmación/canje o la ventana previa al inicio de una función pagada (según `SUP-01`), el sistema disparado por temporizador debe expirar la reserva y restituir los cupos al aforo disponible.
  - **Criterios de Aceptación:**
    1. Toda reserva no validada que supere su fecha/hora de expiración pasa atómicamente a estado `Expirada` y suma sus cupos nuevamente al aforo disponible (actualizando el estado de `Aforo lleno` si vuelve a haber cupos libres).
    2. El proceso es idempotente: ejecuciones repetidas del temporizador sobre la misma reserva no duplican cupos liberados.

- **RF-17 — Generación de aviso automático por aforo lleno y recordatorios programados** *(Trazado a `RU-09`)*
  - **Descripción:** Cuando los cupos disponibles de una función alcanzan `0` o se aproxima el inicio de una actividad guardada en itinerarios personales (ventana configurable, ej. 2 horas antes), el sistema debe emitir el aviso automático correspondiente a los asistentes interesados.
  - **Criterios de Aceptación:**
    1. Al agotarse el último cupo disponible (`disponibles == 0`), la función queda marcada como `Aforo lleno` y se genera el aviso automático hacia asistentes que sigan o tengan la actividad en su itinerario sin reserva confirmada.
    2. Los recordatorios previos al inicio se emiten una única vez por actividad y asistente dentro de la ventana configurada, siempre que el asistente mantenga consentimiento activo.

- **RF-18 — Despacho asíncrono y reintento controlado de notificaciones operativas** *(Trazado a `RU-09`)*
  - **Descripción:** Toda notificación externa originada por cambios operativos, aforo lleno o recordatorios debe despacharse de manera desacoplada y asíncrona, con política de reintentos ante fallos temporales del canal externo.
  - **Criterios de Aceptación:**
    1. Si el servicio externo de entrega (correo/push) falla o excede el tiempo de espera, la operación de origen (reserva de cupo o actualización de programación) **no falla ni se revierte**; el envío queda pendiente de reintento registrado con trazabilidad de estado (`Pendiente`, `Enviado`, `Fallido`).

---

### 5.4 Matriz de Trazabilidad `TDR -> RU -> RF -> Entrada a Vista +1`

| Necesidad del Contexto FIAVL | Requisitos de Usuario (`RU`) | Requisitos Funcionales (`RF`) | Actor Disparador Principal | Impacto en Futura Vista +1 (Prompt 2) |
| :--- | :--- | :--- | :--- | :--- |
| Explorar y filtrar cartelera (>400 actividades en 10 días) | `RU-01` | `RF-01`, `RF-02` | Asistente | Casos de exploración/consulta de cartelera |
| Armar itinerario urbano por la ciudad (uso en calle PWA) | `RU-02` | `RF-03`, `RF-04`, `RF-05` | Asistente | Casos de gestión de itinerario y detección de traslapes |
| Reservar y gestionar cupos en funciones pagadas (5 USD, aforo limitado) | `RU-03` | `RF-06`, `RF-07`, `RF-08` | Asistente | Caso crítico de reserva/cancelación concurrente |
| Recibir avisos oportunos por cambios de sala, horario o aforo lleno | `RU-04`, `RU-09` | `RF-09`, `RF-17`, `RF-18` | Asistente / Tiempo | Casos de consentimiento y notificación asíncrona |
| Mantener programación actualizada y publicar alertas operativas | `RU-05`, `RU-06` | `RF-10`, `RF-11`, `RF-12`, `RF-13` | Organizador | Casos de gestión de programación y emisión de alertas |
| Controlar aforos y liberar cupos vencidos | `RU-07`, `RU-08` | `RF-14`, `RF-15`, `RF-16` | Organizador / Tiempo | Casos de validación de ingreso y expiración automática |

---

## 6. Verificación de Compuerta SDD (Pre-Prompt 2)

- [x] **R1 (Compuerta SDD):** Documento de TDR, alcance, vacíos, supuestos y requisitos trazables creado en `specs/000-tdr/` antes de diseñar vistas o escribir código en `src/`.
- [x] **R4 (Pureza conceptual):** Ningún requisito (`RU-XX` / `RF-XX`) contiene tablas SQL, columnas físicas, `PK`/`FK`, pantallas, botones ni código de implementación.
- [ ] **Aprobación en PR:** Pendiente de revisión humana y aprobación de pares antes de implementar código en `src/`.
