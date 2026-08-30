---
name: gamedev-jugabilidad
description: Diseño de jugabilidad y game feel del videojuego «Acero y Corona»: números de impacto (hit-stop, sacudida, retroceso), economía de vigor y oro, IA de enemigos por estados, diseño de misiones, curva de dificultad y la rúbrica del "test de los 30 segundos". Úsala SIEMPRE antes de tocar combate, controles, enemigos, misiones, economía, recompensas o dificultad; cuando el juego "no se sienta bien" aunque funcione; o al evaluar si una mecánica nueva merece existir.
---

# Jugabilidad y game feel — «Acero y Corona»

## El test de los 30 segundos (rúbrica suprema)
Deja a alguien 30 segundos con el juego sin instrucciones. ¿Movió la cámara y al personaje sin frustrarse? ¿Encontró algo que quiso tocar? ¿Sonrió con algo (galope, salto, golpe)? Dos de tres o la build no avanza. La diversión del movimiento base es previa a todo sistema.

## Números de impacto (valores en producción — cambiarlos solo con motivo)
| Efecto | Valor | Nota |
|---|---|---|
| Hit-stop al conectar golpe | 55 ms (dt=0) | 40–80 ms es el rango sano |
| Sacudida de cámara: golpe dado / recibido | amp 0.35 / 0.5, decae 2.4/s | nunca >0.8 |
| Retroceso del enemigo | 1.1 m en la dirección del golpe | |
| Punch de escala en el golpeado | ×1.12→1 en 0.18 s | sustituye al flash si los materiales son compartidos |
| Cooldown de ataque | 0.55 s (≈ duración clip × 0.85) | daño al 35–45 % del clip |
| I-frames tras recibir daño | 1.0 s (`hurtCd`) | |
| FOV | 55 base · 60 sprint · 66 galope, lerp 4/s | vender velocidad sin mover números |
| Números de daño y oro flotantes | siempre | el jugador necesita VER la causa-efecto |

## Economía (fuentes y sumideros — mantener el equilibrio al añadir)
- **Fuentes:** monedas del mundo (+1), bandido derrotado (+2), [futuro: cofres, misiones].
- **Sumideros:** espada forjada (40), poción (10), [futuro: armadura, montura mejor, propiedad].
- Regla: el jugador debe poder pagar la primera mejora tras ~10 min de juego natural. Todo precio nuevo se justifica contra esa curva.
- El vigor es la economía del combate: correr/galopar drena (22/9 por s), regenera 14/s; atacar es gratis PERO el posicionamiento no — esa asimetría crea decisiones.

## IA de enemigos (máquina de estados, ampliable)
`PATRULLA (deambula por el campamento, radio 8) → ALERTA (jugador <16 m: persigue a 3.4 m/s) → ATAQUE (<2.3 m, telegrafiado, daño 9) → [RECUPERACIÓN 0.8 s vulnerable] → muerte → REAPARICIÓN (40–70 s, nunca con el jugador a <30 m)`.
- Regla Miyazaki: TODO ataque enemigo se telegrafia ≥0.4 s antes del daño. Sin daño sorpresa.
- Próximos escalones: recuperación vulnerable tras su ataque, arqueros (mantener distancia), jefe con 2 patrones + fase al 50 %, guardia con notoriedad (robar/pegar en la aldea ⇒ persecución por niveles).

## Diseño de misiones (plantillas del proyecto)
Tipos: `collect` (N objetos) · `reach` (llegar a marcador) · `defeat` (N enemigos) · [futuro: `escort`, `timed`, `boss`]. Toda misión: título en el HUD, progreso numérico, marcador en minimapa cuando aplica, recompensa explícita al completar (toast + oro). Cadena principal corta (3–5) + misiones repetibles de las mecánicas más divertidas. Nunca una misión de una mecánica que aún no divierte sola.

## Curva de dificultad
Primera zona (meseta del castillo) = santuario sin enemigos. Los campamentos, a >60 m de la puerta. El jugador elige la dificultad por geografía (más lejos = más riesgo, mejor botín). La muerte respawnea en la puerta sin perder oro (por ahora): castigo = tiempo, no progreso.

## Controles (contrato intocable salvo decisión del consejo)
PC: WASD + ratón-arrastre + Espacio + Shift + clic-sin-arrastre/J ataque + E interactuar + Esc/P pausa. Móvil: joystick dinámico izquierda, mirar derecha, botones ⚔️/SALTAR/CORRER + botón contextual (🐴/🛒). Toda interacción nueva pasa por E/botón contextual con pista en pantalla — no se añaden teclas nuevas sin veto del consejo.

## Errores YA PAGADOS
1. Ataque disparado al arrastrar la cámara — el clic solo golpea si el puntero se movió <6 px.
2. Daño aplicado al pulsar en vez de en el frame activo del clip: se corrige al integrar animación esquelética.
3. Enemigos de 1 golpe = combate sin decisiones; 3 PV mínimo y recompensa por matar.
4. Mecánica sin feedback (sin número/partícula/sonido) = el jugador no la percibe aunque exista.
