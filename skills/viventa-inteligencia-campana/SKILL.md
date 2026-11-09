---
name: viventa-inteligencia-campana
description: >-
  Produce el insight y la parte creativa de una campaña de Viventa —de evento o de un tema como
  compra conjunta o proyectos con las mejores tasas, uno o varios temas a la vez—: target, dolores
  anclados a verbatims, investigación de qué hooks y copy funcionan hoy en Instagram, TikTok y la
  Biblioteca de anuncios de Meta, ángulos que van más allá de la idea inicial, y en modo brief las 15
  piezas escritas (5 videos, 5 carruseles, 5 estáticas) con variantes para probar. USA cuando: "¿cuál
  es el ángulo de esta campaña?", "la parte creativa de noviembre", "arma el brief creativo del
  evento de {ciudad}", "brief de optimización: el CPL no está bajando", "dame ángulos nuevos para
  Meta". Es el insight y el creativo: viventa-guiones-evento escribe los guiones de rodaje de un evento.
metadata:
  version: "2.0.0"
  domain: "campaign"
  client: "viventa"
  blueprint: "marketing"
  complexity: "standalone"
---

# Inteligencia de campaña — Viventa

Hoy las campañas de Viventa arrancan sin insumo: *"siempre la necesidad es: necesitamos leads,
necesitamos que la gente vaya a los eventos"* (equipo de producción, reunión de diagnóstico). Este
skill produce el documento que debería anteceder al brief de la agencia, a los guiones y a los
flyers. **No reemplaza a la agencia externa de pauta** —que define keywords, copies y hooks—: le
entrega el insumo, presentado como "esto es lo que sabemos de este público y de este territorio".
No aprueba, no lanza, no compra pauta, no toca la cuenta de Meta ni datos personales de leads:
termina en un entregable para el responsable de marca.

**Principio rector: el ángulo ES el targeting.** Desde el 14-ene-2025 los anuncios de crédito caen
en la Special Ad Category "Financial Products and Services" de Meta, que elimina la segmentación por
edad, género y código postal. Consecuencias, verificables en el entregable:

1. El callout va en las **primeras 8 palabras**; si el sub-segmento no se nombra ahí, no se nombró.
2. El callout describe una **situación, no una demografía**. Prohibido: *"¿Eres colombiano de 35 a 50
   años en Florida?"* Permitido: *"Vives en Broward y todos los eventos son en Miami."*
3. La landing filtra lo que el targeting ya no filtra: por eso se entrega la **pregunta
   calificadora** del formulario (Meta ya es el canal de [cifra interna] de CAC fully-loaded).
4. Nunca prometer aprobación ni tasa: es regla de marca y el terreno donde Meta rechaza creativos.

> **Qué cambió en la v2 (feedback del equipo, noviembre de 2026).** La parte creativa de noviembre
> eran dos temas —*compra conjunta* y *proyectos con las mejores tasas posibles*— y el equipo pidió
> **5 videos, 5 carruseles y 5 piezas estáticas**, *"más flexible y creativo, porque se queda pegado
> con las ideas principales que le dimos"*, con copy de mejor calidad *"investigando redes como
> Instagram para buscar los hooks que funcionan hoy"*. Por eso: (1) hay **campañas de tema** además de
> las de evento, con uno o varios temas; (2) la investigación de hooks ya no es opcional, es una
> pasada acotada (`references/copy-que-convierte.md` §1); (3) el paquete
> por defecto es **5 + 5 + 5** y cada pieza sale **escrita completa**, no descrita; (4) cada tema se
> explora con al menos 4 ángulos y el paquete incluye **2 propuestas libres** rotuladas; (5) cada pieza
> trae su variante B para probar.

## Cuándo usar

- Explícito: "¿cuál es el ángulo de esta campaña?", "necesito el insight para la campaña de
  {ciudad}", "arma el brief de inteligencia para el evento de {ciudad}", "¿de qué va a hablar esta
  campaña, para no repetir lo de siempre?", "dame los dolores y los hooks para pasarle a la agencia",
  "esta campaña quedó igual a la anterior, sácame ángulos nuevos", "qué hooks probamos para
  {territorio}".
- Implícito: hay un brief de evento o un pedido de campaña y todavía no se decidió de qué va a hablar.

## Cuándo NO usar

- Ya hay ángulo y faltan los videos → `viventa-guiones-evento`; faltan los correos →
  `viventa-comunicaciones-campana`.
- Revisar una pieza ya escrita → `viventa-qa-marca`.
- La ficha, el semáforo y los bloqueantes de un evento nuevo → `viventa-brief-evento` (este skill es
  el insight que va después).
- Qué se le manda este mes a cada segmento de la base → `viventa-parrilla-base`.

## Conexiones

- **Búsqueda web** (máximo 6 consultas, en el orden de `copy-que-convierte.md` §1): Biblioteca de
  anuncios de Meta (pública, sin cuenta), reels y TikToks recientes del tema, creadores de finanzas
  para migrantes y sus comentarios. Si la búsqueda no está disponible, **degrada**: se entrega con las
  fuentes internas y el banco de hooks del estándar, y el bloque 6 lo dice.
- **Búsqueda en Instagram y TikTok** (si están instaladas `ig-search` y `tt-search`, se usan en vez de
  la búsqueda web genérica para los puntos 2-4). Claude no entra a Instagram con sesión ni ve métricas
  privadas: lo que se cita es lo público, con URL.

## Input del usuario

| Campo | De dónde sale | Si falta, preguntar |
|---|---|---|
| Tipo de campaña — evento (presencial, virtual o híbrido) / **tema** (compra conjunta, mejores tasas, Mi Casa en Colombia, temporada de primas…) / marca / producto (constructora) | El pedido | Se deduce del pedido. Solo si es imposible: *"¿Es para un evento, un tema, la marca o un proyecto?"* |
| Tema o temas (campaña de tema) | El pedido | Puede venir más de uno ("compra conjunta y proyectos con mejores tasas"): se trabajan los dos en el mismo documento, con ángulos repartidos |
| Territorio — ciudad o condado con nombre propio | El pedido o el brief del evento | **Bloquea solo los eventos presenciales.** En campañas de tema o virtuales, si no viene, el territorio es **USA + España** y el callout se hace por situación ("Vives en Estados Unidos y…"), declarado en el bloque 1 |
| Referentes que le gustan al equipo (reels, anuncios, capturas) | El usuario | No se pregunta. Si vienen, la investigación arranca por ahí |
| Cantidad de piezas | El pedido | No se pregunta: **5 videos + 5 carruseles + 5 estáticas**. Si el pedido dice otra cosa, se respeta |
| Fecha y dirección del evento | Brief del evento | Si es evento presencial y falta la dirección, no se pregunta: el bloque 1 dice `[⚠ falta dirección concreta — no se puede anunciar el evento sin ella, es la fricción #1 del VOC]` y el brief sigue. Virtual: plataforma y `{LINK_REGISTRO}` (`references/virtual-y-lotes.md`). Sin fecha: el chequeo de 45 días dice `no verificado` y se sigue |
| Sub-segmento objetivo | El pedido | No se pregunta: se deriva del VOC según territorio y se declara en el bloque 1 |
| Constructora / proyecto (campaña de producto) | El pedido | No se pregunta: se trabaja a nivel marca y se anota en el bloque 6 |
| Datos de rentabilidad | La gerente de Growth | No se pregunta: el ángulo de retorno queda como pregunta abierta en el bloque 6, no como ángulo |
| Voice of Customer 90 días | `references/voice-of-customer-90d.md` | Si no está accesible: se usa solo la matriz precargada de `references/dolores-y-verbatims.md` y se declara en el bloque 6 |
| Manejo de objeciones | «Manejo de Objeciones.docx», adjunto al chat | *"¿Me adjuntas el Manejo de Objeciones?"* Si no lo tiene: las objeciones precargadas de la matriz; R5 se cumple con esas |
| Banco de ángulos usados | `$AIOS_STATE/viventa-inteligencia-campana/angulos-usados.md` | No se pregunta: si no existe se crea con el encabezado (`references/banco-angulos.md` §1) |
| Comentarios y mensajes de redes de los últimos 30 días | Export o texto que pega la community manager | No se pregunta: sin ellos la Fase 2b se declara vacía en el bloque 6 |
| Resúmenes de llamadas de Aircall | Los pega el usuario, **sin nombres ni teléfonos** | No se pregunta. Si vienen con datos personales, se descartan esos campos antes de leer |
| Modo | El pedido | No se pregunta: "brief creativo", "brief del evento para la agencia" o "brief de optimización" activan la Fase 7 |
| Métricas de la campaña activa (solo optimización) | Ads Manager o `viventa-monitor-evento` | *"¿Qué ángulo y qué hook están corriendo, y cómo van CPL y CTR?"* Sin esto no hay set de optimización |
| Fecha de hoy | El sistema | — |

## Workflow

Seis fases (siete en modo brief), una pasada, sin loops. Lo que ya se sabe del cliente está en este archivo y en `references/`: no se re-investiga. Lo que **sí** se investiga, en una pasada corta, es qué está funcionando hoy en redes (Fase 2b).

### Fase 1 — Encuadre
Confirma tipo de campaña, tema(s), territorio y, si es evento, fecha y dirección o plataforma. Lo que
se puede deducir no se pregunta; si falta un bloqueante real, se pregunta una sola vez, en un solo
mensaje, y lo demás se resuelve con defaults declarados.
- **Chequeo de 45 días** (solo eventos): si faltan **menos de 45 días** (44 incluido), adviértelo en
  la primera línea del entregable; 45 o más, sin advertencia. Es el tiempo que necesita la pauta para
  pasar aprendizaje y llegar a un CPL sano.
- **Chequeo de elegibilidad:** si el país del territorio no está en `references/dolores-y-verbatims.md`
  §6, avisa y no propongas ángulos hasta que el usuario confirme.

### Fase 2 — Minería de dolores (local, obligatoria)
Lee, en este orden:

1. `references/voice-of-customer-90d.md`
2. «Manejo de Objeciones.docx» (adjunto al chat)
3. `references/dolores-y-verbatims.md` de este skill — matriz precargada, los 4 sub-segmentos y
   sus trampas factuales. **No re-investigues esto.**

Filtra por territorio. Saca 5-7 dolores, cada uno con su verbatim literal y su fuente.
Marca cuáles son **miedo** y cuáles son **dolor** (R6).

### Fase 2b — Qué está funcionando hoy en redes (obligatoria, acotada a 6 búsquedas)
Corre `references/copy-que-convierte.md` §1: Biblioteca de anuncios de
Meta (competencia y constructoras que le venden al colombiano afuera), reels y TikToks recientes del
tema, creadores de finanzas para migrantes y los comentarios de esas publicaciones. Si el equipo pegó
referentes o comentarios de sus propias redes, se empieza por ahí. Saca:
- **5 a 8 referentes** con URL, el hook textual, el formato y por qué funciona.
- **Máximo 5 señales** (preguntas que se repiten, objeciones nuevas), cada una con fuente y fecha
  (sin fecha, no entra). Una señal confirma, contradice o agrega un dolor de la Fase 2; si agrega,
  entra con la etiqueta `señal fresca`.
El resultado es la **matriz dolor → ángulo → hook** con la evidencia citada en cada fila. Si la
búsqueda web no está disponible en la sesión, se dice en una línea y se sigue con el banco de hooks
del estándar de copy (§3).

### Fase 3 — Lectura del banco de ángulos
Lee `$AIOS_STATE/viventa-inteligencia-campana/angulos-usados.md` (formato en
`references/banco-angulos.md` §1; si no existe, créalo con el encabezado y sigue). Descarta lo `quemado` y lo `activo` de menos de 30 días. Lo descartado va
al bloque 4 del entregable, con la razón.

### Fase 4 — Construcción de los ángulos (3 por tema, después de explorar al menos 4)

Antes de elegir, explora **al menos 4 ángulos por tema** con la tabla de
`copy-que-convierte.md` §2 (miedo desactivado · dolor concreto · deseo con imagen · contraintuitivo ·
error común · prueba · comparación · momento). La idea que trajo el equipo es el **tema**, no el
ángulo: no se queda en la primera lectura del pedido. Con dos temas, son 3 ángulos por tema.
Cada ángulo se arma sobre las 4 P: **Pain · Plan · Proof · Philosophy**, y declara la **frase
exacta del creativo que hace el callout**. Un ángulo sin esa frase no es un ángulo.

Corre el **test de 3 preguntas** (enemigo distinto · sub-segmento o awareness distinto · prueba
distinta — detalle en `references/banco-angulos.md` §4) y **escribe el resultado**. Si alguna
línea da FALLA, reemplaza el ángulo antes de seguir. No se entrega con un FALLA escrito.

Verifica R5: al menos uno nace de una objeción del playbook.

### Fase 5 — Hooks
Redacta **5 hooks por tema**, completos, entre comillas, listos para pegar en Meta, con las fórmulas
de `copy-que-convierte.md` §3 y al menos 3 fórmulas distintas entre los 5. El hook 1 lleva el
callout de territorio o de situación y tiene **≤ 12 palabras**. Cada hook dice de qué referente de
la Fase 2b sale, si sale de uno.

**Tendencia sin URL y sin fecha no entra**: si no se pudo verificar, escribe *"sin dato de tendencia
esta semana"*. Mejor vacío que inventado.

### Fase 6 — Autovalidación y cierre
Corre el checklist completo (abajo). Escribe las 3 líneas nuevas en `angulos-usados.md` con
estado `sin probar`. Entrega para aprobación del responsable de marca.

### Fase 7 — Brief creativo (solo si se pide)
1. **Paquete estándar: 5 videos, 5 carruseles y 5 estáticas** (antes eran 3 carruseles y 2
   estáticas; el equipo pidió 5 + 5 + 5). Si el pedido dice otra cantidad, se respeta. Asigna a cada
   pieza un ángulo y un hook, el segmento (país y ciudad, o situación) y el CTA. Que los ángulos
   queden repartidos: ninguno con más de 2 videos. Con dos temas, se reparten las 15 piezas entre
   los dos (p. ej. 8 y 7) y se dice cómo.
2. **Libertad creativa con rótulo:** de las 15, **2 son "Propuesta libre — no estaba en el pedido"**:
   un ángulo o un formato que la investigación mostró que funciona (un POV, un "comentario-respuesta",
   un mito vs. realidad, un caso contado como historia) y que conecta con el tema. El equipo decide si
   entran. Los formatos de carrusel no se repiten más de 2 veces (listicle · mito vs. realidad · paso a
   paso · caso · comparación · preguntas frecuentes · antes y después).
3. **Videos, escritos completos**: por cada uno, ángulo, formato (talking head, POV, voz en off con
   b-roll, entrevista, comentario-respuesta), **hook literal de 0-3 s (dicho y en pantalla)**,
   guion por beats con tiempos (15-45 s), texto en pantalla, CTA dicho y escrito, y el hook B para
   probar. En eventos, `viventa-guiones-evento` toma esto y lo vuelve guion de rodaje con shot list.
4. **Carruseles, escritos completos**: lámina por lámina (5 a 8), con el texto exacto de cada una;
   la primera es el hook (y su variante B), la penúltima la prueba, la última el CTA. Más el **caption**
   (primeros 125 caracteres = hook) con su CTA y la palabra clave si aplica.
5. **Estáticas, escritas completas**: titular (≤ 8 palabras) + titular B, bajada, prueba concreta,
   CTA y caption. Nota de diseño de una línea (qué imagen o recurso gráfico lleva).
6. Cada pieza pasa el **test de mediocridad** (`copy-que-convierte.md` §7) antes de entregarse.
7. **Revisión de lo que no se puede decir**: pasa cada texto por
   `references/palabras-prohibidas.md` y las
   reglas R de este skill. Lo que no pasa se reescribe antes de entregar.
8. **Set de optimización** (cuando el CPL no baja): conserva el ángulo con mejor CTR, escribe 3 hooks
   nuevos sobre él y un ángulo nuevo de la matriz; el tamaño del paquete es el mismo. Si faltan las
   métricas de la campaña, no hay set: dilo.
9. La agencia valida contenido, configuración y presupuesto antes de que nada salga al aire.
10. Cierra con el **plan de pruebas** y la **bitácora** de `copy-que-convierte.md` §6, para que la
    próxima corrida arranque de lo que ganó.

### Advertencias que no se borran (aplican a las Fases 4 y 5)

Sobreviven a cualquier reescritura del entregable.

- ⚠️ **VA loan.** El VA loan **no financia propiedad fuera de EE.UU. ni de sus territorios**. El
  ángulo veteranos **no puede ser** "usa tu VA loan en Colombia" — es promesa falsa. El ángulo
  honesto es *"tu ingreso estable de veterano te sirve acá, aunque el VA no cruce la frontera"*,
  y aun ese está `[⚠ no verificado — pendiente de validar con el área de Crédito si el ingreso VA
  computa ante bancos colombianos]`. Mientras no esté validado, va al bloque 6, no al bloque 3.
- ⚠️ **"Raíces" está saturado.** Es el ángulo de los ~8 brokers autorizados por Bancolombia
  (Colombia Raíz, Colraíces, Casa Propia Colombia, Casa en Casa, Unión Andina, Interimage,
  Suvivienda). Se ve hasta en los nombres de marca. No diferencia.
- ⚠️ **Terreno de la agencia.** Hooks, copies y keywords los define hoy la agencia externa. Este
  entregable es el **insumo que Viventa le pasa**, no su reemplazo. Presentarlo así.
- ⚠️ **Puerto Rico:** el callout se escribe "colombiano en Puerto Rico", nunca "puertorriqueño" —
  los bancos exigen ser colombiano o tener parentesco en primer grado.
- ⚠️ **"Mi Casa en Colombia"** (FNA, aprobado 17-jun-2026) se enuncia como programa existente,
  nunca como aprobación garantizada. Verificar estado de reglamentación en cada campaña.

### Checklist de autovalidación (Fase 6)

Antes de entregar, todas deben dar sí.

- [ ] El insight (bloques 1-6) cabe en 2 páginas. El brief creativo de la Fase 7 va aparte, después.
- [ ] Cada dolor tiene verbatim/objeción + fuente, o dice `[sin evidencia]`. **Cero dolores sin ancla.**
- [ ] Los verbatims están literales, con typos, sin maquillar.
- [ ] Al menos 2 de los dolores están marcados como **miedo**.
- [ ] Al menos 1 de los 3 ángulos nace de una objeción del `Manejo de Objeciones.docx`.
- [ ] Los 3 ángulos pasan el test de 3 preguntas y el resultado está escrito. Ningún FALLA.
- [ ] Cada ángulo declara la frase del creativo que hace el callout del sub-segmento.
- [ ] Cada ángulo trae sus 4 P completas.
- [ ] 5 hooks entre comillas, español de Colombia, tuteo. El hook 1 lleva territorio y tiene ≤ 12 palabras.
- [ ] **Filtro de palabras prohibidas**, búsqueda literal en todo el texto: `aprobación`/`aprobado` junto a una promesa (garantizada, asegurada, "te aprobamos", pre-aprobado), `tasa` + cifra, `cuota` + cifra, `plazo` + cifra, `garantizado`, `sin requisitos`, `valorización`, `raíces`. Cero coincidencias.
- [ ] Si hay evento: dirección y ciudad concretas, o advertencia explícita de que faltan.
- [ ] Si faltan menos de 45 días para el evento, lo advierte en la primera línea.
- [ ] `angulos-usados.md` leído antes de proponer y actualizado con 3 líneas nuevas al entregar.
- [ ] Termina en entregable para aprobación del responsable de marca. **No publica, no lanza, no activa pauta.**
- [ ] **Una sola pasada.** Máximo 6 búsquedas; sin búsqueda disponible, se dice y se entrega igual con fuentes internas.
- [ ] Referentes con URL; ningún hook atribuido a un referente que no se abrió.
- [ ] En modo brief: 15 piezas (o las pedidas) escritas completas, 2 propuestas libres rotuladas, variante B en cada una, plan de pruebas y bitácora.

## Reglas duras

Se verifican en la Fase 6. Si una falla, se corrige antes de entregar.

| # | Regla |
|---|---|
| R1 | **Todo dolor va anclado a un verbatim o a una objeción documentada.** Sin fuente → el renglón dice `[sin evidencia]`. Nunca se inventa el dolor. |
| R2 | **Los verbatims se citan literales**, con typos y sin corregir mayúsculas. La frase rara es el activo. |
| R3 | **Cero menciones de tasa, cuota o plazo con cifra.** Son triggering terms de Reg Z y un disclaimer no lo cura. El marketing entrega precio de vivienda; tasa, cuota y plazo los da el asesor en llamada. |
| R4 | **Cero promesas de aprobación**, "garantizado", "sin requisitos", ni proyecciones de valorización. |
| R5 | **De los 3 ángulos, al menos 1 nace de una objeción del playbook**, no de un deseo. |
| R6 | **Al menos 2 de los dolores son miedos**, no fricciones funcionales. En esta categoría el miedo pesa más que el dolor. |
| R7 | **El ángulo "raíces" no entra.** Está saturado: los ~8 brokers autorizados por Bancolombia lo usan todos, se ve hasta en sus nombres. |
| R8 | **El insight: máximo 2 páginas, 6 bloques, cero secciones de contexto.** El brief creativo de la Fase 7 va después y no cuenta en ese tope. |
| R9 | **Una sola pasada.** Máximo 6 búsquedas, en el orden de `copy-que-convierte.md` §1. |
| R10 | **El skill no aprueba, no lanza, no compra pauta.** Termina en entregable para el responsable de marca. |

| R11 | **Cero** "solución integral", "aliado estratégico", "unlock", "leverage" y jerga financiera sin traducir (LTV, DTI, amortización). Español de Colombia, tuteo. |

---

### Anti-patterns (con su antídoto)

| Falla | Cómo se ve | Antídoto |
|---|---|---|
| Dolor de manual | "Buscan seguridad financiera para su familia" | Columna `fuente` obligatoria. Sin fuente → `[sin evidencia]`. Prohibido redactar dolores en tercera persona genérica |
| Tres ángulos que son uno | A: "cumple tu sueño" · B: "haz realidad tu sueño" · C: "tu sueño es posible" | Test de 3 preguntas, con resultado escrito |
| Ignorar que no hay targeting | Ángulo que asume "mostrárselo a hombres 40-55 en Weston" | Cada ángulo declara la frase del creativo que hace el callout |
| Inventar tendencias | "Está en tendencia el formato POV en el nicho inmobiliario latino" | Sin URL y sin fecha no entra. "Sin dato de tendencia esta semana" |
| 4 páginas que nadie lee | Sección "Contexto del mercado inmobiliario colombiano" | Tope duro: 2 páginas, 6 bloques |
| Hooks descritos, no escritos | "Un hook que apele a la nostalgia del migrante" | Los 5 hooks entre comillas, listos para pegar |
| Prometer lo que no puede | "Aprobación garantizada", "tasa desde X%" | Filtro final de palabras prohibidas |
| Copiar el brief de otra ciudad | El mismo doc con Tampa cambiado por Orlando | El territorio es input obligatorio y aparece en el hook 1 |
| Verbatim maquillado | *"colombia todo es robo"* → "existen dudas sobre la seguridad" | Verbatims literales, con typos |
| Re-investigar lo ya sabido | Buscar en web qué es la Special Ad Category | Todo precargado en este SKILL.md y en `references/`. Las búsquedas son para ver qué funciona HOY en redes, no para re-aprender al cliente |
| Quedarse pegado a la idea del pedido | Las 15 piezas dicen lo mismo que el mensaje del equipo con otras palabras | 4 ángulos explorados por tema, 2 propuestas libres, formatos de carrusel que no se repiten más de 2 veces |

## Output esperado

Devuelve en la conversación un markdown de máximo 2 páginas, copiable a Docs para la agencia, y agrega 3 líneas al banco de ángulos. Plantilla exacta:

Seis bloques, en este orden. Sin portada, sin "contexto del mercado", sin glosario.
**Máximo 2 páginas.**

```markdown
# Inteligencia de campaña — {TERRITORIO} · {AAAA-MM}
{si aplica: ⚠ Faltan N días para el evento — por debajo de los 45 que necesita la pauta.}

## 1. Target
{Sub-segmento + territorio + nivel de awareness (Schwartz), en UNA línea.}
Sofisticación: {qué ya vio este público de la competencia.}
Pregunta calificadora para la landing: "{pregunta que filtra post-clic}"

## 2. Dolores priorizados
| # | Dolor | Tipo | Evidencia (verbatim / objeción) | Fuente | Uso en copy |
|---|---|---|---|---|---|
| 1 | ... | miedo | *"verbatim literal, con typos"* | VOC 90D, verbatim NN | ... |
| 2 | ... | dolor | Objeción #N | Manejo de Objeciones.docx | ... |
{5 a 7 filas. Mínimo 2 de tipo "miedo". Sin fuente → [sin evidencia].}

## 3. Ángulos
**Ángulo A — "{nombre}"** ({tipo: territorio / objeción-reversal / reencuadre})
· Pain: {el dolor del que sale, con su #}
· Plan: {la promesa}
· Proof: {la prueba dura — nunca tasa, cuota ni plazo}
· Philosophy: {qué hace distinto a Viventa acá}
· Callout: "{frase exacta del creativo que nombra al sub-segmento, primeras 8 palabras}"
· Auto-selecciona a: {quién se reconoce}
· Por qué ahora: {razón fechada}
· Slug para naming: VIV_{TERRITORIO}_{AAAAMM}_{ANGULO-SLUG}

**Ángulo B — ...** {mismo formato}
**Ángulo C — ...** {mismo formato}

**Test de distinción**
· Enemigos: {A} · {B} · {C} → PASA
· Sub-segmentos: {A} · {B} · {C} → PASA
· Pruebas: {A} · {B} · {C} → PASA

## 4. Ángulos a evitar
- {ángulo}: {razón — quemado en el banco / saturado en la categoría / riesgo regulatorio}

## 5. Hooks (listos para probar)
1. "..."   ← callout de territorio, ≤ 12 palabras
2. "..."
3. "..."
4. "..."
5. "..."

## 6. Qué no sabemos
- {hueco}: `[sin evidencia]` — {quién lo valida y antes de qué}

## 7. Brief creativo (solo en modo brief; va después del insight, sin tope de páginas)
### Mapa del paquete
| # | Pieza | Tema | Ángulo | Formato | Hook A | Segmento | CTA | ¿Propuesta libre? |
(5 videos · 5 carruseles · 5 estáticas, o lo pedido)

### Videos (cada uno completo)
**V1 · {tema} · {ángulo} · {formato} · {duración}**
Hook A (0-3 s, dicho y en pantalla): "…"   ·   Hook B: "…"
| Tiempo | Voz | Texto en pantalla | Imagen |
CTA dicho y escrito: "…"

### Carruseles (lámina por lámina)
**C1 · {tema} · {formato de carrusel}**
L1 (hook A): "…" · hook B: "…" / L2: "…" / … / L{n} (CTA): "…"
Caption (125 primeros caracteres = hook): "…"

### Estáticas
**E1 · {tema}** — Titular A: "…" · Titular B: "…" · Bajada: "…" · Prueba: "…" · CTA: "…" · Caption: "…" · Nota de diseño: "…"

### Plan de pruebas · Referentes que usé (con URL) · Bitácora para la próxima corrida
Revisado contra palabras prohibidas y test de mediocridad: {sin hallazgos | n textos reescritos}
{Set de optimización, si aplica}
```

### Ejemplo (recortado) — parte creativa de noviembre: compra conjunta + proyectos con las mejores tasas

Reparto: 8 piezas de compra conjunta (3 videos, 3 carruseles, 2 estáticas) · 7 de mejores tasas
(2 videos, 2 carruseles, 3 estáticas) · de esas 15, dos son propuestas libres.

**V1 · Compra conjunta · miedo desactivado · talking head de asesora · 30 s**
Hook A: *"¿Comprar con tu hermano? Así queda protegido cada uno en la escritura."*
Hook B: *"El error de los hermanos que compran juntos (y que se arregla antes de firmar)."*
| 0-3 s | Hook A | "COMPRAR ENTRE DOS, SIN PELEAR" | Asesora a cámara |
| 3-12 s | "Cuando dos personas compran juntas, cada una queda en la escritura con su parte. No es un favor: es un papel." | "Cada uno con su parte" | Inserto de escritura (genérica) |
| 12-24 s | "Nosotros revisamos los ingresos de los dos con 5 bancos y te decimos qué figura les sirve. 20 años haciéndolo." | "5 bancos · 20 años" | Asesora + oficina |
| 24-30 s | "Escríbenos JUNTOS y te contamos cómo sería en tu caso." | "Escribe JUNTOS" | Placa de CTA |

**C4 · Mejores tasas · mito vs. realidad (propuesta libre — no estaba en el pedido)**
L1 hook A: *"'Desde afuera te dan peores tasas.' ¿Mito o realidad?"* · hook B: *"Lo que mira un banco antes de darte una tasa (cuando vives afuera)."*
L2: *"Depende de tu perfil, no de dónde vives."* · L3: *"Por eso no comparamos un banco: comparamos 5."* · L4: *"Tú no haces la vuelta: la hacemos contigo."* · L5 (CTA): *"Comenta TASA y te explicamos qué mira cada banco."*
Caption: *"¿Te dijeron que desde afuera las tasas son peores? Te contamos qué mira de verdad un banco. Comenta TASA."*
Nota legal: ninguna lámina da un número de tasa (Reg Z); se habla del proceso de comparar 5 bancos.

### Ejemplo (recortado) — Broward County, FL, evento presencial

**1. Target**
Colombiano no residente en Broward, ingresos en USD, **Solution Aware**: sabe que existen
brokers, no sabe si son confiables ni si él califica. Sofisticación alta — ya vio a Bolívar,
Casa en Casa y a la propia Viventa anunciar ferias.
Pregunta calificadora: *"¿Eres colombiano o tienes familiar colombiano en primer grado?"*

**2. Dolores** (3 de 7, recortado)

| # | Dolor | Tipo | Evidencia | Fuente | Uso en copy |
|---|---|---|---|---|---|
| 1 | Los eventos siempre son en Miami-Dade y no maneja hasta allá | dolor | *"…van a Miami y nunca he visto uno acá en Broward, como que si todos los Colombianos viviéramos en Miami-Dade… mi no se me ha perdido nada por allá abajo para hacer ese viaje"* | VOC 90D, verbatim 24 | El hook nombra Broward y la dirección exacta |
| 2 | "Desde tan lejos uno nunca sabe" | **miedo** | *"Eso realmente es algo de confianza? Porque es que colombia todo es robo."* | VOC 90D, verbatim 19 | Prueba dura antes que adjetivo: 20 años, 5 bancos, oficinas en 3 países |
| 3 | "No sé si califico y no quiero que me digan que no" | **miedo** | Objeción #4 — miedo a presentar taxes por el estatus migratorio | Manejo de Objeciones.docx #4 | "Primero saber, después decidir". Nunca prometer aprobación |

**3. Ángulos** (uno de tres, completo)

> **Ángulo B — "Saber si calificas no te compromete a nada"** (objeción-reversal)
> · Pain: dolor #3, el miedo al "no".
> · Plan: sales de la reunión sabiendo tu situación real, compres o no.
> · Proof: revisión de perfil crediticio con 5 bancos aliados + garantía de devolución si no hay
>   aprobación.
> · Philosophy: Viventa acompaña, no vende un producto suelto.
> · Callout: *"Antes de soñar con el apartamento, hay una pregunta más aburrida."*
> · Auto-selecciona a: el que lleva años posponiendo por miedo al "no".
> · Por qué ahora: la objeción #4 está sin atender en el copy actual.
> · Slug: `VIV_BROWARD_202608_CALIFICAS`

**Test de distinción**
· Enemigos: la distancia · la desconfianza · la aritmética → PASA
· Sub-segmentos: quien vive en Broward / quien pospone por miedo / quien piensa en cifras → PASA
· Pruebas: dirección concreta / 5 bancos + garantía / testimonios de recompra → PASA

**4. Ángulos a evitar**
- *"Cumple tu sueño"*: saturado, sin enemigo, no auto-selecciona a nadie.
- Cualquier promesa de aprobación o de tasa: regla de marca + riesgo con Meta Financial Services.
- Anunciar el evento sin dirección: reactiva la fricción #1 del VOC.

**5. Hooks**
1. *"Vives en Broward y todos los eventos son en Miami. Este no."*
2. *"Antes de soñar con el apartamento, hay una pregunta más aburrida: ¿calificas?"*
3. *"Llevas 8 años mandando plata a Colombia. ¿Cuánto de eso es tuyo?"*
4. *"No, no necesitas venir a Colombia. Sí necesitas saber cómo te ven los bancos."*
5. *"'Es que en Colombia todo es robo.' Lo escuchamos todos los días. Por eso trabajamos con 5 bancos y no con promesas."*

**6. Qué no sabemos**
- Cuál de los 3 ángulos ya se corrió en Broward: `[sin evidencia]` — el histórico de creativos no
  está documentado. Lo valida la agencia de pauta antes de comprar.

Al entregar se dice: *"Insight de {territorio o tema} listo para revisión del responsable de marca: {n} ángulos, {n} hooks{, y las 15 piezas escritas con su variante B}. Anoté los ángulos como `sin probar` en el banco."*

## Skills relacionados

- **Antes:** `viventa-brief-evento` (territorio, fecha y dirección del evento).
- **Durante:** `ig-search` y `tt-search` para las señales de Instagram y TikTok; `ad-creative-generator`
  si la agencia pide variantes de copy por plataforma.
- **Después:** `viventa-guiones-evento` (los 5 videos del brief creativo) y `viventa-pack-evento` toman los ángulos y hooks;
  `viventa-comunicaciones-campana` usa los dolores en E1-E3; `viventa-qa-marca` revisa las piezas.
- **Referencias:** `references/copy-que-convierte.md` (investigación,
  ángulos, hooks, CTA, pruebas, test de mediocridad) · `references/dolores-y-verbatims.md` (matriz de dolores, escala Dolor·Miedo·Deseo,
  las 4 P, los 4 sub-segmentos con sus trampas, países elegibles, fuentes vivas) ·
  `references/banco-angulos.md` (registro, test de distinción, tipos de callout, ángulos vetados y
  semilla) · `references/contexto-viventa.md` (voz y reglas durables).
