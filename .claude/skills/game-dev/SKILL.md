---
name: game-dev
description: Comando maestro del estudio del videojuego «Acero y Corona» — carga las 6 skills de desarrollo (tecnología, dirección, animación, jugabilidad, gráficos, mundo 3D) de una sola vez. Úsala al empezar cualquier sesión de trabajo sobre el juego, cuando Víctor diga "game dev", "carga el estudio del juego", "modo videojuego" o vaya a trabajarse cualquier aspecto del videojuego sin un área única clara.
---

# /game-dev — el estudio completo en una invocación

Este comando carga la biblioteca técnica entera del videojuego «Acero y Corona». Al invocarlo:

1. **Carga (vía la herramienta Skill, en este orden) las seis skills del estudio:**
   1. `gamedev-tecnologia` — SIEMPRE primero si se va a tocar código, probar o publicar (mapa del single-file, contrato `__ayc`, harness, CSP).
   2. `gamedev-direccion` — si hay que decidir qué se construye, cerrar fase o evaluar una build (7 sillas, 3 vetos).
   3. `gamedev-animacion` — si la tarea toca movimiento de personajes, caballo, enemigos o GLB riggeados.
   4. `gamedev-jugabilidad` — si toca mecánicas, IA, misiones, economía o el juego "no se siente bien".
   5. `gamedev-graficos` — si toca luz, materiales, cielo, color o hay que juzgar capturas.
   6. `gamedev-mundo3d` — si toca terreno, edificios, vegetación, escalas, colisiones o zonas nuevas.

   Si la sesión va a trabajar UN área concreta, basta con `gamedev-tecnologia` + la del área; si el alcance es amplio o incierto, cargar las seis.

2. **Sitúa el contexto del proyecto:** plan en `acero-y-corona/docs/PLAN-MAESTRO.md`, decisiones vigentes en `docs/DECISIONES.md`, créditos de assets en `docs/CREDITOS.md`, biblia visual en `concept-art/` y `docs/REFERENCIAS.md`. Equipo de agentes desplegables en `.claude/agents/` (animacion-ingeniero, graficos-ingeniero, jugabilidad-disenador, mundo-constructor, qa-capitan).

3. **Reglas del estudio (viajan con este comando):**
   - Nada se declara "hecho" sin su prueba: `node --check` + smoke del harness + capturas ≥18/25 en la rúbrica gráfica.
   - Los agentes nunca hacen commit/push: integra el orquestador en la rama `claude/gta6-medieval-open-world-1g521c`.
   - Assets solo CC0 o con derechos claros, registrados en `CREDITOS.md`.
   - Leer los "errores ya pagados" de cada skill antes de trabajar su área.
   - Publicación: misma ruta de archivo ⇒ misma URL del artifact, con label y note de versión.

4. **Espejo en Aura Storage:** estas mismas seis skills viven en `personal/acero-y-corona/skills` y el comando `game-dev` de Aura las invoca desde cualquier otro Claude con `aura_storage_invoke({command:"game-dev"})`. Si se edita una skill local, actualizar su copia en Aura (`aura_storage_save` con el mismo título y el sha256 del archivo).
