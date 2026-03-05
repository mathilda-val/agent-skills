# Based Design — Implementation Patterns

CSS/HTML code patterns that translate Based Design principles into actual built interfaces. **Read this before writing any code.** These patterns are intentionally extreme. Do not water them down. Do not add border-radius. Do not soften the collisions. Do not reduce the scale ratios. The whole point is that these look nothing like the safe, predictable web.

Every snippet here is a weapon against the aesthetic median.

---

## THE ANTI-DEFAULTS

Before writing a single line of CSS, override these defaults that pull every website toward the same look:

```css
/* BASED DESIGN RESET — Kill the median */
* { margin: 0; padding: 0; box-sizing: border-box; }

/* No border-radius anywhere. Rectangles are structural. Rounded corners are appeasement. */
* { border-radius: 0 !important; }

/* No transitions on layout elements. Collision, not transition. */
section, div, header, footer { transition: none; }

/* No box-shadow as depth cue. Depth comes from overlap and z-index, not drop shadows. */
* { box-shadow: none; }

/* Body type is small. Reading text should be modest — 15-17px max.
   The scale contrast between this and display type is what creates hierarchy. */
body { font-size: 16px; line-height: 1.4; }
```

---

## LAW 01 — CONTENT EARNS ITS SPACE

Each section gets its own grid. Not the same 12-column wrapper everywhere.

```html
<!-- Hero: asymmetric 2-column, weight left -->
<section class="hero">
  <div class="hero-mass"><!-- 70% of visual weight --></div>
  <div class="hero-breath"><!-- counterweight --></div>
</section>

<!-- Features: tight 4-column data grid -->
<section class="features">
  <div class="feature-item">...</div>
  <div class="feature-item">...</div>
  <div class="feature-item">...</div>
  <div class="feature-item">...</div>
</section>

<!-- Testimonial: single narrow column, literary -->
<section class="testimonial">
  <blockquote>...</blockquote>
</section>

<!-- Gallery: open field, no grid -->
<section class="gallery">
  <img style="position: absolute; top: 5%; left: 8%; width: 40%;" />
  <img style="position: absolute; top: 30%; left: 55%; width: 35%;" />
  <img style="position: absolute; top: 60%; left: 15%; width: 25%;" />
</section>
```

```css
/* Each section is its own world */
.hero {
  display: grid;
  grid-template-columns: 7fr 3fr; /* asymmetric, not 1fr 1fr */
  min-height: 100vh;
  gap: 0; /* collision, not separation */
}

.features {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1px; /* hairline gap — grid as visible structure */
  background: #000; /* gap color = black rules between cells */
  padding: 0;
}
.features .feature-item {
  background: #fff;
  padding: 3rem 2rem;
}

.testimonial {
  max-width: 28ch; /* narrow column = literary register */
  margin: 0; /* NOT centered. Positioned asymmetrically */
  margin-left: 15%;
  padding: 15vh 0;
}

.gallery {
  position: relative;
  min-height: 100vh;
  /* No grid. Elements placed absolutely in an open field. */
}
```

---

## LAW 02 — DENSITY OSCILLATION

Alternate between crushing density and aggressive emptiness. No middle ground.

```css
/* DENSE section — 85-95% fill */
.section-dense {
  padding: 2rem 3rem; /* minimal breathing room */
  background: #111;
  color: #fff;
  columns: 3;
  column-gap: 1.5rem;
  font-size: 14px;
  line-height: 1.3; /* tight — text becomes grey mass */
}

/* SPARSE section — the breath. 10-30% fill */
.section-breath {
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding: 0 15%;
}
.section-breath p {
  font-size: 14px;
  max-width: 20ch; /* tiny element in vast space */
  /* ONE small element. That's it. The emptiness does the work. */
}

/* PERCEPTUAL RESET — near-empty after maximum density */
.section-void {
  min-height: 80vh;
  display: grid;
  place-items: end start;
  padding: 0 0 3rem 5%;
}
.section-void::after {
  content: ''; /* a single mark to prove it's intentional */
  width: 40px;
  height: 2px;
  background: #000;
}
```

---

## LAW 03 — TYPOGRAPHY IS ARCHITECTURE

Type is building material. Not labels. Not headings. Beams, walls, ceilings.

```css
/* THE HORIZONTAL BEAM — headline as ceiling */
.type-beam {
  font-size: clamp(12vw, 18vw, 22vw); /* ENORMOUS. This is a beam. */
  line-height: 0.85; /* tight — no air between lines */
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: -0.03em; /* mass, not elegance */
  overflow: hidden; /* crop at container edge = implies scale beyond */
  white-space: nowrap;
  width: 100%;
}

/* THE VERTICAL COLUMN — rotated type as structural pillar */
.type-column {
  writing-mode: vertical-rl;
  text-orientation: mixed;
  font-size: clamp(6vw, 10vw, 14vw);
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 0.15em;
  position: absolute;
  right: 0;
  top: 0;
  height: 100%;
  /* This is a WALL. Content wraps around it like furniture around a column. */
}

/* Alternative: rotated via transform for when writing-mode won't work */
.type-column-transform {
  transform: rotate(-90deg);
  transform-origin: left bottom;
  position: absolute;
  left: 0;
  bottom: 0;
  white-space: nowrap;
  font-size: 10vw;
  font-weight: 900;
  text-transform: uppercase;
}

/* GHOST TYPOGRAPHY — structural presence, perceptual absence */
.type-ghost {
  font-size: clamp(15vw, 25vw, 35vw);
  font-weight: 900;
  text-transform: uppercase;
  color: rgba(0, 0, 0, 0.04); /* barely visible. felt, not read. */
  position: absolute;
  top: 50%;
  left: -5%;
  transform: translateY(-50%);
  white-space: nowrap;
  pointer-events: none;
  z-index: 0;
  line-height: 0.8;
}

/* TEXT AS TEXTURE — body text so small it becomes visual mass */
.type-texture {
  font-size: 7px;
  line-height: 1.1;
  letter-spacing: 0;
  columns: 5;
  column-gap: 4px;
  color: #666;
  /* Not meant to be read. It's a grey rectangle. A compositional weight. */
}

/* THE LOAD-BEARING THRESHOLD — type that holds space */
.type-loadbearing {
  font-size: clamp(20vw, 30vw, 40vw);
  font-weight: 900;
  line-height: 0.75;
  position: relative;
  /* At this scale, letters are rooms. Counters are windows.
     Strokes are walls. The viewer navigates the letterform. */
}

/* CROPPED LETTERFORM — fragmentation implies scale beyond viewport */
.type-cropped {
  font-size: 50vw;
  font-weight: 900;
  line-height: 0.7;
  overflow: hidden;
  max-height: 40vh; /* crop aggressively — show partial letters */
  /* "IZATIO" implies "DRAMATIZATION" — a word too large for the screen */
}
```

---

## LAW 04 — ASYMMETRIC WEIGHT

Never center. Load one side. Create a tension gradient the eye follows.

```css
/* WEIGHTED LEFT — 70% mass in 40% area */
.layout-weighted-left {
  display: grid;
  grid-template-columns: 2fr 5fr; /* NOT 1fr 1fr. Not even close. */
  gap: 0;
  min-height: 100vh;
}
.layout-weighted-left .mass {
  background: #000;
  color: #fff;
  padding: 3rem;
  display: flex;
  flex-direction: column;
  justify-content: end; /* content sinks to bottom = gravitational weight */
}
.layout-weighted-left .space {
  padding: 8vh 5%;
  /* The emptiness pulls against the mass. Tension. */
}

/* DIAGONAL WEIGHT — mass on a diagonal axis */
.layout-diagonal {
  display: grid;
  grid-template-columns: 3fr 1fr;
  grid-template-rows: 1fr 3fr;
  min-height: 100vh;
  gap: 0;
}
.layout-diagonal .top-left {
  grid-column: 1;
  grid-row: 1;
  background: #000; /* heavy */
}
.layout-diagonal .bottom-right {
  grid-column: 2;
  grid-row: 2;
  /* light — the diagonal tension */
}

/* CORNER LOADED — all weight in one corner */
.layout-corner {
  min-height: 100vh;
  position: relative;
}
.layout-corner .mass {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 55%;
  padding: 4rem;
  background: #000;
  color: #fff;
  /* Everything crammed bottom-left. Top-right is pressurized void. */
}
```

---

## LAW 05 — PRESSURIZED NEGATIVE SPACE

Space is never neutral. It compresses, isolates, confronts.

```css
/* PRESSURE — tight space between heavy elements */
.pressure-zone {
  display: flex;
  gap: 4px; /* almost touching. The air between them vibrates. */
}
.pressure-zone > * {
  flex: 1;
  padding: 3rem;
  background: #000;
  color: #fff;
}

/* ISOLATION — element alone in vast space */
.isolation-field {
  min-height: 100vh;
  display: grid;
  place-items: center;
  padding: 20vh 25vw; /* massive padding. Element is marooned. */
}
.isolation-field > * {
  font-size: 14px;
  max-width: 30ch;
  /* Tiny. Alone. The space around it is heavier than the element itself. */
}

/* CONFRONTATION — space between unlike elements is charged */
.confrontation {
  display: grid;
  grid-template-columns: 1fr 15vw 1fr; /* the gap is a zone, not a gutter */
  min-height: 60vh;
}
.confrontation .left {
  background: #000;
  color: #fff;
  padding: 3rem;
  font-size: 12px;
  columns: 2;
}
.confrontation .gap {
  /* This empty column IS the design. It holds the two worlds apart. */
}
.confrontation .right {
  display: grid;
  place-items: center;
}
.confrontation .right img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

---

## LAW 06 — SCALE CONTRAST (25:1 MINIMUM)

If body text is 16px, display type must be at least 400px. Not 48px. Not 64px. Four hundred.

```css
/* 25:1 SCALE — the minimum for architectural type */
.scale-extreme {
  position: relative;
}
.scale-extreme .display {
  font-size: clamp(250px, 25vw, 500px); /* THIS is scale contrast */
  font-weight: 900;
  line-height: 0.8;
  text-transform: uppercase;
}
.scale-extreme .body {
  font-size: 16px;
  line-height: 1.5;
  max-width: 45ch;
  position: absolute;
  bottom: 2rem;
  left: 2rem;
  /* Body text sits AT THE BASE of the architectural type.
     Like a person standing next to a building. */
}

/* THREE-TIER SCALE — and nothing in between */
.scale-tier-1 { font-size: clamp(180px, 20vw, 400px); font-weight: 900; } /* monumental */
.scale-tier-2 { font-size: 18px; font-weight: 600; letter-spacing: 0.15em; text-transform: uppercase; } /* metadata */
.scale-tier-3 { font-size: 16px; font-weight: 400; line-height: 1.5; } /* body */
/* NO sizes between these three. No 36px. No 48px. No 24px. Three tiers or nothing. */
```

---

## LAW 07 — COLLISION, NOT TRANSITION

Hard edges. No gradients. No mediating space. Things MEET.

```css
/* SECTION COLLISION — unlike sections meet at a hard edge */
.section-dark {
  background: #000;
  color: #fff;
  padding: 8vh 5%;
  /* No margin-bottom. No border. No gradient fade. */
}
.section-light {
  background: #fff;
  color: #000;
  padding: 8vh 5%;
  /* Directly abutting .section-dark. The edge IS the event. */
}
/* NEVER THIS: border-top: 1px solid #eee; */
/* NEVER THIS: background: linear-gradient(to bottom, #000, #fff); */
/* NEVER THIS: margin-top: 4rem; (mediating space) */

/* IMAGE-TEXT COLLISION — no caption gap, no padding */
.collision-image-text {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0; /* ZERO. Image and text share an edge. */
}
.collision-image-text img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}
.collision-image-text .text {
  padding: 2rem;
  align-self: stretch;
  display: flex;
  flex-direction: column;
  justify-content: end;
  /* Text pressed against image. No air. No courtesy spacing. */
}

/* OVERLAP COLLISION — elements deliberately crossing boundaries */
.collision-overlap {
  position: relative;
}
.collision-overlap .background-element {
  width: 100%;
  min-height: 60vh;
  background: #000;
}
.collision-overlap .crossing-element {
  position: absolute;
  bottom: -15%; /* breaks out of parent. Crosses the section boundary. */
  left: 10%;
  width: 45%;
  background: #fff;
  padding: 3rem;
  z-index: 2;
  /* This element VIOLATES the section boundary. Intentionally. */
}
```

---

## LAW 08 — COMPOSE AT THE VIEWPORT

The full screen is one composition. Not a stack of components.

```css
/* FULL VIEWPORT COMPOSITION — everything relates */
.viewport-composition {
  height: 100vh;
  display: grid;
  grid-template-columns: 1fr 3fr 1fr;
  grid-template-rows: auto 1fr auto;
  gap: 0;
  overflow: hidden;
  position: relative;
}

/* Elements at viewport edges create dialogue across the screen */
.viewport-composition .top-left {
  grid-column: 1;
  grid-row: 1;
  padding: 2rem;
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.2em;
}
.viewport-composition .center-mass {
  grid-column: 2;
  grid-row: 1 / -1;
  display: flex;
  align-items: center;
  justify-content: center;
}
.viewport-composition .bottom-right {
  grid-column: 3;
  grid-row: 3;
  padding: 2rem;
  text-align: right;
  align-self: end;
  font-size: 11px;
  /* Dialogues with top-left across the full viewport diagonal */
}

/* SCROLL SEQUENCE — sections as temporal events */
.scroll-sequence > section:nth-child(odd) {
  min-height: 100vh;
  padding: 5vh 5%;
  /* Dense sections */
}
.scroll-sequence > section:nth-child(even) {
  min-height: 60vh;
  padding: 20vh 15%;
  /* Breathing sections — lower density, more space */
}
/* The alternation creates rhythm through scroll. */
```

---

## OVERLAPPING LAYERS (Z-AXIS)

When x/y grid fails, build depth.

```css
/* THREE-LAYER DEPTH */
.depth-composition {
  position: relative;
  min-height: 100vh;
  overflow: hidden;
}

/* Layer 1: Background atmosphere */
.depth-composition .layer-bg {
  position: absolute;
  inset: 0;
  font-size: 30vw;
  font-weight: 900;
  color: rgba(0, 0, 0, 0.03); /* ghost type */
  display: flex;
  align-items: center;
  justify-content: center;
  line-height: 0.8;
  z-index: 1;
  pointer-events: none;
}

/* Layer 2: Image mass */
.depth-composition .layer-image {
  position: absolute;
  top: 10%;
  left: 5%;
  width: 60%;
  height: 70%;
  z-index: 2;
}
.depth-composition .layer-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  mix-blend-mode: multiply; /* image interacts with layers below */
}

/* Layer 3: Foreground text */
.depth-composition .layer-text {
  position: absolute;
  bottom: 8%;
  right: 5%;
  width: 35%;
  z-index: 3;
  background: #fff;
  padding: 2rem;
  /* Overlaps image. Partially occludes it. Establishes who's in front. */
}
```

---

## ROTATED + OFF-GRID ELEMENTS

Elements that break the orthogonal grid. Not decoration — structural.

```css
/* ROTATED TYPE AS STRUCTURAL DIVIDER */
.rotated-divider {
  position: absolute;
  left: 50%;
  top: 0;
  height: 100%;
  transform: rotate(-90deg) translateX(-100%);
  transform-origin: top left;
  font-size: 8vw;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 0.3em;
  white-space: nowrap;
  color: #000;
  /* This splits the layout in half. It IS the grid. */
}

/* ELEMENT BREAKING CONTAINER — deliberate overflow */
.container-break {
  position: relative;
  padding: 5rem;
  background: #f5f5f5;
}
.container-break .escapee {
  position: absolute;
  top: -3rem; /* breaks above */
  right: -5vw; /* breaks right */
  width: 40vw;
  /* This element refuses to be contained. It crosses boundaries. */
}

/* DIAGONAL ELEMENT — earned by content, not arbitrary */
.diagonal-cut {
  clip-path: polygon(0 0, 100% 8%, 100% 100%, 0 92%);
  /* A subtle diagonal — the content is TILTED. Not just decorated. */
}
```

---

## MICRO-ELEMENT COHESION

The tiny recurring details that hold wildly different sections together.

```css
/* SECTION LABEL — the connective thread */
.section-label {
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 0.3em;
  font-weight: 600;
  /* This EXACT treatment appears in every section, no matter how different
     the section's internal layout is. It's the DNA. */
}

/* STRUCTURAL RULE — the hairline that appears everywhere */
.structural-rule {
  width: 40px;
  height: 1px;
  background: currentColor;
  /* Same width. Same weight. Every section. The thread. */
}

/* CONSISTENT METADATA TREATMENT */
.meta {
  font-size: 11px;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  opacity: 0.6;
  /* Dates, categories, labels — all get this exact treatment.
     When sections go wild, these stay identical. That's the cohesion. */
}
```

---

## FULL COMPOSITION EXAMPLE

A section that applies multiple laws simultaneously.

```html
<section class="based-section">
  <!-- Ghost type: atmosphere layer (LAW 03) -->
  <div class="ghost" aria-hidden="true">DESIGN</div>

  <!-- Architectural type: horizontal beam (LAW 03, 06) -->
  <h1 class="beam">STRUCTURAL<br>MECHANICS</h1>

  <!-- Content: asymmetric weight (LAW 04) -->
  <div class="content-zone">
    <div class="mass">
      <p class="meta">ISSUE 27 — 1993</p>
      <p class="body">The grid is not absent — it is established
        specifically so its breaking is legible.</p>
      <div class="structural-rule"></div>
    </div>
    <div class="breath">
      <!-- Pressurized emptiness (LAW 05) -->
    </div>
  </div>

  <!-- Rotated pillar (LAW 03) -->
  <div class="pillar" aria-hidden="true">EMIGRE</div>
</section>
```

```css
.based-section {
  position: relative;
  min-height: 100vh;
  background: #fff;
  overflow: hidden;
  padding: 0;
}

/* Ghost: 35vw type at 4% opacity. Felt, not read. */
.based-section .ghost {
  position: absolute;
  top: 50%;
  left: -2%;
  transform: translateY(-50%);
  font-size: 35vw;
  font-weight: 900;
  color: rgba(0, 0, 0, 0.035);
  white-space: nowrap;
  pointer-events: none;
  z-index: 1;
  line-height: 0.75;
}

/* Beam: viewport-width headline. Architectural. */
.based-section .beam {
  font-size: clamp(10vw, 16vw, 20vw);
  font-weight: 900;
  line-height: 0.82;
  text-transform: uppercase;
  letter-spacing: -0.02em;
  padding: 5vh 5% 0;
  position: relative;
  z-index: 2;
  /* This text divides the viewport into above-beam and below-beam zones. */
}

/* Content: weighted 7:3 left */
.based-section .content-zone {
  display: grid;
  grid-template-columns: 7fr 3fr;
  gap: 0;
  padding: 5vh 5%;
  position: relative;
  z-index: 2;
}
.based-section .mass {
  max-width: 50ch;
}
.based-section .body {
  font-size: 16px;
  line-height: 1.5;
  margin: 1.5rem 0;
}

/* Pillar: rotated type as right-edge wall */
.based-section .pillar {
  position: absolute;
  right: 3%;
  top: 0;
  height: 100%;
  writing-mode: vertical-rl;
  font-size: 8vw;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 0.2em;
  color: rgba(0, 0, 0, 0.08);
  display: flex;
  align-items: center;
  z-index: 1;
}
```

---

## WHAT NOT TO WRITE

These CSS patterns are the enemy. If you catch yourself writing any of these, stop and push harder.

```css
/* THE SAFE CENTER — kill it */
.hero { text-align: center; } /* NO. Asymmetric weight. */
.hero h1 { font-size: 48px; } /* NO. That's 3:1. Need 25:1. */
.hero p { max-width: 600px; margin: 0 auto; } /* NO. Not centered. Weighted. */

/* THE POLITE SPACER — kill it */
section { margin-bottom: 4rem; } /* NO. Sections collide or breathe. Not politely spaced. */
section + section { border-top: 1px solid #eee; } /* NO. Hard collision or nothing. */

/* THE GRADIENT MEDIATOR — kill it */
.divider { background: linear-gradient(to bottom, #000 0%, #fff 100%); } /* NO. Hard edge. */
.fade-in { opacity: 0; transition: opacity 0.5s; } /* NO on layout. Collision, not fade. */

/* THE UNIFORM GRID — kill it */
.container { max-width: 1200px; margin: 0 auto; padding: 0 2rem; } /* NO. Each section is its own world. */
.grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 2rem; } /* NO. Asymmetric columns. Unequal. Weighted. */

/* THE TIMID TYPE — kill it */
h1 { font-size: 3rem; } /* NO. That's 48px against 16px = 3:1. Need 25:1 minimum. */
h2 { font-size: 2rem; } /* NO. Four hierarchy levels already. Three max. */
h3 { font-size: 1.5rem; } /* NO. This is the mush zone. Delete it. */
h4 { font-size: 1.25rem; } /* NO. Five levels?? Based Design allows three. */
```

---

*Every pattern here was derived from the structural physics of Emigre Magazine Issues 01-45. They are not style suggestions — they are the minimum viable implementation of compositional mechanics on the web. If the code feels uncomfortable to write, you're probably doing it right.*
