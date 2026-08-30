---
name: gamedev-graficos
description: Biblia de gráficos en tiempo real del videojuego «Acero y Corona» (Three.js/WebGL): iluminación de 3 puntos, paleta oficial derivada del concept art, atmósfera, materiales, presupuesto de render y rúbrica para evaluar capturas. Úsala SIEMPRE antes de tocar iluminación, cielo, materiales, texturas, post-procesado, niebla, sombras o color del juego; cuando una captura "se vea plana, pastel o Minecraft"; o cuando haya que juzgar si una build ha mejorado visualmente. Contiene los errores ya pagados en producción para no repetirlos.
---

# Gráficos en tiempo real — «Acero y Corona»

## Los cuatro pilares (de la investigación de Shadow of War, ver docs/REFERENCIAS.md)
**Paleta · Iluminación · Atmósfera · Silueta.** Toda tarea gráfica debe poder decir cuál de los cuatro mejora. Si no mejora ninguno, no es una tarea gráfica.

## Paleta oficial (derivada del concept art en acero-y-corona/concept-art/)
| Token | Hex | Uso |
|---|---|---|
| Musgo profundo | `#4e7a33` | pradera base |
| Musgo luz | `#74a04b` | variación de hierba |
| Oro heráldico | `#c9a24b` / brillo `#e6c877` | UI, monedas, ribetes, sol |
| Carmesí | `#9b2c2c` / brillo `#c34141` | estandartes, tabardo, daño |
| Piedra cálida | `#8f887c` | murallas (texturizada) |
| Cénit día | `#2a62b8` · Horizonte día `#d6e6f4` | cielo |
| Noche | cénit `#080d20` · horizonte `#18233c` | cielo nocturno |
| Atardecer | `#e8894a` horizonte | hora dorada |
| Luz noche | `#8fa4d8` · Luz atardecer `#ff8a3d` | tinte del sol |

## Iluminación de 3 puntos (valores en producción)
- **Clave:** DirectionalLight cálida `HSL(0.085, 0.55, 0.75–0.9)`, intensidad `lerp(0.22, 2.7, día)`, sombras PCFSoft 2048 (1024 móvil), shadow camera ±110 m siguiendo al jugador, `bias -0.0004`, `normalBias 0.6`.
- **Relleno:** HemisphereLight cielo→`horizonte actual`, suelo `#57503c`, intensidad `lerp(0.28, 0.6, día)` — **bajo**: el relleno alto lava el color.
- **Borde (rim):** DirectionalLight fría `#a8c4ff` OPUESTA al sol, intensidad `lerp(0.2, 0.65, día)`, sin sombras. Es lo que despega la silueta del fondo.
- Ambient tenue `#2c3550` 0.26–0.4. Fuegos: PointLight `#ff8b2e–#ff9a3a`, distancia 10–16, decay 2, parpadeo por seno+ruido.

## Atmósfera
- Niebla lineal teñida SIEMPRE del color del horizonte actual (`scene.fog.color.copy(bottomSky)`); alcance `far = lerp(300, 680, día)`.
- Cielo: domo con shader de degradado (top/bottom uniforms), estrellas con opacidad `1-día*1.4`, discos de sol/luna posicionados relativos a cámara, halo aditivo del sol (sprite radial) con opacidad por altura solar.
- Ventanas emisivas: material compartido `winMat` con `emissiveIntensity = lerp(1.8, 0.06, día)` — la vida nocturna del mundo.
- Grade cinematográfico: `filter: saturate(1.22) contrast(1.05)` sobre el canvas SOLO en ≥820px (coste en móvil), + viñeta radial CSS + destello rojo de daño.

## Materiales — «cada material cuenta su historia»
- Texturas procedurales por canvas (`canvasTex`): sillares con hiladas y juntas, madera vetada, tejas escamadas, revoco con manchas, paja, suelo moteado claro `#cfccbd` (repeat 90) que MODULA los colores de vértice sin lavarlos.
- El color del material TIÑE la textura: texturas claras + tinte por material.
- Suciedad en bases de muros, variación entre casas, color por instancia (`setColorAt`) en copas/arbustos con material blanco.

## Presupuesto de render (veta la silla del Motor si se rompe)
- 16,6 ms escritorio / 33 ms móvil medio. Draw calls < 150. Una sola luz con sombras.
- Vegetación SIEMPRE `InstancedMesh` (posiciones compartidas tronco/copa — ver error pagado nº1).
- PixelRatio cap: 2 escritorio, 1.6 móvil. Antialias off en móvil.

## Rúbrica para evaluar una captura (puntuar 1–5 cada una)
1. **¿La silueta del héroe se lee a 10 m?** 2. **¿Hay 3 planos de profundidad** (primer plano/medio/fondo con niebla)? 3. **¿La paleta es la oficial** o se coló gris/pastel? 4. **¿La luz tiene dirección** (sombras + rim visibles)? 5. **¿Hay un punto focal** (castillo, fuego, ventana)? Menos de 18/25 → no se publica.

## Errores YA PAGADOS (no repetir)
1. **Copas sin tronco:** dos `InstancedMesh` con posiciones aleatorias independientes NO forman árboles — generar posiciones UNA vez y compartir matrices.
2. **ACES + exposición alta desatura:** con `ACESFilmicToneMapping`, exposición >1.05 lava los medios a pastel. Subir el color con la luz clave y la paleta, no con la exposición.
3. **Relleno hemisférico fuerte = look plano:** por encima de ~0.65 de día mata el contraste clave/sombra.
4. **`canvas{position:fixed}` global** capturó el canvas del minimapa — acotar siempre a `#game canvas`.
5. **SwiftShader (tests headless) desatura:** las capturas del harness salen pálidas; juzgar el color en GPU real, la composición en el harness.
6. Un color definido SOLO dentro de un media query de tema → ilegible en el estado sin estampar (regla de artifacts).
