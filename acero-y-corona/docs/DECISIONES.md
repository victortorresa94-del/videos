# Registro de decisiones — «Acero y Corona»

Cada dictamen del consejo y cada decisión de alcance, con fecha, qué se decidió y **la alternativa descartada** (para no reabrir lo cerrado).

---

## 2026-08-30 — Constitución del estudio
- **Decidido:** organización por fases (PLAN-MAESTRO v1.0), consejo de 7 sillas, 5 agentes de ejecución, 6 skills técnicas.
- **Descartado:** seguir iterando el juego a demanda sin plan — producía sprints correctos pero sin rumbo acumulado (crítica de Víctor aceptada).

## 2026-08-30 — Prioridad de la Fase 1
- **Decidido:** la Fase 1 es animación esquelética de personajes (matar el "Minecraft") antes que más mundo o más sistemas.
- **Descartado:** empezar por colisiones o por la guardia — sin cuerpos creíbles, todo lo demás sigue pareciendo prototipo (vetos de las sillas de Diversión y Personaje).

## 2026-08-30 — Assets de personajes
- **Decidido:** modelos riggeados CC0 (candidato principal: KayKit Adventurers; caballo: Quaternius) embebidos en base64, con el sistema procedural actual como fallback operativo.
- **Descartado:** Mixamo (licencia no permite redistribución embebida en HTML público) y generación 3D por IA como vía principal (calidad de rig no garantizada; queda como experimental).

## 2026-08-31 — Cierre de QA de la v0.7: fuera la etiqueta beta
- **Decidido:** la v0.7 pasa la auditoría completa de QA (arranque limpio, esqueletal jugador+caballo, combate, montura, tienda/guardado, y los 3 puntos abiertos del Sprint 3: los 4 muros del castillo bloquean con la puerta como único paso, layout táctil íntegro en viewport iPhone 390×844 con drag de cámara suave —salto máx. 0,045 rad/frame—, y anti-patinaje conforme a la fórmula `clamp(v/nominal, 0.6, 1.6)`). Se publica como **v0.7** sin etiqueta beta. El campamento de bandidos del noreste se reubica a la orilla seca del lago (74,−48 en formación) con regla `drySpot` como red de seguridad permanente, y la senda `pathB` se reencaminó bordeando el lago.
- **Descartado:** dejar el campamento donde estaba y subir solo el terreno o bajar el agua — retocar el terreno por un emplazamiento rompería caminos y vegetación ya asentados; mover el campamento es local y reversible.
- **Deuda registrada (no bloqueante):** draw calls en castillo/aldea 219–392 (meta ≤150, dieta H1 en Fase 2); con teclado el anti-patinaje satura el clamp superior al andar (rango dinámico solo se aprecia con joystick analógico — para jugabilidad/animación); materializar la suite de QA en `acero-y-corona/tests/` en vez de reconstruir el harness en cada pasada.
