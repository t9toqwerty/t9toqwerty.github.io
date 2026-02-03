**Repository Overview**
- **Purpose:** Personal/static portfolio site served via GitHub Pages (CNAME present). The site is a mostly static HTML/CSS/JS project with legacy assets in `old/` and source SCSS in `sass/`.

**Where to Look First**
- `index.html` — primary page and entrypoint; includes inlined hero CSS and content blocks used across the site.
- `js/init.js` — initializes the responsive breakpoints via `skel.init(...)` and maps breakpoints to `css/style-*.css` files.
- `css/` — compiled styles used in production (multiple `style-*.css` files for breakpoints).
- `sass/` — SCSS source for the styles. Prefer editing here and compiling to `css/` if you need source edits.
- `old/` — legacy templates and third-party libs (Bootstrap, jQuery). Use mainly for reference — production uses root-level files.

**Big-picture architecture & important patterns**
- Static site (no build tooling required by default). Most pages are single-file HTML with supporting CSS and JS in `css/` and `js/`.
- Responsive CSS is split into multiple files and wired by `js/init.js` using the `skel` JS breakpoint loader. To change breakpoint-specific styling, update the corresponding `css/style-*.css` (or the SCSS source and recompile all outputs).
- Assets (images/fonts) live under `css/images/`, `images/`, and `fonts/`. Pay attention to relative paths in `index.html` and CSS.
- External dependencies (CDNs): Devicons, Font Awesome, Google Fonts and Google Analytics are loaded from CDNs in `index.html`.

**Developer workflows & common commands**
- Quick local preview: serve the repo root and open a browser:

  ```bash
  cd /path/to/repo
  python3 -m http.server 8000
  open http://localhost:8000
  ```

- If you edit SCSS, compile to `css/` (install Dart Sass if not available):

  ```bash
  # install (macOS / Homebrew): brew install sass/sass/sass
  sass --no-source-map sass/style.scss css/style.css
  # or watch changes:
  sass --watch sass:css
  ```

- Deployment: push to the `main` (or default) branch; the presence of `CNAME` indicates GitHub Pages is used. Confirm repository Pages settings in the GitHub UI if unsure.

**Project-specific conventions**
- Prefer editing `sass/` files for style changes and compile into the existing `css/style-*.css` outputs. The runtime loads the breakpoint-specific CSS files named in `js/init.js`.
- Keep `index.html` as the canonical content source (single-page). Avoid creating multiple HTML entry points unless adding a new page; link them from nav.
- `old/` contains past versions and can be used to copy components, but do not modify `old/` when making production edits.

**Debugging tips & gotchas**
- Duplicate runtime handlers: `window.onload`, `ontouchmove`, and `onorientationchange` appear both inline and inside `js/init.js`. If you see unexpected behavior while editing, search for duplicated event bindings.
- Skel breakpoint mapping means changing a `style-*.css` file may require a full page reload (not just devtools live-reload) to pick up different breakpoint files.
- Assets referenced with relative paths (e.g., `css/images/bg.jpg`) must be kept in their expected folders when moving files or testing locally.

**Examples in this repo**
- Responsive breakpoint mapping: `js/init.js` uses:

  ```js
  skel.init({ breakpoints: { 'mobile': { range: '-736', href: 'css/style-mobile.css' }, ... } });
  ```

- Inline hero styles are in the top of `index.html` (search for `#wrapper` and `#bg`). Small visual changes are often made inline rather than across `css/` files.

**When to change files in `old/`**
- Use `old/` for reference, especially to recover Bootstrap-based components or to inspect previous JS patterns. Do not deploy from `old/` unless intentionally switching the site to the legacy template.

**If you need to add tooling**
- If contributors prefer an npm-based workflow, add a minimal `package.json` and scripts for SASS compilation and an http-server. Keep changes minimal and document them in the repo README.

Feedback
- Tell me which sections are unclear or where you'd like more examples (for instance, add a `sass -> css` example for a specific file or note about GitHub Pages settings).
