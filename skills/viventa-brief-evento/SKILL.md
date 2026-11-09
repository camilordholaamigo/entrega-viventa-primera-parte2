---
name: viventa-brief-evento
description: >-
  Convierte una o varias solicitudes de evento de Viventa (WhatsApp, correo, nota) —presencial,
  virtual o híbrido— en el brief de 1 página que dispara la cadena de mercadeo: ficha, semáforo de 45
  días, tamaño del pack, bloqueantes con a quién pedirlos y, si se piden, metas con su supuesto.
  Investiga lo público (dirección del venue, festivos, hora en cada zona) en vez de preguntarlo, y
  con varios eventos arma la tabla del lote y el calendario de producción. USA cuando: "arma el brief del
  evento de Alicante", "nos pidieron un Viventa Más Cerca de Ti", "hay evento el 9 de mayo, qué
  necesitamos", "¿alcanzamos con este tiempo para el evento?". Es el primer paso del evento:
  viventa-brief-requerimiento hace lo mismo para pedidos que no son evento y viventa-pack-evento
  cuenta los archivos después de los guiones.
metadata:
  version: "2.0.0"
  domain: "process"
  client: "viventa"
  blueprint: "marketing"
  complexity: "standalone"
---

# viventa-brief-evento

Toma una solicitud de evento como llega en la vida real —un mensaje suelto, sin dirección, con la
fecha encima— y produce un documento de una página, legible en 90 segundos, con el que la gerente
de Growth decide si el evento arranca, con qué meta y qué se le pide a producción. Invariantes de
toda corrida: sin dirección exacta no hay brief completo, toda cifra lleva su línea `Supuesto:`, las
dos metas de negocio nunca se suman, y el skill no aprueba ni lanza nada.

> **Qué cambió en la v2 (feedback del equipo, noviembre de 2026).** *"No ayuda a hacer múltiples
> eventos al mismo tiempo"* → ahora detecta varios eventos y arma el lote (tabla, calendario de
> producción con choques, mapa de reuso). *"Evento virtual no deja cambiar la dirección"* → el
> formato (presencial · virtual · híbrido) es un eje aparte y en virtual la dirección se reemplaza por
> plataforma + enlace + hora en cada zona. *"Pide datos muy específicos que debería poder investigar…
> mezcla preguntas de CRM"* → la skill investiga lo público, y las metas (ejecutivas, presupuesto,
> conversiones) ya no frenan nada: salen solo si hay datos o si la gerente las pide. Todo en
> `references/virtual-y-lotes.md`.

## Cuándo usar

- Explícito: "arma el brief del evento de {ciudad}", "nos pidieron un evento nuevo en {ciudad}",
  "nos pidieron un Viventa Más Cerca de Ti en {ciudad}", "hay evento el {fecha}, qué necesitamos",
  "cuántos leads deberíamos poner de meta para este evento", "¿alcanzamos con este tiempo para el
  evento?", "qué falta para poder anunciar el evento", "Open House / ExpoColombia / Expo Casa en
  {ciudad}".
- Implícito: llega un mensaje de una consultora o de coordinación de eventos anunciando un evento, y
  todavía no se movió nada de guiones, pauta ni diseño.
- Cadencia: una vez por evento, apenas llega la solicitud. Es el paso 1 de la cadena
  `viventa-evento-completo`.

## Cuándo NO usar

- El pedido no es un evento (campaña de ads, kit comercial, landing, impreso) →
  `viventa-brief-requerimiento`.
- Ya hay brief y lo que falta es escribir los videos → `viventa-guiones-evento`; contar archivos y
  redactar el copy de cada pieza → `viventa-pack-evento`.
- El evento ya está al aire y la pregunta es cómo va → `viventa-monitor-evento`.

## Input del usuario

| Campo | De dónde sale | Si falta, preguntar |
|---|---|---|
| Texto de la solicitud | El usuario lo pega tal como llegó | *"Pégame el mensaje del evento tal como te llegó."* Sin texto no se arranca |
| **Formato**: presencial · virtual · híbrido | La solicitud | Se deduce de las palabras del mensaje (`references/virtual-y-lotes.md` §1). Si no se puede, se asume presencial y va al bloque 5 |
| Dirección exacta (venue, calle, número, ciudad, país) — **solo presencial o híbrido** | La solicitud, o **investigada** si viene el nombre del venue | **Bloquea el anuncio.** Si el mensaje trae el nombre del venue, la skill busca la dirección y la escribe con `[investigado — confirmar]`. Si no hay ni venue, va al bloque 5 para pedirla a quien propuso el evento |
| Plataforma + enlace de registro — **solo virtual o híbrido** | La solicitud | La plataforma **bloquea el anuncio**; el enlace **no**: va como `{LINK_REGISTRO}` (`virtual-y-lotes.md` §1.3). Nunca se pide dirección física a un virtual |
| Fecha del evento | La solicitud | **Bloquea.** Sin fecha no hay semáforo. Bloque 5, a quien propuso el evento |
| Hora de inicio y de cierre | La solicitud | **Bloquea el anuncio** (va en flyer, video y voz en off). Bloque 5, a quien propuso el evento |
| Modalidad (Open House · Expo Colombia · Expo Casa · Viventa Más Cerca de Ti) | La solicitud | En presencial, si el mensaje no la deja clara, va al bloque 5 (no frena el resto del brief). En virtual la modalidad es `Virtual (webinar / live)` |
| N.º de ejecutivas que atienden | La solicitud | **No se pregunta.** Es un dato de CRM: solo alimenta el bloque de metas, que es opcional (Paso 4). Si vienen consultoras, se deriva 1 por consultora |
| Consultoras asignadas (nombre y rol) | La solicitud | No bloquea. Sin ellas los flyers por consultora quedan en rango y se declara |
| Audiencia: ¿solo el país local o también colombianos en USA? | La solicitud | No bloquea. Presencial: país local, y el pack con dos monedas se muestra al lado como alternativa. Virtual: USA + España, declarado |
| Histórico de un evento comparable (misma modalidad, ciudad parecida) | El usuario, si lo tiene | No se pregunta. Sin histórico se usan los defaults de `references/formulas-y-defaults.md`, marcados |
| Presupuesto de pauta | La solicitud | **No se pregunta.** Si viene, se anota (lo leen `viventa-monitor-evento` y `viventa-forecast-pauta`); si no, el bloque 1 dice `NO INFORMADO` y nada más |
| Festivos, partidos, puentes que compiten con la fecha · comunidad colombiana de la ciudad | **Investigación** | No se pregunta: se busca (`virtual-y-lotes.md` §3.1) y va marcado `[investigado]` |
| Proyectos o constructoras que se presentan | La solicitud | No bloquea. Se anota como dato pendiente para guiones |
| Cifras canónicas (conversiones, ticket, CAC, CPL) | `references/cifras-canonicas.md` | No se pregunta. Si no está accesible, se usan las de `references/formulas-y-defaults.md` §3 y el bloque 3 lo declara |
| Fecha de hoy | El sistema | — |

La moneda **se deriva del país**, no se pregunta, con la tabla única de `references/naming-y-datamerge.md` §3. País fuera de esa
lista → se pregunta en el bloque 5.

**Varios eventos en el mismo mensaje:** modo lote (`references/virtual-y-lotes.md` §2) — tabla del
lote, calendario de producción con choques, mapa de reuso, una tarjeta por evento y un solo bloque de
datos por pedir agrupado por persona.

No se hace ronda de repreguntas en el chat: todo faltante que no sea el texto de la solicitud se
resuelve **dentro del output**, en el bloque 5.

## Workflow

### Paso 1 — Contar eventos, clasificar formato y modalidad, y extraer la ficha

0. Cuenta cuántos eventos trae el mensaje. Si son dos o más, modo lote (`virtual-y-lotes.md` §2):
   los pasos 1-5 se corren por evento y el Paso 6 arma el lote.
1. Lee la solicitud y extrae: formato, modalidad, ciudad, país, dirección (o plataforma), fecha,
   horario con zona, consultoras, quién lo propuso; y solo si vienen, n.º de ejecutivas y
   presupuesto de pauta.
2. La modalidad va primero porque modula meta, dirección esperada y origen. Las 4 y sus
   implicaciones están en `references/tipologias-evento.md` §1. No inventes una quinta; si la
   solicitud no encaja limpio en una, la modalidad queda faltante.
3. Marca el origen: **compañía** (Open House · Expo Colombia · Expo Casa) o **fuerza comercial**
   (Más Cerca de Ti). En Open House la dirección es la oficina: igual se escribe completa.

### Paso 2 — Investigar lo público y luego el gate de bloqueantes

0. Antes de marcar nada como faltante, busca lo que es público (`virtual-y-lotes.md` §3.1, una
   pasada corta): la dirección si vino el nombre del venue, cómo llegar, la hora en cada zona de la
   audiencia (virtual), los festivos o partidos que compiten con la fecha. Lo encontrado va con
   `[investigado — confirmar]`; si hay dos candidatos, no elijas: va al bloque 5.
1. Contrasta la ficha contra la lista cerrada de `references/tipologias-evento.md` §3.
2. **Virtual:** lo que bloquea es la plataforma y la fecha con hora y zona; el enlace va como
   `{LINK_REGISTRO}` y la dirección no existe. Nunca escribas `⛔ FALTA` en la dirección de un virtual.
3. **Presencial:** si falta la dirección exacta: el brief sale con lo que hay, `⛔ FALTA — bloquea el anuncio` en
   el campo y el mensaje para pedirla en el bloque 5. No escribas "por confirmar", "en Miami" ni un
   venue plausible. Barrio o ciudad sola cuentan como faltante.
4. Si falta la fecha: los bloques 2 y 3 dicen `NO CALCULABLE — falta fecha`; el resto sale.

### Paso 3 — Semáforo de anticipación

1. Calcula `días = fecha_evento − hoy`.
2. Pinta según `references/tipologias-evento.md` §2: **🟢 ≥ 45** · **🟡 30 a 44** · **🔴 < 30**.
   Bordes: 45 es verde, 44 es ámbar, 30 es ámbar, 29 es rojo. Si `días ≤ 0`, no hay semáforo:
   `⚠ la fecha ya pasó o es hoy — confirmar fecha` en el bloque 5.
3. Reporta tres cosas y nada más: los días reales, la consecuencia técnica esperada en CPL, y —si
   es ámbar o rojo— las salidas posibles con su costo. En rojo son siempre tres: correr asumiendo
   CPL alto · mover la fecha · recortar a una sola ciudad. El skill no elige.

### Paso 4 — Metas (opcional), con el supuesto pegado al lado

**Este bloque solo sale si** el mensaje trae el n.º de ejecutivas o consultoras, o el usuario pide
"con metas", o quien corre la skill es la gerente de Growth pidiendo el brief para decidir. Si no,
el bloque 3 dice una sola línea: `Metas: no calculadas — este brief es para producir. Para sumarlas,
corre de nuevo con "con metas" y el n.º de ejecutivas.` **Nunca** se frena el brief ni se pregunta por
datos de CRM para llegar a una meta (`virtual-y-lotes.md` §3.2).

1. Carga `references/formulas-y-defaults.md` §1-§3 y calcula:
   ```
   leads_atendibles = N_ejecutivas × leads_por_ejecutiva_dia    (default 20, [⚠ no verificado])
   meta_registros   = leads_atendibles ÷ (1 − tasa_no_show)     (default 40 %)
   ```
2. Si hay histórico comparable, **gana sobre el benchmark** y la línea `Supuesto:` dice cuál se usó.
3. Si falta `N_ejecutivas` y la solicitud trae consultoras: deriva 1 ejecutiva por consultora,
   decláralo en la misma línea y pide confirmación a la gerente de Growth. Sin consultoras: `NO
   CALCULABLE — falta N.º de ejecutivas`. Ninguna otra estimación.
4. El ajuste por modalidad de §1 es heurística sin validar: solo se aplica si el usuario lo pide, y
   se declara en `Supuesto:`.
5. La meta de negocio va en **dos líneas separadas**, redondeadas hacia abajo a enteros:
   ```
   Pago de Servicio Viventa: meta_registros × [cifra interna] ≈ {n} pagos ≈ USD {n × 480}
   Desembolso / Vive Casas:  meta_registros × [cifra interna]  ≈ {n} cierres
   ```
6. Debajo, la referencia de costo etiquetada: CAC fully-loaded del canal eventos ([cifra interna] **por
   cierre**) y CPL Meta (≈ [cifra interna] **por lead**), ambos de `references/cifras-canonicas.md`.
7. Si el país ≠ USA, el KPI "85 % de leads desde USA" **no aplica**: dilo y pide la meta de
   composición correcta a la gerente de Growth en el bloque 5.

### Paso 5 — Dimensionar el pack

1. Aplica el conteo de `references/formulas-y-defaults.md` §4. Son **entregables**; los archivos
   exactos (slides, formatos) los cuenta `viventa-pack-evento`.
2. Si no se sabe si el evento se promociona a una o dos audiencias, calcula con la audiencia local
   (1 moneda), escribe al lado cuánto sería con dos, y la pregunta va al bloque 5. No asumas 2.
3. Reporta total de entregables, unidades de copy reales (ciudad y consultora multiplican copy;
   moneda y formato no) y la carga: ~5 días sobre una productora de video y una diseñadora.
4. **Virtual:** sin flyers por consultora ni placa de dirección; en su lugar, placa de plataforma y
   hora por zona, y 1-2 piezas de recordatorio ("hoy", "estamos en vivo"). Si es global, sin monedas.

### Paso 6 — Ensamblar y chequear

1. Arma la página con los 5 bloques del Output esperado, en ese orden. En **modo lote**, primero la
   tabla del lote, el calendario de producción (con choques marcados y sus salidas posibles) y el
   mapa de reuso; después una tarjeta por evento; al final un único bloque 5 agrupado por persona.
2. Antes de entregar, busca en el texto: *aprobar, lanzar, activar, cancelar, recomiendo, listo
   para producción*; *tasa, cuota, plazo, cuota inicial*; cualquier cifra sin `Supuesto:`. Si
   aparece algo, reescribe.
3. Cada faltante del bloque 5 dice qué dato es y a quién se le pide, y **solo pide lo que no se pudo
   investigar** (fecha y hora reales, venue confirmado, consultoras, montos que van en la pieza). Nada
   de CRM: ni ejecutivas, ni presupuesto, ni conversiones, salvo que el usuario haya pedido metas.

## Reglas duras

- **Nunca escribe una dirección que no esté en la solicitud ni en una fuente investigada y marcada**
  (`[investigado — confirmar]`), ni "por confirmar" / "TBD". **Nunca pide dirección física a un
  evento virtual.** Si no hay ni venue ni dirección, el campo
  dice `⛔ FALTA — bloquea el anuncio`.
- **Nunca deja una cifra sin su línea `Supuesto:`.** Sin insumo, el campo dice `NO CALCULABLE — falta
  {dato}` y el dato se pide en el bloque 5.
- **Nunca usa un verbo de decisión** (*aprobar, lanzar, activar, cancelar, recomiendo cancelar,
  listo para producción*). Permitido: "Salidas posibles: A · B · C. No es decisión del skill".
  Prohibido: "Recomiendo mover la fecha".
- **Nunca suma ni promedia** pago de servicio ([cifra interna]) y desembolso ([cifra interna]). Van en dos líneas.
- **Nunca escribe CPL o CAC sin su nombre y unidad** ("por lead", "por cierre").
- **Nunca menciona tasa, cuota, plazo ni cuota inicial, ni promete aprobación o valorización.** Si
  la solicitud los trae, la línea va marcada `requiere visto bueno de Crédito` y no se propaga.
- **Nunca suaviza ni dramatiza el semáforo**: días reales y consecuencia técnica, sin adjetivos.
- **Nunca incluye teléfonos, correos ni datos de leads.** De las consultoras, solo nombre y rol.
- **Nunca borra un `[⚠ no verificado]` heredado**, sobre todo el de 20 leads por ejecutiva-día.

## Output esperado

No escribe archivos. Devuelve en la conversación una página en markdown, copiable a Docs, con esta
forma:

```
# BRIEF DE EVENTO — {Modalidad} · {Ciudad, País | Virtual}
Solicitado por: {rol} · Origen: {compañía | fuerza comercial} · Fecha del brief: {hoy}

## 1. Ficha
Formato: {presencial | virtual | híbrido} · Modalidad · Ciudad / país (o audiencia, si es virtual) ·
Presencial: dirección exacta (o ⛔ FALTA — bloquea el anuncio, o [investigado — confirmar]) · cómo llegar
Virtual: plataforma · {LINK_REGISTRO} · hora en cada zona de la audiencia
Fecha y hora · Consultoras asignadas ({n}) · Ejecutivas ({n} | no informado) ·
Presupuesto de pauta: {n} | NO INFORMADO · Riesgos de fecha investigados (festivos, partidos)

## 2. Semáforo de anticipación
{🟢|🟡|🔴} {COLOR} — {n} días de anticipación (mínimo: 45).
Consecuencia esperada: {consecuencia técnica en CPL}
Salidas posibles: {A} · {B} · {C}. No es decisión del skill.

## 3. Metas (con supuesto declarado)
Meta de leads: {n} registros
  Supuesto: {parámetros y origen, con los [⚠ no verificado] visibles}
Composición objetivo: {85 % USA, o la advertencia si el país no es USA}
Meta de negocio:
  Pago de Servicio Viventa: {n} × [cifra interna] ≈ {n} pagos ≈ USD {n} (ticket SV USD 480)
  Desembolso / Vive Casas:  {n} × [cifra interna]  ≈ {n} cierres
  Supuesto: {cohort}. Son dos KPI distintos, no se suman.
Referencia de costo: CAC eventos [cifra interna] por cierre · CPL Meta ≈ [cifra interna] por lead. CPL ≠ CAC.

## 4. Pack de producción
Videos · Carruseles · Estáticas · Flyers ({n} consultoras × A3/A4/redes)
Total entregables · Unidades de copy reales · Carga · Voz en off por ciudad y fecha

## 5. Bloqueantes y datos por pedir
⛔ BLOQUEA EL ANUNCIO
1. {dato} → pedir a: {rol}
Por pedir (no bloquea)
2. {pregunta} → de eso depende {consecuencia concreta}
3. Presupuesto de pauta aprobado (si NO INFORMADO) → pedir a: gerente de Growth; lo usan el monitor y el forecast

Este documento no aprueba ni lanza nada. Decide la gerente de Growth.
```

Ejemplo recortado (solicitud por WhatsApp: *"Vamos a hacer un Viventa Más Cerca de Ti en Alicante
el 9 de mayo. Van tres consultoras. ¿Nos ayudan con las piezas?"*, brief del 27-abr):

```
Dirección exacta: ⛔ FALTA — bloquea el anuncio
Ejecutivas que atienden: 3 ⚠ supuesto — 1 por consultora asignada, confirmar con la gerente de Growth
🔴 ROJO — 12 días de anticipación (mínimo: 45).
  Consecuencia esperada: la pauta no sale de la fase de aprendizaje de Meta (~50 conversiones por
  conjunto en 7 días). CPL por encima del objetivo en la semana crítica.
Meta de leads: 100 registros
  Supuesto: 3 ejecutivas × 20 leads/ejecutiva-día [⚠ benchmark de trade shows B2B, no histórico
  Viventa] = 60 ÷ 0.6 (40 % de no-show) ≈ 100.
Composición: ⚠ el KPI de 85 % USA no aplica — el evento es en España.
  Pago de Servicio Viventa: 100 × [cifra interna] ≈ 2 pagos ≈ USD 960
  Desembolso / Vive Casas:  100 × [cifra interna]  ≈ 3 cierres
Pack: 10 videos (1 moneda: EUR) · 3 carruseles · 2 estáticas · 9 flyers = 24 entregables
  (34 si también se promociona en USA)
```

Al terminar se le dice al usuario: *"Brief listo. Bloquean el anuncio: {n} datos (bloque 5). Con la
dirección confirmada sigue `viventa-guiones-evento`."* Si no hay bloqueantes: *"Brief completo:
sigue `viventa-guiones-evento`."*

## Skills relacionados

- **Después:** `viventa-inteligencia-campana` para el insight de entrada y `viventa-guiones-evento`
  para los 5 videos; los dos leen este brief.
- **Después:** `viventa-pack-evento` toma este brief (escala, consultoras) y los guiones.
- **En vez de este:** `viventa-brief-requerimiento` cuando el pedido no es un evento.
- **Referencias:** `references/virtual-y-lotes.md` (virtual, híbrido, varios eventos, qué se investiga y
  qué no se pregunta) · `references/tipologias-evento.md` (modalidades, semáforo, bloqueantes) ·
  `references/formulas-y-defaults.md` (fórmulas, defaults con su confianza, cifras canónicas,
  conteo del pack).
