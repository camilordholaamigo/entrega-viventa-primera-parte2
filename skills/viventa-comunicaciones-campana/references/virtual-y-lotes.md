# Eventos virtuales, híbridos y en lote — referencia compartida de las skills de eventos

> Nació del feedback del equipo de Viventa (noviembre de 2026): *"el agente no ayuda a hacer
> múltiples eventos al mismo tiempo"*, *"evento virtual no deja cambiar la dirección del evento
> porque solo deja que funcione para presenciales"* y *"en eventos presenciales pide datos muy
> específicos al equipo que debería poder investigar o evitar preguntar; está mezclando preguntas
> como de CRM que no son necesarias para la creación de las piezas"*.
>
> La usan: `viventa-brief-evento`, `viventa-guiones-evento`, `viventa-pack-evento`,
> `viventa-comunicaciones-campana` y `viventa-monitor-evento`. Si una regla de aquí contradice una
> regla de la skill, **gana esta** para eventos virtuales, híbridos y lotes.

---

## 1. El formato es un eje aparte de la modalidad

Las 4 modalidades (Open House · Expo Colombia · Expo Casa · Viventa Más Cerca de Ti) dicen **quién
propone el evento y qué tamaño tiene**. El **formato** dice **dónde ocurre**, y se pregunta (o se
deduce del mensaje) por separado:

| Formato | Cómo se reconoce en el mensaje | Lo que reemplaza a la dirección |
|---|---|---|
| **Presencial** | Venue, ciudad, "en la oficina", "en el hotel" | La dirección exacta (como siempre) |
| **Virtual** | "webinar", "live", "por Zoom", "en línea", "online", "transmisión", "IG Live", "charla virtual" | **Plataforma + enlace de registro + hora en cada zona horaria** |
| **Híbrido** | Hay venue **y** transmisión | Las dos cosas: dirección para quien va y enlace para quien se conecta |

Un evento virtual que no encaja en ninguna de las 4 modalidades **no se fuerza**: la modalidad queda
`Virtual (webinar / live)` y se sigue. **Nunca** se le pide una dirección física a un evento virtual,
y nunca se bloquea un brief virtual por falta de dirección.

### 1.1 Qué bloquea en un evento virtual

| Campo | ¿Bloquea? | Nota |
|---|---|---|
| Fecha y hora de inicio, **con la zona horaria en que la dieron** | **Bloquea** | "7 pm" sin zona no sirve: hay que saber si es 7 pm de Miami, de Madrid o de Bogotá |
| Plataforma (Zoom, Google Meet, Teams, Instagram Live, Facebook Live, YouTube Live, StreamYard…) | **Bloquea el anuncio** | Cambia el CTA: en un Live de Instagram no hay registro, hay "activa el recordatorio" |
| Enlace de registro o de la transmisión | **No bloquea la producción.** Bloquea la publicación | Las piezas se producen con la variable `{LINK_REGISTRO}` (campo de data merge, ver §1.3). Normalmente la landing de Zoho se crea después |
| Duración | No bloquea | Si falta: 60 min, declarado |
| Quién habla (asesor, banco, constructora invitada) | No bloquea | Sube la conversión del anuncio. Si falta, se pide sin frenar |
| Audiencia (qué países) | No bloquea | Si falta: **USA + España**, declarado. Decide las zonas horarias que se muestran |

### 1.2 La hora se muestra en la zona de quien lee

Un virtual de Viventa lo ven colombianos en varios países a la vez. La skill **calcula** (no pregunta)
la hora en cada zona de la audiencia, con el cambio de horario correcto para esa fecha:

```
Jueves 20 de noviembre · 7:00 p. m. Miami / Nueva York (ET) · 6:00 p. m. Houston (CT)
· 4:00 p. m. Los Ángeles (PT) · 1:00 a. m. (viernes) Madrid · 7:00 p. m. Bogotá
```

- Estados Unidos cambia de horario el **primer domingo de noviembre** y el **segundo domingo de
  marzo**; Europa, el **último domingo de octubre** y el **último domingo de marzo**. Colombia no
  cambia. En las semanas en que una cambió y la otra no, la diferencia Miami–Madrid es de 5 horas y
  no de 6: revisarlo siempre.
- Si en alguna zona de la audiencia la hora cae entre las 11 p. m. y las 7 a. m., se dice en el
  brief como **riesgo de asistencia**, con la hora alterna que funcionaría para las dos audiencias.
  No se cambia la hora: decide quien propuso el evento.
- En las piezas de **un solo país** va solo la hora de ese país. En las piezas globales (orgánico,
  correo a toda la base), la línea de zonas completa.

### 1.3 `{LINK_REGISTRO}` sí es un placeholder permitido

Las skills prohíben placeholders de dato del evento (`[fecha]`, `[dirección]`). `{LINK_REGISTRO}` y
`{LINK_TRANSMISION}` son la **excepción**: son variables de data merge que se llenan cuando exista la
landing, igual que el teléfono de la consultora. Van con llaves, en mayúsculas, listadas en el bloque
de "Antes de publicar", y **ninguna pieza se publica con la llave visible**.

### 1.4 Cómo cambia el copy en un virtual

| Presencial | Virtual |
|---|---|
| "Te esperamos en {venue}, {dirección}" | "Conéctate desde tu casa: {plataforma}, {fecha}, {hora en tu zona}" |
| Prueba de cercanía: oficinas en 3 países, dirección | Prueba de cercanía: **asesoras con nombre** que responden en vivo, preguntas de los asistentes |
| Recordatorio el día del evento por la mañana (quien no salió a las 9 no va a ir) | Recordatorios **24 h antes, 1 h antes y "estamos en vivo"** (10 min antes): en virtual la decisión se toma en el momento |
| "Trae tus documentos" | "Ten a mano tus preguntas: te las respondemos en vivo" |
| CTA: "Regístrate y te guardamos el cupo" | CTA: "Regístrate y te llega el enlace" · en Live: "Activa el recordatorio" |
| Fricción a evitar (VOC): evento sin dirección | Fricción a evitar: evento sin enlace claro o sin la hora en mi país |

La moneda: un virtual global **no lleva montos** en el copy general; si hay que mostrar precio de
vivienda, va en las piezas por país (USD para USA, EUR para España), igual que en presencial.

### 1.5 Semáforo en virtual

La regla de 45 días existe por la pauta (fase de aprendizaje de Meta), no por el venue. Por eso:
- Virtual **con pauta**: la misma regla de 45 días y el mismo semáforo.
- Virtual **sin pauta** (solo base de correo, WhatsApp y orgánico): el mínimo operativo es
  **10 días** (producción + revisión de marca + 2 envíos). Se dice explícito en el brief:
  `Semáforo de pauta: no aplica (sin pauta). Margen operativo: {n} días (mínimo 10).`

---

## 2. Varios eventos a la vez (modo lote)

Se activa solo cuando el mensaje trae **dos o más eventos** (una lista, una tabla pegada, varios
mensajes de WhatsApp juntos, "los eventos de noviembre"). No se pregunta si se quiere modo lote: se
detecta. Tope por corrida: **10 eventos** (la cuenta de Claude es compartida y con límite; con más, se
parten en tandas y se dice).

### 2.1 Lo que se entrega en modo lote

1. **Tabla del lote**, primero, una fila por evento:

   | # | Evento | Formato | Fecha | Días | Semáforo | Dirección o plataforma | Faltan |
   |---|---|---|---|---|---|---|---|

2. **Calendario de producción del lote.** Cada pack pesa ~5 días sobre una sola productora de video y
   una sola diseñadora. Se ordenan los eventos por fecha de publicación necesaria
   (`fecha_evento − 45` con pauta, `− 10` sin pauta) y se marca **choque** cuando dos ventanas de
   producción se pisan. El choque se reporta con las salidas posibles (producir los dos con los
   mismos videos y solo cambiar el bloque de ciudad · mover uno · recortar uno a piezas estáticas).
   La skill no elige.
3. **Mapa de reuso.** Lo que se produce UNA vez para todo el lote y lo que se multiplica por evento:
   - Una vez: los 5 conceptos de video, los carruseles de marca, las piezas de prueba social.
   - Por evento: el bloque de cierre de cada video (voz en off con ciudad, fecha, hora), el flyer,
     la placa de dirección o de plataforma, el correo de confirmación.
   - Resultado en una línea: `Unidades de copy del lote: {n} (por separado serían {m})`.
4. **Una tarjeta por evento**, compacta, con el mismo orden de bloques de la skill que corre, y con
   los faltantes de ESE evento.
5. **Un solo bloque de "datos por pedir"** al final, agrupado por persona (todo lo que hay que pedirle
   a Carolina va junto, todo lo de coordinación de eventos va junto). Así sale un mensaje por persona
   y no uno por evento.

### 2.2 Reglas del lote

- Un evento incompleto **no frena a los demás**: sale su tarjeta con `⛔` y los demás siguen.
- Si dos eventos apuntan a la **misma audiencia en la misma semana** (Miami y Broward el mismo fin de
  semana), se dice: la misma persona no debería recibir los dos anuncios. Se propone qué segmento
  recibe cuál; decide quien maneja la base.
- Los conceptos creativos se escriben una vez y se localizan: no se escriben 5 guiones distintos por
  ciudad salvo que el usuario lo pida.

---

## 3. Investigar en vez de preguntar

Regla: **si el dato es público, la skill lo busca; si es interno, la skill no lo pide para producir
piezas.** El equipo solo responde lo que únicamente el equipo sabe.

### 3.1 Lo que la skill investiga sola (búsqueda web, una pasada corta)

| Dato | Cómo | Cómo se marca |
|---|---|---|
| Dirección completa a partir del nombre del venue ("el hotel Hyatt de Coral Gables") | Búsqueda del venue: calle, número, código postal | `[investigado — confirmar con quien propuso el evento]` |
| Cómo llegar, parqueadero, transporte público | Página del venue o Google Maps | Igual. Sirve para el correo de confirmación |
| Zona horaria y cambio de horario de la fecha | Cálculo (§1.2) | Sin marca: es cálculo |
| Festivos, partidos de la Selección Colombia, Acción de Gracias, puentes que compiten con la fecha | Búsqueda del calendario de esa semana en esa ciudad | `[investigado]` y, si choca, va como riesgo en el brief |
| Tamaño de la comunidad colombiana en la ciudad | Censo / fuentes públicas (US Census ACS, INE España) | `[investigado — fuente: {fuente}]`. Sirve para el copy local |
| Referentes de copy y hooks que funcionan hoy | `references/copy-que-convierte.md` §1 | Con URL o no se usa |

Si la búsqueda no encuentra el dato o hay dos candidatos (dos hoteles con el mismo nombre), se dice y
se pide; no se elige uno.

### 3.2 Lo que NO se pregunta para producir piezas (son datos de CRM o de negocio)

Para guiones, pack, flyers, carruseles y comunicaciones **no se piden**: número de ejecutivas que
atienden, meta de leads, presupuesto de pauta, histórico de conversión, CPL o CAC, tasas de no-show,
estado de leads en Zoho, listas de asistentes. Esos datos solo se usan en el **bloque de metas** de
`viventa-brief-evento`, y ese bloque es opcional: sale si la gerente de Growth pide metas, si el
mensaje trae los datos, o si el usuario dice "con metas".

### 3.3 Lo que sí hay que pedirle al equipo (no se puede investigar)

Fecha y hora reales · si el venue está confirmado · formato (cuando el mensaje no lo deja claro) ·
consultoras asignadas (para los flyers por consultora) · montos que van en la pieza (reserva, precio
de un proyecto). **Todo en un solo mensaje por persona**, al final del output, nunca como ronda de
preguntas antes de empezar.
