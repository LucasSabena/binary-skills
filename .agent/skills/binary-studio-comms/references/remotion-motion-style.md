# Estilo de Movimiento — Binary Studio Remotion

Guía de cómo se mueven las cosas en las animaciones de Binary Studio.

## Principio General

El movimiento de Binary es **preciso y confiado** — como un buen diseño de interfaz. No es lento ni pesado, pero tampoco frenético. Es snappy cuando necesita atención, smooth cuando necesita elegancia.

---

## Spring Configurations

Usar siempre `spring()` de Remotion como base. Las configuraciones por contexto:

```tsx
import { spring } from 'remotion';

// SNAPPY — Para textos, títulos, badges que entran con presencia
// Ideal para: títulos entrando, badges apareciendo, elementos de UI
const snappy = { damping: 20, stiffness: 200 };

// SMOOTH — Para fondos, transiciones, elementos decorativos
// Ideal para: background shifts, fade de fondos, elementos sutiles
const smooth = { damping: 200 };

// BOUNCY — Para CTAs, elementos interactivos, momentos de diversión
// Ideal para: botones, iconos, momentos de énfasis
const bouncy = { damping: 8 };

// HEAVY — Para elementos grandes, logos, slides completos
// Ideal para: logo de entrada, transición de slide completo
const heavy = { damping: 15, stiffness: 80, mass: 2 };
```

### Uso por tipo de elemento

| Elemento | Spring Config | Delay típico |
|----------|---------------|-------------|
| Título principal (Thunder) | `snappy` | 0 - primer elemento |
| Subtítulo | `smooth` | 8-12 frames después del título |
| Cuerpo de texto | `smooth` | 15-20 frames después del título |
| Badge de categoría | `snappy` | 5 frames después del título |
| Logo (intro/outro) | `heavy` | 0 |
| Elementos decorativos (grid, líneas) | `smooth` | 0 (con el fondo) |
| CTA / highlight | `bouncy` | Último elemento |

---

## Timing Patterns

### Duración por tipo de slide

```
Carrusel Instagram (10 slides máx):
├── Portada:           3-4 segundos (90-120 frames @ 30fps)
├── Slides de contenido: 4-5 segundos (120-150 frames)
├── Slide de CTA/cierre: 3-4 segundos (90-120 frames)
└── Transición entre slides: 15-20 frames

Story/Reel:
├── Intro con logo:     2 segundos (60 frames)
├── Contenido:          5-8 segundos (150-240 frames)
└── Outro con logo:     2 segundos (60 frames)
```

### Patrón de entrada de elementos (un slide típico)

```
Frame 0:    Fondo aparece (instantáneo o fade rápido)
Frame 5:    Badge de categoría entra (snappy desde arriba)
Frame 10:   Título principal entra (snappy desde abajo)
Frame 20:   Subtítulo entra (smooth, fade + translate)
Frame 30:   Cuerpo de texto entra (smooth, fade)
Frame 40:   Elementos decorativos aparecen (smooth, fade)
Frame 50+:  Todo visible, hold estático
```

### Patrón de salida (antes de transición)

```
Frame -20:  Elementos menores salen (fade out rápido)
Frame -10:  Título sale (translate + fade)
Frame 0:    Transición al siguiente slide
```

---

## Transiciones Preferidas

En orden de preferencia para Binary Studio:

### 1. Slide (principal)
```tsx
import { slide } from '@remotion/transitions/slide';

// Dirección: siempre "from-right" para avanzar, "from-left" para retroceder
presentation={slide({ direction: 'from-right' })}
timing={linearTiming({ durationInFrames: 15 })}
```

### 2. Fade (para momentos suaves)
```tsx
import { fade } from '@remotion/transitions/fade';

presentation={fade()}
timing={linearTiming({ durationInFrames: 20 })}
```

### 3. Wipe (para énfasis)
```tsx
import { wipe } from '@remotion/transitions/wipe';

// Con dirección: para revelar contenido con intención
presentation={wipe({ direction: 'from-left' })}
timing={springTiming({ config: { damping: 200 }, durationInFrames: 25 })}
```

### ❌ Evitar
- `flip` — demasiado "genérico corporativo"
- `clockWipe` — no encaja con la estética minimalista
- Transiciones de más de 25 frames — pierden dinamismo

---

## Binary Lighting en Motion

Las luces de color de marca no son estáticas en video — se mueven sutilmente.

### Glow pulsante
```tsx
// Sombra verde que pulsa sutilmente
const frame = useCurrentFrame();
const { fps } = useVideoConfig();

const glowIntensity = interpolate(
  Math.sin((frame / fps) * Math.PI * 0.5),  // Ciclo de ~4 segundos
  [-1, 1],
  [0.15, 0.35],  // Rango de opacidad del glow
);

// Aplicar como boxShadow o textShadow
const glowStyle = {
  boxShadow: `0 0 60px rgba(0, 204, 102, ${glowIntensity})`,
};
```

### Barrido de luz (light sweep)
```tsx
// Línea de luz verde que recorre horizontalmente
const sweepProgress = spring({
  frame,
  fps,
  config: { damping: 200 },
  durationInFrames: 2 * fps,
});

const sweepX = interpolate(sweepProgress, [0, 1], [-200, 1280]);

// Renderizar como un div con gradiente
<div style={{
  position: 'absolute',
  left: sweepX,
  top: 0,
  width: 200,
  height: '100%',
  background: 'linear-gradient(90deg, transparent, rgba(0, 204, 102, 0.1), transparent)',
}} />
```

### Reglas de lighting en motion
- El glow NUNCA es estático — siempre hay un sutil movimiento
- La velocidad de pulso es LENTA (ciclos de 3-5 segundos)
- Intensidad máxima del glow: 35% opacidad
- Usar como acento, nunca como elemento principal

---

## Binary Decay en Motion

La pixelación gradual funciona diferente en video — se **anima**.

### Pixelación de entrada (reveal)
```tsx
// La imagen empieza pixelada y se "decodifica" hacia nitidez
const frame = useCurrentFrame();
const { fps } = useVideoConfig();

const decayProgress = spring({
  frame,
  fps,
  config: { damping: 200 },
  durationInFrames: 1.5 * fps,
});

// pixelSize va de 40 (muy pixelado) a 1 (nítido)
const pixelSize = interpolate(decayProgress, [0, 1], [40, 1], {
  extrapolateRight: 'clamp',
});

// Aplicar con CSS filter o canvas
// Opción simple con CSS (funciona en Remotion):
const containerStyle = {
  imageRendering: pixelSize > 1 ? 'pixelated' : 'auto',
  filter: pixelSize > 1 ? `blur(0px)` : 'none',
  // Escalar down y up para crear efecto pixel
  transform: pixelSize > 1
    ? `scale(${1 / pixelSize}) scale(${pixelSize})`
    : 'none',
};
```

### Pixelación de salida (dissolve)
```tsx
// La imagen se va "descomponiendo" en píxeles
// Invertir el spring: empieza nítido, termina pixelado
const dissolveProgress = spring({
  frame: frame - startDissolveFrame,
  fps,
  config: { damping: 200 },
});

const pixelSize = interpolate(dissolveProgress, [0, 1], [1, 40]);
```

### Pixelación direccional
```tsx
// Solo una zona de la imagen se pixela (ej: desde la derecha)
// Usar un overlay con clip-path que se expande
const clipProgress = spring({
  frame,
  fps,
  config: { damping: 200 },
});

// Zona pixelada crece desde la derecha
const clipX = interpolate(clipProgress, [0, 1], [100, 50]); // de 100% a 50%

// Renderizar:
// 1. Imagen nítida de fondo (completa)
// 2. Imagen pixelada overlay con clip-path
<div style={{ position: 'relative' }}>
  <img src={image} style={{ width: '100%' }} /> {/* Nítida */}
  <img src={image} style={{
    position: 'absolute',
    top: 0,
    left: 0,
    width: '100%',
    clipPath: `inset(0 0 0 ${clipX}%)`,
    imageRendering: 'pixelated',
    // Escalar para pixelar
  }} />
</div>
```

### Reglas de Binary Decay en motion
- Siempre usa spring con `{ damping: 200 }` — movimiento suave, no abrupto
- Duración típica: 1-2 segundos para reveal, 0.5-1 segundo para dissolve
- Los píxeles grandes (>10px) pueden tener color verde (#00CC66) como tinte
- Nunca pixelar un texto legible — solo imágenes o fondos
- El pixelado siempre tiene una dirección clara (no random)

---

## Reglas Generales de Movimiento

1. **Todo se mueve con propósito** — Cada animación debe guiar la atención, no decorar
2. **Entrada desde abajo o derecha** — Patrón default para elementos nuevos
3. **Salida hacia arriba o izquierda** — Para elementos que desaparecen
4. **Nunca animar opacidad sola** — Siempre combinar con translate o scale
5. **Los tiempos son en segundos** — Escribir `2 * fps` en vez de `60` para legibilidad
6. **Premount siempre** — `premountFor={1 * fps}` mínimo en cada `<Sequence>`
7. **Hold estático mínimo: 1.5 segundos** — Dar tiempo para leer
8. **El logo siempre tiene la misma animación** — Scale + fade, spring heavy
