---
name: viventa-parrilla-base
description: >-
  Convierte el export agregado de la base de Zoho en el conteo real por microsegmento (fase,
  engagement, propósito, presupuesto, país, señal de llamada), la parrilla del mes con correos
  redactados y proyectos de Vivehome sugeridos por segmento, el criterio para pasar candidatos a
  comercial y el plan de impacto dentro del techo de deliverability. No envía. USA cuando: "arma la
  parrilla del mes para candidatos y contactos", "segmenta la base de 250 mil", "qué proyectos les
  mandamos a los candidatos de España". Es la base entera: viventa-match-proyecto-lead resuelve un lead.
metadata:
  version: "2.0.0"
  domain: "convert"
  client: "viventa"
  blueprint: "marketing"
  complexity: "standalone"
---

# viventa-parrilla-base

Produce **la capa agregada del ciclo del lead**: qué hay en la base, cómo se agrupa y qué se le manda
a cada grupo este mes. Lo usan la gerente de Growth y el responsable de email marketing.

Invariantes: **planifica y redacta; no envía, no segmenta en Zoho, no toca la base** (regla durable
#1 del cliente). La parrilla **es un plan de volumen restringido por deliverability**, no un
calendario de ideas: email tiene un CAC fully-loaded de [cifra interna] contra [cifra interna] de Meta y [cifra interna] de un evento,
pero el dominio ya cae en spam y la base de ~250K está sin limpiar. El problema no es tener 250K
registros; es escribirles a los 250K. **Una sola pasada**: sin loops; la única investigación es la
de hooks del estándar de copy, acotada (cuentas de IA compartidas, hasta 4 horas sin créditos).

> **Qué cambió en la v2 (feedback del equipo, noviembre de 2026):** *"la skill de parrilla tiene
> falencias: pide muchas cosas, preguntas innecesarias, irrelevantes o excesivas"*. Ahora **no
> pregunta nada antes de empezar**. Con lo que el usuario pegue (un export de Zoho, unos conteos
> sueltos, o nada) arma la parrilla: si no hay conteos, la arma por **segmentos definidos** con la
> columna "Conteo (lo pone Zoho)" vacía y el techo calculado con el supuesto conservador. Los datos que
> faltan se listan **al final, en un solo bloque**, como "con esto la parrilla mejora", nunca como
> condición. Y los correos se escriben con el estándar de copy
> (`references/copy-que-convierte.md`), con asunto B para probar.

## Cuándo usar

- Explícito: "arma la parrilla del mes para candidatos y contactos", "cuántos leads tengo de
  cada tipo", "a quién le mando qué este mes", "segmentar la base por inversión e ingresos",
  "el plan de comunicaciones de la base", "qué le escribimos a los que están en fase de
  interés", "cómo impacto a cada categoría de la base".
- Implícito: el pedido de "la bienvenida para los leads nuevos" que todavía no se registraron a nada
  (ese onboarding es de este skill, no de `viventa-comunicaciones-campana`).
- Cadencia: mensual, idealmente después del reporte mensual de `viventa-reporte-campanas`.

## Cuándo NO usar

- La secuencia de convocatoria de **un evento** → `viventa-comunicaciones-campana`.
- Emparejar **un lead** con proyectos → `viventa-match-proyecto-lead`.
- El mensaje para **un lead caído** → `viventa-reactivacion-lead`.

## Input del usuario

**Regla de esta tabla: nada se pregunta antes de empezar.** Lo que falte se resuelve con el default
de la columna y se lista al final, en el Bloque E, en un solo mensaje para quien lo tiene.

| Campo | De dónde sale | Si falta |
|---|---|---|
| Export agregado: conteo por fase del funnel | Lo pega el usuario (Zoho) | **No bloquea.** La parrilla se arma con los segmentos definidos y la columna de conteo vacía, marcada "lo pone Zoho". Bloque E lo pide |
| Corte de engagement por tier (abrió ≤90d / 91-180d / +180d / rebote) | Lo pega el usuario | **No bloquea.** Se usan las reglas por tier (tier 3 y 4 fuera) y el techo se escribe como fórmula, no como número |
| Tema o prioridad del mes (p. ej. compra conjunta, proyectos con mejores tasas) | El usuario | No se pregunta. Si viene, los segmentos activos se ordenan para empujar ese tema |
| Ciudad o país de interés, agregado | Export de Zoho | No se pregunta: el eje se omite y se declara |
| Tipo de inversión declarado (formulario CC) | Export de Zoho | No se pregunta: el eje se omite y se declara |
| Rango de ingresos declarado | Export de Zoho | No se pregunta: el eje se omite y se declara |
| Mes objetivo | El usuario | No se pregunta: el mes entrante, declarado |
| Eventos del mes y su volumen de convocatoria | `viventa-brief-evento` / `viventa-comunicaciones-campana` | No se pregunta. Si no se sabe: se advierte en una línea que la parrilla puede chocar con la convocatoria y se resta 0, declarado |
| Insumo del mes anterior | `viventa-reporte-campanas`, sección INSUMO PARA LA PARRILLA | No se pregunta: se arma sin aprendizaje y **se dice explícitamente** |
| Salud del dominio / tasa de rebote actual | Responsable de email marketing | No se pregunta: supuesto conservador de `references/techo-deliverability.md`, marcado `[⚠ no verificado]` |
| Cifras canónicas | `references/cifras-canonicas.md` | Si no está accesible: el copy no usa ninguna cifra de negocio y se declara |
| Presupuesto o preaprobado declarado (formulario CC), agregado por rango | Export de Zoho | No se pregunta: el eje se omite y se declara |
| Señal de llamada (Aircall), agregada por categoría | Tecnología o el responsable de email, como conteos: "objeción cuota inicial", "pidió proyectos en {ciudad}", "no contesta"… | No se pregunta: mientras tecnología no exponga los resúmenes, el eje se omite y se dice. **Nunca** resúmenes individuales |
| Inventario de proyectos | Export de Vivehome / Zoho Analytics o la hoja de `viventa-datos-publicables` (proyecto, ciudad, precio desde, moneda, fecha de corte) | No se pregunta. Sin él, los correos llevan el ángulo sin proyecto y el Bloque E dice que con el inventario se suman proyectos |
| Formulario prellenado | Zoho Forms | No se pregunta: el CTA apunta al formulario de actualización; los parámetros de prellenado los pone Zoho Campaigns |

## Workflow

### Fase 1 — Ver qué hay (sin frenar)

Mira lo que el usuario pegó y elige el modo, sin preguntar:

| Lo que hay | Modo | Qué cambia |
|---|---|---|
| Conteos por fase **y** por tier | **Con conteos** | Todo el workflow como está escrito: mapa con números, techo con número, recorte por techo |
| Algunos conteos, o ninguno | **Por segmentos definidos** | El mapa (Fase 2) lista los segmentos con su **regla de filtro en Zoho** y la columna "Conteo (lo pone Zoho)" vacía; la Fase 3 prioriza por cercanía a la conversión y tier, sin el umbral de 200; el techo (Fase 4) va como fórmula con el supuesto conservador. La parrilla y los correos salen completos |

**No estimes la composición de la base ni inventes conteos.** En modo "por segmentos definidos" el
plan de impacto dice: *"Antes de programar, llenen la columna de conteo en Zoho y recorten con la
regla del techo"*.

### Fase 2 — Mapa de la base

Cruza los ejes que tengas. **Nunca inventes un eje que no vino en el input.**

Ejes, en orden de prioridad:
1. **Fase del funnel** — obligatorio
2. **Tier de engagement** — obligatorio (define quién puede recibir qué)
3. **Tipo de inversión** — vivienda propia / inversión / retorno al país
4. **Rango de ingresos**
5. **Ciudad o país de interés**
6. **Presupuesto o preaprobado** (rango) — habilita sugerir proyectos
7. **Señal de llamada** (categoría agregada de Aircall) — habilita el mensaje que responde a la objeción

Un **microsegmento** es la combinación de ejes que cambia el correo (por ejemplo: interés activo ·
tier 1 · inversión · presupuesto medio · España · objeción de cuota inicial). Aplican las mismas
reglas de la Fase 3: si dos microsegmentos reciben el mismo correo, son uno.

Reglas del mapa:
- Toda celda lleva **conteo absoluto y % de la base**.
- Las celdas **vacías se muestran**: un segmento en cero es información.
- Los registros que no se pueden clasificar van a una fila **`Sin clasificar`** con su conteo.
  **Nunca se reparten proporcionalmente.** Si esa fila es grande, es el hallazgo de la corrida.
- Si un eje tiene **más de 40 %** sin clasificar, se declara **no utilizable** para segmentar este mes
  (40 % exacto todavía se usa). Umbral: heurística propia, ajustable.

### Fase 3 — Priorizar segmentos

No todos los segmentos se atacan. Prioriza por **cercanía a la conversión** (fase del funnel más avanzada
primero), luego **tier** (1 antes que 2), luego **volumen**, y quédate con un máximo de **6 segmentos
activos**.

Un segmento entra a la parrilla si cumple las tres:
1. Tiene **≥ 200 personas** (200 exactas entra; por debajo, el esfuerzo de redacción no se paga —
   heurística propia, ajustable)
2. Está en **tier 1 o 2**
3. Tiene un mensaje que **no es el mismo** que el de otro segmento ya incluido

**Si dos segmentos reciben el mismo mensaje, son un segmento.** Partirlos infla la parrilla sin
cambiar nada.

### Fase 4 — Techo de deliverability

Antes de asignar un solo envío:

```
Techo mensual = contactos tier 1 y 2 alcanzables
              × frecuencia máxima por contacto (default: 4 correos/mes)
              − volumen ya comprometido por convocatorias de evento
```

Detalle y reglas de higiene: `references/techo-deliverability.md`.

- **El volumen de eventos tiene prioridad.** La convocatoria de un evento es fechada e
  irreversible; la parrilla vive en lo que sobra.
- **Frecuencia máxima por contacto: 4 correos/mes**, contando los de evento. Un contacto que ya
  recibe 6 correos de convocatoria **no entra en la parrilla**.
- Si la parrilla deseada no cabe en el techo, **se recorta por prioridad de la Fase 3** y se dice
  qué quedó afuera y por qué.

### Fase 5 — Redactar la parrilla

Una fila por **segmento × envío**. Cada fila trae:

| Campo | Regla |
|---|---|
| Segmento | Del mapa, con su conteo |
| Fecha | Martes, miércoles o jueves. Nunca dos envíos al mismo segmento con <6 días |
| Ángulo | Uno de las 4 P (Pain · Plan · Proof · Philosophy) |
| Propuesta gráfica | Una línea describiendo la pieza. **No la diseña** |
| Asunto | Redactado, ≤ 45 caracteres, **con asunto B para probar** (`copy-que-convierte.md` §3 y §6) |
| Preheader | Redactado, ≤ 90 caracteres, **no repite el asunto** |
| Copy | Redactado, cuerpo completo: primer párrafo = hook, un solo mensaje, prueba concreta. Pasa el test de mediocridad (`copy-que-convierte.md` §7) |
| CTA | Verbo + destino concreto + lo que pasa después ("Quiero que me llamen → te escribe una asesora en 24 h") |
| Volumen | Conteo real del segmento |

Ninguna celda queda descrita "en general". El asunto es el asunto.

**Proyectos sugeridos por segmento** (si hay inventario y eje de presupuesto): elige 2 o 3 proyectos
cuya ciudad coincida con la de interés del segmento y cuyo precio desde quede dentro del rango de
presupuesto. Van en el copy con nombre, ciudad, precio desde (en su moneda) y la fecha de corte del
precio; nunca tasa, cuota ni plazo. Sin proyecto que encaje, el correo no fuerza uno: va el ángulo
sin proyecto y se anota el hueco en el Bloque E.

**Señal de llamada**: si el segmento trae una objeción frecuente de las llamadas, el ángulo del
correo la responde (Plan o Proof), con la prueba concreta, sin prometer aprobación.

### Fase 5b — Criterio de paso a comercial

Por cada segmento activo, define la señal que convierte a un candidato en "listo para comercial" y
qué pasa después:

| Señal | Ejemplo | Qué pasa |
|---|---|---|
| Completa o actualiza el formulario | Responde el CTA prellenado | Se marca para contacto de comercial en 24 h hábiles |
| Pide un proyecto o una asesoría | Responde el correo o hace clic en "quiero que me llamen" | Igual, con el proyecto en la nota |
| Clic repetido sin formulario | 2 clics en el mismo mes | Entra al envío siguiente con el ángulo Proof, no a comercial |

El criterio se entrega como regla para que el responsable de email la configure en Zoho; el skill
no la configura. Los umbrales son heurística propia, ajustable con la gerencia.

### Fase 6 — Plan de impacto y autovalidación

Cierra con: a qué segmentos se ataca, a cuáles **no y por qué**, el volumen total contra el techo,
y los campos que faltaron para clasificar con a quién pedírselos.

Corre el checklist de abajo. Si algo falla, corrígelo antes de emitir — no hay segunda pasada.

### Checklist de autovalidación (Fase 6)

#### A · Datos
- [ ] Cero PII en el input procesado y en el output
- [ ] Todo segmento de la parrilla tiene conteo absoluto
- [ ] La fila `Sin clasificar` existe y no se repartió proporcionalmente
- [ ] Ningún eje con >40 % sin clasificar se usó para segmentar
- [ ] Las celdas en cero están visibles

#### B · Volumen y deliverability
- [ ] El techo está calculado y escrito arriba
- [ ] El volumen total de la parrilla ≤ techo
- [ ] Ningún contacto recibe más de 4 correos en el mes, contando convocatorias
- [ ] Tier 3 no recibe ningún envío de esta parrilla
- [ ] El volumen comprometido por eventos se restó **antes** de asignar

#### C · Copy
- [ ] Máximo 6 segmentos activos
- [ ] No hay dos segmentos con el mismo mensaje
- [ ] Todo asunto ≤45 caracteres y todo preheader ≤90, redactados y finales
- [ ] Ningún preheader repite el asunto
- [ ] Cero placeholders, cero `TBD`
- [ ] Español de Colombia, tuteo

#### D · Legal y cifras
- [ ] Cero tasa, cuota, plazo, cuota inicial
- [ ] Cero promesa de aprobación o de "viabilidad inmediata"
- [ ] Cero superlativos sobre condiciones de crédito
- [ ] CPL y CAC nombrados completos la primera vez, nunca mezclados
- [ ] Cero cifras fuera de `references/cifras-canonicas.md`

#### E · Alcance
- [ ] El entregable no envía, no aprueba y no segmenta en Zoho
- [ ] Si no hubo insumo del mes anterior, está dicho explícitamente

## Reglas duras

Si una se rompe, el entregable no sale.

1. **Cero PII.** La base entra agregada o anonimizada: conteos y atributos, nunca nombres, correos,
   teléfonos ni documentos. Si el input trae PII, el skill **para**, lo dice y pide el export
   agregado. No la procesa "con cuidado": no la procesa.
2. **Nunca accede a Zoho ni simula hacerlo.** Todo entra pegado como tabla.
3. **Ningún conteo inventado.** Sin conteo, la fila sale con la regla de filtro y "Conteo (lo pone
   Zoho)"; el volumen se decide en Zoho con la regla del techo, nunca en el skill.
4. **Nunca supera el techo de deliverability** de la Fase 4. Si el plan no cabe, se recorta el plan;
   nunca se sube el techo.
5. **Tier 3 (+180 días sin abrir) nunca recibe campaña.** Va a re-permiso o a Meta.
6. **Cero placeholders en copy** (salvo el campo de nombre de Zoho Campaigns, y solo si tiene
   configurado un valor por defecto para quien no tiene nombre). Dato que falta → `⛔ INPUT REQUERIDO: {qué} → pedir a: {quién}`.
   Prohibido: `Hola {nombre}, tenemos {proyecto}`; permitido: el copy final sin ese dato, o la marca
   `⛔ INPUT REQUERIDO`.
7. **Nunca tasa, cuota, plazo ni cuota inicial** (triggering terms de Reg Z). Nunca prometer
   aprobación ni "viabilidad inmediata" (MAP Rule). Nunca superlativos sobre condiciones ("el
   crédito ideal") — Ley 1480 art. 29.
8. **Cero cifras fuera de `references/cifras-canonicas.md`.** CPL ≠ CAC. Conversión a desembolso ([cifra interna]) ≠ a
   pago de servicio ([cifra interna]).
9. **No aprueba, no publica, no envía.** Español de Colombia, tuteo, en todos los territorios.

## Output esperado

Devuelve en la conversación un markdown; si se pide archivo,
`salidas/viventa-parrilla-base/{AAAA-MM}-parrilla.md`. Si faltaron los
bloqueantes, el output es solo el bloque `⛔ NO SE PUEDE ARMAR LA PARRILLA` de la Fase 1.


```
# Parrilla de la base — {mes}

Techo de envío: {N} correos · Comprometido por eventos: {N} · Disponible: {N}
Aprendizaje del mes anterior: {sí, de reporte-campanas / no disponible}

## Bloque A — Mapa de la base
{tabla de conteo por segmento, con % y fila "Sin clasificar"}

## Bloque B — Segmentos activos del mes
{los ≤6 priorizados, con por qué entró cada uno}

## Bloque C — Parrilla
{una fila por segmento × envío, con todas las columnas de la Fase 5}

## Bloque D — Plan de impacto
{a quién sí, a quién no y por qué, volumen vs techo}

## Bloque E — Con esto la parrilla mejora (no frena nada)
{un solo mensaje por persona: conteos de Zoho, inventario, eventos del mes; campos sin clasificar;
segmentos sin proyecto que encaje}

## Bloque G — Plan de pruebas y bitácora
{asunto A vs. B por envío, métrica que decide, y la bitácora para pegar el mes siguiente}

## Bloque F — Criterio de paso a comercial
{señal → qué pasa, por segmento activo}
```

Al entregar se dice: *"Parrilla de {mes} lista para revisión: {n} segmentos activos, {N} envíos
contra un techo de {N} (o: techo por calcular en Zoho con la fórmula del Bloque D). Quedan fuera:
{segmentos y por qué}."*

## Skills relacionados

- **Antes:** `viventa-reporte-campanas` (mensual, sección INSUMO PARA LA PARRILLA),
  `viventa-brief-evento` (eventos del mes) y `viventa-datos-publicables` (precios con fecha de corte).
- **Durante:** `email-sequence` si un microsegmento necesita una secuencia automatizada propia.
- **Después:** `viventa-qa-marca` revisa los copies antes de programar.
- **En vez de este:** `viventa-comunicaciones-campana` (un evento), `viventa-match-proyecto-lead` (un
  lead), `viventa-reactivacion-lead` (un lead caído).
- **Referencias:** `references/techo-deliverability.md` (cálculo del techo, tiers, higiene) ·
  `references/ejes-segmentacion.md` (los 5 ejes, valores válidos, lo no clasificable) ·
  `references/cifras-canonicas.md` · `references/contexto-viventa.md`.
