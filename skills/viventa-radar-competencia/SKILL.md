---
name: viventa-radar-competencia
description: >-
  Produce el radar recurrente de mercado de Viventa sin pedirle al equipo datos de la competencia:
  descubre solo quién está pautando y publicando (Biblioteca de anuncios de Meta, Instagram,
  TikTok, web), analiza su contenido orgánico y su pauta, compara con Viventa y con el radar
  anterior, y termina en acciones concretas con responsable y la skill que las ejecuta. Lo no
  verificado va a "no observado". USA
  cuando: "qué está haciendo la competencia", "radar competitivo del mes", "qué ángulos están
  corriendo los otros brokers", "qué dicen de nosotros", "benchmark del sector". Es el mercado
  como proceso: viventa-inteligencia-campana saca el insight de una campaña y radar-competitivo
  corre el scraping con dashboard.
metadata:
  version: "2.0.0"
  domain: "analyze"
  client: "viventa"
  blueprint: "marketing"
  complexity: "standalone"
---

# viventa-radar-competencia

Produce **el radar de mercado del periodo**: competencia (pauta y contenido orgánico), benchmark y
menciones de marca, con una primera página de acciones. **Observa, reporta y propone acciones
concretas**; no escribe las campañas completas ni decide presupuesto: cada acción dice qué skill la
ejecuta. Su salida la consumen `viventa-parrilla-base` y `viventa-inteligencia-campana`.

> **Qué cambió en la v2 (feedback del equipo, noviembre de 2026):** *"le tocó darle toda la
> información de los competidores, no fue proactivo; debería brindar acciones realizables y generar
> un reporte recurrentemente; idealmente análisis del contenido orgánico"*. Ahora (1) **no pregunta
> quiénes son los competidores ni pide sus datos**: arranca con la lista conocida y **descubre** a los
> que están pautando o publicando sobre el tema (Paso 1b); (2) analiza el **contenido orgánico** de
> cada uno en Instagram (y TikTok si tiene), además de su pauta en la Biblioteca de anuncios de Meta;
> (3) abre con **acciones realizables** (qué, por qué, quién, con qué skill, esfuerzo); (4) es
> **recurrente**: cada corrida compara con la anterior y hace seguimiento de las acciones propuestas.

**Una sola pasada, con un tope de búsquedas.** Cuentas compartidas, créditos limitados: máximo
**3 búsquedas por competidor** (Biblioteca de anuncios, perfil de Instagram, web o TikTok) y
**4 búsquedas de descubrimiento**. Lo que no se encuentra ahí va a "no observado". Eso es un
resultado válido.

Dos hechos que ordenan la lectura (detalle en `references/panorama-competitivo.md`):

- **Meta Special Ad Category** (desde el 14-ene-2025): los anuncios financieros perdieron
  segmentación por edad, género y código postal. **El hook sustituye al targeting**, así que este
  radar mira hooks y ángulos, no audiencias.
- **El ángulo "raíces" está saturado**: lo usan todos los brokers. Reportarlo no es un hallazgo.

## Cuándo usar

- Explícito: "qué está haciendo la competencia", "radar competitivo del mes", "qué ángulos están
  corriendo los otros brokers", "qué dicen de nosotros", "benchmark del sector".
- Implícito: antes de armar la parrilla del mes, o cuando alguien afirma que "la competencia está
  haciendo X" y nadie tiene la pieza que lo pruebe.
- Cadencia: **mensual**, sobre el mes cerrado (o **quincenal** en temporada de eventos). Para que sea
  recurrente de verdad: en Claude Code o en ENSŌ se deja como tarea programada el primer lunes de cada
  mes; en Claude (chat), se corre ese día pegando el radar anterior. Cada corrida empieza leyendo la
  anterior.

## Cuándo NO usar

- **El insight de entrada de una campaña puntual** (dolores, ángulos, hooks) →
  `viventa-inteligencia-campana`.
- **El desempeño propio** (CRM, Meta, Google, email) → `viventa-reporte-campanas`.
- **Scraping masivo de varias marcas con dashboard** → `radar-competitivo`.

## Conexiones

| Conexión | Para qué | Si falta |
|---|---|---|
| Búsqueda web (incluye Meta Ad Library pública) | Ver publicaciones, anuncios activos y menciones de marca que el usuario no trajo | Degrada: se trabaja solo con lo pegado; lo no traído va a "no observado" y la cobertura lo declara |

## Input del usuario

| Campo | De dónde sale | Si falta, preguntar |
|---|---|---|
| Periodo | El usuario | *"¿Qué mes cubro?"* — sin respuesta, el mes cerrado anterior, declarado en la cobertura |
| Competidores a mirar | **No se pregunta.** La lista del radar anterior + los brokers autorizados por Bancolombia (`references/panorama-competitivo.md`) + los que descubra el Paso 1b | Si el usuario quiere sumar uno, lo nombra y entra; nunca se le pide que traiga sus datos |
| Piezas o anuncios recolectados | El usuario, si quiere | No se pregunta: se hace la búsqueda del Paso 2. Si los pega, se usan como evidencia adicional |
| Métricas propias del periodo | La salida de `viventa-reporte-campanas`, si está | No se pregunta: sin ella, el benchmark compara solo lo observable (frecuencia, formatos, interacción visible) y lo dice |
| Métricas de referencia del sector | Una fuente publicada, si la hay | No se pregunta: sin fuente, la fila va a "métricas no emitidas" |
| Acciones del radar anterior y qué pasó con ellas | El radar anterior, y el usuario si quiere contar | No se pregunta: si el radar anterior tiene acciones, se listan con estado "¿se hizo?" para que el equipo marque |
| Menciones de marca recolectadas | El usuario | No se pregunta: se hace la búsqueda del Paso 4 |
| Radar del mes anterior | `salidas/viventa-radar-competencia/` | Se busca ahí; si no está, el Paso 5 lo trata como primera corrida |

Baseline de social listening y cifras propias: `references/voice-of-customer-90d.md` y
`references/cifras-canonicas.md` (los inyecta el build del plugin del cliente). No se preguntan.

## Workflow

1. **Encuadra.** Fija periodo, competidores y fuentes que de verdad se pudieron mirar. Escribe la
   línea de cobertura con lo que **no** se cubrió: evita que el lector asuma cobertura total.
1b. **Descubre (sin preguntar).** Con un máximo de 4 búsquedas: en la Biblioteca de anuncios de Meta,
   por palabras clave del negocio ("vivienda en Colombia desde el exterior", "crédito hipotecario
   colombianos en el exterior", "casa en Colombia desde Estados Unidos", "comprar vivienda en Colombia
   desde España"); y en Instagram y TikTok, por esas búsquedas y sus hashtags. Todo anunciante o
   cuenta que aparezca hablándole al mismo público y **no esté en la lista** entra como `nuevo en el
   radar`, con la URL donde se vio. Constructoras que le venden directo al colombiano afuera también
   cuentan. Si no aparece nadie nuevo, se dice.
1c. **Ubica a cada competidor** sin pedírselo a nadie: su página en la Biblioteca de anuncios, su
   perfil de Instagram (y TikTok si tiene) y su web, con URL. Lo que no se encuentre, a "no observado".
2. **Mira la competencia: pauta.** Por competidor: sus anuncios activos en la Biblioteca de anuncios
   de Meta (cuántos, desde cuándo — los que llevan **más de 30 días** activos son los que sostienen —,
   formato, hook, CTA, a dónde llevan). Anota solo lo observado:
   formato y tema publicado, ángulo o hook con **la pieza como evidencia** (qué, dónde, cuándo), y
   si pauta, qué anuncio se vio. Es señal: un ángulo nuevo en dos o más competidores a la vez, un
   formato nuevo, un sub-segmento que alguien empezó a atender, un competidor que dejó de pautar.
   Clasifica cada ángulo por el miedo que ataca (estafa, perder la plata, inmueble que no es).
2b. **Mira la competencia: contenido orgánico.** Por competidor, su perfil público de Instagram (y
   TikTok si tiene) en el periodo:
   - **Ritmo**: cuántas publicaciones y de qué formato (reel, carrusel, foto, historia destacada).
   - **Temas**: de qué hablan (proceso, proyectos, casos, educación financiera, eventos, humor).
   - **Lo que más funcionó para ellos**: las 3 publicaciones con más interacción **visible**
     (reproducciones, me gusta o comentarios que muestra la plataforma), con su hook textual, su
     formato y su CTA. Interacción visible ≠ resultado de negocio: se dice así.
   - **Qué pregunta la gente en sus comentarios**: objeciones y dudas que se repiten (sin identificar
     personas).
   - **Lo mismo para Viventa** (@viventa o la cuenta oficial), para que la comparación sea justa.
   Claude no entra con sesión ni ve métricas privadas: todo es lo público, con URL.
3. **Arma el benchmark.** Una fila por métrica que tenga **fuente, ventana y universo**, y que
   esté medida igual del lado propio y del ajeno. Si falta cualquiera de las tres o la medición
   difiere, la fila va a "Métricas no emitidas y por qué". CPL y CAC se nombran completos y
   nunca se mezclan.
4. **Mide las menciones.** Volumen del periodo, separando **espontáneas** de **respondidas**, y
   saldo de sentimiento. Compara **siempre contra dos referencias a la vez**: el baseline propio
   del VOC (6 % de comentarios, 1,7 % de negativos) y el sector (0-0,3 % de comentarios en
   competidores, 10-15 % de negativos). Ej.: 3 % de negativos = "casi el doble del baseline
   propio, muy por debajo del sector". Igual al baseline → "en línea con el baseline". Sin
   menciones halladas → "no observado", no "cero menciones". Marca si un competidor está
   atendiendo un sub-segmento desatendido (veteranos con VA loan, Puerto Rico, vivienda usada,
   compra sin cuota inicial): es el hallazgo más valioso del mes.
5. **Anota qué cambió.** Solo si hay radar anterior: hasta 3 cambios contra él, cada uno con su
   implicación para la parrilla en una línea. Sin cambios relevantes → "sin cambios relevantes".
   Sin radar anterior → *"primera corrida — no hay comparación posible"*, y este radar queda de
   baseline.
5b. **Convierte hallazgos en acciones.** Entre 3 y 5, priorizadas, cada una con:
   **qué hacer** (concreto y ejecutable esta semana o este mes) · **por qué** (el hallazgo y su
   evidencia, con URL) · **quién** (rol: diseño, video, contenido, pauta/agencia, comercial) · **con
   qué skill** (`viventa-inteligencia-campana`, `viventa-parrilla-base`, `viventa-caso-exito`,
   `viventa-comunicaciones-campana`…) · **esfuerzo** (bajo, medio, alto). Puede llevar **un hook de
   ejemplo** para mostrar la idea; la campaña completa la escribe la skill nombrada.
   Ejemplos de forma (no de contenido): *"Responder con un reel la pregunta '¿y si no tengo cuota
   inicial?', que aparece en los comentarios de 3 competidores y que ninguno contesta → contenido ·
   `viventa-inteligencia-campana` · esfuerzo bajo"*; *"Probar el formato 'comentario-respuesta' que
   Casa en Casa sostiene hace 45 días en pauta → agencia · esfuerzo medio"*.
5c. **Seguimiento de las acciones anteriores.** Si el radar anterior traía acciones, lístalas con
   `¿se hizo? (marcar)` y, si el equipo contó el resultado, qué pasó. Una acción que se repite tres
   radares sin hacerse se dice.
6. **Autovalida y guarda.** Recorre las reglas duras contra el borrador; si alguna falla,
   corrige antes de emitir. Guarda en la ruta de `Output esperado`.

## Reglas duras

- **Nunca estima** gasto, volumen de leads ni resultados de un competidor, ni desde la Ad
  Library ni desde la frecuencia de posteo. Si no se vio, no se dice.
- **Ninguna observación de competencia sin su evidencia** (pieza, dónde, cuándo). Una sola pieza
  no se lee como estrategia.
- **Ninguna fila de benchmark sin fuente, ventana y universo**, y ninguna comparación entre
  métricas medidas distinto.
- **Nunca compara menciones contra cero**: siempre contra el baseline del VOC y el sector.
- **Cero PII**: menciones por volumen y sentimiento; un verbatim público se cita sin identificar
  a la persona más allá de su handle público.
- **Ninguna cifra propia fuera de `references/cifras-canonicas.md`.** El +42 % es participación
  del segmento de no residentes, nunca "crecimiento del mercado" (el mercado total crece 5-12 %).
- **Nunca pide al equipo los datos de la competencia** (quiénes son, sus perfiles, sus anuncios): los
  busca. El equipo puede sumar, no tiene que traer.
- **Toda acción es concreta, tiene evidencia, responsable y skill que la ejecuta.** No escribe la
  campaña completa (eso es de la skill nombrada); sí puede dar un hook de ejemplo. Prohibido: acciones
  vagas como *"mejorar el contenido"* o *"ser más creativos"*.
- **Nunca copia el contenido de un competidor**: se toma la estrategia, no las palabras.
- **Nunca reporta "raíces" ni audiencias como hallazgo.**
- **La sección "No observado" siempre existe**, aunque diga "nada relevante".

## Output esperado

Archivo `salidas/viventa-radar-competencia/radar-{AAAA-MM}.md`, español de
Colombia con tuteo. **La página 1 son las acciones**; el detalle va después:

```
# Radar de mercado — {periodo}

Cobertura: {qué se miró} · No cubierto: {qué no} · Nuevos en el radar: {n}

## 0. Qué hacer este mes (acciones)
| # | Qué hacer | Por qué (evidencia) | Quién | Con qué skill | Esfuerzo |
### Seguimiento de las acciones del radar anterior
| Acción | ¿Se hizo? | Qué pasó |

## 1. Competencia
### 1a. Pauta (Biblioteca de anuncios de Meta)
| Competidor | Anuncios activos | Más de 30 días | Formatos | Hook principal | CTA | Evidencia (URL) |
### 1b. Contenido orgánico (Instagram / TikTok)
| Cuenta | Publicaciones del periodo | Formatos | Temas | Top 3 por interacción visible (hook · formato · URL) | Lo que pregunta la gente |
### 1c. Nuevos en el radar
{quién, dónde se vio, desde cuándo}

## 2. Benchmark
{tabla comparable, con fuente / ventana / universo}
### Métricas no emitidas y por qué

## 3. Menciones de marca
{volumen, espontáneas vs respondidas, sentimiento vs baseline y vs sector}

## 4. Qué cambió desde la corrida anterior
{máx. 3, con implicación para la parrilla — o "primera corrida"}

## 5. No observado
{lo que no se pudo verificar}
```

Al terminar se le dice al usuario la ruta, la cobertura en una línea, cuántos competidores nuevos
aparecieron y la acción número 1 (o "sin hallazgos nuevos este mes"). Y la fecha sugerida de la
próxima corrida.

## Skills relacionados

- **Antes:** `viventa-reporte-campanas`, que da las métricas propias del benchmark.
- **Después:** `viventa-parrilla-base` planifica el mes con esta salida;
  `viventa-inteligencia-campana` la usa como insumo de una campaña.
- **En vez de este:** `radar-competitivo` para un scraping completo de varias marcas con
  dashboard; `viventa-caso-exito` cuando lo que hay que hacer es convertir la prueba social propia
  en piezas.
