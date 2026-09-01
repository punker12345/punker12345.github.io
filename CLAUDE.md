# Project: personal academic website

Static personal/academic site for **Tao Huang**, Assistant Professor of Law at
City University of Hong Kong, working on AI governance and public policy.

## Hard constraints

- **Plain HTML + one CSS file. No frameworks, no build step, no JS tooling.**
  The only script is a two-line inline snippet that sets the footer year.
- Served by **GitHub Pages from the repository root**, so `index.html` stays in
  the root and all asset paths are relative (`style.css`, `assets/...`).
- `.nojekyll` is present so Pages serves files as-is.
- Every page is a complete standalone HTML file. The header, nav, and footer are
  **copied into each page** — there is no include mechanism. When any of them
  changes, update all five pages.

## Files

| File | Page |
|------|------|
| `index.html` | Home / About |
| `research.html` | Publications — articles, books, presentations |
| `teaching.html` | Teaching |
| `cv.html` | CV (embeds `assets/cv.pdf` in the browser PDF viewer) |
| `contact.html` | Contact |
| `style.css` | The single shared stylesheet |
| `assets/` | `portrait.jpg`, `cv.pdf`, other static files |

## Design decisions

The agreed direction: **restrained and text-forward — it should read like a
well-set book page, not a startup landing page.** Do not add hero banners,
card grids, background tints, icons, or multi-column feature blocks.

### Typography

- **Headings:** `Source Serif 4` (Google Fonts), weight 600. Fallback stack
  `Georgia, "Times New Roman", Times, serif`.
  - Loaded in every page `<head>` with `preconnect` to `fonts.googleapis.com`
    and `fonts.gstatic.com` (crossorigin), then the stylesheet link with
    `display=swap`. Weights fetched: 400, 600, and italic 400.
  - To switch to system-only: delete the three font `<link>`s from each
    `<head>` and remove `"Source Serif 4"` from `--font-serif` in `style.css`.
- **Body text:** also `--font-serif` (Source Serif 4 / Georgia) — the page is
  serif throughout for the book feel. Size `1.125rem`, `line-height: 1.75`,
  `font-feature-settings: "onum" 1, "kern" 1` (old-style figures).
- **Sans-serif (`--font-sans`, system stack)** is used only for secondary
  furniture: the nav, the tagline under the name, `.meta` lines, CV dates,
  the contact table labels, and the footer.
- Heading sizes: `h1` 1.95rem, `h2` 1.35rem (with `3rem` top margin), `h3`
  1.1rem. Heading `line-height: 1.25`.
- `.lead` is the one intro paragraph per page: `1.25rem`, `line-height: 1.6`.

### Color

- **One accent color only:** deep maroon `--color-accent: #7a1f2b`
  (hover `#571017`). Used for links, the `aria-current` nav underline, and the
  focus outline on the skip link. Nothing else is colored.
- Text `#1c1b1a`, muted `#605c58`, borders `#e6e3df`, background `#ffffff`.
- A dark-mode palette is defined under `@media (prefers-color-scheme: dark)`
  in `:root` — same structure, lighter maroon accent `#e08e99`. Keep both
  palettes in sync when changing colors.
- Separation between sections is done with hairline `1px` borders in
  `--color-border`, never with fills or shadows.

### Layout constraints

- **Single centered column, `--max-width: 43.75rem` (~700px)**, applied
  identically to `.site-header`, `.site-main`, and `.site-footer`.
  `padding-inline: 1.5rem` on all three.
- `.site-main` vertical padding: `3rem` top, `5rem` bottom.
- Fully responsive with no breakpoints needed for the main column (it just
  narrows). One small `max-width` rule exists (`.cv-entry` grid collapse at
  `32rem`); the nav wraps naturally via `flex-wrap`.
- **CV page:** `cv.html` embeds `assets/cv.pdf` via
  `<object class="cv-pdf">` (full column width, `85vh` tall) plus a
  `download` link above it. The browser's PDF viewer supplies zoom and
  download. The old textual layout used `.cv-entry` — a 2-column CSS grid,
  date beside detail — whose CSS is kept for reference but is currently
  unused.
- Structural exception to "no grids": **Contact** — `.contact-table` is a
  real `<table>`, label beside value.
- Header: name as `.site-title` (serif, 1.5rem), `.site-tagline` beneath it,
  then `.site-nav` (a `<ul>` of links). Current page marked with
  `aria-current="page"`, which draws the maroon underline.
- Every page has a visually-hidden `.skip-link` to `#main` as the first
  element in `<body>`.

## Publications

Live in `research.html` under an `<h1>Publications</h1>` (no intro
paragraphs), grouped into `<h2>` sections — **Journal articles**,
**Book reviews**, **Edited books**, **Translated books** — each an
`<ol class="pub-list">`. Entries are newest first within a section.

### Citation format

Chicago-style, bibliography form, but with the author's name in natural
order (`Tao Huang`, not `Huang, Tao`), as a single run of plain text inside
`<span class="pub-cite">`. Journal and book titles go in `<em>`; article and
chapter titles are in curly quotes (`&ldquo;` / `&rdquo;`), not italic.

- **Journal article:**
  `Tao Huang. &ldquo;Article Title.&rdquo; <em>Journal Name</em> 12, no. 3 (2025): 145&ndash;198.`
- **Co-authored:**
  `Tao Huang and Co-Author Name. &ldquo;Article Title.&rdquo; <em>Journal Name</em> 8, no. 1 (2024): 1&ndash;42.`
- **Book review:**
  `Tao Huang. Review of <em>Book Title</em>, by Book Author (Publisher, 2022). <em>Journal Name</em> 53 (2023): 1277.`
- **Edited book:**
  `Tao Huang and Co-Editor Name, eds. <em>Book Title</em> [Romanized Original Title], vol. 3. City: University Press, 2021. In Chinese.`
- **Translated chapter:**
  `Tao Huang, trans. &ldquo;Chapter Title.&rdquo; Chap. 6 in <em>Book Title</em>, edited by Editor Name. Publisher, 2023. Translated into Chinese.`

Use `&ndash;` for page ranges. When only the volume and first page are known
(common for law reviews cited Bluebook-style), `<em>Journal Name</em> 58
(2025): 1325.` is fine. The author's own name is written out (`Tao Huang.`)
in every entry rather than replaced with a dash.

### How to add a publication entry

1. In `research.html`, find the right `<h2>` section (`<ol class="pub-list">`).
2. Copy an entire existing `<li class="pub"> … </li>` block.
3. Replace the text in `<span class="pub-cite">` with the new citation, in the
   format above.
4. In `<span class="pub-links">`, keep only the `<a>` links that apply and
   delete the rest. The usual set, in this order: **Journal** (or
   **Publisher** for a book), then **PDF**.
5. Put the block in date order (newest first) within its section.

Skeleton:

```html
<li class="pub">
  <span class="pub-cite">Tao Huang. &ldquo;Title.&rdquo; <em>Journal Name</em> 00, no. 0 (YEAR): 00&ndash;00.</span>
  <span class="pub-links">
    <a href="URL">Journal</a>
    <a href="URL">PDF</a>
  </span>
</li>
```

Styling is automatic: `.pub` gives a hanging indent
(`padding-left: 1.5rem; text-indent: -1.5rem`); `.pub-links` renders small,
sans-serif, non-wrapping, spaced by `0.9rem`. No per-entry CSS is ever needed.

## Local preview

There is **no Python or Node on this machine** (`python` is only the Windows
Store stub). To preview, run any static file server over the folder — e.g. a
short PowerShell `System.Net.HttpListener` script bound to
`http://localhost:8000/` — or just open `index.html` directly in a browser
(the Google Fonts link still works from `file://`).

## Placeholder content

All prose is currently placeholder (`Placeholder Law Review`, `University
Name`, `Room 0000`, `#` links, ORCID zeros, a `Co-Author Name`). The real
content will be supplied by the site owner. Keep the structure and formatting;
only swap the text.
