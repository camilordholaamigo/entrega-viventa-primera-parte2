---
name: viventa-comunicaciones-campana
description: >-
  Redacta la secuencia de email + WhatsApp de una campaña de Viventa —un evento presencial, virtual
  o híbrido, o una campaña general de interés (compra conjunta, proyectos con mejores tasas, un
  lanzamiento, un programa)— con copy persuasivo investigado, un CTA claro por pieza, variantes A/B
  para probar y bitácora para iterar. Segmentación que protege el dominio. Markdown listo para Zoho
  Campaigns y Wati; redacta, no envía. USA cuando: "ármame los correos del evento de {ciudad}", "la
  secuencia de emails y WhatsApp para el webinar", "campaña de correos para compra conjunta", "el
  correo de después del evento". Es a un segmento: viventa-parrilla-base planifica el mes de toda la
  base y viventa-reactivacion-lead reabre leads caídos.
metadata:
  version: "2.0.0"
  domain: "campaign"
  client: "viventa"
  blueprint: "marketing"
  complexity: "standalone"
---

# Comunicaciones de campaña de evento — Viventa

Un solo markdown, copiable a **Zoho Campaigns** y **Wati** sin retocar, con la secuencia de una
campaña: la convocatoria y el post-evento de un evento (presencial, virtual o híbrido), o una
campaña general de interés. Invariantes de toda corrida: **redacta, no envía** (regla durable #1 del
cliente: nada sale al aire sin revisión humana); ninguna pieza se dirige a "toda la base"; ninguna
pieza de evento sale sin fecha, hora y dirección (o plataforma) reales; **toda pieza pasa el
estándar de copy** (`references/copy-que-convierte.md`) y sale con su variante para probar.

> **Qué cambió en la v2 (feedback del equipo, noviembre de 2026).** El equipo dijo que el copy de
> correo y WhatsApp era *"súper mediocre"* y pidió que fuera *"persuasivo, iterativo, algo que estemos
> testeando constantemente, con llamados a la acción claros"*, que sirviera *"para campañas generales,
> como campañas de interés"* y que funcionara con eventos virtuales. Por eso: (1) antes de escribir se
> investiga qué está funcionando (§1 del estándar); (2) cada pieza lleva hook trabajado, un CTA y una
> variante B; (3) hay un modo **campaña general** sin fecha de evento; (4) los eventos virtuales
> cambian dirección por plataforma y enlace; (5) el conteo de contactos de Zoho dejó de bloquear: es
> un dato de CRM y no hace falta para escribir; (6) el copy al cliente va en **tuteo**, nunca en voseo.

Por qué la segmentación manda: email es el canal más barato de la compañía (CAC fully-loaded [cifra interna]
contra [cifra interna] de Meta y [cifra interna] de un evento, `references/cifras-canonicas.md`) y el dominio **ya cae en spam**. La
deliverability no es un apéndice: decide a cuánta gente se le escribe. El problema no son los 250K
registros; es escribirles a los 250K.

**Una sola pasada**: las cuentas de IA del cliente son compartidas y se quedan sin créditos hasta 4
horas. La investigación es **una sola y acotada** (máximo 6 búsquedas, `references/copy-que-convierte.md`
§1); sin loops, sin rondas de preguntas.

## Cuándo usar

- Explícito: "ármame los correos del evento de {ciudad}", "la secuencia de emails y WhatsApp para el
  evento", "haz la campaña de correos del Más Cerca de Ti", "los mensajes de Wati para el evento y
  los correos de Zoho", "el correo de después del evento, para los que fueron y los que no", "a quién
  le mandamos esta convocatoria sin quemar el dominio".
- Explícito, campaña general: "campaña de correos para compra conjunta", "WhatsApp para los
  proyectos con mejores tasas", "secuencia para la campaña de {tema}", "mejora estos correos".
- Implícito: hay un brief de evento cerrado (`viventa-brief-evento`) o un tema de campaña del mes, y
  faltan las comunicaciones a la base propia.
- Cadencia: una vez por evento o campaña, idealmente antes de D-18; el bloque post-evento se completa
  en D+1 con la lista de asistentes.

## Los tres modos (se detectan del pedido; no se pregunta)

| Modo | Cuándo | Arquitectura |
|---|---|---|
| **Evento presencial** | Hay venue y fecha | La de 21 días de abajo (Paso 3) |
| **Evento virtual o híbrido** | "webinar", "live", "por Zoom", "online" | La de 21 días con los cambios de `references/virtual-y-lotes.md` §1: plataforma + `{LINK_REGISTRO}` en vez de dirección, hora en cada zona, recordatorios 24 h · 1 h · "estamos en vivo" (Paso 3-bis) |
| **Campaña general** | Un tema sin evento: compra conjunta, proyectos con mejores tasas, lanzamiento de un proyecto, Mi Casa en Colombia, temporada de primas, una campaña de interés | 14 días, objetivo de conversación o de llamada (Paso 3-ter) |

Si el pedido trae **varios eventos**, se arma una sola secuencia por audiencia con un bloque de ficha
por evento, siguiendo `references/virtual-y-lotes.md` §2 (nadie recibe la
convocatoria de dos eventos de la misma semana).

## Cuándo NO usar

- El plan del mes para toda la base, o la bienvenida a leads nuevos que no se registraron a ningún
  evento → `viventa-parrilla-base`.
- Reabrir la conversación con un lead que se cayó (incluido un no-show puntual, 1 a 1) →
  `viventa-reactivacion-lead`.
- El copy de flyers y piezas gráficas del evento → `viventa-pack-evento`.

## Input del usuario

| Campo | De dónde sale | Si falta |
|---|---|---|
| **Modo** (presencial · virtual/híbrido · campaña general) | El pedido | Se deduce. Solo si es imposible: *"¿Es para un evento o una campaña sin evento?"* |
| **Tema o tema(s) de la campaña** (campaña general) | El pedido ("compra conjunta", "proyectos con mejores tasas") | **Bloquea solo la campaña general**. Es lo único que se pregunta |
| Nombre del evento | Brief del evento o el usuario | Si falta, se nombra con modalidad + ciudad ("Viventa Más Cerca de Ti · Valencia") y se marca para confirmar |
| Fecha con día de la semana | Brief del evento | **Bloquea el modo evento** |
| Hora de inicio y de cierre, con zona horaria | Brief del evento | **Bloquea el modo evento**. En virtual, la skill calcula la hora en cada zona |
| Dirección exacta (presencial) | Brief del evento | **Bloquea el modo presencial**. Si viene solo el nombre del venue, se investiga la dirección y va `[investigado — confirmar]`. Nunca se aproxima |
| Plataforma (virtual) | Brief del evento | **Bloquea el anuncio virtual**. El enlace NO bloquea: va como `{LINK_REGISTRO}` |
| Cómo llegar (transporte o parqueadero) | Brief o investigación | **No bloquea**: se investiga (página del venue / mapa) y va `[investigado — confirmar]` |
| País (o países) de la audiencia | Brief | Se deduce de la ciudad; en virtual o campaña general, default **USA + España**, declarado. Define moneda, prefijo y si WhatsApp está habilitado |
| Conteo estimado por tier y geo | Responsable de email marketing (Zoho) | **No se pregunta y no bloquea.** Es un dato de CRM: la skill escribe la regla de segmentación y deja la columna "Conteo (lo pone Zoho)" vacía para que la llene quien programa |
| Nombre de la asesora que firma los WhatsApp | Brief | No bloquea: va `{ASESORA}` como campo de data merge, listado en "Antes de programar" |
| Ángulo | `viventa-inteligencia-campana`, el brief o el tema | No se pregunta: se generan ángulos con `references/copy-que-convierte.md` §2 y se dice cuál usa cada pieza |
| Referentes que le gustan al equipo (capturas, enlaces, correos de otros) | El usuario | No se pregunta. Si los pega, la investigación empieza por ahí |
| Bitácora de pruebas anterior | El usuario (la tabla que cierra el entregable anterior) | No se pregunta. Si viene, la variante ganadora pasa a ser el control (`copy-que-convierte.md` §6) |
| Fecha de arranque de la pauta | Brief | No bloquea: sin ella, "regla de 45 días no verificada" |
| Lista de asistentes (post-evento) | Registro del evento, D+1 | Bloquea solo E5/W5; el resto sale |
| Notas de mesa (post-evento) | Asesoras, D+1 | No bloquea: E5-A sale con la advertencia |
| Testimonios de la geografía | `references/voice-of-customer-90d.md` | No se pregunta. Si no hay de esa geografía, se dice y se usa prueba dura |
| Remitente de los correos | `[correo remitente]` | No bloquea: `De: Viventa <{REMITENTE}>` marcado para completar |
| Fecha de hoy | El sistema | — |

Si falta un bloqueante, sale todo lo que no depende de él y al final un **solo bloque "Lo que falta"**
con un mensaje por persona (`virtual-y-lotes.md` §3.3). La fricción #1 del Voice of Customer sigue
mandando: *"no entiendo por qué no pueden decir la ubicación donde van a atender en Miami, solo
colocan 28 y 1 pero sin dirección"* — por eso ninguna pieza de evento se **programa** sin dirección o
enlace; pero la redacción no espera.

## Workflow

### Paso 0 — Investigar qué está funcionando y elegir ángulos (una pasada)

1. Corre `references/copy-que-convierte.md` §1: máximo 6 búsquedas, empezando por los referentes que
   haya pegado el usuario. Anota cada referente con URL; lo que no se abrió no se cita.
2. Con §2, saca **al menos 4 ángulos** del tema o del evento y asigna uno distinto a cada correo de
   convocatoria (además de su P). Si el pedido trae dos temas (p. ej. *compra conjunta* y *proyectos
   con mejores tasas*), reparte los ángulos entre los dos y propón una pieza libre, rotulada.
3. Si vino una bitácora anterior, leela: lo que ganó es el control de esta corrida.

### Paso 1 — Chequear la anticipación de 45 días

1. Calcula los días entre el arranque de la pauta y el evento.
2. Si son **menos de 45** (44 incluido), el paquete sale igual, con esta advertencia arriba:
   *"⚠️ La pauta arrancó a N días del evento (mínimo sano: 45). No alcanza a pasar aprendizaje: el
   CPL va a salir por encima del histórico y esta secuencia va a cargar más peso del que le toca."*
   45 exactos o más: sin advertencia. Sin fecha de pauta: "regla de 45 días no verificada".
3. Los 45 días son requisito de la pauta, no de la secuencia: la secuencia vive en los últimos 21.

### Paso 2 — Segmentar antes de escribir

Tres capas que se cruzan (detalle: `references/segmentacion-deliverability.md`):

1. **Geografía**: solo contactos a **≤90 minutos** de la ciudad del evento.
2. **Engagement**: Tier 1 (abrió ≤90 días) recibe todo · Tier 2 (91-180 días) recibe **solo E1 y E3**
   · Tier 3 (+180 días) **no recibe nada** de esta secuencia y se desvía a re-permiso o Meta · Tier 4
   (rebote/queja) supresión permanente. Un contacto en el día 90 es Tier 1; en el 180, Tier 2.
3. **Estado**: registrados y no registrados nunca reciben la misma pieza.

Escribe el **criterio** de cada capa tal como se filtra en Zoho; la columna "Conteo" queda para que
la llene quien programa (es un dato de CRM y no se le pide al equipo creativo). Orden de magnitud de
referencia de una convocatoria de ciudad: 2.000-6.000 contactos `[⚠ no verificado — estimación]`.
Si al filtrar Tier 1 + Tier 2 da 0, la secuencia no se envía y se pasa a re-permiso o Meta.
En **virtual** la capa geográfica cambia: no es "≤90 minutos de la ciudad" sino **país y zona
horaria** de la audiencia. En **campaña general**, la geografía la define el tema (un proyecto en
Pereira se le ofrece a quien mostró interés en el Eje Cafetero, si Zoho lo tiene; si no, a todo Tier 1).

### Paso 3 — Armar el calendario

Arquitectura fija en 21 días (D-18 a D+3). **Regla de conteo única** (tabla, checklist y total del
entregable): se cuenta cada pieza redactada, y las variantes A/B cuentan por separado → **8 emails**
(E0-A, E0-B, E1, E2, E3, E4, E5-A, E5-B) **+ 6 WhatsApp** (W1, W2, W3, W4, W5-A, W5-B). En USA, +E4-bis
= 9 emails. Ningún contacto recibe todas: el tope por persona lo controla el punto 4.

| # | Día | Canal | Segmento | P |
|---|---|---|---|---|
| E0-A | inmediato al registro | Email | Quien se registra | Plan |
| E0-B | +48 h del registro | Email | Registrado que abrió E0-A | Proof |
| E1 | D-18 | Email | Tier 1 + Tier 2, geo | Pain |
| W1 | D-16 | WhatsApp | Abrió o clicó E1 | Pain → Plan |
| E2 | D-10 | Email | Tier 1, no registrados | Plan |
| E3 | D-4 | Email | Tier 1 + Tier 2, no registrados | Proof |
| W2 | D-3 | WhatsApp | Abrió E2 o E3 | Proof |
| E4 | D-2 | Email | Solo registrados | Philosophy + logística |
| W3 | D-1 | WhatsApp | Solo registrados | — |
| W4 | D, 08:00 | WhatsApp | Solo registrados | — |
| E5-A | D+2 | Email | Asistieron | Plan |
| E5-B | D+2 | Email | Registrados que no fueron | Proof → Plan |
| W5-A/B | D+3 | WhatsApp | Idem, split | — |

1. Corre E1, E2 y E3 al martes, miércoles o jueves más cercano, manteniendo 6-8 días entre correos
   de convocatoria.
2. **E0-A y E0-B se disparan por registro, no por fecha.** Quien se registra en D-14 recibe E0-A ese
   día y E0-B a las 48 h. Si faltan **menos de 5 días** al registrarse, E0-B no sale (se solapa con
   E3/E4 y satura); con 5 días exactos, sale.
3. **Supresión al registrarse**: quien se inscribe sale de convocatoria y pasa al carril de
   registrados (E0-A → E0-B → E4 → W3 → W4).
4. **Tope por persona: 4 emails + 3 WhatsApp en 21 días**, contando todo lo que Viventa le mande.
   Se chequea contra el contacto antes de cada envío. Nunca caen E0-A, W4 ni E4-bis (logística). Excepción explícita: en USA, E4-bis no cuenta para el tope, así que un contacto puede recibir 5 emails si el quinto es E4-bis.
   Para que el carril de quien se registra a mitad de convocatoria no lo rompa:
   - Quien recibió E1 antes de registrarse **no recibe E0-B**.
   - Quien recibió E0-A hace 5 días o menos **no recibe E4** (E0-A ya llevó la logística).
   - Quien recibió W1 y W2 **no recibe W3**; quien recibió W1 o W2 **no recibe W5**.
   - Si aun así E5-A/E5-B pasaría el tope, E5 no sale y el contacto queda para
     `viventa-parrilla-base` del mes siguiente.
   Ejemplo: Tier 1 que recibió E1 + W1 y se registra en D-12 → E1, E0-A, E4, E5 (4 emails) y W1, W3,
   W4 (3 WhatsApp).
5. Por qué el Proof va en D-4: en esta categoría el miedo (a ser estafado, a perder la plata) pesa
   más que el dolor, y D-4 es justo antes del cierre de registro.
6. Por qué W4 a las 08:00 y no "1 hora antes": es presencial; quien no salió de la casa a las 9 no
   sale a las 9:50.

**Si el evento es en USA:**
- Moneda `USD` (nunca "USA"), prefijo +1, testimonios de clientes en USA.
- Las 5 piezas de WhatsApp se redactan igual, encabezadas con `🚫 NO ENVIABLE HOY — canal WhatsApp
  USA bloqueado (en gestión vía 10DLC/Twilio)`. Sirven de guion para las llamadas y quedan listas
  para cuando se desbloquee.
- Se agrega **E4-bis**: recordatorio de 24 h por email, menos de 80 palabras, solo dirección, hora y
  mapa. La secuencia queda en **9 emails**.

### Paso 3-bis — Evento virtual o híbrido (cambios sobre el Paso 3)

Misma arquitectura de 21 días, con estos cambios (`references/virtual-y-lotes.md` §1):
- **E0-A** lleva plataforma, `{LINK_REGISTRO}` o `{LINK_TRANSMISION}`, la hora en la zona de quien lee
  y "agrégalo a tu calendario". Nada de "qué llevar" ni "cómo llegar".
- **W3** pasa a **24 h antes**, **W4** a **1 hora antes**, y se agrega **W4-bis "estamos en vivo"**
  (10 minutos después de empezar, a los registrados). W4 y W4-bis son logística: no cuentan para el tope.
- **E4** deja de ser logística de llegada: es "lo que vas a poder preguntar en vivo" + quién responde.
- **Post-evento:** E5-A (se conectó) lleva el siguiente paso y la grabación si existe; E5-B (no se
  conectó) lleva la grabación o la próxima fecha. Sin grabación confirmada, no se promete.
- **Híbrido:** dos carriles de registrados (presencial y virtual) que no reciben la misma pieza de
  logística.

### Paso 3-ter — Campaña general (sin evento)

Arquitectura de **14 días**, objetivo: **conversación** (respuesta en WhatsApp o palabra clave) o
**llamada agendada** con una asesora. Conteo: **5 correos + 4 WhatsApp**, con variante B de cada uno.

| # | Día | Canal | Segmento | Función |
|---|---|---|---|---|
| C1 | D0 | Email | Tier 1 + Tier 2 del interés | Hook + dolor o miedo del tema. CTA de bajo esfuerzo (responder, pedir la guía) |
| M1 | D2 | WhatsApp | Abrió o clicó C1 | Pregunta que se responde en una palabra |
| C2 | D4 | Email | Tier 1 que no respondió | Plan: cómo funciona exactamente (p. ej. cómo queda cada uno en la escritura en una compra conjunta) |
| C3 | D7 | Email | Tier 1 + Tier 2 que no respondió | Proof: caso real o prueba dura |
| M2 | D8 | WhatsApp | Abrió C2 o C3 | Caso en una línea + pregunta |
| C4 | D10 | Email | Tier 1 que clicó algo y no agendó | Las 3 objeciones que más se repiten (VOC o comentarios investigados) |
| M3 | D11 | WhatsApp | Clicó C4 | Invitación directa a llamada de 15 min, con dos horarios |
| C5 | D14 | Email | Tier 1 que no respondió | Cierre honesto: "si no es el momento, te escribimos en {mes}" + CTA final |
| M4 | D14 | WhatsApp | Respondió algo en M1-M3 y no agendó | Seguimiento 1 a 1 de la asesora |

- Si el tema tiene una fecha real (cierre de un programa, preventa con fecha), esa fecha es la
  urgencia y la secuencia se ancla a ella. Sin fecha real, **no se inventa urgencia**.
- Tope por persona: **3 correos + 2 WhatsApp** en los 14 días. Quien responde o agenda sale de la
  secuencia y pasa a la asesora.
- **"Proyectos con las mejores tasas"**: se escribe el proceso ("comparamos 5 bancos para buscarte la
  mejor tasa disponible para tu perfil"), nunca un número de tasa ni "la tasa más baja del mercado"
  (`copy-que-convierte.md` §7.7).

### Paso 4 — Redactar los emails

Cada uno con argumento propio:

- **E0-A** — transaccional: dirección exacta, fecha, hora, qué llevar, a quién escribir. Sin venta.
  Es el correo que más se abre: no lo gastes en un "gracias por registrarte".
- **E0-B** — qué va a pasar en el evento y quién atiende, más una prueba dura. Baja el no-show.
- **E1 · Pain** — la cuenta del alquiler que se evapora + los tres obstáculos concretos (papeles que
  no se gestionan por teléfono, la familia que no tiene por qué hacer la vuelta, vacaciones que
  cuestan más que la cuota inicial). Dolor verificable, no adjetivos.
- **E2 · Plan** — el proceso minuto a minuto de la mesa, con *"aunque no nos convenga"* y *"si no
  quieres seguir, no te llamamos"* (responden la fricción #2 del VOC: *"siempre hay que colocar
  correo y luego te saturan"*).
- **E3 · Proof** — testimonios literales del VOC de la misma geografía, luego prueba dura: 20 años,
  oficinas propias en 3 países, 5 bancos, ~20 constructoras.
- **E4 · Philosophy + logística** — la diferencia de Viventa: se mueve físicamente, mientras los ~8
  brokers autorizados usan el mismo ángulo de "raíces". Pegado a la logística.
- **E5-A / E5-B** — dos correos distintos, nunca uno con condicional. E5-A usa las notas de mesa.

El Deseo no ocupa correo propio: se cuela como cierre ("que quede algo a tu nombre en Colombia").

Cada correo se escribe con el estándar (`references/copy-que-convierte.md` §3-§5): **3 asuntos
candidatos → se elige 1 y otro queda como variante B**; el primer párrafo es un hook, no un saludo;
el cuerpo dice una sola cosa; el botón dice lo que pasa al hacer clic (*"Quiero saber si califico"*,
*"Reservar mi cupo"*), nunca "Haz clic aquí" ni "Más información". Una **P.D.** con la prueba o la
fecha es opcional y suele ser lo más leído después del asunto.

Formato de cada email: bloque `De: Viventa <[correo remitente]>` · `Asunto:` 4-8 palabras, en
minúscula natural · `Preheader:` una línea que complementa sin repetir. Cuerpo de 150-280 palabras,
fecha/hora/dirección en bloque destacado, **un** CTA en botón, máximo 3 enlaces al dominio propio sin
acortadores, pie con razón social, dirección postal física y baja en un clic. Lista negra de asuntos:
`references/segmentacion-deliverability.md`.

### Paso 5 — Redactar los WhatsApp

1. Máximo **60 palabras** (lo ideal, 30-45), con el conteo impreso al lado del título de la pieza.
2. **La primera línea es el hook** (es lo que se ve en la notificación): nunca "Hola, ¿cómo estás?"
   ni "Te escribo de Viventa". Ejemplo: *"Marcela, ¿sigues pensando en Pereira? Te tengo algo."*
3. Primera persona con el nombre de la asesora (`{ASESORA}`), **una sola pregunta al final que se
   responda en una palabra o con un botón** ("¿Medellín o Pereira?", "Responde SÍ y te llamo
   mañana"), emojis funcionales (📍🕙📲), no decorativos.
4. **Variante B** de cada WhatsApp que cambia solo la primera línea o solo la pregunta final. La
   métrica que decide es la **tasa de respuesta**.
5. Reglas de orquestación: **48 h mínimo** entre un correo y el WhatsApp que lo sigue · W1 y W2 solo
   a quien abrió o clicó · W3 y W4 solo a registrados · el tope por persona y sus supresiones son los
   del Paso 3, punto 4.

### Paso 6 — Armar el entregable y autovalidar

1. Arma el documento con la forma de `## Output esperado`.
2. Corre el checklist de abajo. Si una casilla falla, corrige antes de entregar; si falla un dato
   bloqueante, vuelve a la lista de faltantes.

Checklist:
- [ ] Ficha con fecha y día, horario con zona, dirección exacta, cómo llegar, moneda, prefijo y
      estado de WhatsApp coherentes con el país (moneda `USD`, nunca "USA"; en España, ningún +1).
- [ ] 8 emails + 6 WhatsApp según la regla de conteo (9 emails en USA); E0-B ausente si faltan <5 días.
- [ ] Cada pieza con fecha exacta, canal, segmento y, en convocatoria, su P.
- [ ] Nadie recibe más de 4 emails + 3 WhatsApp en 21 días (supresiones del Paso 3, punto 4, aplicadas); 48 h
      entre correo y WhatsApp.
- [ ] Los primeros párrafos de E1-E4 no comparten más de 5 palabras significativas.
- [ ] Ningún WhatsApp supera 60 palabras; ningún asunto con mayúscula sostenida, emoji o signo repetido.
- [ ] Cada pieza pasa el test de mediocridad (`references/copy-que-convierte.md` §7) y tiene su
      variante B que cambia una sola cosa; ningún ángulo se repite en dos correos de convocatoria.
- [ ] Un CTA por pieza, con verbo y con lo que pasa después.
- [ ] Copy al cliente en tuteo: cero formas de voseo (terminadas en -ás, -és, -ís como «pod-és» o «calific-ás»).
- [ ] Bloque "Referentes que usé" con URL (o la línea de investigación no disponible) y bitácora al final.
- [ ] Cero tasa, cuota, plazo, aprobación, cupos no verificados.
- [ ] Todo correo con razón social, dirección postal física, baja en un clic y máximo 3 enlaces al
      dominio propio, sin acortadores.
- [ ] Split post-evento A/B presente, o el bloque dice que falta la lista de asistentes.
- [ ] Advertencia de revisión humana arriba y bloque de alcance al final.

## Reglas duras

- **Nunca envía ni programa** un correo o un WhatsApp. Termina en un documento para revisión.
- **Nunca deja un placeholder de dato del evento.** Prohibido: `[Ciudad]`, `[fecha]`, `[dirección]`,
  "por confirmar". Permitido: los campos de sistema que Zoho resuelve (`[Darme de baja]`,
  `[Dirección postal]`, `[enlace de Google Maps]`) y `[Nombre de la consultora]` si está listado como
  pendiente en la ficha.
- **Nunca menciona tasa, cuota ni plazo con cifra, ni promete aprobación.** Permitido: *"revisas si
  calificas"*, *"comparamos 5 bancos para buscarte la mejor tasa disponible para tu perfil"*. "Mi Casa en Colombia" se nombra como programa existente, nunca como aprobación.
- **Nunca inventa cupos ni urgencia** ("últimos cupos", "última oportunidad") sin un número
  verificado.
- **Nunca incluye en una pieza a Tier 3 ni a Tier 4**, ni produce un envío sin las 3 capas escritas.
- **Nunca nombra el miedo** ("no te vamos a estafar"): se desactiva con prueba.
- **Nunca usa registro de banco**: "Estimado cliente", "nos complace invitarle", "aliado
  estratégico", "solución integral", "no dude en contactarnos". Español de Colombia, **tuteo** (el
  voseo, nunca: ni en el copy ni en las instrucciones).
- **Nunca entrega una sola versión**: cada pieza va con su variante B y el plan de prueba.
- **Nunca cita un referente sin su URL** ni presenta como medido lo que solo se vio (repetición de un
  anuncio ≠ rendimiento).
- **Nunca borra un `[⚠ no verificado]`** heredado, incluida la dirección ilustrativa del ejemplo.

## Output esperado

Devuelve en la conversación un markdown; si el usuario lo pide en archivo, se guarda en
`salidas/viventa-comunicaciones-campana/{AAAA-MM-DD}-{evento-slug}.md`.

```markdown
# Comunicaciones · {Evento} · {Ciudad}
> ⚠️ ESTE PAQUETE REQUIERE REVISIÓN HUMANA ANTES DE PROGRAMAR. Nada acá se envía solo.
> {advertencia de 45 días, si aplica} {pendientes: consultora, notas de mesa}

## Ficha del evento
| Evento | Fecha y día | Horario y zona | Dirección | Cómo llegar | Moneda | Anticipación de pauta | Ventana | Canal WhatsApp | Ángulo |

## Segmentación — a quién sí y a quién no
| Capa | Criterio | Conteo estimado |   (Geo · Tier 1 · Tier 2 · Tier 3 excluido · Tier 4 suprimido)

## Calendario
| # | Fecha | Día | Canal | Segmento | P | Función |
Total: N emails · N WhatsApp. Máximo por persona: 4 emails + 3 WhatsApp en 21 días.

## Las piezas
### E0-A · {fecha} — confirmación      (De / Asunto A / Asunto B / Preheader en bloque, luego el cuerpo íntegro)
...
### W1 · {fecha} — WhatsApp ({N} palabras)   {🚫 NO ENVIABLE HOY si es USA}
...

## Plan de pruebas
| Pieza | Qué cambia entre A y B | Métrica que decide | Cuándo se decide |

## Referentes que usé
| Referente (URL) | Hook o línea textual | Por qué funciona | Cómo lo adapté |

## Bitácora de pruebas (llénenla y péguenla en la próxima corrida)
| Fecha | Pieza | Variante A | Variante B | Métrica | Ganó | Aprendizaje |

## Checklist de deliverability (tachar antes de programar)
## ⚠ Alcance — lo que este paquete no arregla   (bloque de references/segmentacion-deliverability.md §5)
```

Al entregar se dice: *"Paquete listo para revisión: N correos y N WhatsApp para {N} contactos.
Pendientes antes de programar: {lista o 'ninguno'}."* Si faltaban bloqueantes, el output es solo la
lista de faltantes con su pregunta.

Ejemplo íntegro (Valencia, post-evento y variante USA): `references/ejemplo-valencia.md`.

## Skills relacionados

- **Antes:** `viventa-brief-evento` (ficha y bloqueantes del evento) y `viventa-inteligencia-campana`
  (el ángulo y los dolores que usa E1-E3).
- **Después:** `viventa-qa-marca` revisa las piezas antes de programar; `viventa-monitor-evento` mide
  el evento.
- **En vez de este:** `viventa-parrilla-base` para el plan mensual de la base;
  `viventa-reactivacion-lead` para un lead caído 1 a 1.
- **Referencias:** `references/copy-que-convierte.md` (investigación, ángulos, hooks, CTA, pruebas,
  test de mediocridad) · `references/virtual-y-lotes.md` (virtual, híbrido,
  varios eventos) · `references/segmentacion-deliverability.md` (capas, tiers, lista negra, legal,
  alcance) · `references/anti-patterns.md` (14 fallas con antídoto) · `references/ejemplo-valencia.md`
  · `references/contexto-viventa.md`.
