# PLAN MAESTRO — «Acero y Corona»
*Proyecto de meses para llevar el prototipo a videojuego completo · versión 1.0 (30-08-2026)*

---

## 1. Objetivo del proyecto

Un **videojuego de mundo abierto medieval completo y publicable**: movimientos corporales y de lucha de calidad profesional (animación esquelética), gráficos con dirección de arte firme (no "programmer art"), un reino vivo con sistemas encadenados, jugable en PC y móvil, con página propia y tráiler. Horizonte honesto: **4–6 meses** de trabajo por fases, cada fase cerrando con una build jugable mejor que la anterior.

## 2. El estudio (cómo nos organizamos)

### Equipo de dirección — «El Consejo» (skill `gamedev-direccion`)
Siete sillas-arquetipo inspiradas en grandes figuras del medio: **Diversión** (Miyamoto), **Combate y Mundo** (Miyazaki), **Motor** (Carmack), **Personaje** (Hennig), **Autor** (Kojima), **Producción** (Reddy), **Ingenio** (Yokoi). Delibera al cerrar cada fase y ante toda decisión de alcance; tiene tres vetos (diversión, rendimiento, cierre de fase) y sus dictámenes quedan en `docs/DECISIONES.md`. Se convoca con: *"reúne al consejo del juego"*.

### Equipo de ejecución — agentes desplegables (`.claude/agents/`)
| Agente | Rol | Skills obligatorias |
|---|---|---|
| `animacion-ingeniero` | Modelos riggeados, máquinas de estados, combate animado, monturas | gamedev-animacion, -tecnologia, -jugabilidad |
| `graficos-ingeniero` | Luz, materiales, atmósfera, post, paleta | gamedev-graficos, -tecnologia |
| `jugabilidad-disenador` | Mecánicas, IA, misiones, economía, feel | gamedev-jugabilidad, -tecnologia |
| `mundo-constructor` | Terreno, estructuras, POIs, colisiones, celdas | gamedev-mundo3d, -tecnologia, -graficos |
| `qa-capitan` | Suite de pruebas, capturas, rendimiento, veredicto | gamedev-tecnologia + rúbricas |

Reglas del despliegue: los agentes entregan cambios verificados con el harness, **nunca** hacen commit/push (integra el orquestador — esta sesión); QA da el PASA antes de cada publicación; los agentes de un mismo sprint que no compartan archivos corren en paralelo.

### Biblioteca técnica — las 6 skills (`.claude/skills/gamedev-*`)
`direccion · graficos · animacion · jugabilidad · mundo3d · tecnologia` — cada una con los valores en producción, las rúbricas y **los errores ya pagados** para no repetirlos. Toda sesión y todo agente las carga antes de tocar su área.

### Proveedores creativos (cuando sus conectores estén activos)
- **Aura Studio**: concept art y arte 2D de apoyo (lienzo `Acero y Corona`, SIEMPRE `onBrand:false`). Los 4 conceptos existentes son la biblia visual (`concept-art/`).
- **Higgsfield/Magnific**: `generate_3d` (imagen→malla GLB) como vía experimental de assets propios; upscaling del tráiler en Fase 5.

## 3. Las fases

### ✅ Fase 0 — Prototipo (HECHA · v0.1–v0.4)
Mundo procedural, castillo/aldea, ciclo día-noche, caballo, combate con game feel (hit-stop, sacudida, daño flotante), herrero y economía, reaparición de enemigos, guardado, harness de QA, arte conceptual, investigación ESDLA. *Deuda declarada: personajes procedurales ("Minecraft"), sin colisiones de edificios, sin audio musical.*

### 🎯 Fase 1 — «Cuerpo y espada» (semanas 1–3) — LA PRIORIDAD
**Objetivo:** matar el "Minecraft". Personajes y combate al nivel visual/animación de un juego profesional estilizado.
- Modelos riggeados CC0 (KayKit Adventurers o equivalente): caballero jugable + bandidos + mercader; espada anclada al hueso de la mano.
- Máquina de estados de animación con blending (idle/walk/run/attack×2/hit/death), daño en el frame activo del clip, ventana de cancelación, combo de 2 golpes.
- Capa procedural: inclinación al girar, head-look, alineación a pendiente.
- Assets embebidos base64 (≤3 MB), loaders vía jsdelivr.
- **Equipo:** animacion-ingeniero (lidera), graficos-ingeniero (integración de luz sobre los modelos), qa-capitan.
- **Cierre:** el consejo aprueba con las 3 rúbricas; captura del combate indistinguible de "un juego de verdad" en miniatura.

### Fase 2 — «Un mundo que se toca» (semanas 3–6)
- Colisiones de estructuras (primitivas), interiores transitables de la puerta del castillo.
- Kit modular de edificios (CC0) o mejora profunda de los actuales; castillo e interiores con nivel de detalle nuevo.
- Caballo riggeado y animado (Quaternius Animals u otro CC0) con walk/canter/gallop.
- Notoriedad ante la guardia (el "se busca" medieval) + guardias en la muralla.
- Música y ambiente: capa musical adaptativa (exploración/combate/pueblo) con WebAudio.
- **Equipo:** mundo-constructor (lidera), animacion-ingeniero (caballo), jugabilidad-disenador (guardia), qa-capitan.

### Fase 3 — «El reino vivo» (semanas 6–10)
- Campaña de 5 misiones con arco (del oro perdido al jefe de los bandidos) + misiones repetibles.
- **Jefe** con dos patrones y fase al 50 %; arqueros; IA con recuperación vulnerable.
- Economía completa: armadura, mejoras de montura, cofres en POIs nuevos (molino, ruinas, ermita — 3 celdas nuevas de mapa).
- Clima (lluvia/niebla densa) y rareza de botín.
- **Equipo:** jugabilidad-disenador (lidera), mundo-constructor, graficos-ingeniero, qa-capitan.

### Fase 4 — «Decisión de motor» (mes 3–4)
Con el contenido validado, el consejo decide GO/NO-GO documentado (`docs/DECISIONES.md`):
- **Camino A — Web pro:** migrar a Vite + three moderno (WebGPU donde haya), post real (bloom/SSAO), seguir 100 % navegador.
- **Camino B — Godot 4:** editor de escenas + export web; migración media.
- **Camino C — UE5:** fidelidad máxima, PC/consola, se pierde web/móvil directo.
Criterios: dónde está el cuello (contenido vs tecnología), a quién queremos llegar (web/móvil vs PC), coste de migración. Hasta la decisión, TODO el contenido se hace agnóstico (datos separados de motor donde sea posible).

### Fase 5 — «Completo y publicado» (meses 4–6)
- Contenido final: mapa completo por celdas, 10+ misiones, 3 jefes, progresión de equipo completa.
- Pulido: menús finales, opciones (calidad/controles/audio), accesibilidad básica, localización EN.
- Lanzamiento: itch.io + web propia, tráiler (aquí sí: Aura/Higgsfield con las escenas reales del juego), página de prensa.
- Online cooperativo: SOLO si el consejo lo aprueba tras el lanzamiento — arquitectura ya diseñada en `ROADMAP.md` §3.

## 4. Cadencia y rituales
- **Sprint semanal** con objetivo único enunciable en una frase; build versionada y publicada cada cierre de sprint (misma URL del artifact).
- **QA antes de cada publish** (pasada estándar del qa-capitan).
- **Consejo al cierre de cada fase** + dictamen escrito.
- Registro continuo: decisiones en `DECISIONES.md`, créditos de assets en `CREDITOS.md`, bitácora de sesión en Aura (`aura_session_log`) para que ningún Claude empiece de cero.

## 5. Definición de HECHO (global, cada build publicada)
1. `node --check` limpio y smoke del harness PASA. 2. Cero errores de consola en arranque y 3 min de juego. 3. Capturas ≥18/25 en la rúbrica gráfica. 4. Jugable con táctil. 5. Guardado compatible o migrado. 6. README actualizado. 7. Commit + push + artifact republicado.

## 6. Riesgos y mitigaciones
| Riesgo | Mitigación |
|---|---|
| Límite de gasto/tokens en sesiones largas | Fases en sprints pequeños; agentes solo cuando hay tarea clara; modelo sonnet en ejecución |
| Assets CC0 no disponibles/rotos | Verificar URLs con curl ANTES de codificar; fallback procedural siempre operativo |
| Peso de página >16 MB (artifact) | Presupuesto de assets 3 MB; compresión; si se supera, hosting propio (Fase 4) |
| Conectores (Aura/Higgsfield) intermitentes | Nada del bucle de juego depende de ellos; solo arte de apoyo y tráiler |
| Scope creep ("y ahora online") | Los tres vetos del consejo; online bloqueado hasta post-lanzamiento |

## 7. Estado y siguiente acción
- **Hoy:** estudio montado (plan + 6 skills + 5 agentes). 
- **Siguiente orden esperada:** *"Lanza la Fase 1"* → despliegue de `animacion-ingeniero` + `qa-capitan` con el encargo de matar el Minecraft.
