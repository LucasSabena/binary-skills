# Generación de Imágenes con IA — Binary Studio

## Estrategia Visual

Binary Studio usa una combinación de recursos reales y generados/editados con IA para mantener un estilo gráfico consistente en todo su contenido.

### Fuentes de Recursos Visuales

| Fuente | Uso | Tratamiento |
|--------|-----|-------------|
| **Fotos reales de internet** | Personas (referentes del diseño), logos, productos | Editar con IA para aplicar el estilo Binary |
| **Generación 100% IA** | Fondos abstractos, texturas, objetos decorativos, mockups | Generar directamente con el prompt base |
| **Fotos propias** | Behind the scenes, equipo, workspace | Editar con IA para mantener consistencia |
| **Capturas/screenshots** | Herramientas, webs, código | Integrar en composición con estilo Binary |

---

## Firma Visual: "Binary Lighting"

Lo que distingue las imágenes de Binary Studio de cualquier otro estilo editorial B&N es el **"Binary Lighting"**: luces de color de la paleta de marca aplicadas como geles sutiles en la iluminación del set.

### Concepto
Es como si en el estudio fotográfico hubiera filtros de color (geles) en las luces. El resultado es una imagen mayormente en blanco y negro, pero con **tintes sutiles del color de marca en los reflejos, bordes y brillos** del sujeto.

### Cómo se ve
| Sin Binary Lighting | Con Binary Lighting |
|---|---|
| B&N puro, genérico, podría ser de cualquiera | B&N con un **reflejo verdoso sutil en el borde** del objeto, un **toque magenta en la sombra** |
| Profesional pero anónimo | **Profesional y reconocible como Binary** |

### Luces disponibles
| Luz | Color | Uso |
|-----|-------|-----|
| **Luz verde** | #00CC66 | Default. Reflejo sutil en bordes, contornos, highlights |
| **Luz magenta** | #CC0A82 | Secundaria. Tinte en sombras, contraluz, borde opuesto al verde |
| **Luz amarilla** | #F8F800 | Uso puntual. Para piezas energéticas, acentos warm |

### Dirección de las luces
Por defecto, la luz verde es general (sin dirección específica). Pero se puede pedir la dirección:
- *"luz verde a la derecha"* → el tinte verde viene desde la derecha
- *"luz verde arriba, magenta abajo"* → verde como luz cenital, magenta desde abajo
- *"luz magenta a la izquierda y verde a la derecha"* → las dos luces desde lados opuestos
- Si no se especifica dirección, se aplica de forma general/lateral

### Regla de sutileza
- La luz de color NO es un neón, NO es un glow evidente
- Es un **tinte sutil** — como un gel fotográfico, no un efecto de Photoshop
- El 80-90% de la imagen sigue siendo B&N/escala de grises
- El color aparece solo en reflejos, bordes, contornos — nunca inunda la imagen

---

## Efecto "Binary Decay" (Pixelación)

Efecto opcional que descompone gradualmente la imagen en píxeles, como si la foto se estuviera "decodificando" en su forma binaria. **Está desactivado por defecto.**

### Concepto
El nombre "Binary" es digital, los píxeles son la unidad mínima de lo digital. La pixelación gradual es la imagen "revelándose" o "descomponiéndose" en su esencia binaria. Es conceptual, no decorativo.

### Cómo funciona
- Una parte de la imagen se mantiene nítida/perfecta
- Gradualmente se va pixelando hacia una dirección
- Los píxeles más grandes pueden tener el color de marca (verde, magenta)
- La transición es gradual, no un corte abrupto

### Parámetros controlables

| Parámetro | Opciones | Default |
|-----------|----------|--------|
| **Activación** | Con/sin pixelado | **Sin** (no se aplica salvo que se pida) |
| **Dirección** | Desde arriba, abajo, izquierda, derecha, esquina | A elección del usuario |
| **Intensidad** | Suave, medio, fuerte | Medio |

### Cómo pedirlo

| Pedido natural | Qué hace |
|---|---|
| *"con pixelado desde la derecha"* | Imagen nítida a la izquierda, se pixela hacia la derecha |
| *"con pixelado suave desde abajo"* | Pixelación leve que sube desde el borde inferior |
| *"con pixelado fuerte desde arriba"* | Pixelación intensa que baja desde arriba |
| *"con pixelado desde la esquina inferior derecha"* | Se pixela en diagonal |
| (no se menciona) | **No se aplica pixelado** |

---

## Cómo Pedir una Imagen (Referencia Rápida)

El formato natural de pedido es: **[sujeto] + [opciones]**. Todo lo que no se especifique usa el default.

### Defaults (si no se pide nada especial)
- Fondo: negro (Modo Dark)
- Luces: verde sutil general
- Pixelado: ninguno

### Ejemplos de pedidos reales

```
│ Pedido                                                          │ Resultado                                                   │
│ "Taza de café"                                                   │ Taza B&N, fondo negro, luz verde sutil, sin pixelado          │
│ "Taza de café con pixelado desde la derecha"                     │ + pixelación gradual de derecha a izquierda                    │
│ "Taza de café con luz magenta a la izquierda y verde a la        │ Luces custom desde lados opuestos                             │
│  derecha"                                                       │                                                               │
│ "Retrato de Dieter Rams con pixelado suave desde abajo"          │ Retrato editorial + pixelado leve subiendo desde abajo        │
│ "MacBook con web, modo cutout"                                   │ Laptop sobre fondo blanco para recortar                       │
│ "Teclado mecánico con luz amarilla y pixelado fuerte              │ Luz energética + pixelación intensa desde la esquina            │
│  desde la esquina inferior derecha"                              │                                                               │
```

---

## Modos de Generación

No todas las imágenes van sobre fondo negro. Según el uso, elegir el modo correcto:

| Modo | Fondo | Cuándo usarlo |
|------|-------|---------------|
| **Modo Dark** (default) | Negro #202020 | Posts de feed, portadas, slides |
| **Modo Cutout** | Blanco puro o transparente | Cuando se necesita recortar el sujeto para usar en otra composición |
| **Modo Color** | Negro con acentos de color | Cuando se quiere más presencia de los colores de marca |

Al pedir una imagen, especificar el modo. Si no se especifica, se usa Modo Dark por defecto.

---

## Prompt Base — Estilo Visual Binary Studio

Este prompt define el estilo visual y debe usarse como **prefijo** en toda generación de imagen. Solo se cambia la parte de `[SUJETO]` y opcionalmente el `[MODO]`.

### Prompt Base (Español)

```
Estilo: fotografía editorial de alto contraste, mayormente blanco y negro, sobre fondo negro sólido (#202020).

Características visuales:
- Iluminación dramática lateral, sombras profundas, highlights marcados
- Fondo completamente negro o muy oscuro, sin texturas ni gradientes
- Estética minimalista, limpia, sin elementos decorativos innecesarios
- Composición centrada o con espacio negativo intencional
- Look profesional, editorial, premium — nunca stock genérico

Binary Lighting (IMPORTANTE — esto es lo que hace el estilo único):
- Aplicar luces de color sutil como geles fotográficos en la iluminación
- Luz principal con tinte verde sutil (#00CC66) visible en los bordes, reflejos y highlights del sujeto
- Opcionalmente, luz secundaria con tinte magenta (#CC0A82) en las sombras o como contraluz
- El color debe ser SUTIL, como un gel en un set de fotos — no un glow ni un neón artificial
- El 80-90% de la imagen sigue siendo escala de grises, con el color solo en reflejos y bordes

Tratamiento del sujeto:
- Si es una persona: retrato editorial, expresión seria o pensativa, fondo negro
- Si es un objeto: aislado sobre fondo negro, iluminación de estudio
- Si es abstracto: formas geométricas, líneas limpias, minimalismo

No incluir: texto, logos, marcas de agua, bordes, marcos, elementos de UI.

[SUJETO]: ...
```

### Prompt Base (Inglés — para Gemini/Midjourney/DALL-E)

```
Style: high-contrast mostly black and white editorial photography on solid dark background (#202020).

Visual characteristics:
- Dramatic side lighting, deep shadows, strong highlights
- Completely black or very dark background, no textures or gradients
- Minimalist, clean aesthetic, no unnecessary decorative elements
- Centered composition or with intentional negative space
- Professional, editorial, premium look — never generic stock

Binary Lighting (IMPORTANT — this is what makes the style unique):
- Apply subtle colored lighting as photographic color gels on studio lights
- Main light with subtle green tint (#00CC66) visible on edges, reflections and highlights of the subject
- Optionally, secondary light with magenta tint (#CC0A82) on shadows or as backlight
- Color must be SUBTLE, like a gel on a photo set — not a glow or artificial neon
- 80-90% of the image remains grayscale, color only appears in reflections and edges

Subject treatment:
- If person: editorial portrait, serious or thoughtful expression, black background
- If object: isolated on black background, studio lighting
- If abstract: geometric shapes, clean lines, minimalism

Do not include: text, logos, watermarks, borders, frames, UI elements.

[SUBJECT]: ...
```

### Variante: Modo Cutout (fondo blanco)

```
[Mismas características que el prompt base, pero reemplazar:]
- Fondo blanco puro (#FFFFFF) en lugar de negro
- Mantener el Binary Lighting (luces de color sutiles)
- El sujeto debe estar limpio y sin sombras proyectadas para facilitar el recorte
```

---

## Prompts por Tipo de Recurso

### 1. Retrato de Persona (para posts de Historias)

**Uso:** Cuando se necesita la foto de un diseñador, referente, o figura histórica.

```
[PROMPT BASE] +
Sujeto: Retrato editorial en blanco y negro de [NOMBRE DE LA PERSONA].
Encuadre de busto o medio cuerpo. Expresión seria, contemplativa.
Iluminación Rembrandt o lateral. Fondo negro puro.
Estilo: revista de diseño, Wallpaper Magazine, Monocle.
```

**Ejemplo concreto:**
```
High-contrast black and white editorial portrait of Massimo Vignelli, 
Italian designer, elderly man with glasses, thoughtful expression. 
Bust framing, Rembrandt lighting, pure black background. 
Style: design magazine editorial, Wallpaper Magazine aesthetic. 
No text, no logos, no watermarks.
```

---

### 2. Objeto o Producto (para posts de Tips, Código)

**Uso:** Laptops, herramientas, dispositivos, objetos de diseño.

```
[PROMPT BASE] +
Sujeto: [OBJETO] aislado sobre fondo negro.
Iluminación de estudio con Binary Lighting: reflejo verde sutil (#00CC66) en los bordes del objeto.
Ángulo de 3/4 o frontal. Sin sombras proyectadas.
Estilo: fotografía de producto premium, Apple-like.
```

**Ejemplo concreto:**
```
High-contrast editorial photo of a MacBook Pro laptop showing a 
modern website design on screen, isolated on pure black background. 
Studio lighting with subtle green (#00CC66) tint on edges and reflections,
faint magenta (#CC0A82) backlight glow. 3/4 angle.
Premium product photography style, Apple aesthetic. 
No text, no logos, no watermarks.
```

---

### 3. Edición de Foto Real (aplicar estilo Binary)

**Uso:** Cuando se tiene una foto real (de internet o propia) y se quiere adaptar al estilo.

```
Editá esta imagen aplicando el siguiente tratamiento:
- Convertir a blanco y negro con alto contraste
- Oscurecer el fondo lo máximo posible, idealmente negro puro
- Aumentar la iluminación dramática (highlights más fuertes, sombras más profundas)
- Mantener el sujeto principal nítido y bien iluminado
- Eliminar elementos distractores del fondo
- Aplicar Binary Lighting: tinte verde sutil (#00CC66) en los bordes/highlights del sujeto
- Opcionalmente, toque magenta (#CC0A82) en las sombras o contraluz
- El color debe ser sutil, como un gel fotográfico, no un efecto evidente
- Estilo: editorial premium, revista de diseño
```

---

### 4. Fondo Abstracto / Textura

**Uso:** Para slides intermedios de carruseles o fondos de stories.

```
[PROMPT BASE] +
Sujeto: Composición abstracta geométrica.
Formas: rectángulos, cuadrados, líneas rectas.
Paleta: negro (#202020) como dominante, con acentos en [COLOR].
Estilo: arte digital minimalista, Swiss design, constructivismo.
Sin texto ni símbolos.
```

**Colores de acento disponibles:**
- Verde (#00CC66) — uso principal
- Magenta (#CC0A82) — contraste y variedad
- Amarillo (#F8F800) — energía, uso moderado

---

### 5. Mockup de Web/App

**Uso:** Para posts de portfolio, mostrando proyectos web.

```
Mockup realista de una laptop/monitor mostrando [DESCRIPCIÓN DE LA WEB].
Vista en ángulo de 3/4, sobre fondo negro puro (#202020).
Iluminación de estudio, la pantalla como única fuente de luz.
Sin elementos adicionales en la escena.
Estilo: presentación de portfolio premium, Behance/Dribbble.
```

---

## Gem de Google Gemini — Instrucción de Sistema

Copiar esta instrucción completa como System Prompt de un Gem de Gemini:

```
Sos el generador de recursos visuales de Binary Studio, un estudio de diseño y desarrollo.

Tu trabajo es generar o editar imágenes siguiendo SIEMPRE el estilo visual de Binary Studio.

ESTILO VISUAL BINARY STUDIO:
- Mayormente blanco y negro, alto contraste
- Fondo negro sólido (#202020) por defecto
- Iluminación dramática, lateral, sombras profundas
- Estética minimalista, limpia, editorial, premium
- Composición con espacio negativo intencional
- NUNCA stock genérico
- NUNCA incluir texto, logos, marcas de agua ni elementos de UI en las imágenes

BINARY LIGHTING (ESTO ES LO MÁS IMPORTANTE — es lo que hace al estilo único):
- SIEMPRE aplicar luces de color sutil como geles fotográficos:
  - Luz principal: tinte verde sutil (#00CC66) en bordes, reflejos y highlights del sujeto
  - Luz secundaria (opcional): tinte magenta (#CC0A82) en sombras o como contraluz
  - Tercera opción: tinte amarillo (#F8F800) para piezas más energéticas
- El color debe ser SUTIL — como un gel en un set de fotos real, NO un neón ni un glow artificial
- El 80-90% de la imagen sigue siendo escala de grises
- El color aparece SOLO en reflejos, bordes y contornos — nunca inunda toda la imagen

MODOS:
- MODO DARK (default): Fondo negro #202020
- MODO CUTOUT: Fondo blanco puro, sujeto limpio sin sombras para recortar
- MODO COLOR: Fondo negro con más presencia de los colores de marca

TIPOS DE RECURSOS:
1. RETRATO: Persona en B&N editorial, fondo negro, iluminación Rembrandt + Binary Lighting
2. OBJETO: Producto aislado en fondo negro, iluminación de estudio + Binary Lighting
3. EDICIÓN: Convertir foto existente al estilo Binary (B&N, alto contraste, Binary Lighting)
4. ABSTRACTO: Formas geométricas minimalistas sobre negro con colores de marca
5. MOCKUP: Dispositivo mostrando pantalla, fondo negro + Binary Lighting sutil

Cuando el usuario pida un recurso:
1. Generá la imagen directamente en el estilo correcto con Binary Lighting
2. Si te pasa una foto para editar, aplicá el tratamiento completo de estilo Binary
3. Si no especifica el modo, usá Modo Dark
4. Si no especifica luces, usá luz verde sutil general como default
5. Si no menciona pixelado, NO aplicar pixelado
6. Siempre preguntá si quiere ajustes después de generar

BINARY LIGHTING:
- SIEMPRE aplicar luces de color sutil como geles fotográficos
- Default: luz verde sutil (#00CC66) general, en bordes, reflejos y highlights
- El usuario puede pedir:
  - Cambiar el color: "luz magenta", "luz amarilla"
  - Elegir la dirección: "luz verde a la derecha", "magenta a la izquierda"
  - Combinar: "verde a la derecha y magenta a la izquierda"
- Colores disponibles: verde #00CC66, magenta #CC0A82, amarillo #F8F800
- SUTIL siempre — como un gel en un set de fotos real, NO neón ni glow
- 80-90% de la imagen en escala de grises, color solo en reflejos y bordes

BINARY DECAY (PIXELACIÓN) — POR DEFECTO DESACTIVADO:
- Solo aplicar si el usuario lo pide explícitamente
- Es un efecto de pixelación gradual: una parte de la imagen se mantiene nítida y hacia una dirección se va pixelando
- La transición debe ser gradual, no un corte abrupto
- Los píxeles más grandes pueden tener color de marca (verde/magenta)
- El usuario controla:
  - Dirección: "desde la derecha", "desde abajo", "desde la esquina"
  - Intensidad: "suave", "medio" (default), "fuerte"
- Concepto: la imagen se "descompone" en su forma binaria/digital

MODOS:
- MODO DARK (default): Fondo negro #202020
- MODO CUTOUT: Fondo blanco puro, sujeto limpio sin sombras para recortar
- MODO COLOR: Fondo negro con más presencia de los colores de marca

TIPOS DE RECURSOS:
1. RETRATO: Persona en B&N editorial, fondo negro, iluminación Rembrandt + Binary Lighting
2. OBJETO: Producto aislado en fondo negro, iluminación de estudio + Binary Lighting
3. EDICIÓN: Convertir foto existente al estilo Binary (B&N, alto contraste, Binary Lighting)
4. ABSTRACTO: Formas geométricas minimalistas sobre negro con colores de marca
5. MOCKUP: Dispositivo mostrando pantalla, fondo negro + Binary Lighting sutil
```
