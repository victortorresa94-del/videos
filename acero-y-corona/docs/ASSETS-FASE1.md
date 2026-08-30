# Inspección de assets — Fase 1 «Cuerpo y espada»

Modelos: **KayKit — Character Pack: Adventurers** (Kay Lousberg, CC0), personajes `Knight` (jugador) y `Rogue_Hooded` (bandidos). Descargados de `github.com/KayKit-Game-Assets/KayKit-Character-Pack-Adventures-1.0` (verificado con `curl -sI` → HTTP 200, ~3,6 MB cada uno en origen), recortados localmente con `@gltf-transform/cli` + `@gltf-transform/core` a 385 KB (`knight.glb`) y 340 KB (`bandit.glb`) — total 725 KB, dentro del presupuesto de 3 MB.

## Qué se recortó y por qué
El pack completo trae 76 clips de animación y variantes de arma/escudo/casco por personaje, todo compartiendo el mismo esqueleto de 41 huesos. Hallazgo del pipeline: **`Animation.dispose()` en gltf-transform NO elimina en cascada sus canales/samplers** (son propiedades de grafo independientes) — sin eliminarlos a mano, `prune()` no libera sus accessors y el archivo apenas baja de tamaño. Solución aplicada:
1. Eliminar por completo (canal + sampler + animación) los 69 clips no usados, quedándonos con 7.
2. Eliminar los nodos de arma/escudo/ballesta que el pack añade como mallas visibles por defecto (el jugador y el bandido llevan la espada procedural del juego anclada al hueso de la mano, no la malla de arma del pack).
3. Eliminar canales de animación "estáticos" (huesos que no se mueven en ese clip — la mayoría solo rota, no traslada/escala) comparando valores con una tolerancia de 1–2 mm.
4. `resample()` + `prune()` + `dedup()` + `weld()` para reclamar los accessors ahora huérfanos.
Resultado: 8 839 → 298 accessors (Knight), 8 827 → 294 accessors (Rogue). `gltf-transform validate`: **0 errores** en ambos (solo avisos informativos preexistentes del pack, no introducidos por el recorte).

## Clips de animación conservados (idénticos en ambos personajes, mismo esqueleto)
| Clip | Duración | Uso en el juego |
|---|---|---|
| `Idle` | 1.067 s | reposo / jinete sentado al montar |
| `Walking_A` | 1.067 s | caminar |
| `Running_A` | 0.800 s | correr / esprint |
| `1H_Melee_Attack_Slice_Horizontal` | 1.067 s | ataque 1 (combo) |
| `1H_Melee_Attack_Chop` | 1.067 s | ataque 2 (combo) |
| `Hit_A` | 0.667 s | reacción al recibir daño |
| `Death_A` | 0.800 s | muerte (bandidos) |

## Huesos (esqueleto de 41 huesos, compartido por Knight y Rogue_Hooded)
`root, hips, spine, chest, upperarml, lowerarml, wristl, handl, handslotl, upperarmr, lowerarmr, wristr, handr, handslotr, head, upperlegl, lowerlegl, footl, toesl, upperlegr, lowerlegr, footr, toesr` + 18 huesos de control IK (`kneeIK*, control-*-roll*, heelIK*, IK-foot*, IK-toe*, elbowIK*, handIK*`).

**Aviso de nombres**: en el `.glb` original los huesos llevan punto (`handslot.r`), pero `THREE.GLTFLoader` sanea los nombres al cargarlos y los deja **sin punto** (`handslotr`). El código del juego debe usar `getObjectByName('handslotr')`.

**Hueso de la mano derecha (ancla de la espada)**: `handslotr` (hijo de `handr`, hijo de `wristr`) — socket vacío pensado exactamente para colgar un arma.

## Bounding box y escala (pose de referencia, T-pose de bind — la altura no cambia entre poses porque solo rotan los brazos)
| | Knight | Rogue_Hooded |
|---|---|---|
| bboxMin | (−0.971, −0.000, −0.542) | (−0.971, −0.000, −0.567) |
| bboxMax | (0.971, 2.467, 0.711) | (0.971, 2.251, 0.589) |
| tamaño (X,Y,Z) | 1.943 × 2.467 × 1.254 | 1.943 × 2.251 × 1.157 |
| escala aplicada para altura objetivo | ×0.7297 → 1,80 m (jugador) | ×0.7774 → 1,75 m (bandidos) |

(El ancho X en T-pose incluye los brazos extendidos horizontalmente — no representa el ancho real del personaje de pie; en `Idle`/`Walking`/`Running` los brazos bajan y el ancho efectivo es normal.)

## Eje frontal
Confirmado con las posiciones de mundo de los huesos de los dedos del pie y el centro de la malla de la capa (bind pose, `root` sin rotación):
- `toesl`/`toesr` → Z ≈ **+0.096** (los pies apuntan a +Z).
- malla `*_Cape` → Z ≈ **−0.215** (la capa cuelga detrás, en −Z).

**El modelo mira hacia +Z local.** Esto coincide exactamente con la convención ya usada en el juego (`player.group.rotation.y = P.yaw` con `P.yaw = Math.atan2(wx,wz)`, que orienta el eje local +Z del objeto hacia la dirección de movimiento) — **no hace falta ningún grupo corrector de orientación**, el modelo se cablea directo.

## Mallas (piezas del cuerpo, sin las variantes de arma/escudo que se eliminaron)
- Knight: `Knight_Helmet, Knight_Cape, Knight_ArmLeft, Knight_ArmRight, Knight_Body, Knight_Head, Knight_LegLeft, Knight_LegRight`
- Rogue_Hooded: `Rogue_Cape, Rogue_ArmLeft, Rogue_ArmRight, Rogue_Body, Rogue_Head_Hooded, Rogue_LegLeft, Rogue_LegRight`

## Herramienta de inspección
Harness dedicado en el scratchpad de la sesión (three.min.js r128 + GLTFLoader.js + SkeletonUtils.js locales, Chromium headless vía Playwright-core, `THREE.GLTFLoader.parse` sobre los `.glb` en base64): `gltf.animations`, `skeleton.bones`, `Box3().setFromObject(scene)` y posiciones de mundo de huesos clave. 0 `pageerror`, 0 errores de consola al cargar ambos modelos.

---

# Inspección de assets — Sprint 2 «La montura»

Modelo: **Quaternius — "LowPoly Animated Animals" (`Horse.fbx`)**, CC0. Descargado del flujo de descarga anónima de itch.io ($0, `Farm Animals Animated by Quaternius.zip`, 6,6 MB, verificado con `curl` contra el endpoint real de descarga firmada — la URL de la ficha de itch.io no sirve el archivo directamente, requiere el flujo `download_url`→`file/{upload_id}`→URL R2 firmada de 60 s, documentado en `docs/CREDITOS.md`). El zip trae 7 animales (Cow, Horse, Llama, Pig, Pug, Sheep, Zebra) en FBX/OBJ/Blend — **sin glTF**.

## Conversión FBX → glTF y recorte
1. `FBX2glTF` v0.9.7 (paquete npm `fbx2gltf`, binario prebuilt) convierte `Horse.fbx` (1,02 MB) → `Horse_conv.glb` (294,6 KB), confirmando **6 animation stacks**: `Death, Idle, Jump, Run, Walk, WalkSlow`.
2. `@gltf-transform/core`: se eliminan (canal+sampler+animación, mismo cuidado que con el Knight — `Animation.dispose()` no basta) los clips `Death`, `Jump`, `WalkSlow` — nos quedamos con los 3 pedidos por el encargo (Idle/Walk/Gallop). `resample()+prune()+dedup()+weld()` → **160,6 KB**, prune reporta 11 nodos y 99 accessors huérfanos liberados.
3. `gltf-transform validate`: **0 errores**. Únicos avisos: `NODE_SKINNED_MESH_NON_ROOT` / `NODE_SKINNED_MESH_LOCAL_TRANSFORMS` — informativos, preexistentes en el export de FBX2glTF (el nodo skinned no es la raíz de la escena), no afectan al render con `SkeletonUtils.clone`.

## Clips conservados
| Clip (nombre real, con el pipe del Armature) | Duración | Uso en el juego |
|---|---|---|
| `Armature\|Idle` | 6.25 s (bucle largo, varias subposes) | caballo parado / jinete no montado |
| `Armature\|Walk` | 2.667 s | trote a velocidad de paseo |
| `Armature\|Run` | 0.833 s (zancada rápida) | galope — `P.speed>=8` montado |

Sin clip de "Attack"/"Kick" en este pack (no hace falta: el caballo no ataca en el diseño actual).

## Huesos (28, esqueleto `Armature`)
`root, Body, Torso, Hips, Back, Shoulders, Neck, Head, Tail1..Tail4, FrontLegL/R, FrontUpLegL/R, FrontLowLegL/R, FrontFootL/R, BackLegL/R, BackUpLegL/R, BackLowLegL/R, BackFootL/R`.

**Hueso usado para escalar a la cruz**: `Shoulders` (a la altura de la base del cuello/inicio de la espalda, justo donde se apoyaría la silla) — ver `boneHeightScale()` en el código: escala = 1,6 / (alturaMundoDe(`Shoulders`) − bboxMin.y).

## Bounding box y escala (bind pose, unidades originales del FBX sin normalizar — el export de FBX2glTF no reescala a metros)
| | Valor |
|---|---|
| bboxMin | (−1.285, 0.018, −4.118) |
| bboxMax | (1.285, 6.924, 5.117) |
| tamaño (X,Y,Z) | 2.570 × 6.905 × 9.235 |
| altura de `Shoulders` (mundo, bind) | 4.11 |
| escala aplicada (cruz→1,6 m) | ×0.3893 → altura total resultante (orejas) ≈ 2,70 m, cabeza en alto/alerta — plausible para un caballo estilizado con la cabeza erguida en `Idle` |

## Eje frontal
`Head` (z≈+3.46), patas delanteras (z≈+1.86) vs. patas traseras (z≈−2.20) y cola (z≈−2.68 a −3.63, bind pose): **el modelo mira hacia +Z local**, igual que el Knight — no hace falta grupo corrector, se cablea con `clone.rotation.y = H.yaw` directo (misma convención que ya usa el juego).

## Mallas
`Horse_1, Horse_2` (dos primitivas), materiales `Material.003`/`Material.006`, **sin texturas** — coloreado 100% por `COLOR_0` (vertex colors), confirmado visualmente (pelaje castaño, crin/cola negras, mancha blanca/blaze en la cara — un detalle de pelaje real de caballo, no un error de render).

## Hallazgo de depuración (para que quede registrado)
Al revisar capturas de la primera integración parecía que la espada del jinete "colgaba tumbada sobre el cuello del caballo" al montar. Tras aislar caballo y jinete por separado (`clone.visible=false` de cada uno) se confirmó que esa forma clara y alargada es el **blaze/mancha blanca de la cara del caballo** (parte del pelaje del modelo), no la espada — la espada apenas se ve desde perfil porque queda pegada al costado, oculta tras la pierna/cadera. Sí se encontró un artefacto real: con el fémur doblado −1.2 rad para la pose de monta, un par de vértices de `Knight_Cape` (con algo de peso esquelético cerca de la cadera) se estiraban en un hilo rojo hacia el suelo — se resolvió ocultando la capa (`Knight_Cape.visible=false`) mientras `H.mounted` es verdadero (técnica habitual: muchos juegos retiran o pliegan la capa al montar).
