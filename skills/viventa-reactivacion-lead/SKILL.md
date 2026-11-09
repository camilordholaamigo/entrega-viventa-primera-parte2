---
name: viventa-reactivacion-lead
description: >-
  Redacta la SECUENCIA completa (3 a 5 toques, con fechas y variantes A/B) para reabrir la
  conversación con un lead o cliente de Viventa que se cayó del proceso —formulario a medias, DM sin
  respuesta, comentario "info", no-show, asesor sin cierre o frío— por Instagram (canal principal),
  TikTok, WhatsApp o email, con el tono de Viventa, copy investigado y check legal; en modo lista
  arma la tanda del día con variantes que no se repiten. Redacta, no envía. USA cuando:
  "reactivar leads que quedaron colgados", "hay interesados que no terminaron de mandar sus datos",
  "DMs sin responder hace semanas". Es 1-a-1: viventa-comunicaciones-campana convoca a un segmento.
metadata:
  version: "2.0.0"
  domain: "convert"
  client: "viventa"
  blueprint: "marketing"
  complexity: "standalone"
---

# Reactivación de lead — Viventa

Escribe el mensaje que reabre una conversación caída. No vende un apartamento: baja el costo de
responder y demuestra que del otro lado hay una persona. En toda corrida preserva cuatro cosas:
**redacta y no envía**, **no pasa del tope de intentos**, **no nombra ninguna cifra financiera** y
**no afirma nada del lead que no esté en los datos que le dieron**.

Lo opera la persona más junior del equipo de Growth, así que el output se copia y se pega sin
criterio experto: nada de "ajustar según contexto". Lo que haya que decidir, lo decide el skill (y
dice qué decidió). Una sola pasada: sin loops; la única búsqueda es la de hooks del estándar de copy,
acotada y opcional.

> **Qué cambió en la v2 (feedback del equipo, noviembre de 2026).** *"Solo me genera uno, no una
> secuencia, y los mensajes son malos: el copy está súper genérico y mediocre, no toma el tono de
> Viventa. El canal de reactivación es más hacia Instagram, pero debería poder hacerse para TikTok."*
> Por eso: (1) **siempre sale la secuencia completa** del tipo, con fechas, salvo que se pida un toque
> suelto; el lead frío (tipo 6) ya no recibe un solo mensaje por trimestre sino una **campaña de
> reactivación de 3 toques**; (2) **Instagram es el canal por defecto** y hay registro propio para
> **TikTok** (`references/instagram-tiktok.md`); (3) cada toque se escribe con el estándar de copy
> (`references/copy-que-convierte.md`) y el **test de tono Viventa**
> del Paso 6, y lleva variante B; (4) ya no se pregunta lo que se puede deducir.

## Cuándo usar

- Explícito: "necesito reactivar leads que quedaron colgados", "hay gente que llenó el formulario
  a medias, ¿qué les escribo?", "quedaron DMs sin responder hace semanas", "se registraron al
  evento y no fueron, ¿les mando algo?", "escríbeme un WhatsApp para un lead frío que no se sienta
  automático", "tengo comentarios de 'info' en el post y nadie les respondió", "el asesor habló con
  ella y no volvió, ¿cómo la retomo?".
- Implícito: aparece en el CRM o en la bandeja de Instagram alguien que levantó la mano y quedó
  sin siguiente paso.
- **Modo lista** (tipos 1 y 2 en lote): "hay interesados que no terminaron de mandar sus datos",
  "manda el recordatorio a los clientes potenciales de Meta", "no están entrando leads nuevos",
  "reactivemos las conversaciones viejas". Ver Paso 9.
- Cadencia: por lead y por punto de abandono. El formulario a medias rinde en las primeras 1–2 h;
  revisar abandonos una vez al día pierde esa ventana: en modo lista, dos cortes al día.

## Cuándo NO usar

- **Convocar un segmento a un evento** (secuencia de 21 días) → `viventa-comunicaciones-campana`.
- **Recomendar proyectos del inventario a un lead** → `viventa-match-proyecto-lead`. Este no vende
  proyecto: reabre la conversación.
- **Reactivar a un negocio aliado que dejó de referir** → `viventa-ciclo-referidor`.
- **El lead ya contestó y hay que responderle** → `reply-handler`.

## Input del usuario

| Campo | De dónde sale | Si falta, preguntar |
|---|---|---|
| Canal | El usuario | **No bloquea.** Default: **Instagram (DM)**, que es el canal principal de reactivación de Viventa. Si el pedido dice TikTok, WhatsApp o email, ese. La secuencia puede combinar canales (Paso 5) |
| Punto de abandono | El usuario o el CRM (tabla del Paso 1 en `references/mapa-abandono.md`) | **Se deduce** del pedido ("clientes que se enfriaron", "base vieja" → 6 · "no fueron al evento" → 4 · "comentaron info" → 3…). Solo si no hay ninguna pista se pregunta, una vez, con las 6 opciones |
| Intento número | El usuario o el historial del CRM | No se pregunta: sale **la secuencia completa desde el primer toque**. Si el usuario dice "ya le mandamos uno", la secuencia arranca en el segundo |
| País de residencia | CRM o formulario | *"¿Dónde vive: Estados Unidos, España, Colombia u otro?"* Obligatoria si el canal es email o WhatsApp. Si no se sabe → Paso 3, caso "desconocido" |
| Consentimiento registrado | CRM | Solo si reside en España o el país es desconocido: *"¿Hay consentimiento registrado para escribirle por este canal?"* Si no sabe → se trata como **no** |
| Estado y respuestas | CRM | *"¿Respondió algo, pidió baja, se quejó, o está hoy con un asesor?"* Si no sabe → se asume que no, y queda como casilla del check previo |
| Datos del contacto | CRM, formulario, hilo del DM | No bloqueante. Nombre · qué preguntó o hizo · proyecto o ciudad de interés · fecha del último contacto · para tipo 4, ciudad y fecha del evento · para tipo 5, nombre del asesor y la objeción anotada. Si falta uno: variante honesta sin ese dato y aviso *"Sin <dato>, la variante 1 pierde fuerza; si lo consigues en el CRM se reescribe en 2 minutos."* |
| Sub-segmento | CRM o nota del asesor | No se pregunta. Si viene: sin cuota inicial ahorrada · vivienda usada / segunda compra · Puerto Rico · veterano con VA loan |
| Quién firma | `[nombre de quien firma]` y `[cargo de quien firma]` | Si la config no está resuelta: *"¿Con qué nombre y cargo firma el mensaje?"* Nunca "el equipo de Viventa" |
| Dirección física (pie de email) | `references/cifras-canonicas.md` | Solo para email: *"¿Qué dirección física va en el pie del correo?"* Sin dirección el email no se entrega (CAN-SPAM) |
| Modo lista: export de interesados incompletos | Meta Business Suite (clientes potenciales y bandeja), formulario de registro o CRM | *"Exporta los interesados que empezaron y no terminaron, con nombre de pila, canal, país, fecha y los campos que sí dejaron."* **Bloquea** el modo lista |
| Modo lista: campos obligatorios del registro | El formulario de registro | Solo la primera vez: *"¿Qué campos son obligatorios para que el registro cuente como lead?"* Se guarda junto al historial |

Nunca se pide ni se guarda el teléfono o el email del lead: el mensaje se pega en la herramienta que
ya los tiene.

## Workflow

### Paso 1 — Recoge los inputs e identifica el punto de abandono

Deduce lo que se pueda del pedido y usa los defaults de la tabla; pregunta solo lo que de verdad
bloquea (el consentimiento en España, la dirección física de un email), en una sola tanda. Si el
usuario no sabe el tipo, usa la tabla "Cómo identificar el punto de abandono en el CRM" de
`references/mapa-abandono.md`.

### Paso 2 — Chequea parada y tope antes de escribir

**Señales de parada** (anulan el conteo; no se redacta mensaje comercial y se le dice al usuario
por qué): cualquier "no", incluso ambiguo ("ahora no", "después miro"); pedido de baja, bloqueo,
marca de spam o pulgar abajo en WhatsApp; visto sostenido después de dos mensajes; queja pública o en DM; lead activo hoy con un asesor
(manda el CRM, no se duplica contacto).

**Tope por tipo** — decisión de diseño de Viventa, deliberadamente por debajo de los 5–7 toques
que recomiendan los benchmarks de B2B:

| Tipo | Tope | Qué se hace con el intento pedido |
|---|---|---|
| 1, 4, 5 | 3 intentos en 12 días | 1 → Pain · 2 → Plan o Proof · 3 → salida digna. Intento 4+ → negarse y explicar el tope |
| 2 | La respuesta pendiente **no cuenta** como intento | Responder siempre, sin importar cuánto pasó. El conteo de 3 arranca en el primer mensaje que Viventa inicia después |
| 3 | 2 intentos | 1 → ubicar (Plan) · 2 → prueba + salida digna, sin pregunta. Intento 3+ → negarse |
| 6 | **Campaña de reactivación: 3 toques en 14 días**, después 1 por trimestre | Toque 1 (día 0) **valor primero**: algo útil y concreto del momento (un proyecto nuevo en su ciudad de interés, un cambio que le sirve, un caso real) + pregunta de una palabra · Toque 2 (día 4-6) **prueba**: caso o dato duro + invitación de bajo esfuerzo · Toque 3 (día 10-14) **salida digna** con permiso: "¿te sigo contando o prefieres que no?". Por **email** a una base fría, el toque 1 es además el *permission reset* (deliverability). Si ya hubo una campaña en los últimos 3 meses → solo el toque 3 |

- **Borde de los 12 días**: si pasaron más de 12 días desde el intento 1 sin respuesta, no se
  reinicia la secuencia: el intento que falte se reemplaza por la salida digna y ahí termina.
  Heurística propia, ajustable solo con aprobación de Growth.
- Después de la salida digna el lead pasa a nurture de baja frecuencia (tipo 6). No se borra, no se
  persigue.
- **Por qué 3 y no 7**: el dominio de email ya cae en spam y la base comparte reputación; WhatsApp
  castiga bloqueos con quality rating y suspensión; y la queja registrada del cliente es *"te
  saturan"*. Si alguien pide subir el número, muéstrale este párrafo.

### Paso 3 — Aplica el piso legal por residencia × canal

Tabla completa y fuentes en `references/piso-legal.md`. Lo que decide el Paso 3:

| Reside en | Canal | Decisión |
|---|---|---|
| USA | Email | Se redacta. Pie obligatorio: dirección física + baja en 1 clic |
| USA | WhatsApp | Se redacta, con la advertencia: *"Requiere opt-in; fuera de la ventana de 24 h solo con plantilla aprobada. Hoy el envío a USA está bloqueado."* Nunca afirmar que se puede enviar ya |
| USA / cualquiera | DM de IG | Se redacta. Sin links en DM frío |
| España | Email o WhatsApp, **con** consentimiento registrado | Se redacta normal |
| España | Email o WhatsApp, **sin** consentimiento (o no se sabe) | **Se detiene.** No hay mensaje comercial. Se entrega la advertencia *"Lead en España sin consentimiento registrado — no se puede enviar comunicación comercial (LSSI art. 21)"* y, si hay trazabilidad del origen del dato, el mensaje de re-consentimiento (6-B de `banco-mensajes.md`); si no la hay, la recomendación de no contactar |
| Desconocido | Email o WhatsApp | Se redacta, y el check previo lleva activada la casilla de España: no se envía hasta confirmar que no reside allá o que hay consentimiento |
| Colombia / otros | Email | Se redacta con el estándar de USA (el más alto que aplica sin consentimiento) |

Tipo 2 por DM (la persona escribió primero) no requiere consentimiento adicional en ningún país.

### Paso 4 — Elige el ángulo por punto de abandono y sub-segmento

| Tipo | Qué debe hacer el mensaje | Ventana del intento 1 |
|---|---|---|
| 1 · Formulario a medias | Quitar la culpa y ofrecer terminarlo por la persona. No re-vender | 1–2 h |
| 2 · DM sin respuesta | Reparar primero (una frase, sin excusas), responder lo que preguntó, recién ahí la pregunta. Si no se sabe qué preguntó, decirlo y dar 3 opciones | Inmediato |
| 3 · Comentario "info" | Calificar, no vender: una pregunta de opción múltiple (máx. 3 opciones). Sin link | 24 h |
| 4 · No-show | Nombrar la ciudad del evento y algo concreto que se resolvió ahí; ofrecer la alternativa remota. Nunca reproche | 24–48 h post-evento |
| 5 · Asesor sin cierre | Retomar la objeción anotada. Si no hay nota, preguntarla derecho: *"¿qué fue lo que te frenó?"* | 7 días desde el último contacto |
| 6 · Frío | Permission reset: de dónde salió el contacto + si quiere seguir recibiendo + baja visible | 1 por trimestre |

Las ventanas son las del diseño original del skill: heurística, no medida en Viventa.

Los intentos siguientes usan las **4 P** en orden: intento 1 **Pain** (el hecho concreto, en el
mismo canal donde abandonó) · intento 2, día 2–5, **Plan** o **Proof** (ángulo nuevo, puede
cambiar de canal; Proof con testimonio solo si hay un caso 🟢 de `viventa-caso-exito` —los verbatims del VOC no tienen consentimiento—, si no, prueba concreta) · intento 3, día 7–12, **Philosophy** (salida digna, sin pregunta). El
sub-segmento cambia el ángulo, nunca la estructura: ✅/❌ por sub-segmento en
`references/mapa-abandono.md` y `references/banco-mensajes.md`.

Hechos que sí dan concreción (fuente: `references/contexto-viventa.md`): 20 años, 5 bancos y ~20
constructoras, oficinas físicas en EE. UU., España y Colombia, el nombre del proyecto por el que
preguntó, la ciudad y fecha del evento. Tasa, cuota y plazo los da el asesor en llamada.

### Paso 5 — Arma la SECUENCIA (nunca un mensaje suelto, salvo que se pida)

El entregable por defecto es la **secuencia completa del tipo**, con fecha de cada toque y una
**variante B** de cada toque que cambia solo la primera línea o solo la pregunta final:

| Tipo | Secuencia que sale |
|---|---|
| 1, 4, 5 | Toque 1 (Pain) · toque 2 (Plan **o** Proof, con las dos versiones) · toque 3 (salida digna) |
| 2 | Reparación con contexto · versión sin contexto · primer toque que Viventa inicia si no responde (día 4) · salida digna (día 10) |
| 3 | Toque 1 (ubicar) + alternativa · toque 2 (prueba + salida digna) |
| 6 | La campaña de 3 toques del Paso 2 (más el de re-consentimiento si aplica España) |

**Combinar canales** cuando el lead está en más de uno (es lo normal en Instagram): el toque 1 va por
donde abandonó; el 2 puede ir por otro canal (p. ej. DM de Instagram → WhatsApp si hay opt-in, o una
historia con sticker de pregunta que le llega a todos los que se enfriaron); el 3 vuelve al canal del
toque 1. Cada toque dice su canal. Detalle de Instagram y TikTok: `references/instagram-tiktok.md`.

Solo si el usuario pide **un** toque concreto ("el segundo mensaje", "solo la salida"), sale ese
toque con su variante B, y se dice cuál es la secuencia completa en una línea.

Test de distinción: si al quitarles el saludo dos variantes se pueden intercambiar sin que su ficha
deje de ser cierta, son la misma → se rehace una.

### Paso 6 — Redacta con el registro del canal

| | WhatsApp | DM de Instagram | TikTok (respuesta a comentario / DM) | Email |
|---|---|---|---|---|
| Extensión | ≤60 palabras (ideal 30-45) | ≤60 palabras (ideal 25-45) | Comentario: ≤150 caracteres · DM: ≤40 palabras · respuesta en video: 15-30 s de guion | 80–150 palabras de cuerpo |
| Primera línea | El hook: el hecho que solo aplica a esa persona | El hook; es lo que se ve en la bandeja | El hook, en lenguaje de TikTok (directo, sin saludo de empresa) | Asunto (≤45 caracteres) + preheader |
| Saludo | Nombre + quién eres (la 1ª vez) | Nombre + quién eres | Sin saludo formal; nombre si lo hay | Nombre de pila |
| Estructura | Hecho → valor o prueba → pregunta | Hecho → valor o prueba → pregunta | Respuesta útil → invitación a escribir por DM o a la palabra clave | Asunto → preheader → hecho → contexto → pregunta → firma → pie legal |
| Emojis | Ninguno o uno que sustituya una palabra | Máximo uno | Máximo uno | Ninguno |
| Enlaces | Ninguno en el 1er mensaje | Ninguno en DM frío | Ninguno (TikTok los limita); se usa palabra clave | Uno solo, más el de baja |
| Pregunta | Una, que se responde en una palabra | Una, binaria o de 3 opciones | Una, o la palabra clave | Una, al final |

Cercanía es una propiedad chequeable, no un tono:

1. **Arranca por el hecho** que solo aplica a esa persona ("vi que empezaste tu registro y quedó a
   medias"). Si el primer verbo es *"quería"*, *"escribo para"* o *"me comunico"*, reescribe.
2. **Si no tienes el hecho, dilo**: *"no quiero responderte cualquier cosa"*.
3. **Firma humana**: nombre y cargo de la fila "Quién firma", y una vía de respuesta que llega a
   esa persona.
4. **Saca los delatores de plantilla**: "solo quería hacer seguimiento", "espero que te encuentres
   bien", "estimado/a", "no dude en contactarnos", "en Viventa nos complace…", "soluciones
   integrales", "aliado estratégico", "asesoría personalizada". Reemplazo: el hecho, el nombre de
   pila, una pregunta binaria.
5. **Pregunta final respondible con una palabra**: ✅ "¿Te lo dejo listo yo o prefieres hacerlo
   tú?" ❌ "¿Cuál es tu presupuesto y en qué ciudad buscas?" (formulario disfrazado).
6. **Español de Colombia**: tuteo ("tú", "quieres", "puedes"), "acá", "te cuento", "con gusto". Sin
   voseo en los mensajes de Viventa. Tampoco "vosotros" para un lead en Madrid: el lead es colombiano.

**Test de tono Viventa** (si falla uno, se reescribe el toque):
- ¿Habla de **patrimonio y familia**, no de "inversión" ni de "oportunidad"?
- ¿Hay al menos **una prueba de las que no se pueden fingir** (20 años, 5 bancos, ~20 constructoras,
  oficinas físicas en EE. UU., España y Colombia, una asesora con nombre, un caso real) en la secuencia?
- ¿El **miedo** (a ser estafado, a perder la plata, a comprar algo que no es) se desactiva con prueba
  y **no se nombra**?
- ¿El lead es el héroe y Viventa el guía ("tú decides", "te acompañamos")?
- ¿Podría firmarlo otro broker? Si sí, falta lo propio de Viventa.
- ¿Pasa el test de mediocridad de `copy-que-convierte.md` §7 (primera línea, un CTA, cero frases de
  la lista negra)?

Calibra contra los mensajes aprobados de `references/banco-mensajes.md`: cuando el caso coincide,
se cambia solo el hecho concreto. En esos ejemplos la firmante y los leads son nombres ficticios;
en el output va el nombre de la fila "Quién firma" y el del lead si existe.

### Paso 7 — Arma la ficha y el check previo

Ficha literal por variante:

```
VARIANTE N — <ángulo> (P: Pain / Plan / Proof / Philosophy)
Punto de abandono: <tipo 1-6>
Intento y cuándo enviarla: <intento N · momento del Paso 4>
Qué debe saberse antes de enviar: <dato mínimo; si falta, qué variante usar>
Texto: <mensaje literal; en email con Asunto y Preheader primero>
Conteo: <N palabras>
```

Y siempre, al final:

```
CHECK PREVIO AL ENVÍO
[ ] Ningún mensaje menciona tasa, cuota, plazo, cuota inicial ni cargo financiero
[ ] Ningún mensaje promete aprobación
[ ] Cada dato personalizado existe en el CRM — si no, usar la variante sin dato
[ ] El lead no respondió "no", no pidió baja y no está activo con un asesor
[ ] Este contacto no supera el tope de su tipo
[ ] Email: pie con dirección física + baja en 1 clic
[ ] Si reside en España o no se sabe: hay consentimiento registrado (si no, NO enviar)
[ ] WhatsApp a USA: el envío está habilitado hoy
[ ] Revisión humana del aprobador de Growth antes de salir
```

### Paso 8 — Autovalida y entrega

Antes de mostrar, pasa cada variante por las Reglas duras (conteo de palabras declarado, una sola
pregunta salvo en la salida digna, cero placeholders de lead, variantes distintas, punto de
abandono declarado). Prueba final: leela en voz alta; si suena a empresa, reescribila.

Guarda el entregable (Output esperado) y cierra con la frase de entrega.

### Paso 9 — Modo lista: la tanda del día de registros incompletos

Se usa para los tipos 1 y 2 cuando son muchos a la vez. Todo lo anterior sigue valiendo por persona
(parada, tope, piso legal, registro del canal); esto agrega lo de volumen:

1. **Lee el historial** de `salidas/viventa-reactivacion-lead/historial.csv`
   (ID, fecha, intento, variante usada). Sin historial, todos son intento 1.
2. **Detecta el dato que falta** de cada interesado contra los campos obligatorios del registro
   (por ejemplo país, ciudad de interés, propósito, forma de contacto). Si falta más de uno, se pide
   **solo el más importante**: un mensaje, un dato.
3. **Filtra**: fuera los que tienen señal de parada, los que superan el tope, los que están con un
   asesor y los de España sin consentimiento (van a la sección "no contactar" con el motivo).
4. **Arma o actualiza el banco de variantes** en `banco-variantes.md`: de 10 a 15 variantes por
   situación (dato que falta × canal × país), todas dentro del registro del Paso 6. El banco es
   interno: sus `{campos}` nunca se pegan sin resolver.
5. **Asigna a cada persona una variante resuelta** (con su nombre de pila y su dato concreto), sin
   repetir variante dentro de la misma tanda para la misma situación y sin repetirle a nadie una
   variante que ya recibió.
6. **Ordena la lista** por ventana: primero los de menos de 2 horas, después los del día, al final
   los de días anteriores dentro del tope.
7. **Actualiza el historial** con lo que se propuso enviar (marcado `propuesto`; la persona que
   envía lo pasa a `enviado`).

## Reglas duras

| # | Nunca | Ejemplo |
|---|---|---|
| R1 | Menciona tasa, interés, %, cuota, mensualidad, plazo, años para pagar, cuota inicial, pago inicial o cargo financiero. Activa los *triggering terms* de Reg Z (12 CFR 1026.24), que obligan a divulgar APR con igual prominencia; un disclaimer no lo cura. Si el usuario pide incluirlos, el skill se niega y ofrece *"eso te lo explica una asesora en la llamada"* | ✅ "20 años, oficina en Miami" · ❌ "cuotas desde 800.000 al mes" |
| R2 | Promete aprobación | ✅ "se revisa", "existe un programa que se estudia caso a caso" · ❌ "calificas", "te aprueban" |
| R3 | Pasa de 60 palabras en WhatsApp o DM, ni hace más de una pregunta (al final, respondible con una palabra). Heurística propia de lectura en teléfono | — |
| R14 | Entrega un mensaje suelto cuando se pidió reactivar: el default es la secuencia completa con fechas y variante B de cada toque | — |
| R4 | Afirma un dato del lead que no vino en los inputs, ni infiere intención | ✅ "me quedó tu mensaje pendiente" · ❌ "vi que estabas muy interesado en invertir en Medellín" con solo un email a medias |
| R5 | Supera el tope de intentos de su tipo (Paso 2) ni escribe tras una señal de parada | — |
| R6 | Inventa urgencia o escasez ("últimos cupos", "solo hoy"). Solo plazos reales y verificables. La FTC persigue *False Limited Time* y *False Low Stock Message* bajo la Sección 5 | ✅ "el evento es el sábado 14 en <dirección>" · ❌ "quedan pocos cupos" |
| R7 | Usa emojis decorativos. Máximo uno si sustituye una palabra; ninguno en email | — |
| R8 | Escribe con "usted", "estimado", jerga corporativa, mexicanismos ("ahorita", "platicamos") o argentinismos ("bárbaro", "che") | — |
| R9 | Deja un `{placeholder}` de datos del lead sin resolver. Sin nombre, va la versión que funciona igual. Única excepción: la etiqueta de baja que inserta la herramienta de email, escrita como `ENLACE DE BAJA (lo inserta la herramienta de email)` | ✅ "Hola, soy Laura de Viventa" · ❌ "Hola {nombre}", "Hola, estimado" |
| R10 | Insinúa que el VA loan financia propiedad fuera de EE.UU., ni menciona el ingreso VA como computable ante bancos colombianos (sin verificar) | ✅ "El VA loan no se puede usar para comprar fuera de Estados Unidos" |
| R11 | Envía, programa o dice "se envió", "programé", "activé el flujo". El entregable es texto + cuándo pegarlo | — |
| R12 | Redacta un mensaje comercial por email o WhatsApp a un lead en España sin consentimiento registrado | — |
| R13 | En modo lista, repite la misma variante a la misma persona, o pide más de un dato por mensaje | — |

## Output esperado

Archivo `salidas/viventa-reactivacion-lead/<AAAA-MM-DD>-tipo<N>-<canal>.md`
con:

1. **Resumen**: canal · punto de abandono · intento · país · decisiones del Paso 2 y 3 (tope,
   advertencias legales, datos que faltaron).
2. **Secuencia**: una tabla arriba (toque · día · canal · ángulo · qué hace) y debajo una ficha del
   Paso 7 por toque, cada una con su **variante B** y la métrica que decide entre A y B (tasa de
   respuesta).
3. **CHECK PREVIO AL ENVÍO**.

**Modo lista**: `salidas/viventa-reactivacion-lead/<AAAA-MM-DD>-lista.md` con
el resumen (cuántos, por dato faltante, por canal y país), la lista ordenada por ventana —una fila
por persona con ID, nombre de pila, canal, dato que falta, intento y el mensaje resuelto— y la
sección "no contactar" con el motivo de cada uno; más `banco-variantes.md` e `historial.csv`
actualizados.

El archivo no lleva teléfono ni email del lead. Si el Paso 2 o el 3 detuvieron la corrida, el
archivo trae solo el resumen con el motivo (y el mensaje de re-consentimiento si aplica).

El mismo contenido se muestra en la conversación. Frase de entrega: *"Listo: <N> variantes para
<canal>, punto de abandono <tipo>, intento <N>. Quedó en <ruta>. Nada se envió: pásalo por el
check y por revisión antes de pegarlo en <Wati / el DM / Zoho Campaigns>."*

## Skills relacionados

- **Antes:** `viventa-comunicaciones-campana`, que convoca al evento; sus registrados que no
  fueron son el tipo 4 de este skill.
- **Referencias:** `references/instagram-tiktok.md` (canal principal y TikTok) ·
  `references/copy-que-convierte.md` (hooks, CTA, pruebas, test de
  mediocridad) · `references/banco-mensajes.md` · `references/mapa-abandono.md` · `references/piso-legal.md`.
- **Después:** `reply-handler` cuando el lead contesta; `viventa-match-proyecto-lead` cuando la
  conversación se reabrió y hay que recomendarle proyectos.
- **En vez de este:** `viventa-ciclo-referidor` si quien se enfrió es un negocio aliado, no un lead.
