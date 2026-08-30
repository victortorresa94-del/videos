# Hoja de ruta técnica — de prototipo a mundo abierto AAA

Este documento explica, con honestidad de ingeniería, **cómo se pasa** de la rebanada vertical de este repo a algo que se acerque a la ambición de un GTA medieval, y **cómo se diseña de verdad el "backend"** de un mundo abierto en línea. Nada de esto requiere copiar código de nadie: es arquitectura pública y conocida del sector.

---

## 0. Aclaración sobre "robar el backend de GTA 6"

No se puede y no se debe. El código de GTA 6 es propiedad de Rockstar/Take‑Two, no está publicado, y obtenerlo o reutilizarlo sería ilegal. **Pero no hace falta:** los patrones que hacen funcionar un mundo abierto en línea (streaming del mundo, servidores dedicados, matchmaking, guardado en la nube, economía, anti‑cheat) están documentados públicamente y se pueden diseñar desde cero. El resto de este documento es exactamente eso.

---

## 1. Elección de motor

El navegador (Three.js) es perfecto para **prototipar y validar** — es lo que tienes hoy — pero no para el objetivo final. Opciones reales:

| Motor | Cuándo elegirlo | Notas |
|---|---|---|
| **Unreal Engine 5** | Si el objetivo es la máxima fidelidad gráfica | Nanite (geometría virtualizada) + Lumen (GI) + World Partition (streaming) resuelven de fábrica el "mundo abierto de altos gráficos". Es lo más cercano a la tecnología de un AAA moderno. |
| **Unity** | Si prioriza el móvil y el time‑to‑market | Mejor pipeline móvil histórico, DOTS/ECS para multitudes, enorme ecosistema. |
| **Godot 4** | Si prioriza open source y control | Muy capaz en 3D ahora; menos probado a escala AAA de mundo abierto. |

**Recomendación:** Unreal 5 para PC como plataforma líder, con una **rama de calidad reducida para móvil** (menos densidad, LODs agresivos, iluminación horneada). El prototipo de este repo sirve como **documento vivo de diseño** para reconstruir sistemas en el motor elegido.

## 2. El mundo (cliente)

Lo que hace que un mundo abierto no se caiga:

1. **Streaming por celdas / World Partition.** El mapa se divide en celdas; solo se cargan en memoria las cercanas al jugador. Sin esto, el mundo no cabe en RAM/VRAM. *(El prototipo genera todo de golpe porque es pequeño; a escala real, esto es lo primero que cambia.)*
2. **LOD y *impostors*.** Cada objeto tiene versiones de menos polígonos a distancia; los árboles y edificios lejanos pasan a *billboards*. Nanite automatiza gran parte en UE5.
3. **Generación + edición a mano.** Terreno base procedural (como aquí) **refinado** por diseñadores; los puntos de interés se colocan y guionizan a mano.
4. **Presupuesto de rendimiento por plataforma.** Objetivos claros: p. ej. 60 fps en PC gama media y móvil reciente, con *draw calls*, triángulos y memoria acotados. El *instancing* que ya usa el prototipo para la vegetación es la misma idea, en pequeño.

## 3. Cómo se diseña el **backend** de un mundo abierto en línea

Si algún día "Acero y Corona" tiene un modo reino‑compartido (el equivalente a GTA Online), esta es la arquitectura estándar, por capas:

### 3.1. Topología de red
- **Servidores dedicados y autoritativos.** El servidor es la fuente de verdad de posiciones, combate y economía; el cliente solo predice y se corrige. Esto es lo que evita la mayoría de trampas. (Alternativa peer‑to‑peer: más barata, mucho más vulnerable — no recomendada para economía real.)
- **Sesiones / *shards*.** El mundo se reparte en instancias de N jugadores. Un servicio de **sesión y matchmaking** te asigna a un *shard* con amigos o por región/latencia.
- **Interés (AOI, *area of interest*).** El servidor solo envía a cada jugador lo que está cerca de él; sin esto el ancho de banda explota. Es el equivalente en red del *streaming* del mundo.
- **Replicación y reconciliación.** Estado autoritativo en el servidor + **predicción del lado cliente** + **interpolación/extrapolación** para que se vea fluido pese a la latencia.

### 3.2. Servicios de plataforma (los "microservicios" del backend)
- **Identidad y cuentas** (login, amigos, plataformas cruzadas).
- **Perfil y guardado en la nube**: el progreso vive en el servidor, no en el dispositivo (así el móvil y el PC comparten partida).
- **Economía e inventario**: base de datos autoritativa del oro, objetos y propiedades; **todas** las transacciones se validan en servidor (nunca se confía en el cliente para dar oro).
- **Catálogo / *live‑ops***: eventos, temporadas, tienda, telemetría para equilibrar el juego con datos reales.
- **Matchmaking y presencia**: quién está en línea, en qué *shard*, con quién.
- **Anti‑cheat**: validación en servidor + detección de anomalías (velocidad imposible, oro que aparece de la nada).

### 3.3. Infraestructura
- **Cómputo elástico** para los servidores de juego (contenedores/orquestación que escalan con la demanda; *fleets* de servidores dedicados por región).
- **Bases de datos**: una transaccional para economía/perfil (consistencia fuerte) y una rápida/caché para estado de sesión.
- **Almacenamiento de objetos** para activos y descargas (CDN para parches).
- **Observabilidad**: métricas, trazas y alertas; sin esto no puedes operar un juego vivo.

> Resumen: el "backend de un GTA" no es un secreto único; es **un conjunto de servicios bien conocidos** (identidad, guardado, economía, matchmaking, servidores dedicados autoritativos, anti‑cheat, live‑ops) sobre infraestructura elástica. Se puede construir desde cero, incrementalmente, empezando por lo mínimo (cuentas + guardado en la nube) mucho antes del modo online.

## 4. Pipeline de contenido y activos
- **Modelado + *rigging* + animación** (o captura de movimiento) para personajes y criaturas.
- **Herramientas de mundo** para que los diseñadores coloquen encargos, IA y eventos sin programar.
- **Sistema de misiones/quests** dirigido por datos (scripts o grafos), no *hardcodeado*.
- **Localización** desde el día uno (el prototipo ya está en español; la estructura debe permitir más idiomas).

## 5. Optimización móvil (transversal)
- Presupuestos separados de triángulos, texturas y memoria.
- Iluminación mayormente horneada; sombras dinámicas solo donde importan.
- *Instancing* y *atlasing* agresivos (el prototipo ya instancia vegetación).
- Escalado dinámico de resolución y densidad (el prototipo ya baja calidad al detectar móvil).

## 6. Realismo de equipo, tiempo y coste

Para calibrar expectativas (esto es lo que un estudio pondría en un plan):

| Hito | Alcance | Equipo típico | Orden de tiempo |
|---|---|---|---|
| **M0 — Prototipo jugable** | *Este repo*: mundo, movimiento, combate y misiones básicas | 1 persona | días–semanas |
| **M1 — Vertical slice en motor** | 1 zona pulida en UE5/Unity con arte propio y 1 misión "de escaparate" | 5–15 | 3–6 meses |
| **M2 — Producción de sistemas** | Combate completo, monturas, economía, IA de facciones, 1er tercio del mapa | 30–80 | 12–24 meses |
| **M3 — Contenido + online** | Mapa completo, campaña, backend y reino compartido | 100+ | 2–4 años |

No hay atajo para M2/M3: es lo que cuesta un mundo abierto de verdad. La estrategia sensata es **encadenar hitos jugables** y decidir en cada uno si el resultado justifica seguir invirtiendo.

## 7. Próximos pasos concretos (sin salir de lo barato)

Cosas que sí se pueden hacer ya sobre este prototipo, en orden de valor:

1. **Montura a caballo** (el sistema "vehículo" que define el género).
2. **Herrero y economía** con oro que ya se recoge (comprar/mejorar equipo).
3. **Notoriedad ante la guardia** (el "wanted level" medieval).
4. **Más puntos de interés** con encargos (torneo, escolta, tesoro).
5. **Guardado local** del progreso (precursor del guardado en la nube).
6. **Migración a Unreal 5** cuando el diseño esté validado y se busque fidelidad.

---

*Todo lo anterior es arquitectura y práctica pública del sector, aplicable a un proyecto original. No incorpora ni depende de propiedad intelectual de terceros.*
