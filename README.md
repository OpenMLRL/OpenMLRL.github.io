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

## CoMLRL docs import

The GitHub Pages workflow clones [`OpenMLRL/CoMLRL`](https://github.com/OpenMLRL/CoMLRL),
builds its Hugo docs site with `HUGO_RELATIVEURLS=false hugo --gc --minify -s docs -b "$SITE_BASE_URL/CoMLRL/"`,
and copies the generated `docs/public/` folder into `static/CoMLRL/` right before
running `hugo` for this site. No CoMLRL HTML lives in Git anymore; the files only
exist in the CI workspace and the published Pages artifact. If you need to test
those docs locally, run `HUGO_RELATIVEURLS=false hugo --gc --minify -s docs -b "http://localhost:1313/CoMLRL/"` from the CoMLRL repo, copy the resulting `docs/public/`
into `static/CoMLRL/`, and then start `hugo server`.
