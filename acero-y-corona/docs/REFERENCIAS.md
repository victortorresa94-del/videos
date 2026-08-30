# Referencias de dirección de arte — juegos de El Señor de los Anillos

Investigación previa al rediseño visual v0.3. Fuentes al final.

## Qué hacen bien los juegos de ESDLA (y qué adoptamos)

### 1. Middle-earth: Shadow of War (Monolith, 2017)
- **Cuatro pilares declarados por su equipo de arte: paleta, iluminación, atmósfera y silueta.** Todo comunica tono e inmersión antes que el detalle del polígono.
- Sus cinco regiones se diseñaron **más grandes y más coloridas** que las de Shadow of Mordor, inspiradas en paisajes reales (este de Washington, Alaska, Islandia). Lección: el "medieval oscuro" no significa gris — significa color con intención.
- **La escala la dictan las métricas de movimiento del jugador** (distancias de agarre, ángulos). Lección: las dimensiones de puertas, murallas y árboles se calibran contra el personaje, no a ojo.
- **Narrativa material**: el texturizador se pregunta "cómo se ensució esta piedra, por qué la lechada es fina o gruesa". Lección: suciedad en la base de los muros, desgaste en tejados, variación entre casas.
- **Jerarquía visual por niveles**: la ciudad se organiza en alturas ascendentes con vistas lejanas despejadas. Lección: el castillo domina la silueta del horizonte; composición por capas (primer plano → aldea → castillo → montañas).

### 2. The Return of the King (EA, 2003)
- Su recuerdo se sostiene en **atmósfera, sonido y encuadre cinematográfico** con el grading de las películas. Lección: viñeta, grade cálido/contraste, niebla dramática, contraluz.

### 3. LOTRO (2007–hoy)
- Mundo "vivo y que respira": puntos de referencia legibles a distancia, aldeas con rutina. Lección: PNJ con variación (talla, ropa), hitos visibles desde lejos.

## Traducción al prototipo (v0.3, WebGL)

| Pilar | Aplicación concreta en «Acero y Corona» |
|---|---|
| **Paleta** | Verdes saturados tipo Comarca en pradera, tierra cálida en caminos, azul profundo en el cénit; grade de saturación/contraste sobre el lienzo (el "grading de película"). |
| **Iluminación** | Esquema de 3 puntos: sol cálido dorado (clave), hemisferio frío (relleno), **luz de borde azulada opuesta al sol** (rim). Noche azul con **ventanas emisivas encendidas** y antorchas con parpadeo. |
| **Atmósfera** | Niebla teñida del color del horizonte que se cierra de noche; nubes; halo solar; viñeta. |
| **Silueta** | Personajes reconstruidos con volúmenes redondeados (esferas/cilindros, ~7 cabezas de proporción), yelmo con cimera, hombreras, escudo redondo; caballo con anatomía (barril, grupa, cuello, corvejones); pinos de dos copas y robles esféricos más altos que las casas. |
| **Escala por métricas** | Jugador 1,8 m como vara de medir: puerta de casa 2,2 m, muralla ~14 m, torre ~22 m, árboles 9–14 m, caballo 1,6 m a la cruz. |
| **Narrativa material** | Base de muros más sucia, tejados desgastados, variación de color por instancia en copas y arbustos, flores en las praderas. |
| **Jerarquía por niveles** | Meseta ensanchada para que el castillo corone la silueta; aldea a media altura; campamentos en los márgenes. |

## Fuentes
- [80.lv — Modeling and Texturing the World of Middle-Earth](https://80.lv/articles/modeling-and-texturing-the-world-of-middle-earth) (técnicas del equipo de Shadow of War: narrativa material, escala por métricas, jerarquía por niveles)
- [Crafting Udûn: Concept Art Exploration (blog del equipo de Monolith)](https://shadowofwar.fandom.com/wiki/User_blog:MonolithAndy/Crafting_Ud%C3%BAn:_Concept_Art_Exploration) (pilares: paleta, iluminación, atmósfera, silueta)
- [Wikipedia — Middle-earth: Shadow of War](https://en.wikipedia.org/wiki/Middle-earth:_Shadow_of_War) (regiones más grandes y coloridas; paisajes reales de referencia)
- [GamesRadar — Best Lord of the Rings games](https://www.gamesradar.com/best-lord-of-the-rings-games/) · [GameSpot — Best LOTR games ranked](https://www.gamespot.com/articles/the-best-lord-of-the-rings-games-ranked/1100-6503738/) (qué hace memorables a RotK, Shadow of War y LOTRO)
