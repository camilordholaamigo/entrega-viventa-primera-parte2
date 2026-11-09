---
name: viventa-datos-publicables
description: >-
  Convierte la información comercial y de proyectos que tienen otras áreas de Viventa (viviendas
  vendidas, negocios cerrados, precios y disponibilidad de proyectos) en la hoja mensual de datos
  publicables: cada dato con fuente, fecha de corte, vigencia, quién lo aprobó y para qué uso sirve.
  Redacta el pedido a cada área dueña y valida lo que vuelve. USA cuando: "necesito las cifras de
  viviendas vendidas para un post", "pásame el precio actualizado del proyecto", "qué datos de ventas
  puedo publicar este mes". Es la fuente: viventa-ficha-proyecto y viventa-versionador-piezas la consumen.
metadata:
  version: "2.0.0"
  domain: "operate"
  client: "viventa"
  blueprint: "marketing"
  complexity: "standalone"
---

# viventa-datos-publicables

Corta la espera de cada mes por los datos que mercadeo necesita y no tiene (ventas, cierres, precios
de proyectos), sin darle a mercadeo acceso a información confidencial. Tres invariantes: **un dato
solo entra a la hoja si el área dueña lo entregó y lo aprobó** (con nombre del área y fecha),
**todo dato tiene fecha de corte y vigencia**, y **lo que no se puede publicar no entra**, aunque
llegue: las cifras internas se quedan internas.

> **Qué cambió en la v2 (feedback del equipo, noviembre de 2026):** *"pedía muchos datos que no
> servían porque no eran compartibles ni oficiales"*. El defecto era de diseño: la skill pedía lo que
> la pieza "necesitaba" sin mirar antes si ese dato **se puede publicar** y **tiene un dueño que lo
> oficialice**, y el área contestaba con cifras que después había que tirar. Ahora (1) antes de pedir
> nada, cada dato pasa por el **catálogo de datos publicables** de abajo: lo que está en la lista roja
> **no se pide**; (2) lo que ya es **público y oficial** (la web de Viventa, Vivehome, la página del
> proyecto en la constructora, el programa del FNA) se toma de ahí, con su URL y fecha, **sin pedírselo
> a nadie**; (3) los precios y la disponibilidad salen de **la vista de Vivehome en Zoho Analytics**
> cuando el usuario la pega o la exporta, que es la fuente oficial; (4) el pedido a las áreas es
> **corto y solo con lo que falta** después de los pasos 1 a 3.

## Catálogo de datos publicables (se mira ANTES de pedir)

| Color | Tipo de dato | Ejemplos | Qué hace la skill |
|---|---|---|---|
| 🟢 **Público y oficial** | Ya está publicado por Viventa o por la fuente dueña | Años de trayectoria, número de bancos y constructoras aliadas, países con oficina, programa Mi Casa en Colombia (FNA), precio "desde" publicado por la constructora o en Vivehome | **Se toma sin pedir**, con URL o fuente y fecha de consulta |
| 🟡 **Publicable si el área lo aprueba** | Agregado de la operación, sin dinero de Viventa | Familias acompañadas en el mes o en el año, viviendas entregadas, proyectos disponibles por ciudad, eventos realizados, países de donde vienen los clientes | **Se pide**, agregado, con fecha de corte y aprobación |
| 🔴 **No publicable — no se pide** | Interno, sensible o no oficial | Ingresos, ventas en dinero, CAC, CPL, leads, tasas de conversión, comisiones, márgenes, metas, rankings internos, tasas de crédito de un banco, casos individuales sin consentimiento, estimaciones de un comercial | **Ni se pide ni se acepta**: si llega en un archivo, se descarta y se cuenta |

Si una pieza "necesita" un dato rojo, la skill no lo pide: propone el dato verde o amarillo que hace
el mismo trabajo (en vez de "vendimos USD 2 M", "acompañamos a {n} familias este año").

## Cuándo usar

- Explícito: "necesito las cifras de viviendas vendidas para un post", "pásame el precio actualizado
  del proyecto", "qué datos de ventas puedo publicar este mes", "actualiza las fichas de proyectos",
  "pídele a comercial los datos del mes para contenidos".
- Implícito: empieza el mes y hay que armar la parrilla o versionar piezas de proyecto.
- Cadencia: mensual (primera semana), y puntual cuando cambia un precio.

## Cuándo NO usar

- La ficha completa de un proyecto para llamada o pieza → `viventa-ficha-proyecto` (con los datos de esta hoja).
- Cruzar proyectos con un lead → `viventa-match-proyecto-lead`.
- Revisar si una cifra está bien usada en una pieza → `viventa-qa-marca`.

## Input del usuario

| Campo | De dónde sale | Si falta, preguntar |
|---|---|---|
| Qué datos se necesitan este mes | Parrilla, eventos y piezas del mes | Se deduce de las piezas del pedido. Si no hay ninguna pista: *"¿Para qué piezas son los datos?"* (una sola pregunta) |
| Vista de Vivehome en Zoho Analytics (export o tabla pegada) | Quien tenga acceso a Zoho Analytics | No se pregunta. Si viene, es **la fuente oficial** de precios y disponibilidad y no se le piden precios a nadie más |
| Hoja del mes anterior | `salidas/viventa-datos-publicables/` | No se pregunta: si existe, se parte de ella y se marcan los datos vencidos |
| Respuestas de las áreas dueñas | Lo que devuelvan comercial y el área de información de proyectos (Excel, export de Vivehome, correo) | No se pregunta en la primera pasada: primero se redactan los pedidos |
| Reglas de cifras | `references/cifras-canonicas.md` | No se pregunta: si no está, los datos agregados de la empresa van `⚠ verificar` |

## Workflow

1. **Lista lo que vence**: datos de la hoja anterior cuya vigencia terminó y precios con más de 30 días de corte.
1b. **Pasa cada dato que se necesita por el catálogo**: 🔴 fuera (y se propone el reemplazo verde o
   amarillo), 🟢 se busca en la fuente pública u oficial y se anota con URL y fecha, 🟡 queda para pedir.
   Si vino la vista de Vivehome en Zoho Analytics, los precios y la disponibilidad salen de ahí.
2. **Redacta un pedido por área dueña, solo con los 🟡 que quedaron** (comercial, información de
   proyectos), con: el dato exacto, el nivel de agregación (total del mes, por país, por proyecto), la
   fecha de corte que se pide, para qué se va a usar y la fecha límite. **Máximo 5 datos por pedido**;
   si son más, se priorizan los de las piezas más cercanas. Un pedido corto que se responda en una tabla.
3. **Cuando llegan las respuestas, valida cada dato**:
   - es agregado y no identifica a ningún cliente (nada de nombres, ciudades + montos que permitan
     reconocer a alguien, ni casos individuales sin consentimiento);
   - trae fecha de corte y quién lo aprobó;
   - no es una cifra interna: ingresos, CAC, comisiones, márgenes, metas, CPL y tasas de conversión
     **no se publican** aunque vengan en el archivo;
   - respeta las reglas de `references/cifras-canonicas.md` (por ejemplo, el +42% es participación del segmento,
     no crecimiento del mercado);
   - precios de proyecto: en la moneda del proyecto, con "desde" si es el menor, y sin tasa, cuota ni
     plazo (eso lo da el asesor en llamada).
4. **Asigna el uso permitido** a cada dato: orgánico, pauta (Colombia, EE. UU., España), impreso. Un
   dato que solo se puede usar en Colombia se marca así.
5. **Escribe la frase publicable sugerida** para los datos de la empresa ("En septiembre acompañamos
   a {n} familias…"), sin adjetivos que el dato no sostenga.
6. **Arma la tabla de proyectos** con lo validado, lista para `viventa-ficha-proyecto` y
   `viventa-versionador-piezas`.

## Reglas duras

- **Nunca pide un dato 🔴** del catálogo, ni le pide a un área lo que ya es público y oficial.
- **Nunca publica ni sugiere un dato que el área dueña no aprobó** (los 🟢 públicos llevan su URL y
  fecha en lugar de aprobación). Lo no aprobado va a `pendiente`.
- **Nunca incluye cifras internas** (ingresos, CAC, comisiones, márgenes, metas, conversiones) en la hoja.
- **Nunca incluye datos que identifiquen a un cliente.**
- **Nunca actualiza un precio por su cuenta** ni lo convierte de moneda.
- **Nunca deja un dato sin fecha de corte**: sin fecha, no entra.

## Output esperado

`salidas/viventa-datos-publicables/<AAAA-MM>.md` y el mismo contenido en
`<AAAA-MM>.csv` para pegar en Sheets:

```
DATOS PUBLICABLES — {mes} · actualizado {fecha}
{n} datos vigentes · {n} pendientes de aprobación · {n} vencidos

Datos de la empresa
| Dato | Color | Valor | Fuente (área o URL) | Aprobó (o "público") | Corte | Vigente hasta | Uso permitido | Frase sugerida |

Proyectos
| Proyecto | Ciudad | Precio desde | Moneda | Rango | Disponibilidad | Corte | Fuente | Aprobó |

Pendientes: {dato} — pedido a {área} el {fecha}.
Descartados por no publicables: {n} (cifras internas recibidas que no entran).
No pedidos por estar en la lista roja: {lista} → reemplazados por: {dato verde o amarillo}.
```

Los pedidos a las áreas se entregan en la conversación, listos para pegar en correo o chat. Al
terminar, di: *"Hoja de {mes} en `<ruta>`: {n} datos listos, {n} pendientes. Los pedidos a
{áreas} están arriba."*

## Skills relacionados

- **Después:** `viventa-ficha-proyecto` (ficha completa del proyecto), `viventa-versionador-piezas`
  (piezas con precio nuevo), `viventa-parrilla-base` y la parrilla orgánica (datos para contenido).
- **Relacionado:** `viventa-qa-marca` revisa el uso de cada cifra en la pieza final.
