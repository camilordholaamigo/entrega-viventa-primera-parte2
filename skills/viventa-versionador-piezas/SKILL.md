---
name: viventa-versionador-piezas
description: >-
  Genera todas las versiones de una pieza maestra de Viventa (flyer, video de primera
  etapa, carrusel, estática o pieza de proyecto) a partir de una tabla de datos variables: evento,
  ciudad, país, fecha, consultora, proyecto, precio. Verifica dato por dato y entrega la tabla de
  control con la fuente y la fecha de corte de cada uno. USA cuando: "adapta el material de primera
  etapa al evento de Miami", "versiona esta pieza para los tres eventos del mes", "saca la pieza del
  proyecto con el precio nuevo". Es el multiplicador: viventa-pack-evento define qué archivos y qué
  copy salen.
metadata:
  version: "1.2.0"
  domain: "produce"
  client: "viventa"
  blueprint: "marketing"
  complexity: "standalone"
---

# viventa-versionador-piezas

Hace el trabajo que hoy se repite a mano: la misma pieza, cambiando ciudad, fecha, moneda, consultora
o proyecto, tres o más veces al mes. Tres invariantes: **ningún dato variable se escribe si no viene
de la tabla** (un teléfono inventado en un A3 se imprime 500 veces), **cada versión pasa la
verificación dato por dato antes de exportarse**, y **la pieza maestra no se toca**: solo se
reemplazan las capas variables. El diseño y la aprobación siguen siendo de diseño y de marca.

## Cuándo usar

- Explícito: "adapta el material de primera etapa al evento de Miami", "cambia el flyer y los datos
  para España", "versiona esta pieza para los tres eventos del mes", "saca la pieza del proyecto con
  el precio nuevo", "actualiza las piezas de optimización con los datos del evento".
- Implícito: `viventa-pack-evento` ya entregó la matriz y el CSV de consultoras, y la pieza maestra
  está aprobada.

## Cuándo NO usar

- Falta decidir qué archivos y qué copy salen del evento → `viventa-pack-evento`.
- La pieza maestra no existe y hay que diseñarla → `viventa-piezas-figma`.
- El video todavía está en crudo → `viventa-corte-video-pauta` primero.
- Revisar marca y legal de las versiones → `viventa-qa-marca` (después de este).

## Conexiones

| Conexión | Para qué | Si falta |
|---|---|---|
| Figma (MCP) | Duplicar el frame maestro y reemplazar las capas de texto e imagen variables | Degrada: entrega el CSV listo para el plugin de data merge que diseño ya usa, con la lista de capas a enlazar. Lo dice en la primera línea. **También degrada así cuando una tipografía de la maestra no carga** (LT Wave, PP Pangram): ver `references/figma-tipografias.md` |
| Zoho Analytics (vista de Vivehome) | Precios y disponibilidad de proyectos para las piezas de proyecto | Usa la tabla que pegue el usuario o la hoja de `viventa-datos-publicables` |
| ffmpeg (local) | Superponer en el video la placa variable (PNG exportado de Figma) en su tramo de tiempo y, si hay, la voz en off de cada ciudad | Degrada: entrega la tabla de placas por versión con tiempos de entrada y salida para montarlas en CapCut |

## Input del usuario

| Campo | De dónde sale | Si falta, preguntar |
|---|---|---|
| Pieza maestra | Link al frame de Figma, o el video editado | *"¿Cuál es la pieza maestra aprobada? Pásame el link del frame o el archivo."* **Bloquea** |
| Capas variables de la maestra | Nombres de las capas en Figma, o tramos del video | *"¿Qué capas cambian por versión? Si las capas se llaman `ciudad`, `fecha`… las tomo por nombre."* |
| Tabla de datos variables | CSV de `viventa-pack-evento`, hoja de `viventa-datos-publicables` o la que pegue el usuario | *"Pásame la tabla con una fila por versión."* **Bloquea** |
| Fuente y fecha de corte de precios y datos de proyecto | `viventa-datos-publicables` | *"¿De qué fecha es el precio y quién lo aprobó?"* Sin esto el precio va `⚠ verificar` y la versión no se exporta |
| Voz en off por ciudad (video) | Archivos grabados | No se pregunta: sin ellos el video se versiona solo con placas y se avisa que la voz dice otra ciudad |

## Workflow

1. **Lee la convención de nombres y la tabla país → moneda** en
   `references/naming-y-datamerge.md`. Es la
   tabla única: no la reescribas acá.
2. **Mapea columnas a capas.** Cada columna de la tabla tiene que caer en una capa de la maestra. Una
   columna sin capa o una capa variable sin columna se reporta antes de generar nada.
2b. **Chequeo de tipografías (v2, noviembre de 2026).** Antes de duplicar nada, lee la tipografía real
   de cada capa variable (familia y estilo de cada tramo) y prueba cargarla, como dice
   `references/figma-tipografias.md` §2. Así se evita el error con **LT Wave** y **PP Pangram** que
   reportó el equipo. Si alguna no carga: no se cambia por otra, no se genera esa capa, y se pasa al
   CSV de data merge (camino B) diciendo qué tipografía faltó y cómo dejarla instalada (camino A).
   Capas con tipografía mezclada: se reemplaza el texto por tramos para conservar cada estilo.
3. **Verifica cada fila** antes de producir su versión:
   - Moneda coherente con el país del evento según la tabla única. Nunca `USA` como moneda ni `$` a secas.
   - Evento con ciudad, dirección exacta, fecha calendario y hora. Si falta uno, la fila queda `⛔ bloqueada`.
   - Teléfono con prefijo de país y escrito tal cual viene. Si el prefijo no corresponde al país del
     evento, `⚠ verificar` (pasó: consultora de un evento en España con teléfono +1).
   - Precio y datos de proyecto con fuente y fecha de corte. Sin ellos, `⚠ verificar`.
   - Ningún campo con tasa, cuota, plazo o aprobación de crédito: esos no son datos variables, son
     claims, y van a `viventa-qa-marca`.
   - Textos que no caben en la capa (desbordan o parten mal): se reporta, no se achica la letra.
4. **Genera las versiones** de las filas que pasaron:
   - Estáticas y flyers: duplica el frame maestro en una página nueva del archivo, reemplaza las capas
     y nombra cada frame con la convención única.
   - Video: superpón la placa de cada versión en su tramo (y la voz en off de esa ciudad si existe).
     El resto del video no se recodifica dos veces.
5. **Arma la tabla de control** con una fila por versión y por dato cambiado.
6. **Revisa la muestra**: abre (o renderiza) la primera y la última versión de cada tipo y
   compara contra la tabla. Si una no coincide, para y corrige el mapeo antes de entregar el resto.

## Reglas duras

- **Nunca escribe un dato que no esté en la tabla**: no deduce teléfonos, correos ni direcciones, no
  convierte monedas y no actualiza precios por su cuenta.
- **Nunca modifica la pieza maestra** ni sus capas fijas; trabaja sobre duplicados.
- **Nunca reemplaza una tipografía que no carga por otra "parecida"**, ni aplana una capa de estilos
  mezclados a un solo estilo: pasa al camino B y lo dice.
- **Nunca exporta una fila bloqueada** ni una con precio sin fecha de corte.
- **Nunca genera voz sintética** para reemplazar la voz en off de una ciudad.
- **Nunca publica ni envía** las versiones: quedan en Figma o en la carpeta para revisión de marca.

## Output esperado

En Figma, una página `Versiones <evento> <AAAA-MM-DD>` con un frame por versión. En disco,
`salidas/viventa-versionador-piezas/<AAAA-MM-DD>-<evento>/` con los videos
versionados y `CONTROL.md`:

```
VERSIONES — {pieza maestra} · {evento o proyecto} · {fecha}
{n} versiones generadas · {n} bloqueadas · {n} con ⚠ verificar
Tipografías: {todas cargaron | ⛔ {familia estilo} no disponible → versiones por CSV de data merge}

| Archivo | Dato | Valor | Fuente | Corte | Estado |
|---|---|---|---|---|---|
| TP_flyer_A4_v01_NombreApellido.pdf | teléfono | +1 305 … | CSV pack-evento | 2026-10-02 | ✅ |
| ALI_redes_1x1_v01_OtraConsultora.jpg | teléfono | +1 … | CSV pack-evento | 2026-10-02 | ⚠ prefijo +1 en evento de España |

Bloqueadas: {archivo} — falta {dato}. Pedírselo a {área dueña}.
```

Al terminar, di: *"{n} versiones listas en {Figma / ruta}. {n} bloqueadas y {n} por verificar
(arriba en CONTROL.md). Pásalas por viventa-qa-marca antes de mandarlas."*

## Skills relacionados

- **Antes:** `viventa-pack-evento` (matriz, nombres y CSV), `viventa-datos-publicables` (precios y
  datos de proyecto con fecha de corte), `viventa-corte-video-pauta` (video editado).
- **Después:** `viventa-qa-marca` revisa las versiones antes de que salgan.
- **En vez de este:** `viventa-piezas-figma` cuando hay que diseñar la pieza, no multiplicarla.
