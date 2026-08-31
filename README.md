# Personal academic website

Static site — plain HTML and CSS, no frameworks, no build step. Served from GitHub Pages
at the repository root.

## Files

| File | Purpose |
|------|---------|
| `index.html` | Home / About |
| `research.html` | Publications, working papers, talks |
| `teaching.html` | Courses and supervision |
| `cv.html` | Curriculum vitae (also link a `assets/cv.pdf`) |
| `contact.html` | Email, office, profiles |
| `style.css` | Single shared stylesheet |
| `assets/` | `portrait.jpg`, `cv.pdf`, and any other static files |
| `.nojekyll` | Tells GitHub Pages to serve files as-is |

## Editing

- The name, title, nav, and footer are copied into each `.html` file. Change them in all
  five pages when they change (search-and-replace `Tao Huang`).
- Content marked with placeholder text (`Placeholder Law Review`, `University Name`,
  `Room 0000`, `#` links, ORCID zeros) is meant to be filled in.
- **Adding a publication:** in `research.html`, copy one `<li class="pub"> … </li>`
  block, edit the citation inside `<span class="pub-cite">`, and keep only the
  `<a>` links (SSRN / Journal / PDF) you need. Full instructions are in a comment
  at the top of the publications section.
- Colours and layout width are variables at the top of `style.css` (`:root`).
- Headings use **Source Serif 4** from Google Fonts (loaded with `preconnect` +
  `display=swap`, Georgia fallback). To go system-only, delete the three font
  `<link>`s in each page's `<head>` and drop `"Source Serif 4"` from `--font-serif`.
- A dark-mode palette is already defined via `prefers-color-scheme`.

## Preview locally

Open `index.html` directly in a browser, or run a local server from this folder:

```
python -m http.server 8000
```

then visit <http://localhost:8000>.

## Deploy on GitHub Pages

1. Commit and push these files to the default branch of the repo.
2. In the repo: **Settings → Pages → Build and deployment**.
3. Set **Source** to *Deploy from a branch*, branch = your default branch, folder = `/ (root)`.
4. Save. The site publishes at `https://<user>.github.io/<repo>/` (or your custom domain).
