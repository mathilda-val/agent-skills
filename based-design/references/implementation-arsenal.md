# Implementation Arsenal

Creative interaction and animation patterns extracted from production interfaces. Use as an inspiration catalog — pick what serves the composition, not as a checklist.

## Navigation Patterns

- **Dock Magnification:** Items scale up as cursor approaches, scale down as it leaves. Pure CSS: `:hover` + adjacent sibling selectors. JS: distance-based scaling.
- **Magnetic Button:** Button element subtly translates toward cursor position within a threshold radius. Pure CSS: limited. JS: track pointer, apply transform.
- **Gooey Menu:** SVG filter (`feGaussianBlur` + `feColorMatrix`) on expanding menu items creates organic blob merging effect.
- **Dynamic Island:** Fixed container that morphs shape/size to display contextual info. CSS: `border-radius` + `width`/`height` transitions on a fixed element. Use `transform: scale()` for performance.
- **Contextual Radial Menu:** Right-click or long-press spawns circular option layout around cursor position.
- **Floating Speed Dial:** FAB that expands into staggered action buttons.
- **Mega Menu Reveal:** Full-width dropdown with grid layout, revealed on hover/click with opacity + translateY transition.

## Layout Patterns

- **Bento Grid:** Mixed-size cards in a CSS Grid with `grid-template-areas` or `span` directives. See Bento Paradigm section below.
- **Masonry:** CSS: `columns` property or `grid-template-rows: masonry` (experimental). JS: calculate positions, apply absolute positioning.
- **Chroma Grid:** Grid where each cell has a distinct background color from a coordinated palette. Color = zoning (Law 10).
- **Split Screen Scroll:** Two columns scroll at different speeds or in opposite directions. Pure CSS: `position: sticky` on one column.
- **Curtain Reveal:** Content revealed as a panel slides away. CSS: `clip-path` animation or translating overlay.

## Card Interactions

- **Parallax Tilt:** Card rotates on X/Y axes based on cursor position. JS: map cursor coords to `rotateX`/`rotateY` via `transform`. Add `perspective` to parent.
- **Spotlight Border:** Radial gradient follows cursor along card border. JS: update `background-position` of a border gradient on mousemove.
- **Glassmorphism Panel:** `backdrop-filter: blur(12px)` + semi-transparent background + 1px inner border (`inset 0 0 0 1px rgba(255,255,255,0.1)`) + subtle inner shadow.
- **Holographic Foil:** Multi-color gradient that shifts with cursor position. CSS `background: linear-gradient(...)` with JS-updated angle/position.
- **Swipe Stack:** Cards stacked with slight offset, top card draggable. JS: touch/pointer events + transform. With libraries: Framer Motion `drag` prop.
- **Morphing Modal:** Card expands into full modal using shared element transition. CSS: View Transitions API. JS: FLIP technique (First, Last, Invert, Play).

## Scroll-Driven Patterns

- **Sticky Scroll Stack:** Sections with `position: sticky; top: 0` stack on top of each other as user scrolls.
- **Horizontal Scroll Hijack:** Vertical scroll mapped to horizontal translation. CSS: `scroll-snap-type: x mandatory` in horizontal container. JS: translate container on wheel event.
- **Zoom Parallax:** Elements scale up/down based on scroll position. CSS: `animation-timeline: scroll()`. JS: Intersection Observer + transform scale.
- **Scroll Progress Path:** SVG path `stroke-dashoffset` animated by scroll position. CSS: `animation-timeline: scroll()`. JS: calculate scroll percentage, update dashoffset.
- **Liquid Swipe Transition:** Section transitions using SVG curve that deforms as you scroll. Requires JS path manipulation.

## Gallery Patterns

- **Coverflow Carousel:** Items in perspective with `rotateY` based on distance from center. CSS: `perspective` + `transform-style: preserve-3d`.
- **Drag-to-Pan Grid:** Oversized grid that responds to drag/touch for navigation. JS: pointer events + transform translate.
- **Accordion Image Slider:** Images as vertical/horizontal strips; hovered strip expands, others compress. Pure CSS: `flex-grow` on `:hover` with transition.
- **Hover Image Trail:** Images spawn at cursor position with delay and fade, creating a trail effect. JS: spawn elements on mousemove, remove after animation.
- **Glitch Effect:** `clip-path` or offset duplicate layers with color channel separation, animated on hover/interval.

## Typography Animation

- **Kinetic Marquee:** Continuous horizontal scroll of text. CSS: `@keyframes` translateX loop. Duplicate content for seamless loop.
- **Text Mask Reveal:** Text used as `clip-path` or `background-clip: text` mask over image/video.
- **Text Scramble:** Characters cycle through random glyphs before settling on final letter. JS: interval-based character replacement.
- **Circular Text Path:** Text on SVG `<textPath>` following a circle. Rotate the circle for animation.
- **Gradient Stroke Animation:** Text with `-webkit-text-stroke` and animated gradient background. Or SVG text with animated stroke-dashoffset.

## Micro-Interactions

- **Particle Explosion Button:** On click, spawn particle elements that animate outward with random vectors. JS: create elements, apply random transform + opacity animation.
- **Liquid Pull-to-Refresh:** SVG-based stretchy indicator that deforms based on pull distance.
- **Skeleton Shimmer:** Placeholder shapes with animated linear-gradient moving left-to-right. Pure CSS: `@keyframes` + `background-size: 200%`.
- **Directional Hover Button:** Fill/highlight enters from the direction the cursor entered. JS: calculate entry edge from pointer position relative to element bounds.
- **Ripple Click:** Material-style expanding circle from click point. CSS: pseudo-element with scale animation triggered by JS setting position.
- **Animated SVG Line Drawing:** Set `stroke-dasharray` equal to path length, animate `stroke-dashoffset` from full to 0.
- **Mesh Gradient Background:** Multiple radial gradients positioned at different points, optionally animated. CSS: layered `radial-gradient()`.
- **Lens Blur Depth:** Foreground sharp, background progressively blurred. CSS: `backdrop-filter` on overlapping layers or individual `filter: blur()` values.

---

## Bento Paradigm

A high-end minimal SaaS aesthetic for dashboard and product interfaces.

### Visual Spec
- Background: neutral off-white (`#f9fafb` or similar)
- Cards: white fill, 1px border (`border-color` in grey-200 range), large border-radius (12–16px)
- Interactions: spring-physics easing (CSS: `cubic-bezier(0.34, 1.56, 0.64, 1)` approximation; JS: spring physics libraries)
- Every card contains at least one perpetual micro-interaction (shimmer, pulse, counter, live indicator)

### 5 Card Archetypes
1. **Intelligent List** — Sortable/filterable items with inline actions. Stagger-animated on load.
2. **Command Input** — Search or command bar with live suggestions. Keyboard-navigable.
3. **Live Status** — Real-time indicators, connection states, heartbeat animations.
4. **Wide Data Stream** — Horizontally expansive, charts/timelines/logs. Spans 2+ grid columns.
5. **Contextual UI** — Content changes based on selection elsewhere. Shared element transitions on state change.

### Layout
CSS Grid with named areas or `auto-fill` with `minmax()`. Cards span different row/column counts. Asymmetric by nature (Law 4). Dense packing with `grid-auto-flow: dense`.
