# Documento de Diseño — «Acero y Corona»

*Mundo abierto medieval · PC y móvil · versión del documento: prototipo v0.1*

---

## 1. Visión de una frase

Un mundo abierto medieval vivo donde cabalgas, exploras un reino, cumples encargos y luchas — la fantasía de "ser un caballero en un reino real" con la libertad y densidad de un GTA, pero en la Edad Media.

## 2. Pilares de diseño

1. **Un reino, no un mapa.** El mundo debe sentirse habitado y coherente: aldeas con oficios, caminos que llevan a algún sitio, un castillo que gobierna la región. La densidad importa más que el tamaño.
2. **Libertad con consecuencias.** Igual que en GTA la ciudad reacciona a ti (policía, peatones, economía), aquí la guardia, los aldeanos y los bandidos reaccionan a lo que haces.
3. **Fricción medieval, no moderna.** El equivalente al coche es el caballo; el del móvil, el rumor; el del tiroteo, el duelo a espada. Cada sistema de GTA tiene su traducción de época.
4. **Legible en el móvil, profundo en PC.** Mismos sistemas, distinta fidelidad. El diseño de UI/controles se hace *mobile-first* y se enriquece en PC.

## 3. Fantasía y bucle de jugador

**Bucle central (30–90 s):** explorar → encontrar un objetivo (encargo, cofre, campamento) → resolverlo (combate, sigilo, diálogo) → recompensa (oro, reputación, equipo) → mejorar → explorar más lejos.

**Bucle de sesión (20–40 min):** avanzar en una línea de encargos, subir la reputación con una facción, desbloquear una zona nueva del mapa.

**Bucle de progresión (largo):** de escudero sin nombre a señor con tierras. El estado del reino cambia con tus actos.

## 4. El mundo

- **Región** dividida en biomas: llanura de la aldea, bosque, colinas, montañas fronterizas, ribera/lago. *(En el prototipo: generación procedural con meseta central para el castillo y montañas que cierran el mapa.)*
- **Núcleos de interés:** el castillo, la aldea, el mercado, campamentos de bandidos, ruinas, molino, ermita. Cada núcleo con su propia razón de existir y su encargo asociado.
- **Verticalidad:** murallas, torres y colinas para puntos de vista y rutas alternativas.

## 5. Sistemas (traducción GTA → medieval)

| Sistema en un GTA | Equivalente en «Acero y Corona» |
|---|---|
| Coches / conducción | **Caballos** y monturas, con establo, robo de montura y persecuciones |
| Estrellas de búsqueda | **Notoriedad ante la guardia**: robar o agredir sube el nivel de alerta; te persiguen ballesteros y caballería |
| Tiroteos | **Combate a espada/arco**: bloqueo, esquiva, resistencia (vigor), golpes cargados |
| Misiones de historia | **Encargos** de facciones (corona, gremio, forajidos) con ramas y consecuencias |
| Economía / tiendas | **Oro, herrero, mercader, botín**; precios que varían por región y reputación |
| Actividades libres | Caza, torneos, apuestas, escoltas, contrabando, buscar tesoros |
| Radio / móvil | **Rumores y heraldos**: cómo te enteras de encargos y cómo el mundo comenta tus hazañas |
| Multijugador (GTA Online) | **Reino compartido** cooperativo/competitivo (ver ROADMAP: arquitectura de servidores) |

## 6. Combate (detalle)

- Recursos: **Vida** y **Vigor** (correr, esquivar y golpear gastan vigor).
- Acciones: golpe ligero, golpe cargado, bloqueo con escudo, esquiva, agarre.
- IA enemiga por estados: *patrulla → alerta → persecución → combate → huida*. *(El prototipo ya implementa una versión reducida: los bandidos deambulan, detectan al jugador en rango, persiguen y atacan.)*
- Muerte no punitiva al principio (reaparición), con penalización creciente según dificultad.

## 7. Progresión y economía

- **Oro** como moneda universal (ya en el prototipo como coleccionable).
- **Reputación por facción** (corona, aldea, forajidos): abre y cierra encargos y precios.
- **Equipo**: armas, armaduras, monturas; mejoras en el herrero.
- **Tierras/propiedades** como meta a largo plazo (equivalente a los negocios de GTA Online).

## 8. Dirección de arte

- **Estilo:** *low-poly estilizado* y iluminación PBR con ciclo día/noche. Prioriza silueta y color legibles sobre densidad de detalle — escala bien a móvil y mantiene identidad. *(Es exactamente lo que hace el prototipo; para la versión completa, ver ROADMAP: paso a activos de alta fidelidad con Nanite/streaming.)*
- **Paleta heráldica:** pizarra nocturna, oro, carmesí, pergamino, verde musgo, acero. Los estandartes y la heráldica dan identidad a facciones y lugares.
- **Tipografía de marca:** Cinzel (capitales grabadas, romana/medieval) para títulos; Spectral para cuerpo y diálogo.
- **Cielo y clima** como herramienta dramática: el amanecer sobre el castillo, la niebla en el bosque, la tormenta antes de un asedio.

## 9. Sonido

- Música orquestal por zonas y estados (exploración, combate, pueblo).
- Ambiente diegético: viento, aves, fragua, mercado, campanas del castillo.
- *(El prototipo usa audio procedural con Web Audio API: viento, pasos, espada y monedas, sin ficheros.)*

## 10. Alcance del prototipo actual (v0.1) vs. visión

| | Prototipo v0.1 (este repo) | Visión completa |
|---|---|---|
| Mundo | 1 región procedural | Región hecha a mano + puntos de interés guionizados |
| Personajes | Primitivas *low-poly* | Modelos con esqueleto, animación y captura de movimiento |
| Combate | Golpe + IA básica | Sistema completo (bloqueo/esquiva/carga), facciones |
| Misiones | 3 encargos encadenados | Campaña ramificada + encargos dinámicos |
| Montura | — | Caballos y persecuciones |
| Online | — | Reino compartido (ver ROADMAP) |
| Motor | Three.js (navegador) | Unreal 5 / Unity / Godot (ver ROADMAP) |

El prototipo existe para **probar la fantasía y el bucle** de forma jugable y barata, no para ser el producto final. Es la base honesta desde la que se decide qué merece la inversión de producción.
