# AstroFlow

A Webflow-inspired development workflow built on Astro 5 — with Client-First v2 CSS conventions, scoped component styles, and a design system that keeps your whole site consistent.

Built for developers who want Webflow's design clarity without the vendor lock-in.

---

## Why AstroFlow?

**Consistency without constraints.** Client-First v2 naming conventions give every section a shared visual language — same spacing rhythm, same color tokens, same layout structure — while Astro's scoped styles keep component internals isolated and collision-free.

**Build sections like Webflow, ship like a developer.** Each section is self-contained: drop it in, pass your brand color, done. The global CSS system handles the rest.

**AI-friendly by design.** The included `CLIENT_FIRST_SYSTEM.md` is a context document you paste into any AI prompt. Every generated section will automatically follow your naming conventions, use your brand tokens, and respect the global/scoped split.

---

## Installation

```bash
git clone https://github.com/your-username/astroflow.git my-project
cd my-project
npm install
npm run dev
```

Visit `http://localhost:4321/styleguide` to see all utility classes, and `/sandbox` to start building.

---

## Set your brand color

One line in `src/styles/client-first.css`:

```css
:root {
  --color--brand: #2563eb; /* ← your color here */
}
```

Everything — buttons, accents, hover states — updates across the whole site.

---

## Building a section

```astro
<section class="section_hero section-style-light section-spacing-large">
  <div class="padding-global">
    <div class="container-large">
      <h1 class="margin-bottom-medium">Your headline</h1>
      <a href="#" class="button-primary is-large">Get started</a>
    </div>
  </div>
</section>

<style>
  /* Internal layout only — rhythm and brand come from global classes */
  .hero_content { max-width: 42rem; }
</style>
```

Global utility classes handle spacing, typography, and color. Scoped styles handle internal arrangement. See `CLIENT_FIRST_SYSTEM.md` for the full reference.

---

## Webflow → Astro 5 Concept Map

| Webflow | Astro 5 equivalent | Key difference |
|---|---|---|
| CMS Collection | Content Collection (Content Layer API) | Schema-validated with Zod; content lives in Markdown/MDX or any headless CMS |
| CMS Collection Fields | Zod schema in `src/content.config.ts` | Type-safe, supports custom validation and references between collections |
| Rich Text field | Markdown / MDX files | MDX lets you embed React/Vue/Svelte components inside content |
| Collection Template page | `src/pages/blog/[...id].astro` | Dynamic routing via `getStaticPaths()`, full control over the template |
| Symbols / Components | `.astro` components | Props, slots, scoped styles, composable — no "unlink" footgun |
| Interactions panel | CSS animations, GSAP, View Transitions API | Code-based, versionable, no 250ms-per-keyframe clicking |
| Page transitions | `<ClientRouter />` + `transition:animate` | Native View Transitions API with cross-browser fallback |
| Webflow Forms | Astro Actions (`src/actions/index.ts`) | Full server-side logic — validate, email, write to DB, anything |
| Logic / Webhooks | API routes (`src/pages/api/*.ts`) | File-based REST endpoints with full Node.js access |
| Webflow Hosting | Netlify / Vercel / Cloudflare Pages | Zero vendor lock-in; one `git push` to deploy |
| Custom code embed | `<script>` tags, framework islands | First-class, bundled, tree-shaken — not a code inject hack |

Every Webflow concept has a direct equivalent — and in most cases a more powerful one. The main shift is from a visual tool with escape hatches to a code-first workflow with visual clarity baked into the conventions.