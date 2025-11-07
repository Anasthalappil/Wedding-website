## Purpose
Short, actionable guidance for an AI coding agent working on this repository (a static wedding invitation web template).

## Big picture
- This is a static HTML/CSS/JS template (no build system or package.json). The site is a single-page invitation: assets live under `css/`, `js/`, `img/`, and source styles in `scss/`.
- Primary entry point: `index.html` — it wires fonts, JS, and styles. Styling is produced from SCSS: `scss/style.scss` → `css/style.min.css` (the repo includes both source and compiled files).
- Key interactive pieces are small, self-contained JS files in `js/` (notably `countdown.js` and Owl Carousel integration via `owl.carousel*.js`).

## Quick developer workflows (concrete)
- Recompile SCSS (use Dart Sass or Koala as the original author noted). Example commands:
  - Compile compressed CSS (overwrite the min file):
    ```powershell
    sass --style=compressed scss/style.scss css/style.min.css
    ```
  - For iterative development, compile expanded CSS then minify with your tool of choice.
- Serve locally to test (open `index.html` directly in the browser or run a static server):
  - Python simple server (from repo root):
    ```powershell
    python -m http.server 8000
    ```
  - Or with a lightweight node tool (if available): `npx serve .`

## Important project-specific conventions & patterns
- Edit SCSS, not `css/style.min.css`. The minified CSS is a generated artifact. The canonical sources are under `scss/` (`_theme.scss`, `style.scss`, etc.).
- Image paths in CSS use relative references from `css/`, e.g. `background-image: url("../img/an5.jpeg")` — keep images in `img/` and reference them relatively from within `css/`.
- Fonts are loaded via Google Fonts in `index.html` (e.g. `Great Vibes`, `Lato`, `Playfair Display`, `Montserrat`) — don’t hard-delete those links without confirming fallback styles.
- JS is vanilla/jQuery-based. Widgets used:
  - Owl Carousel: `js/owl.carousel.min.js` and related CSS in `css/`.
  - Countdown: `js/countdown.js` — `index.html` defines the deadline and calls `initializeClock("clockdiv", deadline)`.

## Integration points / external deps
- External CDN assets loaded in `index.html`: jQuery, Google Fonts, Animate.css, Font Awesome kit.
- The README mentions Koala (a GUI SCSS compiler). You can use it or any Sass toolchain.
- The live preview uses GitHub Pages (see README link). Deploys are static pushes of the repository.

## Concrete examples and quick fixes
- If you change layout styles, update `scss/style.scss`, then run the Sass command above. Verify the output by opening `index.html`.
- If the hero/banner image appears missing, confirm `css/style.min.css` contains `background-image: url("../img/an5.jpeg")` and the file exists at `img/an5.jpeg` relative to repo root.
- To change the countdown target edit `index.html` where `const deadline = new Date("November 09, 2025 16:30:00");` is defined, or switch to programmatic config in `js/countdown.js`.

## Debugging tips specific to this repo
- Missing fonts? Check `index.html` Google Fonts links and network tab. Fallback fonts are defined in `css/style.min.css` (e.g., `Lato`, `Great Vibes`).
- Layout jump or blocked scroll: `#landing` is `position: fixed` and toggled by `.btn-invitation` (which adds `move-gate`). To bypass during debugging, temporarily remove `position: fixed` in DevTools or trigger the button.
- If carousel behaves oddly, inspect initialization in the inline `<script>` inside `index.html` — Owl Carousel options are set there.

## What to avoid
- Don't edit compiled artifacts in `css/` as the sole source of truth — prefer updates in `scss/` and document any manual edits to `css/` if unavoidable.

## PR checklist for small changes
- Update `scss/` sources.
- Compile `css/style.min.css` and include the regenerated file in the PR (or note in PR why you didn't).
- Sanity-check `index.html` in a browser and verify console has no errors.

## If you need more context
- Read `README.md` (project goals and that SCSS was compiled with Koala).
- Open `index.html` to see how scripts and styles are wired together (DOM ids used by JS: `#landing`, `#clockdiv`, `#sticky-navbar`).

If any section above is unclear or you'd like more examples (e.g., sample Sass watch commands, or a minimal Gulp/ npm workflow), tell me what you want and I will iterate.
