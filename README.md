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
