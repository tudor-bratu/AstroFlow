# Client-First v2 System — AI Context Document

> Include this file in your AI prompts when generating new sections or components.
> This ensures the AI respects Client-First naming, brand colors, and layout structure.

---

## Philosophy — Best of Both Worlds

This project combines Client-First v2 with Astro's scoped styles. Understanding the split is essential before writing any code.

**The core question:** Is this style about *site-wide rhythm*, or about *how this component arranges its own elements*?

| Concern | Where it lives | Why |
|---|---|---|
| Section vertical padding | Global utility class (`section-spacing-*`) | Consistency across the whole site — every section breathes the same way |
| Horizontal page padding | Global utility class (`padding-global`) | One value to rule all page edges |
| Container max-width | Global utility class (`container-*`) | Unified content width everywhere |
| Typography scale & color | Global utility class (`text-size-*`, `text-color-*`) | Brand consistency |
| Buttons | Global utility class (`button-*`) | Reused everywhere |
| Internal grid/flex layout | **Scoped `<style>` block** | Each section arranges its own children differently — this is not a global concern |
| Internal gaps between elements | **Scoped `<style>` block, using tokens** | `gap: var(--spacing--large)` — scoped but still pulls from the shared scale |
| Decorative styles (borders, radii, shadows) | **Scoped `<style>` block** | Component-specific, not a site-wide system |

**The key insight:** Scoped styles in Astro prevent naming collisions. Global tokens (`--spacing--*`, `--color--*`) maintain visual consistency. These solve different problems and work together — you're not choosing between them.

```astro
<!-- ✅ Global utilities = site rhythm and brand -->
<section class="section_features section-style-subtle section-spacing-large">
  <div class="padding-global">
    <div class="container-large">

      <!-- ✅ Scoped styles = internal arrangement -->
      <div class="features_grid">...</div>

    </div>
  </div>
</section>

<style>
  /* Internal layout only — no padding, no colors, no typography here */
  .features_grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: var(--spacing--large); /* scoped, but still uses the shared token */
  }
</style>
```

**What NOT to do:**
```astro
<!-- ❌ Don't define section spacing in scoped styles -->
<style>
  .section_features { padding: 6rem 0; } /* breaks site-wide consistency */
</style>

<!-- ❌ Don't put internal grid layout in global classes -->
<!-- Global classes are for properties reused everywhere, not one-off arrangements -->
```

---

## Project Setup

- **CSS System:** Client-First v2 by Finsweet
- **CSS File:** `src/styles/client-first.css`
- **Framework:** Astro (no Tailwind — pure CSS with Client-First utility classes)
- **Brand color variable:** `--color--brand` (edit in client-first.css :root)

---

## The Golden Rules

1. **Utility classes = DASHES only** → `text-color-brand`, `margin-bottom-large`
2. **Custom classes = UNDERSCORE** → `hero_heading`, `nav_logo-wrapper`
3. **Combo classes = `is-` prefix** → `button-primary is-large`
4. **Sections = `section_[name]`** → `section_features`, `section_hero`
5. **No abbreviations** → `pricing_background-texture` not `pricing-bg-tx`
6. **Read the class and know its purpose**

---

## Core HTML Structure (always use this)

```html
<section class="section_[name] section-style-[variant] section-spacing-[size]">
  <div class="padding-global">
    <div class="container-[size]">
      <!-- content -->
    </div>
  </div>
</section>
```

---

## Section Combo Classes

### section-style-* (background/color variants)
- `section-style-light` → white background
- `section-style-subtle` → light grey background
- `section-style-dark` → black background, white text
- `section-style-brand` → brand color background, white text

### section-spacing-* (vertical padding)
- `section-spacing-small` → 48px
- `section-spacing-medium` → 64px
- `section-spacing-large` → 96px
- `section-spacing-huge` → 128px

---

## Container Classes

- `container-small` → max-width 768px
- `container-medium` → max-width 1024px
- `container-large` → max-width 1280px

---

## Typography Utility Classes

```
heading-style-h1  heading-style-h2  heading-style-h3
heading-style-h4  heading-style-h5  heading-style-h6

text-size-xsmall  text-size-small   text-size-medium
text-size-large   text-size-xlarge  text-size-2xlarge

text-color-primary    text-color-secondary  text-color-muted
text-color-inverted   text-color-brand

text-weight-regular   text-weight-medium
text-weight-semibold  text-weight-bold

text-style-italic  text-style-uppercase  text-style-label
```

---

## Spacing Utility Classes

```
margin-top-[xsmall|small|medium|large|xlarge|xxlarge|huge]
margin-bottom-[xsmall|small|medium|large|xlarge|xxlarge|huge]
margin-vertical-[xsmall|small|medium|large|xlarge|xxlarge]

padding-top-[small|medium|large|xlarge|xxlarge|huge]
padding-bottom-[small|medium|large|xlarge|xxlarge|huge]
padding-vertical-[small|medium|large|xlarge|xxlarge]
```

---

## Button Classes

```html
<!-- Variants -->
<a class="button-primary">Primary</a>
<a class="button-secondary">Secondary</a>
<a class="button-neutral">Neutral</a>
<a class="button-dark">Dark</a>

<!-- Size combos -->
<a class="button-primary is-small">Small</a>
<a class="button-primary is-large">Large</a>
<a class="button-primary is-xlarge">XLarge</a>
<a class="button-primary is-full-width">Full Width</a>
```

---

## Background Colors

```
background-color-primary    (white)
background-color-secondary  (light grey)
background-color-dark       (near-black)
background-color-brand      (brand color)
background-color-brand-light (tint)
```

---

## Show/Hide

```
hide              display: none (all)
hide-desktop      hidden on desktop, visible on tablet+
hide-tablet       hidden on tablet and below
hide-mobile       hidden on mobile
```

---

## Custom Class Naming (for your sections)

When writing a new section, name your custom elements like this:

```
[section-name]_[element-description]

Examples:
  features_grid
  features_card-wrapper
  features_icon-wrapper
  testimonials_quote-text
  pricing_plan-wrapper
  pricing_price-amount
  hero_heading-accent
  cta_button-group
```

---

## Brand Colors (edit in :root)

```css
--color--brand:       #2563eb;   /* ← CHANGE THIS */
--color--brand-dark:  #1d4ed8;
--color--brand-light: #eff6ff;
```

---

## Sample Section Template

```astro
---
// MySection.astro
---

<section class="section_my-section section-style-light section-spacing-large">
  <div class="padding-global">
    <div class="container-large">

      <!-- Global utility classes handle typography, color, spacing between siblings -->
      <div class="my-section_header margin-bottom-xlarge">
        <p class="text-style-label text-color-brand margin-bottom-small">Label</p>
        <h2 class="heading-style-h2 max-width-medium">Section heading</h2>
        <p class="text-color-secondary text-size-large max-width-medium margin-top-small">
          Subtext here
        </p>
      </div>

      <!-- Scoped custom class handles internal arrangement -->
      <div class="my-section_grid">
        <!-- cards, items, etc. -->
      </div>

    </div>
  </div>
</section>

<style>
  /*
    SCOPED STYLES — internal arrangement only.
    Never define section padding, brand colors, or typography here.
    Always use CSS tokens for any values that should feel consistent.
  */
  .my-section_grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: var(--spacing--large);     /* token, not a hardcoded value */
  }

  .my-section_card {
    padding: var(--spacing--large);
    border-radius: var(--radius-large);
    border: 1px solid var(--color--border);
  }

  @media (max-width: 767px) {
    .my-section_grid { grid-template-columns: 1fr; }
  }
</style>
```

**Quick checklist before writing a style:**
- Section padding? → `section-spacing-*` (global)
- Text color or size? → `text-color-*` / `text-size-*` (global)
- Space between two sibling elements? → `margin-bottom-*` (global)
- Grid/flex layout inside the section? → scoped `<style>` using `var(--spacing--*)` tokens
- Decorative (border, radius, shadow)? → scoped `<style>`
