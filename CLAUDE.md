# Cannanova — Project Guidelines

Static landing-page website. Plain HTML/CSS (no build step). Edits flow through
Claude Code → PR → preview → merge to `main` to publish.

When doing any visual or layout work, invoke the **`frontend-design`** skill
(in `.claude/skills/`) for aesthetic direction — but every choice it produces
must still conform to the design system defined below.

---

## Design System (single source of truth)

Develop the whole site against **one** design system and keep it consistent
across every page. Define the system once, then reuse it — never re-invent
styles per page.

### Where the system lives

- All design tokens live as **CSS custom properties** in a single global
  stylesheet (e.g. `css/tokens.css`), imported by every page.
- Shared component styles live in `css/components.css`. Pages may add only
  page-specific layout, never new colors, fonts, or spacing values.
- **Never hardcode** a color, font, size, radius, or shadow inline or per-page.
  If a value isn't a token, add it to the token file first, then reference it.

### Tokens to define and always use

- **Color:** brand, accent, neutrals, background, text, success/error/warning.
  Reference only via `var(--color-*)`.
- **Typography:** a deliberate display + body pairing, a fixed type scale
  (e.g. `--font-size-xs … --font-size-3xl`), weights, and line-heights.
  Set the type scale once; do not introduce ad-hoc font sizes.
- **Spacing:** one spacing scale (`--space-1 … --space-12`). All margins,
  paddings, and gaps come from it — no magic pixel values.
- **Radius, shadows, borders, breakpoints, motion durations/easings:** tokenized
  and reused.

### Consistency rules

- Same header and footer on every page, from a shared source — do not copy-paste
  divergent variants.
- Buttons, links, cards, form fields, and section spacing look and behave
  identically site-wide. One component = one canonical style.
- A control says exactly what it does; an action keeps the same name through the
  whole flow (the button that says "Add to cart" leads to a "cart" — not "bag").
- Responsive at every breakpoint; respect `prefers-reduced-motion`; keyboard
  accessible (visible focus states, logical tab order).
- Before adding a new pattern, check whether an existing component covers it.
  Extend the system deliberately; don't fork it.

---

## SEO — ecommerce landing page best practices

Apply on every page. SEO is a requirement, not an afterthought.

### Per-page metadata

- One unique, descriptive `<title>` per page (~50–60 chars), front-loading the
  primary keyword and including the brand.
- Unique `<meta name="description">` (~150–160 chars) with a clear value
  proposition and a soft call to action.
- One — and only one — `<h1>` per page stating the page's core offer. Logical
  `<h2>/<h3>` hierarchy below it; never skip levels for styling.
- Canonical tag (`<link rel="canonical">`) on every page to avoid duplicates.
- `<meta name="robots">` set appropriately (index,follow for public pages).

### Social / sharing

- Open Graph tags: `og:title`, `og:description`, `og:image`, `og:url`,
  `og:type` (`product` for product pages, `website` otherwise).
- Twitter Card tags (`summary_large_image`).
- `og:image` is a real, correctly sized image (1200×630) that exists in the repo.

### Structured data (JSON-LD)

- Add schema.org JSON-LD appropriate to the page:
  - `Organization` + `WebSite` on the home page.
  - `Product` with `offers` (price, currency, availability) on product/offer
    pages; `AggregateRating`/`Review` only if the data is real.
  - `BreadcrumbList` where breadcrumbs exist.
- Never fabricate ratings, prices, or stock — structured data must match the
  visible page, or it's a penalty risk.

### Content & on-page

- Write copy for humans first; weave the primary keyword naturally into the H1,
  first paragraph, and one subheading. No keyword stuffing.
- Every image has meaningful, specific `alt` text (also helps accessibility).
- Descriptive, lowercase, hyphenated URLs (`/cbd-oil` not `/page2`).
- Internal links between related pages with descriptive anchor text.
- A clear primary CTA above the fold on the landing page.

### Technical SEO

- Fast: optimized/compressed images, `width`/`height` on `<img>` to prevent
  layout shift, lazy-load below-the-fold media (`loading="lazy"`).
- Mobile-first and fully responsive (Google indexes mobile).
- Semantic HTML5 landmarks (`header`, `nav`, `main`, `footer`, `section`).
- `lang` attribute on `<html>`.
- Maintain `sitemap.xml` and `robots.txt` at the site root; add new pages to the
  sitemap.
- Valid, accessible markup — accessibility and SEO reinforce each other.

### Compliance note

This site sells cannabis-related products. Keep claims factual and avoid
unverifiable health/medical claims in copy or structured data — both for legal
compliance and because Google penalizes deceptive ecommerce content.

---

## Workflow

- No build step: edit HTML/CSS directly, open in a browser to verify.
- Always work on a branch and open a PR; share the preview link before merging.
- Keep changes small and reviewable so a non-technical reviewer can sanity-check
  the rendered preview.
