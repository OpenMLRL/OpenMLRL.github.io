# OpenMLRL.github.io

Static site for the OpenMLRL collective built with [Hugo](https://gohugo.io/).
The homepage and Research page use local templates and styles; the
[`hugo-theme-gallery`](https://themes.gohugo.io/themes/hugo-theme-gallery/)
submodule supplies fallback templates. GitHub Pages builds the site in CI and
bundles the separately built CoMLRL documentation under `/CoMLRL/`.

## Homepage and research

- `layouts/_default/home.html` defines the homepage, with introductory copy in `content/_index.md`.
- `assets/css/custom.css` controls typography, layout, and responsive breakpoints. Inter is hosted locally in `assets/fonts/`, with its license.
- `data/publications.yaml` contains the publications shown on the `/research/` page.
- `layouts/partials/collective-visual.html` contains the SVG collaboration illustration. Its animation respects reduced-motion preferences.

## Local development

1. Install the Hugo **extended** binary (already available via Homebrew).
2. Pull the theme if needed: `git submodule update --init --recursive`.
3. Start the hot-reload server:

   ```bash
   hugo server --port 1314
   ```

   The site becomes available at <http://localhost:1314>. Hugo keeps the build
   in memory, so no files inside `public/` are written to disk.

To create a production build locally, run `hugo --gc --minify`. The output still
lands inside `public/`, which is ignored by Git so it will not be committed by
mistake.

## CoMLRL docs

The Pages workflow publishes both this website and the CoMLRL documentation.
It runs on website pushes, manual dispatches, and `comlrl-docs-updated` events
sent by CoMLRL after documentation checks pass. Those cross-repository events
use the existing `OPENMLRL_TOKEN` secret in CoMLRL, with Contents write access
to this repository. Both repositories' workflow changes must be pushed to `main`
to enable the complete flow.

Each run resolves one revision of CoMLRL's current `main` branch. Six parallel jobs reuse cached
translations for Simplified Chinese, Japanese, Korean, Spanish, French, and
German. Missing or changed pages are translated on the GitHub runner using
Ollama and `qwen2.5-coder:3b`; no translation API key is needed. A cold build can
take hours on CPU. Cached builds skip Ollama entirely when translations are
current, and valid completed pages are cached even if a later translation fails.

The final job runs `docs/scripts/build.py --multilingual --cache-only`, validates
all seven languages, and bundles the generated site under `public/CoMLRL/`.
Only a successful build is deployed; translation failures leave the previous
site available. Translations are kept in Actions caches and temporary artifacts,
not committed to Git. Legacy files in `static/CoMLRL/` are replaced in the final
deployment artifact, so they cannot overwrite the multilingual build.

For a local multilingual preview, run
`python3 docs/scripts/preview.py --multilingual` in the CoMLRL repository and
preview this website with `hugo server --port 1314`.
