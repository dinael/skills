---
name: CSS
description: Write modern CSS with proper stacking contexts, layout patterns, responsive techniques, and performance optimization.
---

# Senior modern css developer

This skill provides a reference for writing modern, robust, and efficient CSS.

## When to Use

User needs CSS expertise — from layout challenges to production optimization. Agent handles stacking contexts, flexbox/grid patterns, responsive design, performance, and accessibility.

## Quick Reference

| Topic | File |
|-------|------|
| Layout patterns | `layout.md` |
| Responsive techniques | `responsive.md` |
| Selectors and specificity | `selectors.md` |
| Performance optimization | `performance.md` |

## CSS Philosophy

- Layout should be robust—work with any content, not just demo content
- Use modern features—they have better browser support than you think
- Prefer intrinsic sizing—let content determine size when possible
- Test with extreme content—longest names, missing images, empty states

## Stacking Context Traps

- `z-index` only works with positioned elements—or flex/grid children
- `isolation: isolate` creates stacking context—contains z-index chaos without position
- `opacity < 1`, `transform`, `filter` create stacking context—unexpected z-index behavior
- New stacking context resets z-index hierarchy—child z-index:9999 won't escape parent

## Layout Traps

- Margin collapse only vertical, only block—flex/grid children don't collapse
- `overflow: hidden` on flex container can break—use `overflow: clip` if you don't need scroll

## Flexbox Traps

- `flex: 1` means `flex: 1 1 0%`—basis is 0, not auto
- `min-width: 0` on flex child for text truncation—default min-width is min-content
- `flex-basis` vs `width`: basis is before grow/shrink—width is after, basis preferred
- `gap` works in flex now—no more margin hacks for spacing

## Grid Traps

- `fr` units don't respect min-content alone—use `minmax(min-content, 1fr)`
- `auto-fit` vs `auto-fill`: fit collapses empty tracks, fill keeps them
- `grid-template-columns: 1fr 1fr` is not 50%—it's equal share of REMAINING space
- Implicit grid tracks can surprise you—items placed outside explicit grid still appear

## Responsive Philosophy

- Responsive design is accessibility
- Mobile-first is user-first, start mobile-first—`min-width` media queries, base styles for mobile
- Container queries: `@container (min-width: 400px)`—component-based responsive
- `container-type: inline-size` on parent required—for container queries to work
- Test on real devices—emulators miss touch targets and real performance

### Decision Tree

``` text
User requests responsive styles
    ↓
Determine component type
    ↓
Define mobile-first base styles (320px+)
    ↓
Calculate contrast ratios (WCAG AA)
    ↓
Add tablet breakpoint (768px+) if layout changes
    ↓
Add desktop breakpoint (1024px+) if needed
    ↓
Add all interactive states (focus > hover)
    ↓
Add reduced motion support
    ↓
Output mobile-first SCSS with exact values
```

## Sizing Functions

- `clamp(min, preferred, max)` for fluid typography—`clamp(1rem, 2.5vw, 2rem)`
- `min()` and `max()`—`width: min(100%, 600px)` replaces media query
- `fit-content` sizes to content up to max—`width: fit-content` or `fit-content(300px)`

## Modern Selectors

- `:is()` for grouping—`:is(h1, h2, h3) + p` less repetition
- `:where()` same as `:is()` but zero specificity—easier to override
- `:has()` parent selector—`.card:has(img)` styles card containing image
- `:focus-visible` for keyboard focus only—no outline on mouse click

## Scroll Behavior

- `scroll-behavior: smooth` on html—native smooth scroll for anchors
- `overscroll-behavior: contain`—prevents scroll chaining to parent/body
- `scroll-snap-type` and `scroll-snap-align`—native carousel without JS
- `scrollbar-gutter: stable`—reserves scrollbar space, prevents layout shift

## Shorthand Traps

- `inset: 0` equals `top/right/bottom/left: 0`—less repetition
- `place-items` is `align-items` + `justify-items`—`place-items: center` centers both
- `margin-inline`, `margin-block` for logical properties—respects writing direction

## Performance Mindset

- `contain: layout` isolates repaints—use on independent components
- `content-visibility: auto` skips offscreen rendering—huge for long pages
- `will-change` sparingly—creates layers, uses memory
- Avoid layout thrash—batch reads and writes to DOM

## Accessibility Baseline

- `prefers-reduced-motion: reduce`—disable animations for vestibular disorders
- `prefers-color-scheme`—`@media (prefers-color-scheme: dark)` for dark mode
- `forced-colors: active`—adjust for Windows high contrast
- Focus indicators must be visible—don't rely on color alone

## Layout Patterns

### Flexbox Patterns

- `display: flex; gap: 1rem` for spaced rows—clean and simple
- `justify-content: space-between` for nav/footer with logo and links
- `flex-wrap: wrap` + `flex: 1 1 300px` for card grids—responsive without media queries
- `align-items: stretch` is default—children fill height unless explicitly sized

### Grid Patterns

- `grid-template-columns: repeat(auto-fit, minmax(250px, 1fr))` for responsive cards
- `grid-template-areas` for complex layouts—visual and maintainable
- `grid-column: 1 / -1` for full-width items in grid
- Subgrid for aligned nested content—parent grid lines extend to children

### Centering Patterns

- `place-items: center` on grid—centers both axes
- `margin: auto` on flex child—pushes to edges or centers
- `position: absolute; inset: 0; margin: auto` for overlay centering
- Grid/flex on parent, auto margins on child—most robust approach

### Sticky Patterns

- `position: sticky; top: 0` for sticky headers—needs scrolling ancestor
- Sticky doesn't work with `overflow: hidden` on ancestor—clips the sticky area
- Multiple stickies can stack—adjust `top` values to account for each other
- Use `z-index` with sticky—it stacks above siblings

### Overflow Handling

- `overflow: hidden` clips content—use `overflow: clip` if you don't need scroll
- `overflow: auto` vs `scroll`: auto only shows scrollbar when needed
- `text-overflow: ellipsis` needs `overflow: hidden` AND `white-space: nowrap`
- `overflow-x: clip; overflow-y: visible` is tricky—often becomes `overflow-x: clip; overflow-y: auto`

### Box Model Patterns

- `box-sizing: border-box` on everything—width includes padding and border
- Margin collapse only vertical, only block—flex/grid children don't collapse
- `padding: max(1rem, 5vw)` for responsive padding—clamped minimum
- `outline` doesn't affect layout—useful for debugging without side effects

### Logical Properties

- `margin-inline`, `padding-block` for writing-mode aware spacing
- `inset-inline-start` instead of `left`—respects RTL
- `inline-size` instead of `width`—adapts to writing direction
- Use logical properties for international-ready CSS

### Common Layout Bugs

- 100% height not working—parent chain must also have defined height or use flexbox
- Content overflows container—`min-width: 0` on flex children
- Footer not at bottom—use flexbox or grid on body, `margin-top: auto` on footer
- Unexpected scrollbar—check for content slightly bigger than container

## Performance Optimization

### Render Pipeline Understanding

- Style → Layout → Paint → Composite—changes early in pipeline are expensive
- `transform` and `opacity` only trigger composite—cheapest animations
- `width`, `height`, `margin` trigger layout—expensive, avoid animating
- `background-color`, `box-shadow` trigger paint—moderate cost

### Containment

- `contain: layout` isolates layout calculations—changes inside don't affect outside
- `contain: paint` creates paint boundary—clips like overflow:hidden
- `contain: strict` is all containment—maximum isolation, use on independent widgets
- `content-visibility: auto` skips rendering offscreen—huge savings on long pages

### Animation Performance

- Only animate `transform` and `opacity`—everything else causes repaint
- Use `will-change: transform` to hint—but creates layer, uses memory
- Don't overuse `will-change`—hundreds of layers = memory issues
- `transform: translateZ(0)` to force layer—but prefer `will-change`

### Layout Thrashing

- Reading layout property forces synchronous layout—batch reads together
- Write all changes, then read if needed—don't interleave
- Use `requestAnimationFrame` for visual changes—batches with next frame
- Virtual DOM frameworks handle this—but still know the concept

### Selector Performance

- Right-to-left matching—browser finds all matches of rightmost, filters up
- Qualified selectors slower—`div.class` slower than `.class`
- Deep nesting expensive—`.a .b .c .d .e` searches a lot
- ID selectors fastest but least reusable

### Font Loading

- FOUT (Flash of Unstyled Text) with `font-display: swap`—shows fallback first
- FOIT (Flash of Invisible Text) with `block`—text hidden until loaded
- `font-display: optional` best for performance—may not show custom font
- Preload critical fonts: `<link rel="preload" as="font" crossorigin>`

### CSS File Optimization

- Unused CSS still downloaded and parsed—audit and remove
- `@import` is render-blocking—use `<link>` tags instead
- Critical CSS inline in `<head>`—rest can load async
- Consider CSS-in-JS tradeoffs—runtime cost vs HTTP cache

### Paint Optimization

- `box-shadow` on scroll elements is expensive—moves repaint on scroll
- Large `border-radius` with overflow can be costly
- `filter: blur()` on large elements expensive
- Use compositor-only properties when possible

### Measuring Performance

- Chrome DevTools Performance panel—see paint, layout, script timing
- "Show paint rectangles"—visualize what's being repainted
- Lighthouse for overall audit—catches common issues
- Test on real low-end devices—your MacBook is not representative

## Interactive states

- `:hover` - Mouse pointer over element
- `:focus` - Keyboard navigation focus
- `:focus-visible` - Keyboard focus (not mouse click)
- `:active` - During click/tap
- `:disabled` - Inactive state

**Priority**: Focus states more important than hover (accessibility)

### Spacing

### Vertical rhythm

- **8point grid**: 8px, 16px, 24px, 32px, 40px, 48px, 64px
- **rem-based**: 0.5rem, 1rem, 1.5rem, 2rem, 3rem, 4rem

**Apply consistently**:

- Margins, padding, gaps use same scale
- Avoid arbitrary values (17px, 23px)

## Layout & Responsive Design

This skill automatically activates when:

- User asks to "make it responsive"
- User mentions breakpoints, mobile, tablet, or desktop
- Creating styles for a component
- User asks about media queries
- Responsive design is needed for implementation

### Core Beliefs

1. **Mobile First is Performance First**: Start with constraints, enhance progressively
2. **Accessibility is Non-Negotiable**: WCAG AA compliance is the baseline, not a bonus
3. **Exact Specifications Build Confidence**: Calculate exact contrast ratios, don't estimate
4. **Touch-Friendly by Default**: 2.75rem/44px minimum targets prevent user frustration

### Why Mobile-First Responsive Styling Matters

- **User Experience**: Most users access sites on mobile devices
- **Performance**: Smaller initial payload, enhance for larger screens
- **Accessibility**: Ensures usability for all users and devices
- **Professional Quality**: Shows attention to detail and best practices

**WCAG 2.1 Level AA compliance**:

- ✅ **Touch targets**: ≥ 44x44px on mobile, ≥ 40x40px on desktop
- ✅ **Focus indicators**: 2px outline minimum, distinct from hover
- ✅ **Motion sensitivity**: `@media (prefers-reduced-motion: reduce)`

### Viewport Units andDynamic Viewport Units

- `100vh` on mobile includes toolbar—content behind address bar
- `dvh` / `dvw` -  (dynamic viewport) changes as toolbar hides/shows—causes reflow
- `svh` / `svw` - (smallest possible viewport) always excludes toolbar—most predictable
- `lvh` / `lvw` - Large (largest possible viewport)

- Mix: `height: 100svh; min-height: 100dvh` for hero sections

### Media Query Patterns

- Mobile-first: use `min-width`—base styles for mobile, layer up for larger
- `min-width` is mobile-first; `max-width` is desktop-first—don't mix approaches
- Common breakpoints: 640px, 768px, 1024px, 1280px—but adapt to your content
- Prefer intrinsic sizing over breakpoints—fewer media queries needed

**Standard mobile-first breakpoints**:

- **Base styles** (320px+) - Mobile default
- **Tablet** (768px+) - `@media (min-width: 768px)`
- **Desktop** (1024px+) - `@media (min-width: 1024px)`
- **Large desktop** (1440px+) - Optional for max-width constraints

### Reduced Motion

```scss
// Respect user's motion preferences
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

**When to add breakpoints**:

- ✅ Layout changes significantly (columns stack/unstack)
- ✅ Typography scales (mobile 16px → desktop 18px)
- ✅ Touch targets adjust (mobile 44px → desktop 40px)
- ❌ Minor pixel adjustments (avoid breakpoint bloat)

## Common Mistakes to Avoid

❌ **Desktop-first** (using max-width)
❌ **Magic numbers** (random breakpoints)
❌ **Forgetting touch targets**
❌ **Fixed pixel widths** that don't scale
❌ **Tiny text on mobile** (<16px)
❌ **Horizontal scrolling**
❌ **Ignoring landscape mobile**
❌ **Breaking at intermediate sizes**

✅ **Mobile-first** (using min-width)
✅ **Consistent breakpoints**
✅ **2.75rem/44px minimum touch targets**
✅ **Flexible widths** with max-width
✅ **16px minimum text**
✅ **Contained content**
✅ **Test at 320px, 768px, 1024px**
✅ **Test at all widths**

### Media Query Range Syntax

```css
@media (width <= 1024px) { }
@media (360px < width < 1024px) { }
```

### Container Queries

- `container-type: inline-size` on parent—enables queries on that container
- `@container (min-width: 400px) { ... }`—component responds to ITS container
- `container-name` for targeting specific ancestor—`@container sidebar (min-width: ...)`
- Better than media queries for reusable components—same component, different contexts

```css
.card {
  container: --my-card / inline-size;
}

@container --my-card (width < 40ch) {
  /* Component-based responsive design */
}

@container (20ch < width < 50ch) {
  /* Range syntax */
}
```

**Container units:** `cqi`, `cqb`, `cqw`, `cqh` - size relative to container dimensions

**Anchored container queries:** Style positioned elements based on anchor fallback state

```css
.tooltip {
  container-type: anchored;
}

@container anchored(top) {
  /* Styles when positioned at top */
}
```

## Layout Helper

### Purpose

Explain CSS layout issues and propose fixes.

### Inputs to request

- HTML structure and CSS snippet.
- Desired layout and screenshots.
- Target browsers and breakpoints.

### Workflow

- Identify the container and child roles.
- Recommend flex or grid with key properties.
- Provide a minimal CSS snippet to test.

### Output

- Proposed CSS changes with explanation.

### Quality bar

- Prefer minimal changes over rewrites.
- Call out responsive implications.

## Typography

### Text Wrapping

```css
h1 {
  text-wrap: balance; /* Balanced multi-line headings */
  max-inline-size: 25ch;
}

p {
  text-wrap: pretty; /* No orphans */
  max-inline-size: 50ch;
}
```

### Text Box Trim

```css
h1, p, button {
  text-box: trim-both cap alphabetic; /* Optical vertical centering */
}
```

### Fluid Typography

- `clamp(min, preferred, max)` for smooth scaling—`font-size: clamp(1rem, 2vw + 0.5rem, 2rem)`
- Don't use only vw—user can't resize text, accessibility issue
- Always include rem component—`2vw + 0.5rem` allows zoom to work
- Test at extreme widths—very wide screens can make huge text

```css
.heading {
  font-size: clamp(1rem, 1rem + 0.5vw, 2rem); /* Respects user preferences */
}
```

### Fluid Spacing

- `clamp()` for padding/margin too—`padding: clamp(1rem, 5vw, 3rem)`
- CSS `min()` and `max()` for one-sided constraints—`width: min(100%, 800px)`
- Combine with calc: `calc(1rem + 2vw)`—smooth transition between sizes
- Use CSS custom properties for consistent fluid scales

### Image Responsive

- `max-width: 100%; height: auto` on images—prevents overflow, maintains ratio
- `object-fit: cover` vs `contain`: cover crops, contain letterboxes
- `aspect-ratio` on container holds space—prevents layout shift during load
- `srcset` and `sizes` for art direction—browser picks best image

#### aspect ratio

Use CSS `aspect-ratio` when dimensions aren't known or for container-based layouts.

| Ratio | CSS | Use Case |
|-------|-----|----------|
| **16:9** | `aspect-[16/9]` | Video thumbnails, hero images, modern displays |
| **4:3** | `aspect-[4/3]` | Standard photos, older displays |
| **3:2** | `aspect-[3/2]` | DSLR photos, 35mm film |
| **1:1** | `aspect-square` | Profile pictures, Instagram-style |
| **21:9** | `aspect-[21/9]` | Ultrawide banners, cinematic |
| **2:3** | `aspect-[2/3]` | Portrait photos, book covers |
| **9:16** | `aspect-[9/16]` | Vertical video (TikTok, Stories) |

#### Lazy Loading

Modern browsers support native lazy loading via the `loading` attribute.

Use `fetchpriority="high"` on LCP images to boost download priority.

#### srcset and sizes Attributes

Width descriptors tell the browser the **actual width of each image file** in pixels. The browser uses this with the `sizes` attribute and device pixel ratio to choose the optimal image.


```html
<img
  src="/image-800.jpg"
  srcset="
    /image-400.jpg 400w,
    /image-800.jpg 800w,
    /image-1200.jpg 1200w,
    /image-1600.jpg 1600w
  "
  sizes="(max-width: 768px) 100vw, 800px"
  alt="Responsive image"
/>
```

**Browser Selection Logic:**

1. Calculate display width from `sizes` attribute
2. Multiply by device pixel ratio (DPR)
3. Choose smallest image ≥ calculated width
4. Consider network conditions and cache

**Example**:

On a 375px wide phone (2x DPR)

- `sizes` evaluates to `375px` (100vw on mobile)
- Multiply by DPR: `375 * 2 = 750px`
- Browser chooses `image-800.jpg` (smallest ≥ 750px)

### Density Descriptors (x) - For Fixed Sizes Only

Density descriptors specify images for different device pixel ratios. Only use for **fixed-size images** like logos.

```html
<!-- Logo - fixed 150px width -->
<img
  src="/logo.png"
  srcset="/logo.png 1x, /logo@2x.png 2x, /logo@3x.png 3x"
  alt="Company logo"
  width="150"
  height="50"
/>
```

**When to Use Density Descriptors:**

- Logos and icons with fixed display size
- UI elements that don't scale with viewport
- Images where you control exact display dimensions

**Don't Use For:**

- Images that scale with viewport (use width descriptors)
- Content images in responsive layouts
- Grid or column-based images

#### Modern Image Formats

| Format | Quality | File Size | Transparency | Animation | Browser Support |
|--------|---------|-----------|--------------|-----------|-----------------|
| **JPEG** | Good | Medium | ❌ | ❌ | 100% |
| **PNG** | Lossless | Large | ✅ | ❌ | 100% |
| **GIF** | Limited | Medium | ✅ | ✅ | 100% |
| **WebP** | Excellent | Small | ✅ | ✅ | 97%+ |
| **AVIF** | Excellent | Smallest | ✅ | ✅ | 90%+ |

**File Size Comparison (Real Example)**:

**Original**: 1920×1080 photo

| Format | Quality | File Size | vs JPEG |
|--------|---------|-----------|---------|
| JPEG | 90 | 500 KB | Baseline |
| PNG-24 | Lossless | 2.1 MB | +320% |
| WebP | 90 | 250 KB | **-50%** |
| AVIF | 90 | 150 KB | **-70%** |

**Key Insight**: AVIF can be 70% smaller than JPEG at same visual quality.

### Layout Shifts

- Reserve space for images—use `aspect-ratio` or explicit dimensions
- Font loading causes shift—use `font-display: swap` or preload critical fonts
- Ads/embeds shift content—reserve space with `min-height`
- `scrollbar-gutter: stable`—prevents shift when scrollbar appears/disappears

### Grid Enhancements

- **Subgrid:** Inherit parent grid lines for nested layouts
- **Masonry:** `display: grid-lanes` for Pinterest-style layouts with logical tab order. (Previously proposed as `grid-template-rows: masonry`).

---

### Testing Responsive

- Test on real devices—emulators miss real performance and touch targets
- Test with real content—long words, missing images, user-generated text
- Test both orientations—landscape tablet is not the same as desktop
- Check over-scroll behavior—pull-to-refresh, bounce effects

## Selectors and Specificity

### Specificity Rules

- Inline styles: 1000, ID: 100, Class: 10, Element: 1—cumulative
- `:where()` has ZERO specificity—great for defaults that are easy to override
- `:is()` takes HIGHEST specificity of its arguments—can surprise you
- `!important` beats all—but avoid in component styles, breaks cascade

### Modern Selector Patterns

- `:is(h1, h2, h3)` for grouping—less repetition than commas
- `:where(.btn, .link):hover` for low-specificity shared styles
- `:has(.error)` parent selector—style parent based on child
- `:focus-visible` instead of `:focus`—keyboard only, no outline on click

### :has() Patterns

- `.card:has(img)` style card based on contents—true parent selector
- `form:has(:invalid)` style form when any input invalid
- `.item:has(+ .active)` select item BEFORE active—previous sibling
- Performance cost—use sparingly on large documents

### Pseudo-class Traps

- `:nth-child` vs `:nth-of-type`—child counts all siblings, type only same element
- `:first-child` inside wrapper—first child of wrapper, not first matching element
- `:empty` is strict—whitespace text nodes count as content, element not empty
- `:not()` doesn't add specificity in old syntax—does in modern syntax with complex selectors

### Pseudo-element Patterns

- `::before`/`::after` need `content: ""`—empty string at minimum
- Can't add pseudo-elements to `<img>`, `<input>`—replaced elements
- Double colon `::` is CSS3—single `:` still works but deprecated
- `::placeholder` for input placeholders—needs vendor prefixes in older browsers

### Attribute Selectors

- `[disabled]` has attribute—regardless of value
- `[href^="https"]` starts with—external links
- `[href$=".pdf"]` ends with—PDF downloads
- `[data-state="active" i]` case-insensitive match—the `i` flag

### Combinator Performance

- Browser matches selectors right-to-left—`.nav li a` finds all `a`, then filters
- Deep descendant selectors can be slow—`.a .b .c .d .e` is expensive
- Direct child `>` is more efficient—limits search scope
- ID selectors are fastest—but poor for reusable components

### Specificity Management

- Use BEM or similar—keeps specificity flat and predictable
- Layer: reset, base, components, utilities—utilities can override
- CSS custom properties ignore specificity—great for theming
- Use `@layer` for explicit cascade ordering—modern approach

## Color & Theming

### Color Scheme & Light-Dark Function

```css
:root {
  color-scheme: light dark;
  --surface-1: light-dark(white, #222);
  --text-1: light-dark(#222, #fff);
}
```

### Modern Color Spaces

```css
/* OKLCH: uniform brightness, P3+ colors */
.vibrant {
  background: oklch(72% 75% 330);
}

/* Display-P3 for HDR displays */
@media (dynamic-range: high) {
  .neon {
    --neon-red: color(display-p3 1 0 0);
  }
}

/* Better gradients with in oklch */
.gradient {
  background: linear-gradient(
    to right in oklch,
    color(display-p3 1 0 .5),
    color(display-p3 0 1 1)
  );
}
```

### Color Manipulation

**Color mix**:

```css
/* color-mix() */
.lighten {
  background: color-mix(in oklab, var(--brand), white);
}

/* Relative color syntax */
.lighter {
  background: oklch(from blue calc(l + .25) c h);
  background: oklch(from blue 75% c h); /* Set to specific lightness */
}

.semi-transparent {
  background: oklch(from var(--color) l c h / 50%);
}

.complementary {
  background: hsl(from blue calc(h + 180) s l);
}
```

### light-dark()

Leverage `color-scheme` for easy adaptive color

```css
    :root {
      color: light-dark(#333, white);
      background: light-dark(white, black);
    }

    section {
      border: 2px solid light-dark(lightgray, darkgray);
    }

    :root {
      --surface-1: light-dark(white, #222);
      --surface-2: light-dark(#eee, #444);
      --text-1:    light-dark(#222, #fff);
      --text-2:    light-dark(#444, #ddd);
    }
```

### Accent Color

```css
:root {
  accent-color: hotpink; /* Tints checkboxes, radios, range inputs */
}
```

---

## Animations & Motion

### Scroll-Driven Animation

```css
/* Animate on scroll position */
.parallax {
  animation: slide-up linear both;
  animation-timeline: scroll();
}

/* Animate on viewport intersection */
.fade-in {
  animation: fade linear both;
  animation-timeline: view();
  animation-range: cover -75cqi contain 20cqi;
}

.animate-on-scroll {
  animation: somethin-coo linear both;
  animation-timeline: scroll();
}

.animate-on-viewport-intersection {
  animation: somethin-coo linear both;
  animation-timeline: view();
}

@supports (animation-timeline: view()) {
  animation: slide-in linear both;
  animation-timeline: view(x);
  animation-range: cover -75cqi contain 20cqi;
}
```

### View Transitions

**Status:** Baseline Newly Available (Same-document).
Cross-document transitions are in Limited Availability (Chrome/Safari 18.2+).

```css
@view-transition {
  navigation: auto; /* Automatically animate page transitions (MPAs) */
}

nav {
  view-transition-name: --persist-nav; /* Persist specific elements */
  view-transition-class: --site-header; /* Group transitions with classes */
}

/* Style the active transition */
html:active-view-transition {
  overflow: hidden;
}
```

**Nested View Transition Groups:** Preserve 3D transforms and clipping during transitions.

### Page Transitions

Easily transition elements or entire pages

```css
    @view-transition {
      navigation: auto;
    }

    nav {
        view-transition-name: --persist-nav;
    }
```

[nerdy.dev](https://nerdy.dev) [next slide](/02-media-query-ranges/)

### Advanced Easing with linear()

```css
.springy {
  --spring: linear(
    0, 0.14 4%, 0.94 17%, 1.15 24% 30%, 1.02 43%, 0.98 51%, 1 77%, 1
  );
  transition: transform 1s var(--spring);
}
```

### @starting-style

```css
.dialog {
  transition: opacity .5s, scale .5s;

  @starting-style {
    opacity: 0;
    scale: 1.1;
  }
}
```

---

## Custom Properties & Advanced Features

### @property

Type-safe, animatable custom properties:

``` css
/* A data type name */
syntax: "<color>";

/* A '|' combinator for multiple data types */
syntax: "<length> | <percentage>";

/* Space-separated list of values */
syntax: "<color>+";

/* Comma-separated list of values */
syntax: "<length>#";

/* Keywords */
syntax: "small | medium | large";

/* Combination of data type and keyword */
syntax: "<length> | auto";

/* Universal syntax value */
syntax: "*";

```

```css
@property --gradient-angle {
  syntax: "<angle>";
  inherits: false;
  initial-value: 0deg;
}

.animate {
  transition: --gradient-angle 1s ease;

  &:hover {
    --gradient-angle: 360deg;
  }
}
```

```css
    @property --unbreakable-color {
      syntax: "<color>";
      inherits: false;
      initial-value: #decade;
    }

    @property --interpolatable-percentage {
      syntax: "<percentage>";
      inherits: true;
      initial-value: 0%;
    }

    .animate-the-property {
      transition: --interpolatable-percentage 1s ease-out;

      &:hover {
        --interpolatable-percentage: 100%;
      }
    }

    @property --animate {
      syntax: '<percentage>';
      initial-value: 0%;
      inherits: false;
    }

    @keyframes use-keyframes { to {
      --animate: 100%;
    }}
```

---

## Math Functions & calc-size()

**Newly Available:** `calc-size()` allows calculations and transitions on intrinsic sizes (auto, min-content).

```css
/* Finally: Animate to auto height! */
.accordion-content {
  height: 0;
  overflow: hidden;
  transition: height 0.3s ease;
}

.accordion-item.open .accordion-content {
  height: calc-size(auto);
}

.radial-layout {
  --_angle: calc(var(--sibling-index) * var(--_offset));
  translate:
    calc(cos(var(--_angle)) * var(--_circle-size))
    calc(sin(var(--_angle)) * var(--_circle-size));
}
```

### Tree Counting Functions (Coming Soon)

```css
.staggered {
  animation-delay: calc(sibling-index() * .1s);
  background-color: hsl(sibling-count() 50% 50%);
}
```

### Conditional CSS with if() (Coming Soon)

```css
.dynamic {
  color: if(
    style(--theme: dark),
    white,
    black
  );
}
```

---

## Architecture & Organization

### Cascade Layers

```css
@layer reset, design-system, components, utilities;

@import "open-props/colors" layer(design-system);
@import "components/nav/base.css" layer(components.nav);

@layer components.nav.primary {
  nav {
    position: sticky;
    inset-block-start: 0;
  }
}
```

Benefits:

- Import third-party CSS with lower specificity
- Organize styles by concern, not selector weight
- Nested layers create clear hierarchies

---


## Architecture & Organization

### Cascade Layers

```css
@layer reset, design-system, components, utilities;

@import "open-props/colors" layer(design-system);
@import "components/nav/base.css" layer(components.nav);

## Interactive Components

### Dialog

```html
<dialog id="modal">
  <form method="dialog">
    <button value="cancel">Cancel</button>
    <button value="confirm">Confirm</button>
  </form>
</dialog>

<button commandfor="modal" command="showModal">Open</button>
<button commandfor="modal" command="close">Close</button>
```

**New:** `closedby` attribute enables light-dismiss behavior

### Popover

```html
<button popovertarget="menu">Show Menu</button>
<div popover id="menu">...</div>
```

**popover=hint:** Ephemeral tooltips that don't dismiss other popovers

```css
[popover] {
  transition:
    display .5s allow-discrete,
    overlay .5s allow-discrete,
    opacity .5s;

  @starting-style {
    &:popover-open {
      opacity: 0;
    }
  }
}
```

### Anchor Positioning

```css
.tooltip-anchor {
  anchor-name: --tooltip;
}

.tooltip[popover] {
  position-anchor: --tooltip;
  position-area: block-start;
  position-try-fallbacks: flip-block;
  position-try-order: most-height;
}
```

**Pseudo-elements:** `anchor()`, `::scroll-button()`, `::scroll-marker()`

### Exclusive Accordion

```html
<details name="accordion">...</details>
<details name="accordion">...</details>
<!-- Only one can be open at a time -->
```

### Customizable Select

```css
select {
  appearance: base-select; /* Full CSS control */
}

/* Style options with rich HTML */
select option::before {
  content: ""; /* Can include images, icons */
}
```

### Search Element

```html
<search>
  <form>
    <input type="search" name="q">
    <button type="submit">Search</button>
  </form>
</search>
```

---

## Form Enhancements

### Field Sizing

```css
textarea, select, input {
  field-sizing: content; /* Auto-grow to content */
}

textarea {
  min-block-size: 3lh; /* Line-height units */
  max-block-size: 80dvh;
}
```

### Better Validation Pseudo-Classes

`:valid` and `:invalid` are eager, `:user-valid` and `:user-invalid` are lazy

```css
/* Wait for user interaction before showing errors */
:user-invalid {
  outline-color: red;
}

:user-valid {
  outline-color: green;
}

label:has(+ input:user-invalid) {
  text-decoration: underline wavy red;
}
```

### HR in Select

```html
<select>
  <option>Option 1</option>
  <hr>
  <option>Option 2</option>
</select>
```

---


## Visual Effects

### Scrollbar Styling

```css
.custom-scrollbar {
  scrollbar-color: hotpink transparent;
  scrollbar-width: thin;
}
```

### Shape Function

```css
.complex-clip {
  clip-path: shape(
    from 0% 0%,
    curve by 50% 25% via 25% 50%,
    line to 100% 100%
  );
}
```

### Corner Shapes

```css
.fancy-corners {
  corner-shape: squircle;
  corner-shape: notch;
  corner-shape: scoop;
  corner-shape: superellipse(0.7);
}
```

## Progressive Enhancement Patterns

### Feature Detection

```css
@supports (animation-timeline: view()) {
  .fade-in {
    animation: fade linear both;
    animation-timeline: view();
  }
}

@supports (container-type: inline-size) {
  .responsive-card {
    container-type: inline-size;
  }
}
```

### Respect User Preferences

```css
@media (prefers-reduced-motion: no-preference) {
  .animated {
    animation: slide 1s ease;
  }
}

@media (prefers-color-scheme: dark) {
  :root {
    --surface: #222;
  }
}

@media (prefers-contrast: more) {
  .text {
    font-weight: 600;
  }
}
```

---

## Logical Properties (rtl)

RTL (Right-to-Left) CSS for Hebrew and Arabic. Use when building UI that needs RTL support, fixing RTL layout issues, or auditing CSS for RTL compliance.

## The Golden Rule

NEVER use physical properties. ALWAYS use logical properties.

## Property Mapping

### Spacing Properties

| Physical (❌)                    | Logical (✅)         |
|----------------------------------|----------------------|
| padding-top                      | padding-block-start  |
| padding-bottom                   | padding-block-end    |
| padding-left                     | padding-inline-start |
| padding-right                    | padding-inline-end   |
| padding: [top] [right] [bottom] [left] | padding-block / padding-inline |
| margin-top                       | margin-block-start   |
| margin-bottom                    | margin-block-end     |
| margin-left                      | margin-inline-start  |
| margin-right                     | margin-inline-end    |
| margin: [top] [right] [bottom] [left] | margin-block / margin-inline |

### Positioning Properties

| Physical (❌) | Logical (✅)       |
|---------------|---------------------|
| top           | inset-block-start   |
| bottom        | inset-block-end     |
| left          | inset-inline-start  |
| right         | inset-inline-end    |
| top + bottom  | inset-block         |
| left + right  | inset-inline        |
| top, right, bottom, left | inset    |

### Sizing Properties

| Physical (❌)  | Logical (✅)    |
|----------------|-----------------|
| width          | inline-size     |
| height         | block-size      |
| min-width      | min-inline-size |
| min-height     | min-block-size  |
| max-width      | max-inline-size |
| max-height     | max-block-size  |

### Border Properties

| Physical (❌)              | Logical (✅)              |
|----------------------------|---------------------------|
| border-top                 | border-block-start        |
| border-top-color           | border-block-start-color  |
| border-top-style           | border-block-start-style  |
| border-top-width           | border-block-start-width  |
| border-bottom              | border-block-end          |
| border-bottom-color        | border-block-end-color    |
| border-bottom-style        | border-block-end-style    |
| border-bottom-width        | border-block-end-width    |
| border-top + border-bottom | border-block              |
| border-block [shorthand]   | border-block-color / border-block-style / border-block-width |
| border-left                | border-inline-start       |
| border-left-color          | border-inline-start-color |
| border-left-style          | border-inline-start-style |
| border-left-width          | border-inline-start-width |
| border-right               | border-inline-end         |
| border-right-color         | border-inline-end-color   |
| border-right-style         | border-inline-end-style   |
| border-right-width         | border-inline-end-width   |
| border-left + border-right | border-inline             |
| border-inline [shorthand]  | border-inline-color / border-inline-style / border-inline-width |

### Border Radius Properties

| Physical (❌)         | Logical (✅)          |
|-----------------------|-----------------------|
| border-top-left-radius    | border-start-start-radius |
| border-top-right-radius   | border-start-end-radius   |
| border-bottom-left-radius | border-end-start-radius   |
| border-bottom-right-radius| border-end-end-radius     |

### Text Alignment

| Physical (❌)     | Logical (✅)    |
|-------------------|-----------------|
| text-align: left  | text-align: start |
| text-align: right | text-align: end   |

## Flow relative values

block-start
block-end
inline-start
inline-end
start
end

---

## Scrollbars

Easily customize the scrollbar

```css
.custom-scrollbar {
  scrollbar-color: hotpink transparent;
}

.custom-scrollbar-size {
  scrollbar-width: none;
  scrollbar-width: thin;
}
```

---

## Real-World Example: Modern Component

Here's a card component using many modern CSS features:

```css
/* Cascade layer for organization */
@layer components.card {

  /* Custom properties with @property */
  @property --card-hue {
    syntax: "<number>";
    inherits: false;
    initial-value: 200;
  }

  .card {
    /* Container for responsive design */
    container: card / inline-size;

    /* Logical properties */
    inline-size: 100%;
    padding-inline: var(--space-md);
    padding-block: var(--space-lg);

    /* Modern color system */
    background: light-dark(
      oklch(98% 0.02 var(--card-hue)),
      oklch(20% 0.02 var(--card-hue))
    );

    /* Border with relative color */
    border: 1px solid oklch(from var(--surface) calc(l * 0.9) c h);

    /* Smooth corners */
    border-radius: var(--radius-md);

    /* View transition */
    view-transition-name: --card;

    /* Scroll-driven animation */
    animation: fade-in linear both;
    animation-timeline: view();
    animation-range: entry 0% cover 30%;

    /* Anchor for tooltips */
    anchor-name: --card-anchor;

    /* Transition custom property */
    transition: --card-hue 0.5s var(--ease-spring-3);

    &:hover {
      --card-hue: 280;
    }

    /* Responsive typography in container */
    @container card (width > 30ch) {
      .card__title {
        font-size: clamp(1.5rem, 3cqi, 2.5rem);
        text-wrap: balance;
      }
    }

    @container card (width < 30ch) {
      .card__image {
        aspect-ratio: 16 / 9;
        object-fit: cover;
      }
    }
  }

  .card__title {
    /* Text box trim for optical alignment */
    text-box: trim-both cap alphabetic;
    text-wrap: balance;

    /* Logical margin */
    margin-block-end: var(--space-sm);
  }

  .card__body {
    text-wrap: pretty;
    max-inline-size: 65ch;
  }

  .card__cta {
    /* Inherit font */
    font: inherit;

    /* Accent color */
    accent-color: var(--brand);

    /* Field sizing */
    field-sizing: content;

    /* Logical properties */
    padding-inline: var(--space-md);
    padding-block: var(--space-sm);

    /* Modern color with relative syntax */
    background: oklch(from var(--brand) l c h);
    color: oklch(from var(--brand) 95% 0.05 h);

    &:hover {
      background: oklch(from var(--brand) calc(l * 1.1) c h);
    }

    &:user-invalid {
      outline: 2px solid light-dark(red, #ff6b6b);
    }
  }

  /* Popover tooltip anchored to card */
  .card__tooltip[popover] {
    position-anchor: --card-anchor;
    position-area: block-start;
    position-try-fallbacks: flip-block;

    /* Entry animation */
    @starting-style {
      opacity: 0;
      scale: 0.9;
    }

    transition:
      opacity 0.2s,
      scale 0.2s,
      display 0.2s allow-discrete,
      overlay 0.2s allow-discrete;
  }

  /* Scroll state container queries */
  @supports (container-type: scroll-state) {
    .card__sticky-header {
      container-type: scroll-state;
      position: sticky;
      inset-block-start: 0;

      @container scroll-state(stuck: top) {
        box-shadow: 0 2px 8px oklch(0% 0 0 / 0.1);
      }
    }
  }

  /* Respect user preferences */
  @media (prefers-reduced-motion: reduce) {
    .card {
      animation: none;
      transition: none;
    }
  }

  @media (prefers-contrast: more) {
    .card {
      border-width: 2px;
    }
  }
}

/* Keyframes for scroll animation */
@keyframes fade-in {
  from {
    opacity: 0;
    scale: 0.95;
  }
  to {
    opacity: 1;
    scale: 1;
  }
}
```

### HTML for the Example

```html
<article class="card">
  <img
    class="card__image"
    src="image.jpg"
    alt="Description"
    loading="lazy"
  >

  <h2 class="card__title">Card Title</h2>

  <p class="card__body">
    Card description with pretty text wrapping that avoids orphans.
  </p>

  <button
    class="card__cta"
    popovertarget="card-tooltip"
  >
    Learn More
  </button>

  <div
    class="card__tooltip"
    popover="hint"
    id="card-tooltip"
  >
    Additional information appears here
  </div>
</article>
```

## Canonical Resources

- [CSS Wrapped 2025](https://chrome.dev/css-wrapped-2025/) - The year's CSS features
- [The Coyier CSS Starter](https://frontendmasters.com/blog/the-coyier-css-starter/) - Opinionated modern baseline
- [Adam Argyle's CascadiaJS 2025 Deck](https://cascadiajs-2025.netlify.app/) - (markdownified locally in ./argyle-cacadia-2025-deck.md)
- [Modern CSS in Real Life](https://chriscoyier.net/2023/06/06/modern-css-in-real-life/) - Practical applications

## Usage Guidelines

1. **Prioritize Stability:**
    - Recommend **Newly Available** or **Widely Available** features for production code.
    - Use **Limited Availability** features with progressive enhancement, graceful degredation, or `@supports`. Or ask the user how they want to handle it.

2. **Use the web platform:**
    - Always prefer standard CSS solutions over JavaScript libraries for layout, animation, and interaction (e.g., use CSS Masonry instead of Masonry.js, Popover API instead of custom tooltip scripts).

3. **Code Style:**
    - Use modern color spaces (`oklch`) for new palettes.

## Checking Browser Support: Baseline

**What is Baseline?** A unified way to understand cross-browser feature availability. Features are marked as:

- **Widely Available:** Supported in the last 2.5 years of all major browsers
- **Newly Available:** Available in all major browsers
- **Limited Availability:** Not yet in all browsers

### How to Check Baseline Status

0. BEST: Fetch https://web-platform-dx.github.io/web-features-explorer/groups/ and find the feature in there, then fetch it's detail page.
1. **Can I Use:** [caniuse.com](https://caniuse.com) shows Baseline badges at the top of each feature
2. **MDN:** Look for the Baseline badge in the browser compatibility table
3. **web.dev:** Feature articles include Baseline status

**Remember:** Always check Baseline status, use `@supports` for cutting-edge features, and respect user preferences with media queries. Modern CSS is about progressive enhancement and building resilient interfaces that work for everyone.
