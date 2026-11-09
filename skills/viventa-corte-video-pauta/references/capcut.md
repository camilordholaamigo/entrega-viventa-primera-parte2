# CapCut — leer un proyecto de la compu y entregar el descargable

> Nació del feedback del equipo (noviembre de 2026): *"en video, poder conectar con el archivo de
> CapCut o si lo tienen otros en la compu; tener el descargable para CapCut"*. Lo usa
> `viventa-corte-video-pauta` en los pasos 0b y 4b.

---

## §1. Dónde guarda CapCut los proyectos

CapCut de escritorio guarda cada proyecto (borrador) como una carpeta. Ubicación por defecto
`[⚠ verificar en la versión de CapCut del equipo — se puede cambiar en Configuración → Ubicación de
los borradores]`:

| Sistema | Carpeta por defecto |
|---|---|
| Windows | `%LOCALAPPDATA%\CapCut\User Data\Projects\com.lveditor.draft\<nombre del proyecto>\` |
| macOS | `~/Movies/CapCut/User Data/Projects/com.lveditor.draft/<nombre del proyecto>/` |

Dentro de cada proyecto:
- `draft_content.json` (en algunas versiones de Mac, `draft_info.json`): la línea de tiempo — qué
  clips, en qué orden, con qué recortes. **En versiones recientes puede venir cifrado**: si al
  abrirlo no es JSON legible, no se intenta descifrar.
- `draft_meta_info.json`: nombre del proyecto, fecha y lista de medios importados con su ruta.

**Cómo se usa:**
1. Si el usuario no da una ruta, se lista la carpeta de proyectos (nombre y fecha de cada uno) y se
   le pide que elija. Nunca se abre el proyecto en CapCut ni se escribe dentro de su carpeta.
2. Si el borrador es legible: se sacan los clips con su ruta y los tramos ya cortados (entrada y
   salida). Así el corte respeta lo que la productora ya decidió.
3. Si no es legible: se toman las rutas de los medios de `draft_meta_info.json`; si tampoco, se pide
   exportar los clips o el video desde CapCut a una carpeta.
4. **Otras personas y otras compus:** si la grabación está en una carpeta compartida (Drive para
   escritorio, OneDrive, una unidad de red), basta con la ruta montada en esta compu. Un proyecto de
   CapCut **no se puede abrir desde otra compu** copiando solo la carpeta del borrador: los medios
   apuntan a rutas de la máquina original. Por eso el descargable (§2) lleva los medios adentro.

---

## §2. El descargable para CapCut (`<video>_capcut.zip`)

```
<video>_capcut/
  <video>_9x16_sin-subtitulos.mp4
  <video>_4x5_sin-subtitulos.mp4
  <video>_1x1_sin-subtitulos.mp4
  <video>.srt                     ← subtítulos con los tiempos del corte
  <video>_audio.wav               ← audio normalizado (−14 LUFS de referencia)
  placas/                         ← PNG con fondo transparente, 1 por placa variable
  COMO-ABRIR-EN-CAPCUT.md
```

### Plantilla de `COMO-ABRIR-EN-CAPCUT.md`

```markdown
# Cómo abrir {video} en CapCut

1. Descomprime este .zip en tu compu (no lo abras desde el .zip).
2. CapCut → Nuevo proyecto → Importar → elige `{video}_9x16_sin-subtitulos.mp4` (o el formato que
   vayas a terminar) y arrástralo a la línea de tiempo.
3. Subtítulos: Texto → Subtítulos → Importar subtítulos locales → elige `{video}.srt`.
   Aplícales el estilo de la plantilla de marca. [⚠ el nombre exacto del menú cambia entre
   versiones de CapCut]
4. Placas: arrastra cada PNG de `placas/` a una pista encima del video, en estos tiempos:
   | Placa | Entra | Sale |
   |---|---|---|
   | {placa} | {mm:ss.ff} | {mm:ss.ff} |
5. Lo que falta y es tuyo: color, música, animaciones, revisión final.
6. Exporta con el nombre de la matriz del pack (`v2` cuando lo cierres; nunca "final" sin aprobación).
```

### Reglas
- El corte del paquete es **el mismo** que el de la versión con subtítulos quemados: solo cambia que
  no lleva subtítulos. Si no coinciden, es un error.
- Los tiempos del `.srt` se calculan **sobre el corte**, no sobre los clips crudos.
- El `.zip` no lleva los clips crudos (pueden pesar gigas); si la productora los necesita, la EDL dice
  de qué clip y de qué segundo sale cada tramo.

---

## §3. EDL CMX 3600 (para Premiere, DaVinci y otros editores)

CapCut **no importa EDL**; Premiere Pro y DaVinci Resolve sí. Un evento por tramo, con el nombre del
clip original en el comentario, a 30 fps salvo que el material diga otra cosa:

```
TITLE: guion-1_v1-corte
FCM: NON-DROP FRAME

001  AX       V     C        00:00:12:05 00:00:15:20 00:00:00:00 00:00:03:15
* FROM CLIP NAME: C0012.MP4
002  AX       V     C        00:01:02:10 00:01:09:00 00:00:03:15 00:00:10:05
* FROM CLIP NAME: C0015.MP4
```

Al importar, el editor pide la carpeta de los clips originales para reconectarlos.
