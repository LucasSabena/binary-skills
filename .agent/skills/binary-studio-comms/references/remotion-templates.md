# Templates de Composiciones — Binary Studio Remotion

Estructuras de composiciones comunes listas para usar.

## 1. Carrusel Animado (Instagram Feed)

### Root.tsx setup

```tsx
import { Composition } from 'remotion';
import { CarouselComposition } from './CarouselComposition';

export const RemotionRoot = () => (
  <Composition
    id="BinaryCarousel"
    component={CarouselComposition}
    durationInFrames={900}  // ~30 seg para 7 slides @ 30fps
    fps={30}
    width={1080}
    height={1080}
    defaultProps={{
      slides: [
        {
          type: 'cover',
          category: 'DISEÑO',
          title: '5 PRINCIPIOS\nDE DISEÑO QUE\nNADIE TE ENSEÑA',
          subtitle: 'Y que los mejores diseñadores aplican sin pensar',
        },
        {
          type: 'content',
          number: '01',
          title: 'JERARQUÍA VISUAL',
          body: 'Si todo es importante, nada lo es. Definí qué es lo primero que tiene que ver el usuario.',
        },
        // ... más slides
        {
          type: 'cta',
          title: '¿NECESITÁS DISEÑO\nQUE FUNCIONE?',
          cta: 'Hablemos → binarystudio.com.ar',
        },
      ],
    }}
  />
);
```

### Estructura del carrusel

```tsx
// CarouselComposition.tsx
import { TransitionSeries, linearTiming } from '@remotion/transitions';
import { slide } from '@remotion/transitions/slide';

export const CarouselComposition: React.FC<CarouselProps> = ({ slides }) => {
  const SLIDE_DURATION = 4 * 30;     // 4 segundos por slide
  const TRANSITION_DURATION = 15;     // 0.5 seg transición

  return (
    <TransitionSeries>
      {slides.map((slideData, index) => (
        <React.Fragment key={index}>
          <TransitionSeries.Sequence durationInFrames={SLIDE_DURATION}>
            {slideData.type === 'cover' && <CoverSlide {...slideData} />}
            {slideData.type === 'content' && <ContentSlide {...slideData} />}
            {slideData.type === 'cta' && <CTASlide {...slideData} />}
          </TransitionSeries.Sequence>
          {index < slides.length - 1 && (
            <TransitionSeries.Transition
              presentation={slide({ direction: 'from-right' })}
              timing={linearTiming({ durationInFrames: TRANSITION_DURATION })}
            />
          )}
        </React.Fragment>
      ))}
    </TransitionSeries>
  );
};
```

### Slide de portada (Cover)

```tsx
const CoverSlide: React.FC<CoverProps> = ({ category, title, subtitle }) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // Badge de categoría — entra desde arriba con snappy spring
  const badgeSpring = spring({ frame, fps, config: { damping: 20, stiffness: 200 } });
  const badgeY = interpolate(badgeSpring, [0, 1], [-30, 0]);

  // Título — entra desde abajo con delay
  const titleSpring = spring({ frame: frame - 8, fps, config: { damping: 20, stiffness: 200 } });
  const titleY = interpolate(titleSpring, [0, 1], [40, 0]);

  // Subtítulo — fade in suave con más delay
  const subSpring = spring({ frame: frame - 20, fps, config: { damping: 200 } });

  return (
    <AbsoluteFill style={{ backgroundColor: COLORS.background, padding: 60 }}>
      {/* Badge de categoría */}
      <div style={{
        opacity: badgeSpring,
        transform: `translateY(${badgeY}px)`,
        color: COLORS.green,
        ...FEED_TYPOGRAPHY.badge,
      }}>
        + {category}
      </div>

      {/* Título principal */}
      <div style={{
        opacity: titleSpring,
        transform: `translateY(${titleY}px)`,
        color: COLORS.white,
        fontFamily: 'Thunder',
        ...FEED_TYPOGRAPHY.titleXL,
        marginTop: 40,
      }}>
        {title}
      </div>

      {/* Subtítulo */}
      <div style={{
        opacity: subSpring,
        color: COLORS.whiteAlpha80,
        ...FEED_TYPOGRAPHY.bodyLG,
        marginTop: 24,
      }}>
        {subtitle}
      </div>

      {/* Logo reducido en esquina inferior */}
      <div style={{
        position: 'absolute',
        bottom: 60,
        right: 60,
        opacity: 0.6,
      }}>
        <Img src={staticFile('logo-reduced.svg')} width={40} />
      </div>
    </AbsoluteFill>
  );
};
```

---

## 2. Reel/Story (Vertical)

### Estructura base

```tsx
<Composition
  id="BinaryReel"
  component={ReelComposition}
  durationInFrames={300}  // 10 segundos
  fps={30}
  width={1080}
  height={1920}
/>
```

### Estructura de un reel

```tsx
const ReelComposition: React.FC = () => {
  const { fps } = useVideoConfig();

  return (
    <AbsoluteFill style={{ backgroundColor: COLORS.background }}>
      {/* Intro: Logo animation (2 seg) */}
      <Sequence durationInFrames={2 * fps} premountFor={1 * fps}>
        <BinaryLogoIntro />
      </Sequence>

      {/* Contenido principal (6 seg) */}
      <Sequence from={2 * fps} durationInFrames={6 * fps} premountFor={1 * fps}>
        <MainContent />
      </Sequence>

      {/* Outro: CTA + Logo (2 seg) */}
      <Sequence from={8 * fps} durationInFrames={2 * fps} premountFor={1 * fps}>
        <BinaryOutro />
      </Sequence>
    </AbsoluteFill>
  );
};
```

---

## 3. Portfolio Showcaser

### Mockup de web con zoom

```tsx
const PortfolioShowcase: React.FC<{ screenshotUrl: string }> = ({ screenshotUrl }) => {
  const frame = useCurrentFrame();
  const { fps, durationInFrames } = useVideoConfig();

  // Zoom progresivo lento
  const zoomProgress = interpolate(
    frame,
    [0, durationInFrames],
    [1, 1.15],
    { easing: Easing.inOut(Easing.quad), extrapolateRight: 'clamp' }
  );

  // Fade in inicial
  const fadeIn = spring({ frame, fps, config: { damping: 200 } });

  return (
    <AbsoluteFill style={{ backgroundColor: COLORS.background }}>
      {/* Screenshot con zoom lento */}
      <div style={{
        opacity: fadeIn,
        transform: `scale(${zoomProgress})`,
        transformOrigin: 'center center',
        // Mockup frame
        border: `2px solid ${COLORS.whiteAlpha20}`,
        borderRadius: 12,
        overflow: 'hidden',
        margin: 60,
      }}>
        <Img src={screenshotUrl} style={{ width: '100%', height: '100%', objectFit: 'cover' }} />
      </div>

      {/* Green glow sutil detrás del mockup */}
      <div style={{
        position: 'absolute',
        top: '50%',
        left: '50%',
        transform: 'translate(-50%, -50%)',
        width: 800,
        height: 500,
        background: `radial-gradient(ellipse, ${COLORS.greenGlow}, transparent 70%)`,
        zIndex: -1,
      }} />
    </AbsoluteFill>
  );
};
```

---

## 4. Código Animado (Typewriter)

```tsx
const CodeAnimation: React.FC<{ code: string; language: string }> = ({ code, language }) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // Typewriter: mostrar caracteres progresivamente
  const charsPerSecond = 30;
  const visibleChars = Math.floor((frame / fps) * charsPerSecond);
  const displayCode = code.slice(0, visibleChars);

  // Cursor blink
  const cursorVisible = Math.floor(frame / (fps * 0.5)) % 2 === 0;

  return (
    <AbsoluteFill style={{
      backgroundColor: COLORS.background,
      padding: 60,
      justifyContent: 'center',
    }}>
      {/* Badge */}
      <div style={{ color: COLORS.green, ...FEED_TYPOGRAPHY.badge, marginBottom: 24 }}>
        + CÓDIGO
      </div>

      {/* Code block */}
      <div style={{
        backgroundColor: COLORS.backgroundDark,
        borderRadius: 12,
        padding: 40,
        border: `1px solid ${COLORS.whiteAlpha20}`,
        fontFamily: 'monospace',
        fontSize: 22,
        color: COLORS.white,
        lineHeight: 1.7,
        whiteSpace: 'pre-wrap',
      }}>
        {displayCode}
        {cursorVisible && <span style={{ color: COLORS.green }}>▌</span>}
      </div>
    </AbsoluteFill>
  );
};
```

---

## 5. Tipografía Animada (Quote/Frase)

```tsx
const QuoteAnimation: React.FC<{ quote: string; author: string }> = ({ quote, author }) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  // Palabra por palabra con delay escalonado
  const words = quote.split(' ');

  return (
    <AbsoluteFill style={{
      backgroundColor: COLORS.background,
      padding: 80,
      justifyContent: 'center',
      alignItems: 'center',
    }}>
      <div style={{
        fontFamily: 'Thunder',
        ...FEED_TYPOGRAPHY.titleLG,
        color: COLORS.white,
        textAlign: 'center',
        display: 'flex',
        flexWrap: 'wrap',
        justifyContent: 'center',
        gap: 12,
      }}>
        {words.map((word, index) => {
          const wordSpring = spring({
            frame: frame - (index * 3),  // 3 frames de delay por palabra
            fps,
            config: { damping: 20, stiffness: 200 },
          });
          const wordY = interpolate(wordSpring, [0, 1], [30, 0]);

          return (
            <span key={index} style={{
              opacity: wordSpring,
              transform: `translateY(${wordY}px)`,
              display: 'inline-block',
            }}>
              {word}
            </span>
          );
        })}
      </div>

      {/* Author */}
      <div style={{
        opacity: spring({ frame: frame - (words.length * 3 + 15), fps, config: { damping: 200 } }),
        color: COLORS.green,
        ...FEED_TYPOGRAPHY.bodyMD,
        marginTop: 40,
      }}>
        — {author}
      </div>
    </AbsoluteFill>
  );
};
```
