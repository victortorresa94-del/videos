# Autopsia de Mordor — análisis a fondo de los juegos de la Tierra Media en PS4

*Middle-earth: Shadow of Mordor (Monolith, 2014) y Middle-earth: Shadow of War (Monolith, 2017) — los dos únicos juegos de El Señor de los Anillos en PS4 y la referencia de jugabilidad de «Acero y Corona». Versión 1.0 · fuentes al final.*

---

## 0. Ficha y por qué estos dos

| | Shadow of Mordor (2014) | Shadow of War (2017) |
|---|---|---|
| Estudio | Monolith Productions | Monolith Productions |
| Mundos | 2 zonas abiertas (Udûn, Núrn) | 5 regiones + fortalezas asediables |
| Rendimiento PS4 | 1080p/30 estable | 1080p/30 estable (Pro: ~1620p dinámico) |
| Metacrítica | ~84 | ~80 |
| Legado | Sistema Nemesis (patentado por WB en 2021) | Nemesis expandido + asedios |

Son la referencia correcta para nosotros por tres razones: (1) es la traducción más lograda del **combate de masas tercera persona** a un mundo de fantasía; (2) su mundo se siente **habitado por enemigos con identidad**, no por relleno; (3) lo lograron con un presupuesto de consola de 2014 — técnicas alcanzables, no magia.

---

## 1. La tesis de diseño

Los dos juegos se sostienen sobre UNA fantasía: **"soy letal, y el mundo me recuerda"**. Todo sistema sirve a una de las dos mitades:
- *Soy letal* → combate freeflow donde un jugador hábil no es tocado jamás.
- *El mundo me recuerda* → Nemesis: los enemigos tienen nombre, memoria y cicatrices tuyas.

Lección para «Acero y Corona»: nuestra fantasía es **"soy un caballero y este reino es mío que defender"** — cada sistema debe servir a "caballero" (montura, espada, honor) o a "reino que responde" (guardia, aldea, bandidos con memoria).

## 2. El combate al microscopio (lo que Víctor debe sentir en su pulgar)

Herencia directa del *freeflow* de Batman Arkham, simplificado y letalizado. **Cinco verbos, un botón cada uno**:

| Verbo | Botón PS4 | Qué hace por debajo |
|---|---|---|
| Golpe | ◻ | El personaje **se teletransporta sutilmente** hacia el enemigo elegido (attack snapping/magnetismo): el motor elige el objetivo por dirección del stick + proximidad + amenaza, y la animación cubre metros. NUNCA golpeas al aire. |
| Contraataque | △ | Aparece un **icono sobre la cabeza** del enemigo que va a golpear (telegrafiado explícito). Pulsar a tiempo = contraataque cinemático. Es la mecánica que enseña a LEER la horda. |
| Salto/esquiva | ✕ | Saltas POR ENCIMA del enemigo (vault) — reposicionamiento ofensivo, no huida. Los ataques "imbloqueables" (destello) obligan a esto. |
| Aturdir | ○ | Rompe la guardia de escudos: abre una ráfaga de golpes gratis. |
| A distancia | L2+R1 | Tiempo-bala gastando un recurso (Focus/might) — válvula de escape cuando te rodean. |

**La economía del combate**: cada golpe/contra sin ser tocado sube el **hit streak**; al llegar al umbral (8 en Mordor) se enciende la **ejecución** (dos botones): muerte instantánea a un orco normal, daño masivo a un capitán. Recibir un golpe REINICIA la racha → la tensión perfecta: la avaricia (un golpe más) contra la prudencia. **Last chance**: al caer a 0, un QTE bien medido te levanta — la muerte casi siempre se siente negociada, no robada.

**Reacciones y peso**: cada impacto tiene reacción direccional del enemigo, micro hit-stop, y las ejecuciones llevan cámara lenta + encuadre. El daño ocurre EN el frame del contacto de la hoja (animación manda, no el botón).

**Qué robamos (legalmente — son técnicas, no IP)**, mapeado a nuestros dolores del playtest:
1. **Magnetismo de ataque** → nuestro dolor nº1 en táctil: en móvil no se apunta con precisión; el golpe debe elegir solo al bandido correcto y llevar al caballero hasta él. *(Sprint próximo de combate.)*
2. **Contraataque telegrafiado con icono** → ya estaba en backlog por la regla Miyazaki; Mordor nos da el patrón exacto: icono sobre la cabeza + ventana generosa en normal.
3. **Racha visible + ejecución** → nuestra economía de combate necesita esta zanahoria (ya tenemos hit-stop y daño flotante; falta la racha).
4. **Last chance** → sustituir nuestra muerte seca por un último aliento.

## 3. El sistema Nemesis al microscopio

La arquitectura (verificada en el análisis de Game Developer):
- **Jerarquía de 4 niveles**: capitanes → capitanes veteranos → jefes de guerra → overlord de fortaleza. Los orcos ascienden matándote o matándose entre ellos: **un orco raso que te mata es promovido a capitán con nombre**.
- **Generación procedural**: 6 roles (guerrero, arquero, cazador, salvaje, defensor, olog) × 8 clanes × rasgos (fortalezas, debilidades, inmunidades, miedos) × nombre + apodo + voz + armadura. La debilidad convierte cada capitán en un **puzzle**: al Inmune a Flechas lo cazas con sigilo; al que Teme a los Caragors le sueltas uno.
- **Memoria y cicatrices**: si escapa o te mata, VUELVE — con la quemadura que le dejaste, recordándotelo en voz alta. Puede resucitar si no lo decapitaste, tenderte emboscadas, o (dominado) traicionarte.
- **Dominación** (SoW): conviertes capitanes en agentes — espías, guardaespaldas, usurpadores de fortalezas.

⚠️ **Advertencia legal seria**: Warner Bros **patentó el sistema Nemesis** (patente concedida en 2021, vigente hasta ~2036). Copiar el bucle específico (jerarquía procedural de enemigos que asciende por matar al jugador con memoria de encuentros) es riesgo real. La vía segura y probada es la que usó *Assassin's Creed Odyssey* con sus mercenarios: **enemigos con nombre, rasgos e identidad, SIN el bucle de promoción-por-tu-muerte ni la jerarquía dinámica**. Para «Acero y Corona» (Fase 3): *bandidos con nombre y rasgo* («Ruy el Tuerto, teme al fuego»), un **jefe de campamento** fijo por zona, y carteles de "se busca" — sabor Nemesis, cero patente.

## 4. Movimiento, monturas y cámara

- **Traversal magnetizado**: la escalada es automática hacia agarres (sin puzzle de manos); el objetivo es que MOVERSE nunca frustre. Nuestro equivalente: el caballero jamás debe engancharse en una esquina de la muralla (→ las colisiones que están entrando en el sprint actual deben empujar SUAVE, deslizando por la pared, no frenar en seco).
- **Monturas como verbo de poder**: caragors (rápidos), graugs (arietes), drakes (aéreos, SoW). No son "transporte": son armas. Nuestro caballo debe poder **arrollar** (daño por atropello al galope) — misión para jugabilidad, Fase 2.
- **Cámara**: por encima del hombro, suelta en exploración; en combate se ALEJA y baja para encuadrar la horda (el jugador decide por siluetas). Auto-encuadre suave, nunca brusco. Nuestro equivalente móvil: cámara que se aleja sola cuando hay 2+ bandidos cerca.

## 5. Mundo, misiones y economía

- Regiones **densas antes que enormes**; los capitanes están VIVOS en el mapa (patrullando, duelo entre ellos, festines) y puedes interrumpirlos — el mundo existe sin ti (validación de nuestra decisión de reaparición y patrullas).
- Misiones de arma (espada/arco/daga) con **retos por pieza de equipo**: cada objeto sube de nivel cumpliendo un desafío concreto ("mata 5 con ejecución") — progresión que ENSEÑA mecánicas. Robable tal cual para nuestro herrero (Fase 3): la espada forjada mejora completando retos, no solo pagando.
- Los coleccionables cuentan historia (artefactos con audio) — mejor que nuestras monedas mudas: cada cofre futuro debería decir algo del reino.

## 6. Rendimiento (la lección de presupuesto)

PS4 base: **1080p/30 estable, sin caídas** — Monolith prefirió 30 clavados con hordas de 30 orcos antes que 45 inestables. Lección Carmack para nosotros: en iPhone, mejor 30 fps ESTABLES con sombras recortadas que 45 a tirones (→ el presupuesto de draw calls pendiente, H1, es prioridad de Fase 2; y toca añadir modo calidad AUTO que recorte sombras/densidad en móvil si el frame pasa de 33 ms).

## 7. La tabla de robo legal completa (sistema → aplicación → cuándo)

| Sistema de Mordor/War | Qué hace sentir | En «Acero y Corona» | Fase |
|---|---|---|---|
| Magnetismo de ataque | "Nunca fallo" | Golpe elige bandido por dirección+cercanía y desliza al caballero | **Ya** (sprint combate) |
| Contra telegrafiado (icono) | "Puedo leer la horda" | Icono ⚠️ sobre bandido que ataca + botón de contra | **Ya** |
| Hit streak + ejecución | Avaricia vs prudencia | Racha visible; a 5, ejecución cinemática (1 botón) | Fase 2 |
| Last chance | Muerte negociada | QTE de último aliento antes de caer | Fase 2 |
| Cámara de horda | Leo el combate | Zoom-out auto con 2+ enemigos | Fase 2 |
| Montura-arma | El caballo es poder | Atropello al galope con daño y knockback | Fase 2 |
| Enemigos con identidad (vía AC Odyssey, sin patente) | El mundo me recuerda | Bandidos con nombre/rasgo, jefe por campamento, carteles "se busca" | Fase 3 |
| Retos por equipo | Progresar enseña | La espada del herrero sube por desafíos | Fase 3 |
| Fortaleza asediable | Clímax territorial | Asalto al campamento grande como final de campaña | Fase 3–5 |
| 30 fps clavados | Fluidez sagrada | Modo calidad AUTO en móvil + dieta de draw calls | Fase 2 |

---

# PARTE II — La sala de máquinas (motor, gráficos, pipeline y backend)

## 8. El motor: LithTech V6, un motor propio de 16 años de linaje

Shadow of Mordor NO corre en Unreal ni Unity: corre en **LithTech**, el motor interno de Monolith — el linaje que viene de F.E.A.R. (versión "Jupiter EX", 2005) evolucionado durante una década; la propia web de Monolith designó la encarnación de Mordor como **LithTech V6**, con el sistema Nemesis integrado a nivel de motor. Lecciones: (1) un equipo mediano compitió con los motores gigantes porque su motor estaba **especializado en SU juego** (hordas + procedural de personajes), no en todo; (2) el motor propio les dio el Nemesis a nivel nativo, imposible de enchufar a un motor ajeno de la época. Para nosotros: valida el camino "motor pequeño especializado" (nuestro Three.js single-file está especializándose en ESTE juego) y da el criterio de la decisión de Fase 4 — se migra cuando el motor deje de estar especializado en lo que el juego necesita.

## 9. Renderizado y técnicas gráficas

- **Presupuesto sagrado**: PS4 base **1080p/30 sin caídas** con ~30 orcos en pantalla. En PS4 Pro, **resolución dinámica** (~1620p en modo calidad): la resolución baja ANTES de que el frame se caiga — la técnica exacta que nos falta en iPhone (nuestro plan: escalar `pixelRatio` dinámicamente cuando el frame pase de 33 ms).
- **La grieta conocida**: Digital Foundry cazó **texturas de baja resolución evidentes en 4K** en la versión Pro — el streaming de memoria no acompañó al salto de resolución. Lección: resolución sin presupuesto de texturas/memoria es maquillaje; nuestro equivalente es el presupuesto de draw calls (H1) antes que más píxeles.
- Iluminación dinámica de antorchas/fuegos sobre hordas + tiempo atmosférico por región; LODs agresivos de personajes (la horda lejana baja de esqueleto y malla). Nuestra versión: LOD de bandidos lejanos (congelar mixer + malla simple) cuando crezca la población, Fase 3.

## 10. La fábrica de orcos (el pipeline de contenido procedural)

Cientos de capitanes "únicos" salen de una **fábrica de composición** (confirmado por el material de GDC): *presets de cabeza* + *rasgos faciales* + *tintes de piel* + *piezas de armadura combinables* + *cicatrices aplicadas post-evento* (te recuerdan la quemadura que les hiciste) + *presets de voz* × *personalidades de diálogo* (los "barks": miles de líneas grabadas que se ensamblan con título y nombre). La unicidad es **combinatoria, no artesanal**.

**Aplicación directa e inmediata para nosotros**: KayKit Adventurers es MODULAR (el ingeniero ya recortó "piezas de equipo variante" del GLB — ¡existen!). Nuestra fábrica de bandidos (Fase 3): piezas de armadura intercambiables + tintes por instancia + nombre/rasgo generados = el sabor "cada bandido es alguien" con un solo modelo base. Cero coste de assets nuevos.

## 11. Animación y combate por dentro (las técnicas con nombre)

- **Root motion warping** (el magnetismo técnico): la animación de ataque lleva movimiento de raíz, y el motor la **deforma en curva hacia el objetivo elegido** — por eso el héroe "vuela" metros hasta el orco correcto. Nuestra versión Three.js: al atacar, lerp de P.pos hacia el bandido objetivo durante la anticipación del clip (0–35 %), techo 2,5 m. Es EL fix técnico del combate táctil.
- **Tokens de ataque**: de la escuela F.E.A.R./Arkham — la horda pide "permiso" para atacar; solo N enemigos (2–3) tienen token simultáneo, el resto orbita amenazando. Por eso 30 orcos no son injustos. Nuestra versión: máximo 2 bandidos con token de ataque; el resto rodea. Barato y transformador.
- **Reacciones aditivas direccionales**: el golpe mezcla una animación aditiva según el ángulo del impacto sobre la locomoción — no interrumpe, se suma. Con nuestro mixer: capa aditiva o blend rápido del Hit_A con peso parcial.
- **IK de pies + alineación a pendiente** en terreno irregular (nuestra capa procedural pendiente de Fase 1 va por el mismo camino).

## 12. El backend real: Hydra, WBPlay y la muerte de las features

La arquitectura online verificable:
- **WBPlay** (cuenta transversal de WB) sobre la cuenta de PSN + **Hydra**, la plataforma de servicios backend interna de WB Games: identidad, entitlements, datos cross-juego y telemetría. En PC iba ligada a Denuvo.
- **Online Conquest (SoW), la joya asíncrona**: subes TU fortaleza con TUS orcos al servidor; otros jugadores asedian una **copia defendida por IA**. PvP sin una sola línea de netcode en tiempo real — sin lag, sin servidores de partida, solo estado subido y simulación local. **Es exactamente el primer online que nuestro ROADMAP §3 debería construir**: "asedia el campamento de otro jugador" = subir un JSON del campamento, defender con IA. Orden de magnitud más barato que el multijugador vivo.
- **Vendettas sociales** (SoM): el orco que mató a tu amigo aparece en TU mundo — otra feature asíncrona pura sobre datos compartidos.
- **Economía server-side**: Gold, War Chests y Market (cajas con orcos y equipo) — validación en servidor. Tras el rechazo de la comunidad, **Monolith los retiró por completo en julio de 2018**: lección de diseño Y de arquitectura (la economía estaba lo bastante desacoplada como para amputarla sin matar el juego).
- **La lección mortal**: Nemesis Forge (llevar tu némesis de Mordor a War) vivía sobre Hydra; **cuando WB retiró Hydra, todas esas features murieron para siempre** — vendettas, leaderboards, transferencias. Regla para «Acero y Corona»: **local-first** — la partida vive en el dispositivo (ya lo hace), y todo lo online que construyamos debe degradar con dignidad cuando el servidor no esté. Nada del juego base puede depender de un servidor para existir.

## 13. Tabla técnica de adopción

| Técnica de Monolith | Qué resuelve | Nuestra versión (stack Three.js) | Fase |
|---|---|---|---|
| Root motion warping | Golpear sin apuntar | Lerp al objetivo durante anticipación del clip (techo 2,5 m) | **Ya** (combate) |
| Tokens de ataque | Hordas justas | Máx. 2 bandidos atacando; resto orbita | 2 |
| Resolución dinámica | 30 fps sagrados | `pixelRatio` autoescalado si frame >33 ms | 2 |
| Fábrica de composición | 100 enemigos de 1 modelo | Piezas modulares KayKit + tintes por instancia + nombre/rasgo | 3 |
| Reacciones aditivas | Golpes con peso sin cortar | Hit_A con blend parcial sobre locomoción | 2 |
| LOD de personajes | Población grande | Congelar mixer + malla simple a distancia | 3 |
| PvP asíncrono (Online Conquest) | Online sin netcode | "Asedia el campamento de otro": subir JSON + defensa IA | 5 |
| Economía desacoplada | Poder amputar sin matar | Toda feature online opcional y separable | 5 |
| Local-first (lección Hydra) | Sobrevivir al servidor | La partida nunca depende del backend para existir | siempre |

## Fuentes
- [Game Developer — Core System Analysis of Middle-Earth: Shadow of War](https://www.gamedeveloper.com/design/core-system-analysis-of-middle-earth-shadow-of-war) (bucle central, verbos, Nemesis, jerarquía, dominación, monturas)
- [Kotaku — The Combat In Shadow Of Mordor Sure Feels Familiar](https://kotaku.com/the-combat-in-shadow-of-mordor-sure-feels-familiar-in-1641217162) (mapeo de botones, herencia Arkham, last chance)
- [TheGamer — Nemesis System Complete Guide](https://www.thegamer.com/middle-earth-shadow-of-war-orc-nemesis-system-complete-guide/) · [Medium — How the Nemesis System Creates Stories](https://medium.com/@niklaseckstein/how-the-nemesis-system-creates-stories-d26754b30d2e) (memoria, promociones, cicatrices)
- [Windows Central — Digital Foundry: Shadow of War X1X vs PS4 Pro](https://www.windowscentral.com/digital-foundry-compares-middle-earth-shadow-war-xbox-one-x) · [GamingBolt — comparativa 4K](https://gamingbolt.com/middle-earth-shadow-of-war-pc-vs-ps4-pro-vs-ps4-vs-xbox-one-graphics-comparison-in-4k) (1080p/30 estable en PS4, resolución dinámica en Pro)
- Conocimiento de dominio del medio (patente del Nemesis por WB concedida en 2021; sistema de mercenarios de AC Odyssey como alternativa sin patente).

**Fuentes de la Parte II:**
- [Wikipedia — LithTech](https://en.wikipedia.org/wiki/LithTech) (linaje Jupiter EX→F.E.A.R.→Mordor; designación V6 con Nemesis integrado)
- [GDC Vault — Helping Players Hate (or Love) Their Nemesis (Chris Hoge, Monolith, GDC 2018)](https://www.gdcvault.com/play/1025150/Helping-Players-Hate-(or-Love)) · [Game Developer — Upgrading the Nemesis system for Shadow of War](https://www.gamedeveloper.com/design/upgrading-the-nemesis-system-for-i-middle-earth-shadow-of-war-i-) (fábrica de composición: presets de cabeza, rasgos, tintes, armaduras, voces × personalidades)
- [WB Games Support — Free Updates Coming to Shadow of War + FAQ](https://wbgamessupport.wbgames.com/hc/en-us/articles/1500002839382-Important-Free-Updates-Coming-to-Shadow-of-War-FAQ-04-02-2018) (retirada de Gold/War Chests/Market en julio de 2018)
- [Game Rant — Online Conquest Mode Revealed](https://gamerant.com/middle-earth-shadow-of-war-conquest-mode/) (asedios asíncronos con fortalezas subidas y defendidas por IA)
- [Windows Central — Digital Foundry X1X vs PS4 Pro](https://www.windowscentral.com/digital-foundry-compares-middle-earth-shadow-war-xbox-one-x) (resolución dinámica; texturas low-res en 4K)
- Conocimiento de dominio del medio (Hydra como plataforma BaaS interna de WB y su retirada matando Nemesis Forge/vendettas; WBPlay+Denuvo en PC; tokens de ataque y root-motion warping como técnicas estándar del linaje F.E.A.R./Arkham).
