---
name: gamedev-mundo3d
description: Construcción del mundo 3D del videojuego «Acero y Corona»: terreno procedural, escala por métricas del jugador, colocación de estructuras y vegetación, caminos, colisiones, LOD y el plan de crecimiento del mapa por celdas. Úsala SIEMPRE antes de tocar el terreno, añadir o mover edificios, poblar zonas nuevas, ajustar tamaños de nada que exista en el mundo, implementar colisiones o navegación, o ampliar el mapa.
---

# Mundo 3D — «Acero y Corona»

## Escala por métricas (regla de Shadow of War: las dimensiones las dicta el jugador)
Jugador = **1,80 m**. Todo se mide contra él:
| Elemento | Medida | Elemento | Medida |
|---|---|---|---|
| Puerta de casa | 2,2 m | Muralla | ~14 m |
| Planta de casa | 4,2 m | Torre | ~22 m |
| Caballo (cruz) | 1,6 m | Torre del homenaje | ~34 m |
| Árbol | 9–14 m | Puente/paso | ≥3 m ancho |

Nada se escala "a ojo": si un elemento nuevo no tiene fila en esta tabla, se añade la fila primero.

## Terreno (sistema actual)
- Ruido de valor + fBm 4 octavas: `getHeight(x,z)` = colinas (freq 0.011, amp 24) + detalle (0.055, 3.2) + **montañas de borde** (potencia 2.1 desde el 68 % del radio, amp 82) que cierran el mundo sin muros invisibles.
- **Meseta del castillo**: `smoothstep(48, 88, distancia)` funde altura fija 1.6 con el terreno — el castillo corona la silueta y la explanada queda jugable.
- Color por vértice según altura/bioma (paleta oficial de gamedev-graficos) + textura de detalle clara modulando.
- **Caminos**: polilíneas (`pathA` puerta→aldea, `pathB` puerta→campamento), distancia punto-segmento tiñe tierra <3,4 m con borde suave; la vegetación respeta <4 m. Todo POI nuevo se conecta con camino o no existe.

## Colocación (reglas duras)
1. **Una sola fuente de posiciones** por conjunto: `scatterPositions()` genera, y TODAS las capas del mismo objeto (tronco+copa+copa2) comparten matrices. (Error pagado: bosque de copas flotantes.)
2. Exclusiones al dispersar: explanada del castillo (<46 m del centro), aldea (<26 m de su centro), caminos (<4 m), agua (y<WATER+1.2), nieve (y>50).
3. Todo `InstancedMesh`; color por instancia para variación natural.
4. Los POI se colocan A MANO con intención (visibilidad desde el camino, composición): castillo centro, aldea SO, campamentos NE/SO lejos, hitos futuros (molino, ruinas, ermita) a 1 camino de distancia de algo.

## Colisiones (estado y plan)
- Hoy: solo terreno (clamp a `getHeight`) + límites de mundo. Se atraviesan muros — deuda conocida.
- **Plan (Fase 2)**: colisionadores primitivos por estructura — cilindros (torres, árboles gordos) y AABB rotados (muros, casas) en una lista `colliders[]`; resolución por empuje del círculo del jugador (radio 0,4 m) en XZ, ANTES del clamp de altura. El castillo necesita hueco navegable en la puerta. Sin física de terceros: 20 líneas bien probadas.

## Crecimiento del mapa (Fase 3+)
- Mundo actual 560×560 m denso > mundo enorme vacío. Crecer por **celdas** de 280 m: cada celda nueva llega con bioma, 1 POI, 1 misión y presupuesto propio de instancias; se activa/desactiva por distancia (visible tras la niebla ≈ 680 m máx).
- Navegación IA (cuando haya escoltas/patrullas largas): grid 4 m + A* perezoso; hasta entonces, steering directo + caminos.

## Rúbrica de una zona nueva
1. ¿Se ve un hito desde su entrada? 2. ¿El camino te lleva sin mapa? 3. ¿Hay razón para salirse del camino (botín/vista)? 4. ¿Silueta nocturna reconocible (fuego/ventana)? 5. ¿FPS estables dentro? Las 5 o no se integra.
