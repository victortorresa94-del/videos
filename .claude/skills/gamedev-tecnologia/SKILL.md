---
name: gamedev-tecnologia
description: Arquitectura técnica, rendimiento, QA y despliegue del videojuego «Acero y Corona»: mapa del código single-file, harness de pruebas automatizadas con Chromium headless, contrato de depuración __ayc, presupuesto de fotograma, restricciones CSP del artifact, esquema de guardado, y el plan de modularización y evaluación de motor. Úsala SIEMPRE antes de tocar la estructura del código, añadir dependencias o assets, publicar una build, escribir o modificar pruebas, o cuando algo vaya lento o falle solo en móvil/artifact.
---

# Tecnología — «Acero y Corona»

## Mapa del código (acero-y-corona/index.html, single-file)
Orden de secciones (¡el orden importa, hay dependencias!): utilidades/ruido → renderer/escena/cámara → cielo/nubes/luces (clave+relleno+rim) → caminos+`distToPath` → terreno → agua animada → materiales `M` → texturas canvas → `winMat` → castillo (escalado ×1.25/1.32) → `GATE{0,44}` → aldea+pozo → hogueras/antorchas → **partículas+floatText+hitStop/shake** → vegetación (scatterPositions/instancedFrom) → humanoid() → jugador `P` → caballo `H`/toggleMount → aldeanos+mercader+nearMerchant → bandidos → monedas → misiones → HUD refs → entrada → acciones combate → audio WebAudio → bucle `frame()` → minimapa → menú/tienda/guardado → `__ayc`.

## Contrato de depuración (para QA y agentes)
`window.__ayc = {P, H, bandits, toggleMount, openShop, nearMerchant, setTime(t), kills()}` — teletransporte por `P.pos.set`, hora por `setTime(0..1)` (0.3 mañana, ~0.75 atardecer, 0.95 noche). Cualquier sistema nuevo AÑADE su gancho aquí; las pruebas dependen de él.

## Harness de QA (en scratchpad, patrón establecido)
1. Extraer script y `node --check` (caza sintaxis).
2. `test.html`: three.min.js LOCAL + sin fuentes remotas (el Chromium del sandbox no alcanza CDNs).
3. Playwright-core + Chromium `/opt/pw-browsers/chromium-1194/chrome-linux/chrome`, flags `--use-gl=swiftshader --enable-webgl --no-sandbox`, SIN proxy.
4. Smoke: cargar → `#playBtn` → simular teclas → aserciones vía `__ayc` → capturas (día/noche/aldea/combate/montado).
5. **SwiftShader va a ~5 fps**: el tiempo simulado < real (dt clamp 0.05). En tests, resetear cooldowns vía `__ayc`, no esperar tiempos reales. Y las capturas salen desaturadas: composición sí, color no.
6. Espera asíncrona: `Bash run_in_background` con sleep (el sleep en foreground está bloqueado).

## Presupuesto de fotograma
16,6 ms escritorio / 33 ms móvil. Sospechosos habituales: `computeVertexNormals` del agua (56×56 ok, no subir), sombras (una direccional, mapsize por plataforma), point lights (≤8), partículas (pool 200 fijo), floatText (DOM: ≤10 vivos). Todo lo nuevo con contador: `renderer.info.render.calls` < 150.

## Restricciones del artifact (CSP) — reglas de despliegue
- Scripts SOLO de cdnjs / jsdelivr(npm) / tailwind-play / code.jquery. Three r128 pinneado en cdnjs; loaders de examples vía jsdelivr npm `three@0.128.0/examples/js/...` (UMD).
- **Fetch/XHR/imágenes/GLB externos: BLOQUEADOS.** Assets → base64 en el HTML (`atob`→ArrayBuffer→`loader.parse`). Página ≤16 MB.
- `localStorage` funciona (guardado); envolver SIEMPRE en try/catch.
- Publicar: misma ruta de archivo ⇒ misma URL del artifact. Label+note de versión en cada publish.

## Guardado (SAVE_KEY `ayc_save_v1`)
`{c:oro, s:nivelEspada, h:vida, d:bajas, q:misiónIdx, w:ganado, x,z:posición, t:horaDelMundo, m:montado, hx,hz:caballo}` — autosave 5 s. Cambios de esquema ⇒ nueva clave versionada + migración o descarte limpio. Monedas del mapa NO se persisten (decisión: sesión nueva las repone).

## Flujo de entrega (innegociable)
`node --check` → smoke PASA → capturas revisadas con la rúbrica de gamedev-graficos → commit descriptivo en `claude/gta6-medieval-open-world-1g521c` → push → republicar artifact (misma URL) → resumen con evidencias. Nada se declara "hecho" sin su prueba.

## Plan de evolución técnica
- **Fase 2–3**: modularizar a Vite + three moderno (módulos ES), build único embebible para el artifact; suite de pruebas versionada en `acero-y-corona/tests/`.
- **Fase 4 — decisión de motor** (GO/NO-GO documentado en docs/DECISIONES.md): seguir en web (three moderno/WebGPU, postprocesado real) vs. Godot 4 (export web decente, editor de escenas) vs. UE5 (fidelidad máxima, sin web viable). Criterios: ¿el cuello es contenido o tecnología?, ¿objetivo distribución (web/móvil) o fidelidad (PC)?, coste de migrar lo construido.
- Backend online: arquitectura ya documentada en `acero-y-corona/docs/ROADMAP.md` §3 — no empezar antes de Fase 5.
