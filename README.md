# OpenMLRL.github.io

Static site for the OpenMLRL collective built with [Hugo](https://gohugo.io/) and
the [`hugo-theme-gallery`](https://themes.gohugo.io/themes/hugo-theme-gallery/)
template. The repository mirrors the layout that powers the `docs/` site inside
[`OpenMLRL/CoMLRL`](https://github.com/OpenMLRL/CoMLRL/tree/main/docs): only the
source files live in Git, while the rendered HTML is produced in CI.

## Local development

1. Install the Hugo **extended** binary (already available via Homebrew).
2. Pull the theme if needed: `git submodule update --init --recursive`.
3. Start the hot-reload server:

   ```bash
   hugo server --buildDrafts
   ```

   The site becomes available at <http://localhost:1313>. Hugo keeps the build
   in memory, so no files inside `public/` are written to disk.

To create a production build locally, run `hugo --gc --minify`. The output still
lands inside `public/`, which is ignored by Git so it will not be committed by
mistake.

## CoMLRL docs

The GitHub Pages workflow clones [`OpenMLRL/CoMLRL`](https://github.com/OpenMLRL/CoMLRL),
builds its Hugo docs site with `HUGO_RELATIVEURLS=false hugo --gc --minify -s docs -b "$SITE_BASE_URL/CoMLRL/"`,
runs this gallery site with `hugo --gc --minify`, and finally copies the generated
`docs/public/` folder straight into the deployment directory (`public/CoMLRL/`)
before uploading to Pages. No CoMLRL HTML or caches live in either repository; they
only exist inside the CI workspace and the published artifact. For local previews,
run `hugo server -s docs` in the CoMLRL repo independently (it serves on another port),
and preview this gallery via `hugo server` as usual.
