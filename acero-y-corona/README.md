# ⚔️ Acero y Corona

**Prototipo jugable de mundo abierto medieval** — corre en el navegador, en PC y móvil, con un solo archivo (`index.html`) y sin instalación.

> 🏛️ **Esto ya es un proyecto de estudio, no un experimento**: plan de meses por fases en [`docs/PLAN-MAESTRO.md`](docs/PLAN-MAESTRO.md), consejo de dirección y equipo de agentes en `.claude/` (skills `gamedev-*` + agentes desplegables), decisiones en [`docs/DECISIONES.md`](docs/DECISIONES.md).

> Nota honesta sobre el encargo: se pidió "crear un GTA 6 medieval y robar el backend de GTA 6".
> **No se ha copiado ni un byte de GTA 6** (es código propietario de Rockstar/Take‑Two, ni siquiera publicado; copiarlo sería ilegal). Tampoco es posible entregar de una vez un AAA de mundo abierto de esa escala: son cientos de personas, varios años y cientos de millones de dólares.
> Lo que **sí** es este repositorio: una **rebanada vertical** honesta y funcional del concepto, más un plan realista y documentado para hacerlo crecer hacia esa visión — incluyendo cómo se diseña de verdad el *backend* de un mundo abierto en línea (conocimiento público de ingeniería). Ver [`docs/ROADMAP.md`](docs/ROADMAP.md) y [`docs/GDD.md`](docs/GDD.md).

---

## ▶️ Cómo jugar

- **Ahora mismo:** abre `index.html` en cualquier navegador moderno (Chrome, Safari, Firefox, Edge). Funciona en PC y en el móvil.
- Requiere WebGL (activado por defecto en todos los navegadores actuales).
- Todo el juego está en un único archivo; carga la librería 3D (Three.js) desde un CDN, así que necesita conexión la primera vez.

### Controles

| | PC | Móvil |
|---|---|---|
| Moverse | `W A S D` / flechas | Joystick (mitad izquierda) |
| Mirar | Arrastrar con el ratón · rueda para zoom | Arrastrar mitad derecha |
| Saltar | `Espacio` | Botón **SALTAR** |
| Correr | `Shift` (gasta vigor) | Botón **CORRER** |
| Atacar | Clic (sin arrastrar) / `J` | Botón **⚔️** |
| Montar a caballo | `E` (junto al caballo) | Botón **🐴** (aparece al acercarte) |
| Pausa | `Esc` / `P` | Botón **❚❚** |

---

## 🏰 Qué incluye el prototipo (v0.5)

> **v0.5 — Fase 1: animación esquelética real.** El jugador es ahora un caballero de malla skinned (KayKit, CC0) y los bandidos van encapuchados, con `AnimationMixer` y máquina de estados con fundidos (idle/andar/correr/2 ataques en combo/reacción/muerte animada), espada anclada al hueso de la mano y daño aplicado en la ventana activa del clip. Con retorno automático al sistema anterior si los modelos fallan. Auditado por QA: PASA.

> **v0.4 — pase de *game feel* y bucle de juego**: bandidos con vida (3 golpes), retroceso, congelación de impacto, sacudida de cámara, números de daño y chispas; los bandidos sueltan oro y **reaparecen** en sus campamentos; **herrero en la aldea** (espada forjada que mata de un tajo, pociones); FOV dinámico al galopar, polvo, destello rojo al recibir daño; y **partida guardada** con botón «Continuar» en el menú.

> **v0.3 — pase de dirección de arte** inspirado en los juegos de El Señor de los Anillos (ver [`docs/REFERENCIAS.md`](docs/REFERENCIAS.md)): personajes y caballo reconstruidos con volúmenes redondeados y proporciones reales, iluminación de 3 puntos con luz de borde, paleta saturada, ventanas que se encienden de noche, antorchas, flores, árboles de 9–14 m con color por instancia, castillo a escala (~14 m de muralla) y grade cinematográfico.

- **Mundo abierto procedural** (~560×560 m): colinas onduladas, montañas que cierran el mapa, lagos y meseta central. Generado con ruido de valor + fBm; nada está "pintado a mano" salvo las estructuras.
- **Caballo montable**: acércate y pulsa `E` (o 🐴 en móvil) para montar; galopa al doble de velocidad con animación de patas, pose de jinete y cámara elevada.
- **Texturas procedurales** generadas por código (canvas): sillares de piedra en murallas y torres, tejas escamadas, madera vetada, revoco, paja y suelo moteado. Sin descargar ni un solo fichero de imagen.
- **Caminos de tierra** pintados en el terreno que conectan la puerta del castillo con la aldea y con el campamento de bandidos (y la vegetación los respeta).
- **Agua con oleaje** animado por vértices, **nubes a la deriva** que se atenúan de noche, **halo del sol** aditivo y niebla cuyo alcance cambia entre el día y la noche.
- **Castillo** con murallas almenadas, cuatro torres, torre del homenaje (keep), caseta de la puerta y estandartes.
- **Aldea** de casas con entramado de madera, tejados variados, pozo y hogueras animadas.
- **Bosques y rocas** dispersos con `InstancedMesh` (cientos de árboles con un solo *draw call* cada capa, clave para el rendimiento en móvil).
- **Caballero en tercera persona**: caminar, correr, saltar, golpe de espada con animación por *rigging* de primitivas; cámara orbital con colisión básica contra el suelo.
- **Ciclo día/noche dinámico**: sol y luna que recorren el cielo, colores de cielo/niebla interpolados (amanecer, día, atardecer, noche), estrellas, sombras que siguen al jugador.
- **NPCs**: aldeanos que deambulan y bandidos que patrullan, persiguen y atacan (y a los que puedes derrotar).
- **Bucle de juego**: monedas coleccionables, barras de vida y vigor, reaparición al caer, y un **hilo de 3 misiones** encadenadas hasta "asegurar el reino".
- **HUD completo**: minimapa con marcadores, seguimiento de misión, reloj del ciclo, sonido procedural (viento, pasos, espada, monedas) y pausa.
- **Cross-platform real**: mismo código detecta táctil vs. teclado y baja la calidad (resolución de sombras, densidad de vegetación) automáticamente en móvil.

---

## 🧱 Estructura

```
acero-y-corona/
├── index.html        # el juego completo (HTML + CSS + JS + Three.js por CDN)
├── README.md         # este archivo
└── docs/
    ├── GDD.md        # documento de diseño (visión, pilares, sistemas, arte)
    └── ROADMAP.md    # hoja de ruta técnica hacia la visión AAA + arquitectura de backend
```

## 🛠️ Tecnología

- **Three.js (r128)** para el render WebGL. Sin build, sin dependencias que instalar.
- Terreno, edificios y personajes construidos por código a partir de geometría primitiva (estética *low-poly* estilizada).
- Audio procedural con la Web Audio API (sin ficheros de sonido).
- Un único archivo → se puede publicar como *artifact*, subir a cualquier hosting estático, o abrir en local.

## ⚖️ Legal

Obra original. Los únicos activos de terceros son piezas con licencia libre correctamente atribuidas en [`docs/CREDITOS.md`](docs/CREDITOS.md) (modelos de personaje CC0 de KayKit/Kay Lousberg, Three.js MIT, tipografías OFL). No usa marcas ni datos de terceros; cualquier parecido con otros juegos de mundo abierto es únicamente de **género**. El nombre "Acero y Corona" es original para este proyecto.
