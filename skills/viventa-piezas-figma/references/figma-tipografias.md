# Tipografías en Figma — por qué falla con LT Wave y PP Pangram, y cómo no fallar

> Nació del feedback del equipo (noviembre de 2026): *"el multiplicador de piezas da error con Figma
> por dos tipografías: LT Wave y PP Pangram"*. Lo usan `viventa-versionador-piezas` y
> `viventa-piezas-figma`.

---

## 1. Por qué pasa

Cuando Claude cambia un texto en Figma, lo hace con la API de plugins de Figma, y esa API **exige
cargar la tipografía exacta** (familia **y** estilo) de cada capa de texto antes de tocarla. Si la
tipografía no está disponible para el Figma que está corriendo la conexión, el cambio falla con un
error del tipo *"Cannot write to node with unloaded font"* o *"font not found"*. LT Wave y PP Pangram
**no son tipografías de Google**: Figma solo las tiene si están instaladas en esa compu o subidas
como tipografías compartidas de la organización.

Las cuatro causas, de la más común a la menos:

| # | Causa | Cómo se ve |
|---|---|---|
| 1 | La tipografía no está instalada en la compu donde corre Figma (o el instalador de tipografías de Figma no está corriendo, si es Figma en el navegador) | En Figma la capa se ve con la tipografía faltante resaltada; la conexión falla en todas las capas con esa familia |
| 2 | El **nombre del estilo** no coincide: se pidió "SemiBold" y el archivo dice "Semibold", o "Bold" y la familia real es "PP Pangram Sans Rounded" | Falla solo en algunos pesos |
| 3 | Una capa de texto **mezcla** dos tipografías o dos pesos (p. ej. "Te esperamos en **Tampa**") y solo se cargó una | Falla solo en esas capas |
| 4 | La licencia o la versión instalada es distinta de la que usó diseño (otra versión del archivo de la tipografía) | Cambian los nombres de estilo entre una compu y otra |

---

## 2. El chequeo previo (obligatorio antes de duplicar nada)

1. Para cada capa variable de la maestra, **leer la tipografía que ya tiene** (familia y estilo de
   cada tramo de texto) — nunca escribirla a mano ni deducirla del manual.
2. Intentar cargar cada combinación familia + estilo encontrada, una vez.
3. Reportar en una tabla, antes de generar versiones:

   | Capa | Familia | Estilo | ¿Carga? |
   |---|---|---|---|
   | ciudad | LT Wave | Bold | ✅ |
   | fecha | PP Pangram Sans | Semibold | ⛔ no disponible en este Figma |

4. Si **todo carga**: se generan las versiones. En capas con tipografía mezclada, se reemplaza el
   texto **por tramos** para conservar cada estilo; nunca se aplana a un solo estilo.
5. Si **algo no carga**: no se intenta igual, no se cambia la tipografía por otra parecida y no se
   genera ninguna versión con esa capa. Se pasa automáticamente al camino B (§3) y se dice por qué.

---

## 3. Qué hacer cuando no carga

**Camino A — arreglarlo para que la próxima vez funcione** (lo hace diseño o TI, una vez):
1. Instalar LT Wave y PP Pangram (todos los pesos que usa la librería, con su licencia) en la compu
   donde corre Figma, y reiniciar Figma. En Figma en el navegador, además, tener instalado el
   **instalador de tipografías de Figma** (Figma Font Installer / agente de tipografías).
2. Si el equipo tiene plan de organización de Figma, subirlas como **tipografías compartidas** de la
   organización: así funcionan en cualquier compu y para cualquier conexión.
3. Verificar en el selector de tipografías de Figma que los **nombres de estilo** son los mismos que
   en la maestra (los de la tabla del §2).

**Camino B — sacar las versiones igual, hoy** (lo hace la skill automáticamente):
- Entregar el **CSV para el plugin de data merge** que diseño ya usa en su propio Figma (donde las
  tipografías sí están), con una columna por capa variable y el nombre exacto de cada capa.
- Para video: exportar las placas variables desde el Figma de diseño (PNG) y montarlas con ffmpeg, o
  en CapCut con la tabla de tiempos.

**Recomendación de diseño para las maestras** (para Alejandra, una vez): que cada dato variable
(ciudad, fecha, dirección, consultora, precio) viva en **su propia capa de texto con un solo
estilo**. Una capa "Te esperamos en {ciudad} el {fecha}" con dos pesos es la que más falla.
