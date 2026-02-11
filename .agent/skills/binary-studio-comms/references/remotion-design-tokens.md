# Design Tokens para Remotion — Binary Studio

Constantes TypeScript listas para usar en composiciones de Remotion.

## Colores

```tsx
// colors.ts — Design tokens de Binary Studio
export const COLORS = {
  // Fondos
  background: '#202020',
  backgroundLight: '#2A2A2A',
  backgroundDark: '#181818',

  // Marca
  green: '#00CC66',
  magenta: '#CC0A82',
  yellow: '#F8F800',
  white: '#FFFFFF',
  whiteAlpha80: 'rgba(255, 255, 255, 0.8)',
  whiteAlpha50: 'rgba(255, 255, 255, 0.5)',
  whiteAlpha20: 'rgba(255, 255, 255, 0.2)',

  // Glow / Lighting (para sombras, bordes luminosos)
  greenGlow: 'rgba(0, 204, 102, 0.3)',
  magentaGlow: 'rgba(204, 10, 130, 0.3)',
  yellowGlow: 'rgba(248, 248, 0, 0.3)',
} as const;
```

## Tipografía

### Carga de fuentes

```tsx
// fonts.ts — Carga de fuentes para Remotion

// Space Grotesk — desde Google Fonts (automático)
import { loadFont as loadSpaceGrotesk } from '@remotion/google-fonts/SpaceGrotesk';

const { fontFamily: spaceGrotesk } = loadSpaceGrotesk('normal', {
  weights: ['400', '500', '700'],
  subsets: ['latin'],
});

// Thunder — fuente local (colocar archivos .woff2 en public/)
import { loadFont } from '@remotion/fonts';
import { staticFile } from 'remotion';

await loadFont({
  family: 'Thunder',
  url: staticFile('Thunder-Bold.woff2'),
  weight: '700',
});

export const FONTS = {
  title: 'Thunder',          // Títulos, headlines, display
  body: spaceGrotesk,        // Cuerpo, subtítulos, UI
} as const;
```

> **Nota:** Si Thunder no está disponible como .woff2, usar `Space Grotesk Bold` como fallback con `text-transform: uppercase` y `letter-spacing` negativo para lograr un look similar condensado.

### Tamaños tipográficos

```tsx
// typography.ts — Escalas por formato de composición

// Para 1080x1080 (Instagram Feed)
export const FEED_TYPOGRAPHY = {
  // Títulos (Thunder)
  titleXL: { fontSize: 120, fontWeight: '700', fontFamily: 'Thunder', textTransform: 'uppercase' as const, letterSpacing: -2 },
  titleLG: { fontSize: 90, fontWeight: '700', fontFamily: 'Thunder', textTransform: 'uppercase' as const, letterSpacing: -1 },
  titleMD: { fontSize: 64, fontWeight: '700', fontFamily: 'Thunder', textTransform: 'uppercase' as const },

  // Cuerpo (Space Grotesk)
  bodyLG: { fontSize: 36, fontWeight: '400', lineHeight: 1.4 },
  bodyMD: { fontSize: 28, fontWeight: '400', lineHeight: 1.5 },
  bodySM: { fontSize: 22, fontWeight: '400', lineHeight: 1.5 },

  // Acentos
  badge: { fontSize: 18, fontWeight: '500', textTransform: 'uppercase' as const, letterSpacing: 2 },
  counter: { fontSize: 20, fontWeight: '500' },
} as const;

// Para 1080x1920 (Stories/Reels)
export const STORY_TYPOGRAPHY = {
  titleXL: { fontSize: 140, fontWeight: '700', fontFamily: 'Thunder', textTransform: 'uppercase' as const, letterSpacing: -2 },
  titleLG: { fontSize: 100, fontWeight: '700', fontFamily: 'Thunder', textTransform: 'uppercase' as const },
  bodyLG: { fontSize: 40, fontWeight: '400', lineHeight: 1.4 },
  bodyMD: { fontSize: 32, fontWeight: '400', lineHeight: 1.5 },
} as const;
```

## Composiciones — Dimensiones

```tsx
// compositions.ts — Formatos estándar
export const FORMATS = {
  // Instagram / LinkedIn Feed
  feed: { width: 1080, height: 1080, fps: 30 },

  // Stories / Reels / TikTok / YouTube Shorts
  story: { width: 1080, height: 1920, fps: 30 },

  // YouTube / Presentaciones
  landscape: { width: 1920, height: 1080, fps: 30 },

  // LinkedIn post rectangular
  linkedin: { width: 1200, height: 628, fps: 30 },
} as const;
```

## Espaciado

```tsx
// spacing.ts — Márgenes y padding estándar
export const SPACING = {
  // Padding del canvas
  canvasPadding: 60,                // Margen interno del canvas
  canvasPaddingStory: 80,           // Margen para stories (más alto)

  // Espaciado entre elementos
  gap: {
    xs: 8,
    sm: 16,
    md: 24,
    lg: 40,
    xl: 60,
  },

  // Logo
  logoMarginBottom: 40,             // Espacio debajo del logo en outro
  logoSize: { width: 200, height: 60 }, // Tamaño del logo reducido (01)
} as const;
```

## Grid Decorativo

```tsx
// grid.ts — Parámetros del grid de fondo
export const GRID = {
  cellSize: 40,         // Tamaño de cada celda de la grilla
  lineWidth: 1,         // Grosor de las líneas
  lineColor: 'rgba(255, 255, 255, 0.04)',  // Casi invisible
  accentColor: COLORS.green,
  accentOpacity: 0.15,  // Opacidad de las celdas acentuadas
} as const;
```
