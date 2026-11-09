---
name: viventa-corte-video-pauta
description: >-
  Arma el primer corte de los videos de pauta de Viventa a partir del material grabado o de un
  proyecto de CapCut que está en la compu: transcribe, elige la mejor toma de cada línea del guion,
  quita silencios, subtitula y exporta en 9:16, 4:5 y 1:1, más el paquete descargable para seguir
  editando en CapCut (video sin subtítulos, SRT, placas, audio) y la EDL para Premiere o DaVinci.
  Corre con ffmpeg en Claude Code, no en el chat. USA
  cuando: "tengo las grabaciones del evento, sácame los cortes", "edita los cinco videos del
  brief", "ponle subtítulos y sácalo en formatos para pauta", "versiones de 15 y 30 segundos".
  Es postproducción gruesa: viventa-guiones-evento escribe lo que se graba y la productora pule.
metadata:
  version: "2.0.0"
  domain: "produce"
  client: "viventa"
  blueprint: "marketing"
  complexity: "standalone"
---

# viventa-corte-video-pauta

Saca el trabajo mecánico de la edición de los videos de pauta para que la productora de contenidos
—una sola persona para más de 100 videos al mes— pase de editar desde cero a pulir. Tres
invariantes en toda corrida: **el corte nunca cambia lo que se dijo** (se quitan silencios y tomas
fallidas, no palabras que cambien el sentido), **cada decisión queda en la EDL con su motivo**, y
**nada se publica ni se sube a Ads Manager**: el entregable es un borrador para que ella lo termine.

Corre en **Claude Code con ffmpeg** sobre la carpeta de grabaciones. En el chat no edita video: si
alguien lo pide ahí, explica que necesita la carpeta local y ffmpeg, y ofrece la EDL y el SRT a
partir de una transcripción pegada (que sí se pueden importar en CapCut o Premiere).

> **Qué cambió en la v2 (feedback del equipo, noviembre de 2026):** *"en video, poder conectar con el
> archivo de CapCut o si lo tienen otros en la compu; tener el descargable para CapCut"*. Ahora (1) la
> skill puede partir de un **proyecto de CapCut** guardado en el computador (lee qué clips usa y
> dónde están) o de cualquier carpeta de la compu, incluida una compartida; (2) cada video sale además
> como un **paquete para CapCut** (`<video>_capcut.zip`): el corte **sin** subtítulos quemados, el
> `.srt` para importarlos como subtítulos editables, las placas en PNG con fondo transparente, el
> audio normalizado y un `COMO-ABRIR-EN-CAPCUT.md` paso a paso; (3) para quien edite en Premiere o
> DaVinci, una **EDL CMX 3600** que esos programas importan. Detalle: `references/capcut.md`.

## Cuándo usar

- Explícito: "tengo las grabaciones del evento, sácame los cortes", "edita los cinco videos del
  brief", "ponle subtítulos y sácalo en formatos para pauta", "corta este material en versiones de
  15 y 30 segundos", "necesito los videos de optimización para mañana".
- Implícito: terminó el día de grabación de un evento (5 videos, en dólares y en euros) y empieza la
  postproducción de 2 a 3 días.
- Cadencia: por evento (mínimo 3 al mes) y otra vez por cada tanda de optimización.

## Cuándo NO usar

- Todavía no hay guion → `viventa-guiones-evento`.
- Hay que redactar el copy de cada archivo del pack o el CSV de los flyers → `viventa-pack-evento`.
- Cambiar solo las placas de ciudad, fecha o consultora en un video ya editado →
  `viventa-versionador-piezas`.
- Un reel orgánico con edición de retención fina (ritmo, b-roll, efectos) → `retention-edit-plan`.

## Input del usuario

| Campo | De dónde sale | Si falta, preguntar |
|---|---|---|
| Carpeta con las grabaciones **o** proyecto de CapCut | Ruta local (también una carpeta compartida o de red montada en la compu) | Si no se da ninguna, se busca primero la carpeta de proyectos de CapCut del usuario (`references/capcut.md` §1) y se le muestran los proyectos encontrados para que elija uno. Si no hay CapCut ni carpeta: *"¿En qué carpeta están los archivos crudos?"* |
| Los guiones (los 5 del evento o los de optimización) | Salida de `viventa-guiones-evento` o el documento del brief | *"Pásame los guiones: con ellos sé qué toma corresponde a cada línea."* Sin guion se corta solo por silencios y se avisa |
| Evento, ciudad y moneda de cada versión | El brief | *"¿Para qué evento y ciudad es, y qué versiones van en USD y cuáles en EUR?"* |
| Formatos y duraciones | El pedido | No se pregunta: por defecto 9:16, 4:5 y 1:1 a duración completa. 15 s y 30 s solo si los piden |
| Estilo de subtítulos (fuente, color, caja) | Manual de marca o un video aprobado de referencia | *"¿Tienes un video aprobado con los subtítulos de marca? Copio ese estilo."* Si no, estilo neutro (blanco con borde negro) marcado `estilo provisional` |
| Transcripciones (si no hay transcriptor local) | SRT exportado de CapCut o Premiere | Solo si falla el Paso 1: *"No tengo transcriptor instalado. ¿Me exportas el SRT de CapCut de cada clip?"* |

## Workflow

### Paso 0 · Puesta en marcha (solo la primera vez)

1. Verifica `ffmpeg -version` y `ffprobe -version`. Si falta, para y dale el comando de instalación
   de su sistema; la primera instalación se hace acompañada por alguien técnico, como pidió el equipo.
2. Verifica un transcriptor local (`whisper`, `faster-whisper` o `whisper.cpp`). Si no hay, el skill
   funciona con los SRT que exporte CapCut.
3. Prueba con **un solo clip** antes de procesar el evento entero y muéstrale el resultado.

### Paso 0b · Si se parte de un proyecto de CapCut

1. Ubica el proyecto (`references/capcut.md` §1) y lee su archivo de borrador para sacar la lista de
   clips de video y audio con su ruta en disco y, si se puede, los puntos de entrada y salida que ya
   cortó la productora.
2. Si el archivo del borrador está **cifrado** (pasa en versiones recientes de CapCut) o no se puede
   leer, no se fuerza: se usan las **rutas de los medios** que aparecen en la carpeta del proyecto, o se
   le pide a la productora *"exporta los clips o el video desde CapCut a esta carpeta"*. Se dice cuál
   de los dos caminos se tomó.
3. **Nunca se escribe ni se modifica el proyecto de CapCut**: se trabaja sobre copias en la carpeta de
   salida. Un borrador dañado es trabajo de la productora perdido.

### Paso 1 · Inventario y transcripción

1. Lista los clips con `ffprobe`: duración, resolución, orientación, fps y si traen audio.
   Marca los clips sin audio o de menos de 2 s como `descartado: sin audio / falso arranque`.
2. Transcribe cada clip **con marcas de tiempo por palabra**, en español. Guarda un `.json` por clip.
3. Si la confianza del transcriptor es baja en un tramo (ruido, viento), márcalo: esas líneas no se
   eligen solas, van a la EDL como `revisar a mano`.

### Paso 2 · Emparejar tomas con el guion

1. Parte cada guion en líneas (hook, cuerpo, CTA).
2. Para cada línea busca en las transcripciones las tomas que la dicen (coincidencia aproximada de
   palabras, tolerante a muletillas).
3. Elige la mejor toma con este orden de criterios, y escribe cuál pesó:
   1. dice la línea completa, sin cortes ni repeticiones;
   2. no tiene tropiezos ni reinicios en medio ("eh… no, otra vez");
   3. entre tomas igual de limpias, la última (suele ser la que salió bien);
   4. nivel de audio usable (sin picos ni saturación).
4. Si una línea **no tiene ninguna toma limpia**, no la reemplaces con otra frase: márcala
   `falta toma` y sigue. El corte sale con un hueco señalado y la EDL lo dice primero.

### Paso 3 · Recortar silencios sin comerse palabras

1. Detecta silencios con `silencedetect` (punto de partida: -30 dB, 0,35 s; ajústalo si el ambiente es ruidoso).
2. Deja un margen de ~0,12 s antes y después de cada frase para no cortar consonantes.
3. Nunca cortes dentro de una palabra ni quites una palabra que cambie el sentido. En particular,
   **nunca recortes una aclaración legal** ("sujeto a estudio de crédito", "aplican condiciones").

### Paso 4 · Montar, subtitular y exportar

1. Une los tramos en el orden del guion (recorte con recodificación para que los cortes sean exactos).
2. Normaliza el audio para redes (−14 LUFS integrado como referencia).
3. Genera los subtítulos **desde la transcripción de lo que quedó en el corte**, no desde el guion:
   si la persona dijo otra cosa, el subtítulo dice lo que dijo. Máximo 2 líneas, ~32 caracteres por línea.
4. Exporta **dos versiones** de cada formato (1080×1920, 1080×1350, 1080×1080): una con los
   subtítulos quemados en el estilo de marca (para revisar rápido) y una **sin subtítulos** para el
   paquete de CapCut, donde la productora los importa del `.srt` y les pone el estilo de la plantilla.
   Si el material es horizontal, recorta centrado en la cara y avisa qué tomas perdieron encuadre.
   Mantén los subtítulos fuera de las zonas que tapa la interfaz de Reels y Stories
   `[verificar con la guía vigente de zonas seguras de Meta]`.
5. Si piden 15 s y 30 s: conserva hook y CTA, y llena el medio con las líneas de más peso del cuerpo
   hasta la duración. Si no cabe sin cortar una idea a la mitad, entrega la versión más cercana y dilo.

### Paso 4b · Armar el descargable para CapCut (y la EDL para otros editores)

Por cada video, una carpeta `capcut/` y su `.zip` (`references/capcut.md` §2):
- el corte sin subtítulos en cada formato;
- `<video>.srt` con los tiempos del corte;
- las placas variables (ciudad, fecha, dirección, CTA) en PNG transparente si existen, con su tiempo
  de entrada y salida en `COMO-ABRIR-EN-CAPCUT.md`;
- el audio normalizado en `.wav`;
- `COMO-ABRIR-EN-CAPCUT.md`: los pasos para importar todo en CapCut de escritorio y dónde va cada placa;
- `<video>.edl` (CMX 3600) con los cortes contra los clips originales, para Premiere o DaVinci.
  CapCut no importa EDL: para CapCut el camino es el corte + SRT + placas.

### Paso 5 · Revisar antes de entregar

- [ ] Cada video tiene su EDL y cada línea del guion aparece como usada, `falta toma` o `revisar a mano`.
- [ ] La moneda que se dice y la de los subtítulos coinciden con la versión (USD para EE. UU., EUR para España).
- [ ] Ciudad y fecha que se dicen coinciden con el evento del brief.
- [ ] Ningún subtítulo menciona tasa, cuota, plazo o aprobación de crédito que no esté en el guion aprobado.
- [ ] Ningún archivo se llama `final`: todos son `v1-corte`.
- [ ] Cada video tiene su `<video>_capcut.zip` con corte sin subtítulos, SRT, audio, placas (si hay) y
      las instrucciones; el SRT se abrió y sus tiempos coinciden con el corte.
- [ ] Si se partió de un proyecto de CapCut, el proyecto original no se modificó.

## Reglas duras

- **Nunca publica, sube ni programa** nada en Meta, YouTube ni redes. Termina en una carpeta local.
- **Nunca cambia lo que se dijo**: no reordena frases para que digan otra cosa ni quita una negación,
  una condición o una aclaración legal.
- **Nunca inventa una toma** ni rellena una línea faltante con otra frase o con voz sintética.
- **Nunca agrega música ni material de stock**: la música y el pulido (color, animaciones, placas) son
  de la productora.
- **Nunca modifica un proyecto de CapCut** ni un archivo original de la grabación: todo sale en la
  carpeta de salida, como copia.
- **Nunca sube el material a un servicio externo** para transcribir: la transcripción es local o la
  hace CapCut del equipo.
- **Nunca entrega un archivo como final**: el nombre lleva `v1-corte` hasta que la productora lo cierre.

## Output esperado

Una carpeta por evento en
`salidas/viventa-corte-video-pauta/<AAAA-MM-DD>-<evento>/`:

```
<evento>/
  RESUMEN.md                         ← lo primero que se lee
  guion-1/
    guion-1_<ciudad>_<moneda>_9x16_v1-corte.mp4
    guion-1_<ciudad>_<moneda>_4x5_v1-corte.mp4
    guion-1_<ciudad>_<moneda>_1x1_v1-corte.mp4
    guion-1.srt
    guion-1_EDL.csv                  ← línea, clip, entrada, salida, criterio, nota
    guion-1.edl                      ← CMX 3600, para Premiere / DaVinci
    capcut/                          ← el descargable para seguir en CapCut
      guion-1_9x16_sin-subtitulos.mp4 (y 4x5, 1x1)
      guion-1.srt
      guion-1_audio.wav
      placas/*.png
      COMO-ABRIR-EN-CAPCUT.md
    guion-1_capcut.zip               ← la carpeta capcut/ comprimida, para mandar o descargar
  guion-2/ …
  transcripciones/                   ← un .json por clip
```

Si existe la matriz de `viventa-pack-evento`, los archivos llevan su nombre normalizado en vez del de arriba.

`RESUMEN.md` abre con lo que necesita atención:

```
CORTE v1 — {evento} · {ciudad} · {fecha}
{n} videos cortados · {n} líneas sin toma limpia · {n} tramos para revisar a mano

Atención primero
- guion-3, línea 4 (CTA): falta toma. Opciones: regrabar o usar la toma 2 con el tropiezo en 00:07.
- guion-5: material horizontal; en 9:16 la toma 3 pierde la mano que señala el flyer.

Por video: duración, formatos exportados, tomas usadas, criterio que más pesó.
Pendiente de la productora: color, música, placas del flyer, animaciones, revisión final.
```

Al terminar, di: *"Cortes v1 listos en `<ruta>`. {n} líneas necesitan tu decisión (están arriba en
RESUMEN.md). Nada se subió a ningún lado."*

## Skills relacionados

- **Antes:** `viventa-guiones-evento` (lo que se graba) y `viventa-pack-evento` (el nombre de cada archivo).
- **Después:** `viventa-versionador-piezas` para las placas por ciudad, consultora o fecha;
  `viventa-qa-marca` para la revisión de marca antes de mandarlo a la agencia de pauta.
- **En vez de este:** `retention-edit-plan` y `retention-editor` cuando es un video orgánico que
  necesita plan de retención, no un primer corte de pauta.
