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

## 2026-09-01 — Sprint 4A: «realismo tipo Fortnite» en el escenario (v0.8)
- **Decidido:** directriz de Víctor («más realismo, al menos tipo Fortnite, en el escenario»). Primero la dieta de draw calls (fusión de geometría estática por material: castillo 219–392 → ~120, aldea 339 → ~108) y con ese presupuesto ganado: hierba 3D instanciada (8.500 matas escritorio / 2.800 móvil, 3 planos cruzados con textura de briznas y alphaTest), robles multi-lóbulo (4 icosaedros horneados en una geometría), pinos de 3 pisos, farallones en laderas (filtro por pendiente), juncos en orillas (scatter propio en la franja de agua), roca por pendiente y calvas secas en el color de vértice del terreno, textura de suelo a 512px con briznas, y agua más profunda (0x1f6f86). Insignia de versión visible en el menú (v0.8) para diagnosticar cachés en el iPhone de Víctor.
- **Descartado:** billboards con sprites para la hierba (peor de cerca, y el presupuesto tras la fusión permitía geometría real); LOD por celdas (pospuesto a la ampliación de mapa de Fase 3).
- **Ejecutado por el orquestador**: el agente constructor cayó por el límite mensual de gasto nada más arrancar (reset 12:00 UTC). La pasada de LUZ (sol de mediodía que lava el color, cielo, agua, grade) queda para el ingeniero de gráficos en el Sprint 4B.
- **Verificado:** 0 errores de arranque, combate (bandido 3→0 en 3 golpes, kills+1), montar/desmontar, guardado íntegro, ventanas y hogueras nocturnas tras la fusión. Triángulos ~210–240k por vista (escritorio).

## 2026-09-02 — Sprint 4B: pasada de luz y atmósfera (v0.9)
- **Decidido:** corregir el «mediodía lavado» en las LUCES y los TINTES, no en la exposición (el tope ACES de 1.02 no se toca — error pagado nº2 de gamedev-graficos). Diagnóstico del ingeniero de gráficos: (a) sol de día a L=0.9 e intensidad 2.7 casi blanco; (b) tintes multiplicativos crema (stoneL 0xf2ede4, plaster 0xffffff, terreno 0xe8e4d2) comiéndose una paleta que ya era la oficial; (c) radiancia total >1 comprimida por ACES canal a canal. Cambios solo en el EXTREMO DÍA de cada lerp (sol 2.7→2.05 y L 0.9→0.8, hemi 0.6→0.48, ambient 0.26→0.22) para que atardecer/noche no regresionen; tintes un punto más oscuros; cielo exp 0.7→0.55 y nubes con más volumen; agua 0x1c7f97 con metalness 0.55→0.42 (sin envMap, el metalness solo apagaba el color) y FRANJA DE ORILLA por color de vértice según profundidad (precalculada, coste cero por frame); grade CSS 1.18/1.22→1.10/1.14 para no hacer doble trabajo. Rúbrica: 18–22/25 por captura, media 20.6/25; delta de píxel medido (muro R-B 18.5→31.5, pradera G-B 37→52).
- **Descartado:** subir la saturación por CSS (sobresaturaría atardecer/noche que ya iban bien); PMREMGenerator para reflejo real del agua (anotado como mejora futura, fuera de alcance).
- **Deuda anotada:** glint especular del agua poco visible en las tomas del harness (verificar en GPU real); la vista frontal del castillo apenas enseña cielo (encuadre, no luz); scatter sin semilla → composiciones distintas por recarga (dificulta comparar capturas).
