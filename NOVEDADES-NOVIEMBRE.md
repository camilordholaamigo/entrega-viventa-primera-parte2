# Novedades de noviembre de 2026 — lo que cambió con su feedback

Esta versión responde, punto por punto, a lo que el equipo nos dijo después de usar las skills. Abajo
está cada comentario tal como llegó, qué cambiamos, **cómo probarlo hoy** (un pedido para copiar y
pegar en Claude) y qué esperar. Al final, lo que todavía no se puede hacer y por qué.

**Antes de empezar:** vuelvan a subir **las 28** skills (o a correr el instalador). Aunque el
feedback tocó 15, en las 28 corregimos el tono: varias hablaban de «vos» («podés», «calificás») y
la voz de Viventa es de **tú**. La versión nueva reemplaza a la anterior.

---

## Resumen en una tabla

| # | Lo que nos dijeron | Skill | Qué cambió |
|---|---|---|---|
| 1 | No ayuda a hacer varios eventos al mismo tiempo | `brief-evento`, `guiones-evento`, `pack-evento`, `comunicaciones-campana`, `monitor-evento` | Pegan varios eventos y sale el lote: tabla, calendario de producción con choques y qué se reutiliza |
| 2 | El evento virtual no deja cambiar la dirección | Las mismas cinco | Formato presencial · virtual · híbrido. En virtual: plataforma, enlace y hora en cada país; nunca pide dirección |
| 3 | En presenciales pide datos que podría investigar, y mezcla preguntas de CRM | `brief-evento`, `guiones-evento`, `pack-evento`, `comunicaciones-campana` | Busca sola la dirección del venue, cómo llegar, festivos y zonas horarias. Ya no pide ejecutivas, presupuesto ni conteos de Zoho para producir piezas |
| 4 | La parte creativa de noviembre (compra conjunta + mejores tasas) debía dar 5 videos, 5 carruseles y 5 estáticas, y se queda pegada en nuestras ideas | `inteligencia-campana` | Campañas de **tema** (uno o varios). 5 + 5 + 5 escritas completas, 4 ángulos por tema y 2 propuestas libres |
| 5 | El copy es de baja calidad: investiguen qué funciona hoy en Instagram | Todas las que escriben copy | Un estándar común: investiga hooks en la Biblioteca de anuncios de Meta, reels y TikToks, y pasa un test de mediocridad |
| 6 | Correo y WhatsApp mediocres: tienen que ser persuasivos, iterativos y con CTA claros | `comunicaciones-campana` | Hook en la primera línea, un CTA por pieza, variante B de cada mensaje, plan de pruebas y bitácora |
| 7 | Que sirva para campañas generales, como campañas de interés | `comunicaciones-campana` | Modo **campaña general**: 14 días, 5 correos + 4 WhatsApp, sin evento |
| 8 | La parrilla hace preguntas innecesarias | `parrilla-base` | No pregunta nada antes de empezar. Sin conteos, arma la parrilla por segmentos y deja la columna para Zoho |
| 9 | El kit de referidores hacía muchas preguntas al inicio | `kit-referidor` | Ninguna pregunta al arrancar. Lo que falte va en un solo bloque al final |
| 10 | No se ve la diferencia entre la skill de Figma y el sistema de diseño | `piezas-figma` | Ahora está dicha: el sistema de diseño son las reglas; la skill es la producción en lote que las obedece |
| 11 | El multiplicador de piezas falla en Figma con LT Wave y PP Pangram | `versionador-piezas`, `piezas-figma` | Chequeo de tipografías antes de tocar nada; si no cargan, saca las versiones por CSV y dice cómo instalarlas |
| 12 | Proyectos: el proceso es manual y depende de otros; conectar con Vivehome en Zoho Analytics y avisar a Aleja | `ficha-proyecto` | Modo Vivehome: lee la vista de Zoho Analytics, detecta novedades y deja listo el aviso para Aleja, que aprueba el diseño |
| 13 | Video: conectar con el archivo de CapCut y tener el descargable para CapCut | `corte-video-pauta` | Lee un proyecto de CapCut de la compu y entrega un .zip para seguir en CapCut (video sin subtítulos, SRT, placas, audio) |
| 14 | La reactivación solo genera un mensaje, el copy es genérico y no tiene el tono de Viventa; el canal es Instagram y debería servir para TikTok | `reactivacion-lead` | Siempre la secuencia completa con variante B. Instagram por defecto, TikTok con su propio formato, test de tono Viventa |
| 15 | Datos publicables pedía datos que no se podían compartir y no eran oficiales | `datos-publicables` | Catálogo verde, amarillo y rojo: lo rojo no se pide, lo público se toma sin preguntar |
| 16 | El radar no fue proactivo: tocó darle los competidores. Debería dar acciones, ser recurrente y analizar el orgánico | `radar-competencia` | Descubre sola a la competencia, analiza su orgánico y su pauta, y abre con 3-5 acciones con responsable |

---

## 1 y 2 · Varios eventos a la vez, y eventos virtuales

**Lo que nos dijeron:** *«El agente no ayuda a hacer múltiples eventos al mismo tiempo»* y *«el evento
virtual no deja cambiar la dirección porque solo funciona para presenciales»*.

**Qué cambió**
- Si el mensaje trae dos o más eventos, la skill lo detecta sola y entrega el **lote**, con hasta 10
  eventos por vez:
  1. una tabla con todos: fecha, días que faltan, semáforo y qué falta de cada uno;
  2. el **calendario de producción**, que marca cuando dos eventos se pisan en la semana de Johana y
     Alejandra y da las salidas posibles;
  3. qué se produce una sola vez para todos (los 5 conceptos de video, los carruseles de marca) y qué
     cambia por evento (el cierre del video con ciudad y fecha, el flyer, la placa);
  4. una tarjeta por evento y **un solo mensaje por persona** con lo que falta pedir.
- **Formato** presencial, virtual o híbrido. En un virtual:
  - nunca se pide dirección;
  - va la plataforma (Zoom, Meet, Instagram Live…) y el enlace como `{LINK_REGISTRO}`, que se llena
    cuando exista la landing;
  - **la hora se calcula para cada país**, teniendo en cuenta los cambios de horario (ejemplo: jueves
    20 de noviembre a las 7:00 p. m. en Miami es la 1:00 a. m. del viernes en Madrid, y la skill avisa
    que esa hora es mala para España);
  - los recordatorios son 24 h antes, 1 h antes y «estamos en vivo».
- Sin dirección, los guiones y la matriz de producción **se escriben igual**. Solo el cierre (la voz
  en off con la dirección) queda marcado como pendiente: es justo lo que se graba aparte.

**Pruébenlo así**
> Arma los briefs de estos eventos de diciembre: Open House en Miami el 6 de diciembre en la
> oficina; Más Cerca de Ti en Alicante el 13 con Carolina y Mirley en el Hotel Meliá Alicante; y un
> webinar por Zoom el 10 de diciembre a las 7 pm hora de Miami para colombianos en Estados Unidos y
> España.

**Qué esperar:** una tabla de tres eventos, el aviso de que el webinar cae de madrugada en España, la
dirección del Meliá buscada y marcada como «investigado — confirmar», y un solo bloque final con lo
que hay que pedirle a cada persona.

---

## 3 · Investigar en vez de preguntar, y nada de preguntas de CRM para producir

**Lo que nos dijeron:** *«En eventos presenciales pide datos muy específicos al equipo que debería
poder investigar o evitar preguntar; está mezclando preguntas como de CRM que no son necesarias para
la creación de las piezas».*

**Qué cambió**
- **Lo que es público, la skill lo busca:** la dirección a partir del nombre del venue, cómo llegar,
  parqueadero, festivos o partidos de la Selección que compiten con la fecha, la zona horaria y el
  tamaño de la comunidad colombiana de la ciudad. Lo que encuentra va marcado
  `[investigado — confirmar]`.
- **Lo que es de CRM, no se pregunta para producir piezas:** número de ejecutivas, meta de leads,
  presupuesto de pauta, conversiones, conteos de Zoho. Las metas del brief son **opcionales**: salen
  si las piden («con metas») o si el mensaje trae los datos.
- **Lo que solo el equipo sabe** (fecha y hora reales, si el venue está confirmado, consultoras,
  montos de la pieza) se pide al final, **un mensaje por persona**, nunca como ronda de preguntas
  antes de empezar.

---

## 4 · La parte creativa: 5 videos, 5 carruseles y 5 estáticas, más creativa

**Lo que nos dijeron:** *«La parte creativa de noviembre era compra conjunta y el otro proyectos con
las mejores tasas posibles; debería crear 5 videos, 5 carruseles y 5 piezas estáticas, hacerlo más
flexible y creativo, porque se queda pegado con las ideas principales que le dimos».*

**Qué cambió** (skill `viventa-inteligencia-campana`)
- Acepta **campañas de tema**, no solo de evento, y **varios temas a la vez**.
- El paquete por defecto es **5 videos + 5 carruseles + 5 estáticas**, y cada pieza sale **escrita
  completa**:
  - video: hook de 0-3 s, guion por tiempos, texto en pantalla, CTA y hook B;
  - carrusel: el texto de cada lámina y el caption;
  - estática: titular, titular B, bajada, prueba, CTA y caption.
- **Para no quedarse pegada:** explora al menos **4 ángulos por tema** (miedo desactivado, dolor,
  deseo, contraintuitivo, error común, caso, comparación, momento) y entrega **2 propuestas libres**
  rotuladas «no estaba en el pedido», que ustedes deciden si entran. Los formatos de carrusel no se
  repiten más de dos veces.
- **«Mejores tasas» sin meterse en problemas legales:** habla del proceso («comparamos 5 bancos para
  buscarte la mejor tasa disponible para tu perfil»), nunca de un número de tasa. En pauta a Estados
  Unidos, un número de tasa obliga a publicar todos los términos del crédito (Reg Z). Validen la
  fórmula con Crédito antes de pautar.

**Pruébenlo así**
> Arma la parte creativa de diciembre: compra conjunta (hermanos o parejas que compran juntos) y
> proyectos con las mejores tasas. Necesito 5 videos, 5 carruseles y 5 estáticas. Público en Estados
> Unidos y España.

---

## 5 y 6 · Copy que convierte: investigado, persuasivo y que se prueba

**Lo que nos dijeron:** *«La calidad del copy está muy baja; deberíamos investigar redes como
Instagram para buscar los hooks y el copy que está funcionando hoy»* y *«en correo y WhatsApp el copy
es súper mediocre: tiene que ser persuasivo, algo que estemos testeando constantemente, con llamados a
la acción claros».*

**Qué cambió:** un **estándar de copy** común a todas las skills que escriben (está en
`references/copy-que-convierte.md` de cada una):
1. **Investiga antes de escribir**, con un tope de 6 búsquedas para no agotar la cuenta compartida:
   - la Biblioteca de anuncios de Meta (los anuncios que la competencia lleva más de 30 días
     corriendo);
   - reels y TikToks recientes del tema;
   - creadores de finanzas para migrantes y los comentarios de esas publicaciones.

   Si ustedes pegan referentes que les gustan (capturas, enlaces), empieza por esos. **Todo referente
   va con su enlace**: lo que no se pudo abrir no se cita.
2. **Ángulos:** al menos 4 por tema y ninguno repetido en el mismo set.
3. **Hooks:** 12 fórmulas probadas, adaptadas a Viventa, y 3 hooks por pieza.
4. **Cuerpo:** dolor o miedo, plan, prueba y por qué Viventa. **El miedo no se nombra: se desactiva con
   prueba** (20 años, 5 bancos, oficinas físicas, una asesora con nombre).
5. **Un CTA por pieza**, con verbo y con lo que pasa después («Responde SÍ y te llamo mañana»,
   «Comenta CASA y te mando la guía»).
6. **Variante B de cada pieza**, que cambia una sola cosa (el hook o el CTA), con la métrica que
   decide (apertura, respuesta, CPL).
7. **Bitácora:** cada entrega cierra con una tabla para que anoten qué ganó. **Péguenla la próxima vez
   que corran la skill**: la ganadora pasa a ser el control y la nueva prueba compite contra ella. Así
   el copy mejora campaña a campaña en vez de empezar de cero.
8. **Test de mediocridad** antes de entregar. Cada pregunta, si falla, obliga a reescribir:
   - ¿lo podría firmar la competencia?
   - ¿la primera línea detiene el dedo?
   - ¿hay al menos dos datos concretos?
   - ¿suena a persona?
   - ¿aparece alguna frase de la lista negra («el hogar de tus sueños», «no te lo pierdas»,
     «oportunidad única»…)?

---

## 7 · Campañas generales (de interés), no solo eventos

**Qué cambió** (skill `viventa-comunicaciones-campana`): además del modo evento, hay un modo
**campaña general** de 14 días, con 5 correos y 4 WhatsApp. El objetivo es una conversación o una
llamada agendada. Quien responde sale de la secuencia y pasa a la asesora. Sirve para compra conjunta,
proyectos con mejores tasas, un lanzamiento, Mi Casa en Colombia o una temporada.

**Pruébenlo así**
> Hazme la secuencia de correos y WhatsApp para la campaña de compra conjunta, para la base de
> Estados Unidos.

---

## 8 · Parrilla sin preguntas innecesarias

**Lo que nos dijeron:** *«La skill de parrilla tiene falencias: pide muchas cosas, preguntas
innecesarias, irrelevantes o excesivas».*

**Qué cambió:** no pregunta **nada** antes de empezar.
- Si pegan el export de Zoho, lo usa.
- Si no, arma la parrilla **por segmentos definidos**: la regla de filtro de cada segmento y una
  columna «Conteo (lo pone Zoho)» para quien programa.
- Lo que mejoraría el plan va al final, en un solo bloque, como sugerencia y no como condición.
- Los correos salen con asunto B para probar.

---

## 9 · Kit de referidores sin ronda de preguntas

**Qué cambió:** antes había cuatro datos que detenían la skill. Ahora deduce lo que puede del mensaje,
usa valores prudentes para lo demás y lo dice. Por ejemplo, si no sabe la relación del referidor con
Viventa, asume «conocido», para no ponerle en la boca una experiencia que no tuvo. Lo que falta para
dejar el kit enviable va en un solo bloque al final. El kit **siempre sale**.

---

## 10 y 11 · Figma: la diferencia con el sistema de diseño, y las tipografías LT Wave y PP Pangram

**Sistema de diseño frente a skill.** El sistema de diseño que armó Alejandra (más la librería y las
maestras de Figma) son **las reglas**: cómo se ve Viventa. `viventa-piezas-figma` es **la producción en
lote** que obedece esas reglas:
- todas las piezas de un brief por todos los formatos, en una pasada;
- las zonas seguras de Meta en cada formato;
- los textos que no caben, devueltos al brief;
- las capas variables nombradas para versionar después.

Si las dos chocan, **gana el sistema de diseño**. Para una pieza suelta y genérica, usen el sistema de
diseño; para un brief o un pack con muchas piezas, la skill.

**El error de tipografías.** Pasa porque, para cambiar un texto en Figma, Claude tiene que cargar la
tipografía exacta de esa capa. LT Wave y PP Pangram no son tipografías de Google: si no están
instaladas en la compu donde corre Figma, o si el nombre del estilo no coincide («SemiBold» frente a
«Semibold»), falla. Ahora:
1. antes de tocar nada, la skill revisa la tipografía de cada capa y dice cuál carga y cuál no;
2. si alguna no carga, **no la reemplaza por otra** y saca las versiones por el **CSV del plugin de
   data merge** que Alejandra ya usa en su Figma, donde las tipografías sí están.

**Para que no vuelva a pasar** (una vez, lo hace diseño o TI):
- instalar todos los pesos de LT Wave y PP Pangram en la compu donde corre Figma, y en Figma web
  también el instalador de tipografías de Figma;
- o, si tienen plan de organización, subirlas como tipografías compartidas;
- en las maestras, poner cada dato variable (ciudad, fecha, precio…) en **su propia capa de texto con
  un solo estilo**.

---

## 12 · Proyectos conectados a Vivehome en Zoho Analytics, con aviso a Aleja

**Lo que nos dijeron:** *«El proceso se vuelve muy manual porque depende de otros… tenemos Zoho
Analytics; ella tiene que ser la que apruebe el diseño; necesitamos que consuma de Vivehome, que está
en Zoho Analytics, y le avise a Aleja por novedades».*

**Qué cambió:** `viventa-ficha-proyecto` tiene un **modo Vivehome**.
1. **Lee la vista de Vivehome** en Zoho Analytics por la primera vía disponible:
   - un conector de Zoho Analytics en Claude, si su plan lo tiene;
   - el **enlace publicado de la vista en CSV**;
   - el export pegado.
2. **La compara con la lectura anterior** y saca las novedades: proyectos nuevos, precios que
   cambiaron, proyectos agotados, fechas de entrega que se movieron, proyectos que salieron del
   inventario.
3. Por cada novedad prepara:
   - la ficha;
   - la **fila de variables** para `viventa-versionador-piezas`;
   - la lista de piezas que hay que actualizar o pausar.
4. Deja listo **el aviso para Alejandra**, que **es quien aprueba el diseño**, para pegarlo en el canal
   del equipo.
5. A comercial solo le pide lo que Vivehome no tiene, en un solo mensaje.

**Para dejarlo andando:**
- en Zoho Analytics, publiquen la vista de Vivehome con un enlace privado de exportación en CSV y
  guárdenlo; ese enlace es lo que se le pega a Claude cada semana;
- la skill entrega un archivo `vivehome-ultima-lectura.csv`: guárdenlo y péguenlo la semana siguiente,
  porque es la base de la comparación.

**Pruébenlo así**
> Revisa las novedades de Vivehome con este export [adjunto] contra el de la semana pasada [adjunto] y
> prepárale el aviso a Aleja.

---

## 13 · Video: CapCut de ida y vuelta

**Lo que nos dijeron:** *«En video, poder conectar con el archivo de CapCut o si lo tienen otros en la
compu; tener el descargable para CapCut».*

**Qué cambió** (skill `viventa-corte-video-pauta`, que corre en Claude Code con ffmpeg):
- **Entrada:** puede partir de un **proyecto de CapCut** guardado en la compu (busca la carpeta de
  proyectos de CapCut y les muestra cuáles hay) o de cualquier carpeta, incluida una compartida.
  **Nunca modifica el proyecto de CapCut**: trabaja sobre copias.
- **Salida: el descargable para CapCut**, un `.zip` por video con:
  - el corte **sin** subtítulos en 9:16, 4:5 y 1:1;
  - el `.srt` para importarlos como subtítulos editables;
  - las placas en PNG transparente;
  - el audio normalizado;
  - un `COMO-ABRIR-EN-CAPCUT.md` paso a paso, con los tiempos de cada placa.
- Para quien edite en Premiere o DaVinci, una **EDL** que esos programas importan. CapCut no importa
  EDL; para CapCut el camino es el .zip.

---

## 14 · Reactivación: secuencias completas, Instagram y TikTok, tono Viventa

**Lo que nos dijeron:** *«Solo me genera uno, no una secuencia; los mensajes son malos, el copy está
súper genérico y mediocre, no toma el tono de Viventa. El canal de reactivación es más hacia
Instagram, pero debería poder hacerse para TikTok».*

**Qué cambió** (skill `viventa-reactivacion-lead`)
- **Siempre sale la secuencia completa**, con el día de cada toque y una variante B de cada uno.
  - Para un lead frío ya no es un solo mensaje por trimestre: es una **campaña de 3 toques en 14
    días**: algo útil primero, después prueba, y al final una salida digna que pide permiso.
- **Instagram es el canal por defecto.** La secuencia combina DM, una historia con sticker de pregunta
  para todos los que se enfriaron y la salida por DM.
- **TikTok tiene su propio formato:**
  - respuesta en video al comentario de la persona (con guion de 20 s);
  - DM solo si la persona escribió o lo permite;
  - un video fijado que responde las preguntas que más se repiten, con palabra clave.
- **Test de tono Viventa** en cada toque: patrimonio y familia, nunca «inversión» ni «oportunidad»;
  una prueba que no se puede fingir; el miedo desactivado sin nombrarlo; la persona como protagonista;
  y la pregunta de si lo podría firmar otro broker.
- Ya no pregunta el canal, el tipo ni el número de intento: los deduce y dice qué asumió.

**Pruébenlo así**
> Necesito reactivar por Instagram a los leads que preguntaron por proyectos en Pereira hace unos
> meses y no siguieron. Hazme la secuencia.

---

## 15 · Datos publicables: solo lo que se puede compartir y es oficial

**Lo que nos dijeron:** *«Pedía muchos datos que no servían porque no eran compartibles ni
oficiales».*

**Qué cambió:** antes de pedir nada, cada dato pasa por un **catálogo**:

| Color | Qué es | Qué hace la skill |
|---|---|---|
| 🟢 | Público y oficial: años de trayectoria, bancos y constructoras aliadas, el programa del FNA, el precio «desde» publicado | Lo toma sin pedirlo, con enlace y fecha |
| 🟡 | Publicable con aprobación: familias acompañadas, viviendas entregadas | Lo pide, agregado y con fecha de corte |
| 🔴 | Ingresos, ventas en dinero, CAC, CPL, conversiones, comisiones, metas, tasas | **No se pide** y, si llega, se descarta |

Si una pieza «necesita» un dato rojo, propone el verde o amarillo que hace el mismo trabajo. Los
precios salen de la vista de Vivehome en Zoho Analytics cuando la pegan. Cada pedido a un área lleva
máximo 5 datos.

---

## 16 · Radar de competencia proactivo, con acciones y recurrente

**Lo que nos dijeron:** *«Le tocó darle toda la información de los competidores, no fue proactivo;
debería brindar acciones realizables y generar un reporte recurrentemente; idealmente análisis del
contenido orgánico».*

**Qué cambió** (skill `viventa-radar-competencia`)
- **No pregunta quiénes son los competidores.** Arranca con los brokers conocidos y **descubre** a los
  que están pautando o publicando para el mismo público: busca en la Biblioteca de anuncios de Meta
  por palabras clave del negocio y en Instagram y TikTok. Los nuevos aparecen como «nuevo en el radar»,
  con su enlace.
- **Analiza el contenido orgánico** de cada competidor y el de Viventa:
  - cuánto publican y en qué formatos;
  - de qué hablan;
  - sus 3 publicaciones con más interacción visible, con el hook y el enlace;
  - qué pregunta la gente en sus comentarios.
- **Analiza la pauta:** anuncios activos y cuáles llevan más de 30 días (los que sostienen), con su
  hook y su CTA.
- **La página 1 son acciones**, entre 3 y 5. Cada una dice qué hacer, por qué (con evidencia), quién
  la hace, con qué skill y cuánto esfuerzo toma.
- **Es recurrente:** cada radar compara con el anterior y hace **seguimiento de las acciones** del
  anterior («¿se hizo?»). Córranlo el primer lunes de cada mes pegando el radar anterior; en temporada
  de eventos, cada quince días.

**Pruébenlo así**
> Radar de competencia de noviembre. Este es el radar de octubre [pegar o adjuntar].

---

## Lo que todavía no se puede (dicho de frente)

- **Instagram por dentro:** Claude no entra a Instagram con su sesión ni ve métricas privadas
  (alcance, guardados, CTR de otros). Ve lo público y lo cita con enlace. Las métricas propias salen de
  `viventa-reporte-campanas`.
- **Enviar o publicar:** ninguna skill envía correos, WhatsApp ni DMs, ni publica. Todo termina en un
  documento para revisión humana. Es una regla que puso el equipo.
- **Zoho Analytics en vivo:** la lectura automática depende de que la vista esté publicada con enlace
  o de que su plan de Claude tenga un conector de Zoho. Si no, se pega el export; funciona igual, solo
  que a mano.
- **Que corra solo cada semana o cada mes:** en Claude (chat) hay que correrla cada vez. Para que corra
  sola con horario, tiene que ser desde Claude Code o ENSŌ con una tarea programada.
- **Proyectos de CapCut cifrados:** las versiones recientes de CapCut pueden guardar el proyecto
  cifrado. En ese caso la skill usa los medios del proyecto o pide exportar los clips, y lo dice.
- **Video en el chat:** cortar video necesita Claude Code con ffmpeg en la compu. En el chat, la skill
  entrega la EDL y el SRT a partir de una transcripción.

## Orden recomendado para probar (una tarde)

1. `viventa-inteligencia-campana` con la parte creativa de diciembre (punto 4).
2. `viventa-comunicaciones-campana` en modo campaña general con el mismo tema (punto 7).
3. `viventa-brief-evento` con dos o tres eventos reales, uno virtual (puntos 1 y 2).
4. `viventa-reactivacion-lead` por Instagram (punto 14).
5. `viventa-radar-competencia` (punto 16).

Cuando algo no salga como esperan, mándennos **el pedido que escribieron y lo que devolvió**. Con eso
lo ajustamos.
