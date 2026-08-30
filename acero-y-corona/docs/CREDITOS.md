# Créditos de assets — «Acero y Corona»

Todo asset de terceros incorporado al juego se registra aquí (obligatorio antes de integrar).

| Asset | Autor | Licencia | Fuente | Dónde se usa |
|---|---|---|---|---|
| Three.js r128 | three.js authors | MIT | cdnjs | motor de render |
| Tipografías Cinzel y Spectral | Google Fonts | OFL | fonts.google.com | UI del juego |
| Concept art (caballero, castillo, aldea, caballo) | Generado por encargo (gpt-image-2 vía Aura Studio, cuenta de Víctor) | uso propio del proyecto | lienzo «Acero y Corona» | biblia visual (`concept-art/`) |
| KayKit — Character Pack: Adventurers (`Knight.glb`) | Kay Lousberg (kaylousberg.itch.io) | CC0 1.0 Universal (uso libre personal/comercial, sin atribución requerida) | github.com/KayKit-Game-Assets/KayKit-Character-Pack-Adventures-1.0 (`addons/kaykit_character_pack_adventures/Characters/gltf/Knight.glb`) | jugador — `acero-y-corona/assets/knight.glb` (recortado de 3,66 MB a 385 KB: 7 de 75 clips, canales estáticos y piezas de equipo variante eliminados con `@gltf-transform`; ver `docs/ASSETS-FASE1.md`) |
| KayKit — Character Pack: Adventurers (`Rogue_Hooded.glb`) | Kay Lousberg (kaylousberg.itch.io) | CC0 1.0 Universal (uso libre personal/comercial, sin atribución requerida) | github.com/KayKit-Game-Assets/KayKit-Character-Pack-Adventures-1.0 (`addons/kaykit_character_pack_adventures/Characters/gltf/Rogue_Hooded.glb`) | bandidos — `acero-y-corona/assets/bandit.glb` (mismo recorte: 3,60 MB → 340 KB) |
| GLTFLoader.js / SkeletonUtils.js (three.js r128 examples) | three.js authors | MIT | cdn.jsdelivr.net/npm/three@0.128.0/examples/js/{loaders/GLTFLoader.js,utils/SkeletonUtils.js} | carga y clonado de las mallas skinned |
| Quaternius — "LowPoly Animated Animals" (`Horse.fbx`) | Quaternius (quaternius.com / quaternius.itch.io) | CC0 1.0 Universal ("Creative Commons Zero", uso libre personal/comercial, sin atribución requerida — licencia confirmada en la ficha de itch.io) | quaternius.itch.io/lowpoly-animated-animals (paquete «Farm Animals Animated by Quaternius.zip», descarga anónima de $0 verificada con el flujo de descarga pública de itch.io, `FBX/Horse.fbx`) | montura — `acero-y-corona/assets/horse.glb` (convertido de FBX a glTF con `FBX2glTF` v0.9.7 y recortado con `@gltf-transform`: de 6 clips [Death, Idle, Jump, Run, Walk, WalkSlow] a 3 [Idle, Walk, Run] — 294 KB → 160 KB; ver `docs/ASSETS-FASE1.md`) |

Presupuesto de GLBs embebidos: 385 KB (knight) + 340 KB (bandit) + 160 KB (horse) = 885 KB (límite de fase: 3 MB). Margen amplio para Fase 2+.

## Candidatos de caballo descartados (con motivo, para no repetir la búsqueda)
- **KayKit (org `KayKit-Game-Assets` / kaylousberg.itch.io)**: no existe ningún pack de caballo/montura en su catálogo (revisado el listado completo de packs). Descartado por inexistencia, no por licencia.
- **Quaternius "Ultimate Animated Animal Pack"** (12+ animales, incluiría Gallop explícito): el enlace de descarga público en quaternius.com no resuelve a un archivo directo (botón `#inline`, sin `href`) y no tiene página itch.io propia visible — parece exclusivo de Patreon. Descartado por no poder verificarse la URL con `curl -sI` (regla dura del encargo).
- **three.js examples `Horse.glb`** (`examples/models/gltf/Horse.glb`, usado en `webgl_morphtargets_horse`): descarga verificada (HTTP 200 vía `raw.githubusercontent.com`), pero (a) es animación por **morph targets**, no esqueleto/huesos — no encaja con "caballo riggeado" ni con la técnica `SkeletonUtils.clone`; y (b) el modelo está acreditado a "Mirada" / proyecto "ROME" sin licencia CC0 explícita en el repo (distinto del resto de `examples/models`, que sí llevan credit files) — riesgo de licencia no descartable. Descartado por (a) técnica y (b) licencia dudosa.
- **OpenGameArt "Rigged Horse"** (CC0 confirmado): solo `.blend`, **sin animaciones** (el rigger dejó el esqueleto listo pero no animó ningún clip) — habría que animarlo a mano, fuera de alcance del sprint. Descartado por falta de clips.
- **KAG3D "3D Animated Horse"**: de pago ($4 mínimo), sin confirmación de derecho de redistribución embebida. Descartado por licencia (regla dura: solo CC0 o redistribución clara).
