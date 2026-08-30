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

Presupuesto de GLBs embebidos: 385 KB + 340 KB = 725 KB (límite de fase: 3 MB). Margen amplio para Fase 2 (caballo riggeado).
