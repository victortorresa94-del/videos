---
name: gamedev-animacion
description: Pipeline de animación corporal y de lucha del videojuego «Acero y Corona» — animación esquelética profesional en Three.js (GLTF skinned, AnimationMixer, máquinas de estados, blending, armas ancladas a huesos), fuentes de modelos riggeados CC0, y la técnica de capa procedural. Úsala SIEMPRE antes de tocar cualquier movimiento de personaje, caballo o enemigo: caminar, correr, atacar, montar, morir, reaccionar al golpe; cuando los movimientos "parezcan de muñeco"; al integrar un modelo GLB nuevo; o al diseñar ventanas de ataque y cancelación. Es la respuesta técnica a "movimientos complejos con la tecnología más avanzada".
---

# Animación corporal y de lucha — «Acero y Corona»

## La tecnología correcta (la que usan los juegos reales)
**Malla skinned + esqueleto + clips de animación + blending.** En Three.js: `GLTFLoader` carga un `.glb` riggeado, `AnimationMixer` reproduce clips, `crossFadeTo` funde entre ellos. Nada de rotar cajas: cada hueso deforma la malla como en Unreal/Unity.

## Pipeline en Three.js r128 (nuestro stack)
1. **Cargadores** (scripts UMD permitidos por el CSP del artifact, host jsdelivr/npm):
   - `https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/loaders/GLTFLoader.js` → `THREE.GLTFLoader`
   - `https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/utils/SkeletonUtils.js` → `THREE.SkeletonUtils`
2. **Asset embebido**: el artifact bloquea fetch externo → el `.glb` va **base64 dentro del HTML**. Decodificar con `atob` a `ArrayBuffer` y usar `loader.parse(buffer, '', onLoad)` — cero red. Límite de página 16 MB (base64 = tamaño×1.33): presupuesto ~3 MB de GLBs.
3. **Clones**: NUNCA `mesh.clone()` para skinned — usar `THREE.SkeletonUtils.clone(gltf.scene)` (cada clon necesita su esqueleto). Un `AnimationMixer` por clon; los `AnimationClip` se comparten.
4. **Al integrar un modelo nuevo, inspeccionar SIEMPRE** (en el harness): `gltf.animations.map(a=>a.name)`, bounding box (escalar a 1.8 m el humano, 1.6 m cruz el caballo), eje frontal (¿mira +Z o −Z? compensar con un grupo padre), y nombres de huesos (`skeleton.bones.map(b=>b.name)`).

## Máquina de estados (plantilla del proyecto)
Estados mínimos humano: `IDLE · WALK · RUN · ATTACK · HIT · DEATH` (+`RIDE` si el clip existe).
- Locomoción por velocidad: `speed<0.5→IDLE`, `<7→WALK`, `≥7→RUN`. Fundido `crossFadeTo(next, 0.18, true)`; a/desde ataque `0.12`.
- **Ataque**: `LoopOnce` + `clampWhenFinished=true`; al evento `finished` del mixer → volver al estado de locomoción.
- **Muerte**: `LoopOnce` clampeado; ocultar tras el clip; al reaparecer, `mixer.stopAllAction()` y reset.
- `mixer.update(dt)` con el MISMO dt del juego (respeta el hit-stop: dt=0 congela la animación — correcto y deseable).

## Ventanas de combate (frame data — lo que separa un combate bueno de uno tosco)
- **Anticipación → golpe activo → recuperación.** El daño se aplica en el tramo activo: al **35–45 % del clip** de ataque (por tiempo normalizado `action.time/clip.duration`), NUNCA al pulsar el botón.
- **Ventana de cancelación**: la recuperación (últ. 30 %) se puede cancelar en esquiva o nuevo ataque (combo). Cooldown ≈ duración del clip × 0.85.
- **Reacción al golpe (HIT)**: clip corto interrumpible; si no hay clip, capa procedural: punch de escala 1.12→1 en 0.18 s + retroceso 1.1 m (ya en producción).
- Alternar 2 clips de ataque si existen (slash/chop) para el combo visual.

## Fuentes de modelos riggeados con licencia segura
- **KayKit (Kay Lousberg) — CC0**: pack "Adventurers" (Knight, Barbarian, Rogue, Mage) con ~30 animaciones incluidas (idle/walk/run/attack/hit/death). GitHub `kaylousberg`. Ideal: temática exacta.
- **Quaternius — CC0**: "Universal Animation Library" y packs RPG/Animals (¡caballo animado!). quaternius.com / poly.pizza.
- **three.js examples**: `Soldier.glb`/`Xbot.glb` (idle/walk/run) — solo para probar el pipeline, temática errónea.
- **Mixamo: NO redistribuible** como asset embebido en un HTML público — evitarlo en este proyecto.
- Toda incorporación se apunta en `docs/CREDITOS.md` (pack, autor, licencia, URL).
- **Verificar la URL de descarga con `curl -sI` ANTES de escribir código que dependa de ella.**

## Armas y props anclados a hueso
`modelo.getObjectByName('nombre_del_hueso')` (típicos: `handslot.r`, `HandR`, `Fist_R`, `arm_right_hand`) → `bone.add(espada)`. La espada procedural existente (`sword` group) se reutiliza tal cual. Ajustar offset/rotación local mirando una captura, no a ciegas.

## Capa procedural SOBRE la esquelética (lo avanzado de verdad)
- **Inclinación al girar/galopar**: rotar el root ±0.08 rad según velocidad angular.
- **Head-look**: rotar el hueso del cuello hacia el objetivo cercano (máx ±0.6 rad, lerp 8/s).
- **Pies al terreno (IK barato)**: en pendientes, alinear el root a la normal del terreno (máx 0.2 rad).
- Caballo: frecuencia de patas ∝ velocidad; transición walk→canter→gallop por umbrales 3/8 m/s.

## Errores YA PAGADOS
1. Muñecos de primitivas rotando miembros = "Minecraft". Es fallback de emergencia, no producto.
2. En tests headless con SwiftShader el tiempo simulado va más lento que el real: los cooldowns parecen "fallar". Compensar en el TEST (forzar `attackCd=0`), no en el juego.
3. Tronco y copa (o cuerpo y sombrero) colocados con aleatoriedad independiente se desalinean: una sola fuente de posiciones.
