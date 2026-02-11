# Componentes Reutilizables — Binary Studio Remotion

Componentes React listos para usar en composiciones de Remotion.

## BinaryBackground

Fondo #202020 con grid sutil opcional y partículas de acento.

```tsx
import { AbsoluteFill, useCurrentFrame, useVideoConfig, interpolate } from 'remotion';
import { COLORS, GRID } from './design-tokens';

type BinaryBackgroundProps = {
  showGrid?: boolean;       // Mostrar grilla de fondo
  accentColor?: string;     // Color de acentos (default: green)
  glowPosition?: 'center' | 'top-left' | 'top-right' | 'bottom-left' | 'bottom-right';
};

export const BinaryBackground: React.FC<BinaryBackgroundProps> = ({
  showGrid = false,
  accentColor = COLORS.green,
  glowPosition = 'center',
  children,
}) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // Glow pulsante sutil
  const glowIntensity = interpolate(
    Math.sin((frame / fps) * Math.PI * 0.5),
    [-1, 1],
    [0.05, 0.15],
  );

  const glowPositions = {
    'center': { top: '50%', left: '50%', transform: 'translate(-50%, -50%)' },
    'top-left': { top: '0%', left: '0%' },
    'top-right': { top: '0%', right: '0%' },
    'bottom-left': { bottom: '0%', left: '0%' },
    'bottom-right': { bottom: '0%', right: '0%' },
  };

  return (
    <AbsoluteFill style={{ backgroundColor: COLORS.background }}>
      {/* Glow de acento */}
      <div style={{
        position: 'absolute',
        ...glowPositions[glowPosition],
        width: 600,
        height: 600,
        borderRadius: '50%',
        background: `radial-gradient(circle, ${accentColor}${Math.round(glowIntensity * 255).toString(16).padStart(2, '0')}, transparent 70%)`,
        pointerEvents: 'none',
      }} />

      {/* Grid (opcional) */}
      {showGrid && <GridOverlay />}

      {/* Contenido */}
      {children}
    </AbsoluteFill>
  );
};

// Grid de fondo
const GridOverlay: React.FC = () => (
  <div style={{
    position: 'absolute',
    inset: 0,
    backgroundImage: `
      linear-gradient(${GRID.lineColor} 1px, transparent 1px),
      linear-gradient(90deg, ${GRID.lineColor} 1px, transparent 1px)
    `,
    backgroundSize: `${GRID.cellSize}px ${GRID.cellSize}px`,
    pointerEvents: 'none',
  }} />
);
```

---

## BinaryTitle

Título con Thunder bold, entrada spring + glow verde opcional.

```tsx
import { useCurrentFrame, useVideoConfig, spring, interpolate } from 'remotion';
import { COLORS, FEED_TYPOGRAPHY } from './design-tokens';

type BinaryTitleProps = {
  text: string;
  size?: 'xl' | 'lg' | 'md';
  delay?: number;         // Delay en frames
  withGlow?: boolean;     // Agregar glow verde sutil
  color?: string;
};

export const BinaryTitle: React.FC<BinaryTitleProps> = ({
  text,
  size = 'lg',
  delay = 0,
  withGlow = false,
  color = COLORS.white,
}) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const entrance = spring({
    frame: frame - delay,
    fps,
    config: { damping: 20, stiffness: 200 }, // snappy
  });

  const translateY = interpolate(entrance, [0, 1], [40, 0]);

  const typography = {
    xl: FEED_TYPOGRAPHY.titleXL,
    lg: FEED_TYPOGRAPHY.titleLG,
    md: FEED_TYPOGRAPHY.titleMD,
  }[size];

  return (
    <div style={{
      opacity: entrance,
      transform: `translateY(${translateY}px)`,
      color,
      fontFamily: 'Thunder',
      ...typography,
      ...(withGlow ? {
        textShadow: `0 0 40px ${COLORS.greenGlow}`,
      } : {}),
    }}>
      {text}
    </div>
  );
};
```

---

## BinarySubtitle

Subtítulo con Space Grotesk, entrada suave con delay.

```tsx
type BinarySubtitleProps = {
  text: string;
  delay?: number;
  color?: string;
};

export const BinarySubtitle: React.FC<BinarySubtitleProps> = ({
  text,
  delay = 15,
  color = COLORS.whiteAlpha80,
}) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const entrance = spring({
    frame: frame - delay,
    fps,
    config: { damping: 200 }, // smooth
  });

  return (
    <div style={{
      opacity: entrance,
      color,
      ...FEED_TYPOGRAPHY.bodyLG,
    }}>
      {text}
    </div>
  );
};
```

---

## BinaryLogo

Logo animado con scale + fade. Usa los SVGs de la marca.

```tsx
import { Img, spring, interpolate, useCurrentFrame, useVideoConfig, staticFile } from 'remotion';
import { COLORS } from './design-tokens';

type BinaryLogoProps = {
  variant?: 'full' | 'reduced';   // B1NARY STUDI0 o 01
  size?: number;                   // Ancho en px
  delay?: number;
  withGlow?: boolean;
};

export const BinaryLogo: React.FC<BinaryLogoProps> = ({
  variant = 'reduced',
  size = 120,
  delay = 0,
  withGlow = true,
}) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // Entrada heavy — lenta, con peso
  const entrance = spring({
    frame: frame - delay,
    fps,
    config: { damping: 15, stiffness: 80, mass: 2 },
  });

  const scale = interpolate(entrance, [0, 1], [0.8, 1]);

  const logoFile = variant === 'full'
    ? staticFile('logo-full.svg')
    : staticFile('logo-reduced.svg');

  // Glow pulsante
  const glowIntensity = interpolate(
    Math.sin((frame / fps) * Math.PI * 0.4),
    [-1, 1],
    [0.1, 0.3],
  );

  return (
    <div style={{
      opacity: entrance,
      transform: `scale(${scale})`,
      display: 'flex',
      alignItems: 'center',
      justifyContent: 'center',
      ...(withGlow ? {
        filter: `drop-shadow(0 0 30px rgba(0, 204, 102, ${glowIntensity}))`,
      } : {}),
    }}>
      <Img src={logoFile} width={size} />
    </div>
  );
};
```

### Uso como Intro

```tsx
// Intro de 2 segundos con logo centrado
const BinaryLogoIntro: React.FC = () => (
  <BinaryBackground glowPosition="center">
    <AbsoluteFill style={{ justifyContent: 'center', alignItems: 'center' }}>
      <BinaryLogo variant="full" size={400} withGlow />
    </AbsoluteFill>
  </BinaryBackground>
);
```

### Uso como Outro

```tsx
const BinaryOutro: React.FC<{ cta?: string }> = ({ cta }) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  return (
    <BinaryBackground glowPosition="center">
      <AbsoluteFill style={{
        justifyContent: 'center',
        alignItems: 'center',
        flexDirection: 'column',
        gap: 30,
      }}>
        <BinaryLogo variant="reduced" size={80} />
        {cta && (
          <div style={{
            opacity: spring({ frame: frame - 15, fps, config: { damping: 200 } }),
            color: COLORS.whiteAlpha80,
            ...FEED_TYPOGRAPHY.bodyMD,
          }}>
            {cta}
          </div>
        )}
      </AbsoluteFill>
    </BinaryBackground>
  );
};
```

---

## CategoryBadge

Badge de categoría con entrada snappy.

```tsx
type CategoryBadgeProps = {
  category: string;       // "DISEÑO", "CÓDIGO", "TIPS", etc.
  delay?: number;
  color?: string;
};

export const CategoryBadge: React.FC<CategoryBadgeProps> = ({
  category,
  delay = 0,
  color = COLORS.green,
}) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const entrance = spring({
    frame: frame - delay,
    fps,
    config: { damping: 20, stiffness: 200 },
  });

  const translateY = interpolate(entrance, [0, 1], [-20, 0]);

  return (
    <div style={{
      opacity: entrance,
      transform: `translateY(${translateY}px)`,
      color,
      ...FEED_TYPOGRAPHY.badge,
    }}>
      + {category}
    </div>
  );
};
```

---

## SlideCounter

Indicador de posición en carrusel.

```tsx
type SlideCounterProps = {
  current: number;
  total: number;
  delay?: number;
};

export const SlideCounter: React.FC<SlideCounterProps> = ({
  current,
  total,
  delay = 5,
}) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const entrance = spring({
    frame: frame - delay,
    fps,
    config: { damping: 200 },
  });

  return (
    <div style={{
      opacity: entrance * 0.5,  // Siempre sutil
      color: COLORS.white,
      ...FEED_TYPOGRAPHY.counter,
      position: 'absolute',
      bottom: 60,
      left: 60,
    }}>
      {String(current).padStart(2, '0')}/{String(total).padStart(2, '0')}
    </div>
  );
};
```

---

## BinaryDecayEffect

Efecto de pixelación gradual animada.

```tsx
type BinaryDecayEffectProps = {
  direction?: 'from-right' | 'from-left' | 'from-top' | 'from-bottom';
  intensity?: 'soft' | 'medium' | 'strong';
  mode?: 'reveal' | 'dissolve';  // reveal: pixelado → nítido, dissolve: nítido → pixelado
  delay?: number;
  durationInSeconds?: number;
  children: React.ReactNode;
};

export const BinaryDecayEffect: React.FC<BinaryDecayEffectProps> = ({
  direction = 'from-right',
  intensity = 'medium',
  mode = 'reveal',
  delay = 0,
  durationInSeconds = 1.5,
  children,
}) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const maxPixelSize = { soft: 20, medium: 40, strong: 60 }[intensity];

  const progress = spring({
    frame: frame - delay,
    fps,
    config: { damping: 200 },
    durationInFrames: Math.round(durationInSeconds * fps),
  });

  // Para dissolve, invertir el progreso
  const effectProgress = mode === 'reveal' ? progress : 1 - progress;

  // Pixel size: grande a chico (reveal) o chico a grande (dissolve)
  const pixelSize = interpolate(effectProgress, [0, 1], [maxPixelSize, 1], {
    extrapolateRight: 'clamp',
  });

  const needsPixelation = pixelSize > 1.5;

  return (
    <div style={{
      position: 'relative',
      width: '100%',
      height: '100%',
      ...(needsPixelation ? {
        imageRendering: 'pixelated' as const,
      } : {}),
    }}>
      {children}
    </div>
  );
};
```

> **Nota sobre pixelación real:** Para un efecto de pixelación real en Remotion, lo ideal es renderizar el contenido a un canvas a menor resolución y escalarlo. El approach CSS con `imageRendering: pixelated` funciona para imágenes pero no para elementos HTML complejos. Para composiciones avanzadas, considerar usar `@remotion/canvas` o renderizar a una textura offscreen.
