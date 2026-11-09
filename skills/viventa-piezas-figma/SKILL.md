---
name: viventa-piezas-figma
description: >-
  Produce en Figma el primer borrador de las piezas gráficas de Viventa a partir de un brief completo:
  usa la librería de componentes y el manual de la marca, aplica los textos finales del brief y arma
  cada formato de canal (feed, historia, carrusel, estático de pauta, correo, landing, impreso) con
  nombre estándar. Diseño pasa de producir desde cero a ajustar y aprobar. USA cuando: "diseña las
  piezas de este brief", "necesito el carrusel y las estáticas del evento", "haz las piezas de la
  parrilla de esta semana". Es diseñar la maestra: viventa-versionador-piezas la multiplica después.
metadata:
  version: "2.0.0"
  domain: "produce"
  client: "viventa"
  blueprint: "marketing"
  complexity: "standalone"
---

# viventa-piezas-figma

Le quita a la única diseñadora del área la parte de armar desde cero, sobre más de 500 piezas al mes.
Tres invariantes: **el borrador solo usa componentes, estilos y colores de la librería de Viventa en
Figma** (nada inventado ni copiado de memoria), **los textos van exactamente como vienen en el brief**
(no se corrigen ni se mejoran: eso pasa antes, en el brief), y **todo frame sale marcado como
borrador**: la dirección creativa y la aprobación siguen siendo humanas.

## Esta skill y el sistema de diseño no son lo mismo (v2, noviembre de 2026)

El equipo lo dijo así: *"no identifiqué diferencia entre la skill y el sistema de diseño"*. Tenía
razón: la v1 repetía en palabras lo que el sistema de diseño ya resuelve. La división ahora es esta:

| | **Sistema de diseño de Viventa** (el que armó Alejandra en Claude + la librería y las maestras en Figma) | **Esta skill** |
|---|---|---|
| Qué es | **Las reglas y las piezas**: colores, tipografías, componentes, plantillas, cómo se ve Viventa | **El proceso de producción en lote** que usa esas reglas |
| Cuándo | Una pieza suelta y genérica de repetición | Un brief o un pack con **varias piezas y varios formatos** a la vez |
| Qué decide | Cómo se ve | Qué frame va con qué componente, en qué formato, con qué texto, con qué nombre, y qué falta |
| Quién manda si chocan | **El sistema de diseño, siempre.** Esta skill no tiene reglas visuales propias | — |

Lo que la skill agrega encima del sistema (y por eso existe):
1. **Lote**: todas las piezas del brief × todos los formatos del canal en una pasada (p. ej. 15 piezas
   × 3 formatos), cada una con su nombre de archivo normalizado.
2. **Adaptación por formato** con las zonas seguras de Meta (9:16 con 14 % arriba y 35 % abajo libres,
   4:5 en vez de 1:1 para feed): el sistema define la pieza; la skill la acomoda a cada formato.
3. **Textos que no caben**: los detecta y los devuelve al brief con cuántos caracteres sobran, en vez
   de achicar la letra.
4. **Chequeo de tipografías antes de tocar nada** (`references/figma-tipografias.md`),
   para que LT Wave y PP Pangram no rompan la corrida a la mitad.
5. **Capas variables nombradas** para que `viventa-versionador-piezas` multiplique la pieza después
   sin rehacer nada.
6. **Entrega a diseño**: la lista exacta de lo que le queda a Alejandra por decidir.

Si el sistema de diseño está disponible en la conversación (como skill instalada o como documento
pegado), **se lee primero y se obedece**. Si no, se usa solo la librería de Figma.

## Cuándo usar

- Explícito: "diseña las piezas de este brief", "necesito el carrusel y las estáticas del evento",
  "haz las piezas de la parrilla de esta semana", "saca un primer borrador del correo", "prepara los
  archivos finales para impresión".
- Implícito: hay un brief aprobado con textos finales y todavía no hay frames.

## Cuándo NO usar

- El brief no está completo o los textos no son finales → `viventa-brief-requerimiento` primero.
- La pieza ya existe y hay que sacar versiones por ciudad, consultora o precio → `viventa-versionador-piezas`.
- Un carrusel de contenido orgánico con su guion → `carrusel-figma`.
- Una presentación comercial → `presentacion-persuasiva-figma`.

## Conexiones

| Conexión | Para qué | Si falta |
|---|---|---|
| Figma (MCP) | Leer la librería (componentes, estilos de texto, variables de color), crear los frames y exportar | Degrada a **spec por frame**: tamaño, componente a usar y texto por capa, para armarlo a mano. Lo dice en la primera línea |

## Input del usuario

| Campo | De dónde sale | Si falta, preguntar |
|---|---|---|
| Brief con textos finales | Salida de `viventa-brief-requerimiento` o `viventa-pack-evento` | *"Pásame el brief aprobado con los textos finales."* **Bloquea**: diseñar con textos provisionales es el reproceso que más tiempo se come |
| Archivo de Figma con la librería de Viventa | Link | *"¿En qué archivo está la librería de componentes y estilos de Viventa?"* Sin librería no hay borrador en Figma: se degrada a **spec por frame** (tamaño, componente, texto por capa) |
| Sistema de diseño de Viventa en Claude | Skill instalada o documento pegado | No se pregunta. Si está, manda sobre cualquier otra regla |
| Formatos pedidos | El brief | No se pregunta: si el brief no los dice, los del canal (tabla del Paso 3) |
| Imágenes y fotos | El brief o la carpeta compartida | No se pregunta: sin ellas, marco de imagen vacío con la descripción de lo que va |
| Archivo de destino | El pedido | No se pregunta: página nueva `Borradores <brief> <fecha>` en el archivo del brief o de la librería |

## Workflow

1. **Revisa el brief**: objetivo, canal, formatos, textos finales, CTA y fecha. Si algún texto dice
   "por definir", "TBD" o tiene opciones ("A o B"), para y devuelve la lista de textos pendientes.
2. **Lee el sistema de diseño (si está) y la librería**: componentes disponibles por tipo de pieza,
   estilos de texto y variables de color. Si existen plantillas maestras para ese tipo de pieza, usa
   la maestra (no armes otra).
2b. **Chequea las tipografías** de los componentes y maestras que vas a usar
   (`references/figma-tipografias.md` §2). Si LT Wave o PP Pangram no
   cargan en este Figma, no sigas con esas piezas: entrega la spec por frame y los pasos para
   instalarlas, y arma el resto.
3. **Elige los formatos** del canal:

   | Canal | Formatos |
   |---|---|
   | Feed | 1080×1350 (4:5) y 1080×1080 |
   | Historia / Reel (portada) | 1080×1920 |
   | Carrusel | 1080×1350 por lámina |
   | Estático de pauta | 1080×1350, 1080×1080 y 1080×1920 |
   | Correo | 600 px de ancho, cabezote + bloques |
   | Landing | Sección hero en escritorio y en móvil |
   | Impreso | A3 / A4 con 3 mm de sangrado |

4. **Arma cada frame** en la página de borradores: instancia del componente o de la maestra, textos
   del brief en sus capas, imágenes en sus marcos. Si un texto no cabe, no achiques la letra por debajo
   del estilo: anótalo como `texto largo: recortar N caracteres` para el brief.
4b. **Nombra las capas variables** (ciudad, fecha, dirección, consultora, precio, CTA) con esos
   nombres exactos y una capa de texto por dato, con un solo estilo cada una: es lo que permite
   versionarlas después sin errores de tipografía.
5. **Nombra cada frame** con la convención de
   `references/naming-y-datamerge.md` y la
   etiqueta `BORRADOR` en el nombre de la página.
6. **Anota las reglas aplicadas** en un frame de notas al lado: qué componente se usó, qué estilo, qué
   regla del manual se respetó y qué quedó a decisión de diseño.
7. **Exporta solo si lo piden**: web (PNG o JPG optimizado) o impresión (PDF con sangrado). Para
   impresión, avisa que la conversión a CMYK y la prueba de color las cierra diseño.

## Reglas duras

- **Nunca usa colores, tipografías o logos que no estén en la librería de Viventa**, ni reemplaza una
  tipografía que no carga por otra parecida.
- **Nunca contradice el sistema de diseño**: si una instrucción de esta skill choca con él, gana él.
- **Nunca cambia un texto del brief**, ni para corregirlo: lo anota para que lo corrija quien escribe.
- **Nunca genera imágenes de personas ni renders de proyectos** que pasen por reales: los marcos sin
  foto quedan vacíos con su descripción.
- **Nunca toca componentes ni plantillas maestras** de la librería: trabaja con instancias.
- **Nunca marca una pieza como aprobada** ni la exporta como final: aprobación de marca primero.

## Output esperado

En Figma, la página `BORRADOR · <brief> · <AAAA-MM-DD>` con un frame por formato y un frame de notas.
En disco, `salidas/viventa-piezas-figma/<AAAA-MM-DD>-<brief>.md`:

```
BORRADORES — {brief} · {fecha}
{n} frames · {n} con texto largo · {n} sin imagen
Link a la página: {link}

| Frame | Formato | Componente / maestra | Pendiente para diseño |

Textos que no cupieron (para el brief): {frame}: recortar {n} caracteres.
Siguiente paso: ajuste de diseño → viventa-qa-marca → aprobación de marca.
```

Al terminar, di: *"{n} borradores en la página {nombre}. Quedan para tu ajuste: {resumen de pendientes}."*

## Skills relacionados

- **Antes:** `viventa-brief-requerimiento` (brief con textos finales) o `viventa-pack-evento` (copy por archivo).
- **Después:** `viventa-versionador-piezas` multiplica la pieza aprobada; `viventa-qa-marca` la revisa.
- **En vez de este:** `carrusel-figma` para carruseles orgánicos con guion propio;
  `presentacion-persuasiva-figma` para presentaciones.
